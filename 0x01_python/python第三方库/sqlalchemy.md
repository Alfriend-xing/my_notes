# SQLAlchemy
---

>建议在虚拟环境中安装
pip install sqlalchemy
## 一些概念
对象|数据库
---|---
Engin|连接
Session|会话
Model|表
Column|列
Query|查询结果，若干行(SELECT,DELETE,UPDATE)

## 初始化
```python
# 连接数据库
from sqlalchemy import create_engine
engine = create_engine('sqlite:///:test:', echo=True)
check_same_thread=False
或
engine = create_engine('sqlite:///foo.db?check_same_thread=False', echo=True)
#sqlite默认建立的对象只能让建立该对象的线程使用，而sqlalchemy是多线程的所以
#我们需要指定check_same_thread=False来让建立的对象任意线程都可使用。
#否则不时就会报错

# 创建数据库会话
from sqlalchemy.orm import sessionmaker,scoped_session
Session = sessionmaker(bind=engine)
session = Session()

# 创建线程安全的会话
DBSession = scoped_session(sessionmaker(bind=engine,
    twophase=TEST_TWOPHASE, extension=ZopeTransactionExtension()))

# 创建元类
from sqlalchemy.ext.declarative import declarative_base
Base = declarative_base()

```

## 定义模型

```python
from sqlalchemy import Column, Integer, String

class User(Base):
    __tablename__ = 'users'

    id = Column(Integer, primary_key=True)
    name = Column(String)
    fullname = Column(String) 
    password = Column(String)

    def __repr__(self):
       return "<User(name='%s', fullname='%s', password='%s')>" % (self.name, self.fullname, self.password)

```
>repr()方法是非必须的。使用它是为了让对象有更好看的格式。
由基类继承而来的类最少要包括一个 tablename 参数(表名)，还有一列primary key参数(主键)

## 完整模型
```python
class User(Base):
    __tablename__ = 'users'
    id = Column(Integer, Sequence('user_id_seq'), primary_key=True)
    name = Column(String(50))
    fullname = Column(String(50))
    password = Column(String(12))

    def __repr__(self):
        return "<User(name='%s', fullname='%s', password='%s')>" % (
                                self.name, self.fullname, self.password)
```

## 一对多关系
```python
class Book(Base):
    __tablename__ = "books"    # 表名
    id = Column(Integer,Sequence('book_seq'),primary_key=True)
    name = Column(String(50))
    author_id = Column(Integer,ForeignKey('authors.id'))
    author = relationship("Author",backref="books")


class Author(Base):
    __tablename__  = "books"    # 表名
    id = Column(Integer,Sequence('book_seq'),primary_key=True) 
    name = Column(String(50))

# 配置关系后可以使用
oBook = DBSession.query(Book).filter_by(name="Harry Potter and the methods of rationality").first()
oAuthor = oBook.author
# oAuthor现在是一个Author实例. oAuthor.id == oBook.author_id

#it works the other way as well
oAuthor = DBSession.query(Author).filter_by(name="Orsan Scott Card")
for oBook in oAuthor.books:
    print oBook.name

#adding a new book
oNewBook = Book()
oBook.name = "Ender's Game"
oBook.author = oAuthor

#adding a new book in a different way...
oNewBook = Book()
oBook.name = "Ender's Shadow"
oAuthor.books.append(oBook)
```

## 一对一关系
```python
class Parent(Base):
    __tablename__ = 'parent'
    id = Column(Integer,Sequence('p_seq'),primary_key=True) 
    child_id = Column(Integer, ForeignKey('child.id'))
    child = relationship("Child", backref=backref("parent", uselist=False))
    # 如果使用"parent"，则是多对一关系

class Child(Base):
    __tablename__ = 'child'
    id = Column(Integer,Sequence('c_seq'),primary_key=True)


# 配置关系后可以使用
oChild = DBSession.query(Child).get(1)
oParent = oChild.parent

oParent2 = Parent()
oParent.child = Child()
```

## 多对多关系
>多对多关系需要额外的表来创建行之间的映射

