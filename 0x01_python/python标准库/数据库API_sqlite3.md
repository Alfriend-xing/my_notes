# sqlite3
```python
import sqlite3
conn = sqlite3.connect('example.db')
# 指定数据库名为:memory: 则创建缓存数据库

# 创建游标
c = conn.cursor()

# 创建表
c.execute('''CREATE TABLE stocks
             (date text, trans text, symbol text, qty real, price real)''')
# con.execute("create table person (id integer primary key, \
# firstname varchar unique)")


# 插入一行数据
c.execute("INSERT INTO stocks VALUES ('2006-01-05','BUY','RHAT',100,35.14)")

# 批量插入
purchases = [('2006-03-28', 'BUY', 'IBM', 1000, 45.00),
             ('2006-04-05', 'BUY', 'MSFT', 1000, 72.00),
             ('2006-04-06', 'SELL', 'IBM', 500, 53.00),
            ]
c.executemany('INSERT INTO stocks VALUES (?,?,?,?,?)', purchases)

# 提交更改
conn.commit()

# 查询
t = ('RHAT',)
c.execute('SELECT * FROM stocks WHERE symbol=?', t)
print(c.fetchone()) #fetchall(返回所有结果组成的list)

# 把游标当作迭代器
for row in c.execute('SELECT * FROM stocks ORDER BY price'):
        print(row)

# 断开连接
# 确保所有更改已被提交，未提交的更改会丢失
conn.close()

```

