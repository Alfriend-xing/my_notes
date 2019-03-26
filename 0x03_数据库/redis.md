# redis

>缓存数据库

安装
```shell
$ wget http://download.redis.io/releases/redis-5.0.4.tar.gz
$ tar xzf redis-5.0.4.tar.gz
$ cd redis-5.0.4
$ make
```

运行服务端
```shell
$ src/redis-server
```

运行客户端
```shell
src/redis-cli

存取
set myKey abc
get myKey
```

## python-redis

```python
pip install redis

import redis

r = redis.Redis(host='localhost', port=6379, decode_responses=True)
#decode_responses=True表示将字符串存储为str类型，否则为字节类型
r.set('name', 'junxi')
print(r['name'])
print(r.get('name'))
print(type(r.get('name')))

# 使用连接池
# 避免每次建立、释放连接的开销。默认，每个Redis实例都会维护一个自己的连接池。
# 可以直接建立一个连接池，指定为Redis实例的参数，这样就可以实现多个Redis实例共享一个连接池
pool = redis.ConnectionPool(host='localhost', port=6379, decode_responses=True)
r = redis.Redis(connection_pool=pool)
```
### API

#### String API
```python

set(name, value, ex=None, px=None, nx=False, xx=False)
# ex，过期时间（秒）
# setex(name, value, time)
# px，过期时间（毫秒）
# psetex(name, time_ms, value)
# nx，如果设置为True，则只有name不存在时，当前set操作才执行
# setnx(name, value)
# xx，如果设置为True，则只有name存在时，当前set操作才执行

# 批量设置 获取
r.mset({'k1': 'v1', 'k2': 'v2'})
r.mset(k1="v1", k2="v2") # 这里k1 和k2 不能带引号 一次设置对个键值对
r.mget(['k1', 'k2'])
r.mget("k1", "k2")

# 更新并获取原来的值
getset(name, value)

# 获取子序列
getrange(key, start, end)

# 返回字节长度
r.strlen("foo")

# 自增(适用于记录点击量等突增数据)
r.incr("foo", amount=1)
# 浮点自增
incrbyfloat(self, name, amount=1.0)

# 自减
r.decr("foo1", amount=1)

# 追加(在数据后面添加)
r.append("name", "haha")
```
#### Hash API

```python

# 设置字段的键值对
conn.hset('dic','k1','v1')
conn.hget('dic','k1')#>>b'v1'
# 批量设置
conn.hmset('dic', {'k1': 'v1', 'k2': 'v2'})
conn.hget('dic','k2')
# 获取字段中所有键值对
conn.hgetall('dic')
# 键值对的个数
conn.hlen('dic')
# 获取字段中所有的key
conn.hkeys('dic')
# 获取字段中所有的value
conn.hvals('dic')
# 查询键是否存在
conn.hexists('dic','k1')
# 删除key
conn.hdel('dic','k1')
# 自增
conn.hincrby('dic','number',10)
conn.hincrbyfloat('dic','float',0.3)
# 迭代获取 防止读取进程内存占用过多
hscan(name, cursor=0, match=None, count=None)
    # name	    redis的name
    # cursor	游标（基于游标分批取获取数据）
    # match	    匹配指定key，默认None 表示所有的key
    # count	    每次分片最少获取个数，默认None表示采用Redis的默认分片个数
# 分批获取，上述方法的生成器版本，不同之处本方法可for
hscan_iter(name, match=None, count=None)
# 设置过期时间
conn.hset('info','BlogUrl','https://blog.ansheng.me')
conn.expire('info', 10)
```



#### List API
```python
# 在最左边添加值
conn.lpush('li', 11,22,33)
# 在最右边添加值
conn.rpush('lli', 11,22,33)
# 只有name已经存在时，值添加到列表的最左边
conn.lpushx('li', 'aa')
# 只有name已经存在时，值添加到列表的最右边
conn.rpushx('li', 'bb')
# 获取name元素的个数
conn.llen('li')
# 在name的某一个值前面或者后面插入一个新值
conn.linsert('li','AFTER','11','cc')
# 对name中index索引位置的值进行重新赋值
conn.lindex('li', 4)
# 删除指定value后面或者前面的值
conn.lrem('li', 'hello')
    # num=2,从前到后，删除2个；
    # num=-2,从后向前，删除2个
# 删除name中左侧第一个元素
conn.lpop('li')
# 删除name中右侧第一个元素
conn.rpop('li')
# 获取name中对应索引的值
conn.lindex('li', 0)
# 使用切片获取数据
lrange(name, start, end)

```

#### SET API
```python
# 为name添加值，如果存在则不添加
conn.sadd('s1', 1)
# 返回name的元素数量
conn.scard('s1')
# 在keys集合中不在其他集合中的数据
conn.sdiff('s1','s2')
# 在keys集合中不在其他集合中的数据保存到dest集合中
conn.sdiffstore('news','s1','s2')
# 获取keys集合中与其他集合中的并集
conn.sinter('s1','s2')
# 获取keys集合中与其他集合中的并集数据并保存到dest集合中
conn.sinterstore('news1','s1','s2')
# 获取value是否是name集合中的成员
conn.sismember('news1','1')
# 获取name集合中所有的成员
conn.smembers('news1')
# smove(src, dst, value)
# 将src中的value移动到dst中
conn.smove('s1','s2','v')
# srem(name, *values)
# 删除name集合中的values数据
conn.srem('s1','1','2')
# 获取keys集合与其他集合的并集
conn.sadd('a2',1,2,3,4,5,6,7)
# sunionstore(dest, keys, *args)
# 获取keys集合与其他集合的并集并保存到dest中
conn.sunionstore('a3', 'a2','a1')


```

#### Ordered set API

#### Global API

```python
# 在redis中删除names
conn.delete('ooo')
# 检测name是否存在
conn.exists('iii')
# 匹配数据库中所有 key
conn.keys(pattern='*')
# rename(src, dst)
# 把src重命名成dst
conn.rename('k', 'kk')
# 查看name的类型
conn.type('kk')
```







[参考链接](http://python.jobbole.com/87305/)