方法一
```python
class Category(Base):
    __tablename__ = 'categories'
    id = Column(Integer,Sequence('cat_seq'),primary_key=True) 
    name = Column(String(20))

class Product(Base):
    __tablename__ = 'products'
    id = Column(Integer,Sequence('prod_seq'),primary_key=True) 
    name = Column(String(20))
    
class Map(Base):
    __tablename__ = 'map'
    id = Column(Integer,Sequence('map_seq'),primary_key=True) 
    cat_id = Column(Integer,ForeignKey('categories.id'))
    prod_id = Column(Integer,ForeignKey('products.id'))
```
方法二
```python
map_table = Table('maps', Base.metadata,
    Column('cat_id', Integer, ForeignKey('categories.id')),
    Column('prod_id', Integer, ForeignKey('products.id'))
)

class Category(Base):
    __tablename__ = 'categories'
    id = Column(Integer,Sequence('cat_seq'),primary_key=True) 
    name = Column(String(20))

    products = relationship("Product",
                    secondary=map_table,   # you can also use the string name of the table, "maps", as the secondary
                    backref="categories")

class Product(Base):
    __tablename__ = 'products'
    id = Column(Integer,Sequence('prod_seq'),primary_key=True) 
    name = Column(String(20))
```
用法
```python
#construct a category and add some products to it
oCat = Category()
oCat.name = "Books"

oProduct = Product()
oProduct.name = "Ender's Game - Orsan Scott Card"
oCat.products.append(oProduct)

oProduct = Product()
oProduct.name = "Harry Potter and the methods of Rationality"
oProduct.categories.append(oCat)

# interact with products from an existing category
for oProduct in oCat.products:
    print oProduct.name
    
#interact with categories of an existing product
oProduct = DBSession.query(Product).filter_by(name="")
for oCat in oProduct.categories:
    print oCat.name
```

## 自指关系
>有时你有一个表，外键指向同一个表。例如，假设我们在有向树中有一堆节点。节点可以有许多子节点，但最多只有一个父节点
```python
class TreeNode(Base):
    __tablename__ = 'nodes'
    id = Column(Integer,Sequence('node_seq'),primary_key=True) 
    parent_id = Column(Integer,ForeignKey('nodes.id'))
    name = Column(String(20))

    children = relationship("TreeNode",
                backref=backref('parent', remote_side=[id])
            )
```
用法
```python
# 同多对一
#fetch the root node (assume there is one node with no parents)
oRootNode = DBSession.query(TreeNode).filter_by(parent_id=None).first()

#interact with children of existing node
for oChild in oRootNode.children:
    print oChild.name

#create new relationships

oParent = TreeNode()
oParent.name = "parent"
oRootNode.children.append(oParent)

oChild = TreeNode()
oChild.name = "Child"
oChild.parent = oParent
```
## 同一个表的多关系
```python
class WikiPost(Base):
    __tablename__ = 'posts'
    id = Column(Integer,Sequence('post_seq'),primary_key=True) 
    name = Column(String(20))
    author_id = Column(Integer,ForeignKey('users.id'))
    editor_id = Column(Integer,ForeignKey('users.id'))

    editor = relationship("User", primaryjoin = "WikiPost.editor_id == User.id",backref="edited_posts")
    author = relationship("User", primaryjoin = "WikiPost.author_id == User.id",backref="authored_posts")

class User(Base):
    __tablename__ = 'users'
    id = Column(Integer,Sequence('usr_seq'),primary_key=True) 
    name = Column(String(20))
```
用法
```python
# 同两个多对一关系
oAuthor = DBSession.query(User).filter_by(name="Sheena O'Connell")

#an author writes a post
oPost = WikiPost()
oPost.name = "Sqlalchemy Cheat Sheet"
oPost.author = oAuthor

#later on another user edits it
oEditor = DBSession.query(User).filter_by(name="Yi-Jirr Chen")
oEditor.edited_posts.append(oPost)

#interact with existing relationships
for oPost in oAuthor.authored_posts:
    print oPost.name
    
for oPost in oEditor.edited_posts:
    print oPost.name
```
## 创建数据库
```python
Base.metadata.create_all(engine)
```