## 模块函数
```python
sqlite3.connect(database[, timeout, detect_types, isolation_level, \
check_same_thread, factory, cached_statements, uri])
# database 数据库路径
# 当一个数据库被多连接时，同一时间只允许一个进程修改
# sqlite支持TEXT, INTEGER, REAL, BLOB and NULL类型
# detect_types参数和register_converter()可用于自定义数据类型
# detect_types 默认为0，PARSE_DECLTYPES and PARSE_COLNAMES
# check_same_thread 默认为True，表示只有创建连接的线程可以使用
# 当该参数设为false时，可被多线程调用，此时用户需要自行为写操作上锁
# factory 参数，可以为factory 参数指定你自己写的Connection子类，否则
# connect工厂函数将返回标准Connection类
# uri参数，设为True时可在文件路径后加特别参数
# db = sqlite3.connect('file:path/to/database?mode=ro', uri=True)只读方式打开

sqlite3.register_converter(typename, callable)
# 注册一个可调函数，用于将数据库中typename类型的二进制字符串转换为python类型
# 并配置connect函数的detect_types 参数PARSE_DECLTYPES and PARSE_COLNAMES

sqlite3.register_adapter(type, callable)
# 将sqlite支持的类型转为python支持的类型

sqlite3.complete_statement(sql)
# 检测sql语句的完整性，不检查语法

sqlite3.enable_callback_tracebacks(flag)
# flag为True，则返回回调函数的执行情况，用于调试
```
[sqlite指定URI参数](https://www.sqlite.org/uri.html)

Python type|SQLite type|
--|--|
None|NULL|
int|INTEGER|
float|REAL|
str|TEXT|
bytes|BLOB|

## 模块类
### Connection类
```python
class sqlite3.Connection

isolation_level
# 获取或设置当前的隔离等级，None DEFERRED IMMEDIATE EXCLUSIVE

in_transaction
# 如果有未提交的更改，返回True

cursor(factory=Cursor)
# 返回 Cursor实例

commit()
# 提交当前变更

rollback()
# 回滚上次提交之后的所有变更

close()
# 关闭连接，未提交的更改会丢失

execute(sql[, parameters])
executemany(sql[, parameters])
executescript(sql_script)
# 非标准的快捷方式，调用cursor()执行sql并返回cursor对象

create_function(name, num_params, func)
# 创建自定义的函数，之后可用于sql函数
def md5sum(t):
    return hashlib.md5(t).hexdigest()
con = sqlite3.connect(":memory:")
con.create_function("md5", 1, md5sum)
cur = con.cursor()
cur.execute("select md5(?)", (b"foo",))

create_aggregate(name, num_params, aggregate_class)
# 自定义聚合函数，聚合类须有step finalize方法

create_collation(name, callable)

interrupt()
# 从其他线程调用，终止连接中正在执行的查询，引发异常

set_authorizer(authorizer_callback)
# 定义访问数据库时的回调函数

set_progress_handler(handler, n)
# 每执行n条指令回调一次函数

set_trace_callback(trace_callback)
# 每执行一条sql回调的函数

enable_load_extension(enabled)
# 激活或禁用扩展

load_extension(path)
# 从共享库载入扩展

row_factory
# 绑定函数，更改访问结果行的方式

text_factory
# 指定sqlite的text数据返回的类型

total_changes
# 返回影响的行数

iterdump()
# 迭代返回数据库的sql语句，用于导出表结构(猜测)

backup(target, *, pages=0, progress=None, name="main", sleep=0.250)
# 设置备份库


```

### Cursor类
```python
class sqlite3.Cursor

execute(sql[, parameters])
# 支持两种占位符，问号占位 命名占位
cur.execute("insert into people values (?, ?)", ('mike', 20))
cur.execute("insert into people values (:who, :age)",\
{"who": 'mike', "age": 20}
cur.execute("select * from people where name_last=? and age=?",\
('mike', 20))
cur.execute("select * from people where name_last=:who and age=:age",\
 {"who": 'mike', "age": 20})

executemany(sql, seq_of_parameters)
# seq_of_parameters可迭代

executescript(sql_script)
# 执行多条命令的不标准方法，sql_script为```sql1;sql2;sql3```

fetchone()
# 从查询结果集中取第一条

fetchmany(size=cursor.arraysize)
# 返回多条结果列表，默认条数为cursor.arraysize，自定义数量建议每次保持不变

fetchall()
# 返回所有结果

close()
# 关闭游标

rowcount
# 受影响的行

lastrowid
# 只读属性，表示上次修改的行id

arraysize
# 可读写属性，指定多行获取时返回的结果数，默认1

description
# 只读属性，提供上次查询的列名

connection
# 只读属性，引用连接对象
```

### Row类
```python
class sqlite3.Row
# 用于Connection对象的row_factory属性，指定用于处理行的工厂函数
# 它支持按列名和索引，迭代，表示，相等性测试和len()进行映射访问。

keys()
# 返回列名组成的list
# 在一次查询后，它将是Cursor.description返回元组中的第一个元素

conn.row_factory = sqlite3.Row
c = conn.cursor()
c.execute('select * from stocks')   #<sqlite3.Cursor object at 0x7f4e7dd8fa80>
r = c.fetchone()    #<class 'sqlite3.Row'>
len(r)  #5
r[2]    #'RHAT'
r["date"]
tuple(r)    #('2006-01-05', 'BUY', 'RHAT', 100.0, 35.14)
for member in r:
    print(member)
# 2006-01-05
# BUY
# RHAT
# 100.0
# 35.14
```

### 异常类
```python
exception sqlite3.Warning
# Exception.的子类

exception sqlite3.Error
# 本模块的异常基类

exception sqlite3.DatabaseError
# 数据库异常

exception sqlite3.IntegrityError
# 数据完整性异常

exception sqlite3.ProgrammingError
# 编程错误引发的异常，如表未找到

exception sqlite3.OperationalError
# 数据库操作错误

exception sqlite3.NotSupportedError
# 调用功能不被数据库支持
```
## 适配python类型
有两种方法将python类型存入数据库
```python
# 修改对象，自动适配
import sqlite3

class Point:
    def __init__(self, x, y):
        self.x, self.y = x, y

    def __conform__(self, protocol):
        if protocol is sqlite3.PrepareProtocol:
            return "%f;%f" % (self.x, self.y)

con = sqlite3.connect(":memory:")
cur = con.cursor()

p = Point(4.0, -3.2)
cur.execute("select ?", (p,))
print(cur.fetchone()[0])
```
```python
# 注册适配函数
import sqlite3

class Point:
    def __init__(self, x, y):
        self.x, self.y = x, y

def adapt_point(point):
    return "%f;%f" % (point.x, point.y)

sqlite3.register_adapter(Point, adapt_point)

con = sqlite3.connect(":memory:")
cur = con.cursor()

p = Point(4.0, -3.2)
cur.execute("select ?", (p,))
print(cur.fetchone()[0])
```
从数据库返回python类型
```python
import sqlite3

class Point:
    def __init__(self, x, y):
        self.x, self.y = x, y

    def __repr__(self):
        return "(%f;%f)" % (self.x, self.y)

def adapt_point(point):
    return ("%f;%f" % (point.x, point.y)).encode('ascii')

def convert_point(s):
    x, y = list(map(float, s.split(b";")))
    return Point(x, y)

# Register the adapter注册适配，python类型适配sqlite类型
sqlite3.register_adapter(Point, adapt_point)

# Register the converter注册转换，sqlite类型转换python类型
sqlite3.register_converter("point", convert_point)

p = Point(4.0, -3.2)
#########################
# 1) Using declared types使用python类型名
con = sqlite3.connect(":memory:", detect_types=sqlite3.PARSE_DECLTYPES)
cur = con.cursor()
cur.execute("create table test(p point)")
#类名同register_converter的参数，忽略大小写

cur.execute("insert into test(p) values (?)", (p,))
cur.execute("select p from test")
print("with declared types:", cur.fetchone()[0])
cur.close()
con.close()

#######################
# 1) Using column names使用列名
con = sqlite3.connect(":memory:", detect_types=sqlite3.PARSE_COLNAMES)
cur = con.cursor()
cur.execute("create table test(p)")
# 不用指定数据类型吗？

cur.execute("insert into test(p) values (?)", (p,))
cur.execute('select p as "p [point]" from test') #这里应该是指定了类型
print("with column names:", cur.fetchone()[0])
cur.close()
con.close()
```
```python
# 默认适配和转换
import sqlite3
import datetime

con = sqlite3.connect(":memory:", detect_types=sqlite3.PARSE_DECLTYPES|sqlite3.PARSE_COLNAMES)
cur = con.cursor()
cur.execute("create table test(d date, ts timestamp)")

today = datetime.date.today()
now = datetime.datetime.now()

cur.execute("insert into test(d, ts) values (?, ?)", (today, now))
cur.execute("select d, ts from test")
row = cur.fetchone()
print(today, "=>", row[0], type(row[0]))
print(now, "=>", row[1], type(row[1]))

cur.execute('select current_date as "d [date]", current_timestamp as "ts [timestamp]"')
row = cur.fetchone()
print("current_date", row[0], type(row[0]))
print("current_timestamp", row[1], type(row[1]))


```

## 常用语句
```sql
--  创建索引
CREATE INDEX index_name ON table_name;
--  单列索引
CREATE INDEX index_name ON table_name (column_name);
--  唯一索引
CREATE UNIQUE INDEX index_name on table_name (column_name);
-- 组合索引
CREATE INDEX index_name on table_name (column1, column2);
-- 删除索引
DROP INDEX index_name;
-- 避免使用索引的情况
-- 索引不应该使用在较小的表上。
-- 索引不应该使用在有频繁的大批量的更新或插入操作的表上。
-- 索引不应该使用在含有大量的 NULL 值的列上。
-- 索引不应该使用在频繁操作的列上。
```
```sql
-- 修改表结构
-- 修改表名称
ALTER TABLE old_table_name RENAME TO new_table_name;
-- 添加字段
ALTER TABLE table_name ADD COLUMN column_name Text; 
-- 查询表结构
PRAGMA TABLE_INFO (table_name);
-- 修改表结构字段类型(SQLite 不支持)
-- 不能删除一个已经存在的字段，或者更改一个已经存在的字段的名称、数据类型、限定符等等
-- 替代方案，重建表
--1.将表名改为临时表
ALTER TABLE "Student" RENAME TO "_Student_old_20140409";
--2.创建新表
CREATE TABLE "Student" ("Id"  INTEGER PRIMARY KEY AUTOINCREMENT, "Name"  Text);
--3.导入数据
INSERT INTO "Student" ("Id", "Name") SELECT "Id", "Title" FROM "_Student_old_20140409";
--4.更新sqlite_sequence
UPDATE "sqlite_sequence" SET seq = 3 WHERE name = 'Student';
-- 由于在Sqlite中使用自增长字段,引擎会自动产生一个sqlite_sequence表,
-- 用于记录每个表的自增长字段的已使用的最大值，
-- 所以要一起更新下。如果有没有设置自增长，则跳过此步骤。
--5.删除临时表(可选)
DROP TABLE _Student_old_20140409;
```

