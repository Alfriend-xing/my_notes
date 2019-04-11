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
cur.execute("insert into people values (?, ?)", (who, age))
cur.execute("select * from people where name_last=:who and age=:age",\
 {"who": who, "age": age})

```