## 增
```python
ed_user = User(name='ed', fullname='Ed Jones', password='edspassword')
session.add(ed_user)
session.commit()
```
## 删
```python
session.delete(ed_user)
session.commit()
```
## 改
```python
ed_user = session.query(User).filter_by(name='a').first()
ed_user.password='newpassword'
session.add(ed_user)
session.commit()
# 或者
session.query(User).filter_by(age>20).update({User.tag:'oldpeople'})
session.commit()
```
## 查
```python
# 全部查询
lBooks = DBSession.query(Book)  #返回一个 Query 对象. 
for oBook in lBooks:
    print oBook.name

# 简单filters
lBooks = DBSession.query(Book).filter_by(author_id=1)
#返回author_id=1的书

# 复杂filters
lBooks = DBSession.query(Book).filter(Book.price<20)
#返回价格小于20的书，使用filter而不是filter_by

# 联合filters
lBooks = DBSession.query(Book).filter_by(author_id=1).filter(Book.price<20)
# 根据价格和作者过滤

# 逻辑操作 filters
from sqlalchemy import or_
lBooks = DBSession.query(Book).filter(or_(Book.price<20,promote==True))
# 返回价格小于20的书 或 promote字段为真的书(表示正在促销)

# 排序
from sqlalchemy import desc
DBSession.query(Book).order_by(Book.price)
#返回所有书 按价格排序
DBSession.query(Book).order_by(Book.price.desc())
#降序排列

# 其他
DBSession.query(Book).count() # 返回数量
DBSession.query(Book).offset(5) # 偏移查询
DBSession.query(Book).limit(5) # 最多返回5个结果
DBSession.query(Book).first() # 返回第一个或None
DBSession.query(Book).get(8) # 返回primary_key=8的结果或None 
```
>`filter`和`filter_by`的区别
```python
session.query(MyClass).filter(MyClass.name == 'some name')
session.query(MyClass).filter_by(name = 'some name')
```
## 回滚
```python
# 在commit之前使用
session.rollback()
```
## 连接配置
```python
#the general form of a connection string:
`dialect+driver://username:password@host:port/database` 

#SQLITE:
'sqlite:///:memory:' #store everything in memory, data is lost when program exits
'sqlite:////absolute/path/to/project.db')  #Unix/Mac
'sqlite:///xiangduilujing/project.db')  #Unix/Mac
'sqlite:///C:\\path\\to\\project.db' #Windows
r'sqlite:///C:\path\to\project.db' #Windows alternative

#PostgreSQL
'postgresql://user:pass@localhost/mydatabase'
'postgresql+psycopg2://user:pass@localhost/mydatabase'
'postgresql+pg8000://user:pass@localhost/mydatabase'

#Oracle
'oracle://user:pass@127.0.0.1:1521/sidname'
'oracle+cx_oracle://user:pass@tnsname'

#Microsoft SQL Server
'mssql+pyodbc://user:pass@mydsn'
'mssql+pymssql://user:pass@hostname:port/dbname'
```
## 引擎和会话
```python
#set up the engine
engine = create_engine(sConnectionString, echo=True)   #echo=True makes the sql commands issued by sqlalchemy get output to the console, useful for debugging

#bind the dbsession to the engine 
DBSession.configure(bind=engine)

#now you can interact with the database if it exists

#import all your models then execute this to create any tables that don't yet exist. This does not handle migrations
Base.metadata.create_all(engine)   
```

## 添加索引
```python
#事前
topic_id = Column(Integer,index=True)

#事后
from sqlalchemy import Index
tp_id_index = Index('tp_id_idx',Answer.topic_id)
if __name__=='__main__':
    tp_id_index.create(bind = enginea)
```
## 错误信息
```python
from sqlalchemy.exc import(
DBAPI Errors
InterfaceError
DatabaseError
DataError
OperationalError
IntegrityError
InternalError
ProgrammingError
NotSupportedError)
```


参考
https://zhuanlan.zhihu.com/p/27400862
https://www.ctolib.com/topics-96759.html
https://www.codementor.io/sheena/understanding-sqlalchemy-cheat-sheet-du107lawl
