# multiprocessing

---
>multiprocessing模块允许程序员充分利用给定机器上的多个处理器

## Pool
```python
from multiprocessing import Pool

def f(x):
    return x*x

if __name__ == '__main__':
    p = Pool(5)
    print(p.map(f, [1, 2, 3]))
```
>稍复杂的例子
```python
from multiprocessing import Pool, TimeoutError
import time
import os

def f(x):
    return x*x

if __name__ == '__main__':
    pool = Pool(processes=4) # 进程池容量为4

    # 打印 "[0, 1, 4,..., 81]"
    print pool.map(f, range(10))

    # 结果相同，顺序随机
    for i in pool.imap_unordered(f, range(10)):
        print i

    # 异步计算
    res = pool.apply_async(f, (20,)) # 在一个进程中运行
    print res.get(timeout=1) # 打印 "400"

    # 异步运行 "os.getpid()" 
    res = pool.apply_async(os.getpid, ()) # 在一个进程中运行
    print res.get(timeout=1) # 打印进程PID

    # 运行多个异步计算可能使用多个进程
    multiple_results = [pool.apply_async(os.getpid, ()) for i in range(4)]
    print [res.get(timeout=1) for res in multiple_results]

    # 10秒后返回
    res = pool.apply_async(time.sleep, (10,))
    try:
        print res.get(timeout=1)
    except TimeoutError:
        print "We lacked patience and got a multiprocessing.TimeoutError"
```

```python
class multiprocessing.Pool([processes[, initializer[, initargs[, maxtasksperchild]]]])
    apply(func[, args[, kwds]])
    #等价于apply() built-in函数，返回结果前会阻塞，所以并行工作使用apply_async()更好，此外他只工作于进程池的一个workers 中
    apply_async(func[, args[, kwds[, callback]]])
    #apply() 的变体，返回结果对象，如果指定了回调函数，那么回调函数应该接收一个参数，回调应该立即完成，否则将阻塞处理结果的线程。
    map(func, iterable[, chunksize])
    #等价于map() built-in函数，返回结果前阻塞
    map_async(func, iterable[, chunksize[, callback]])
    #map() 方法的变体，返回结果对象，如果指定了回调函数，那么回调函数应该接收一个参数，回调应该立即完成，否则将阻塞处理结果的线程。
    imap(func, iterable[, chunksize])#返回的结果对象支持timeout参数
    imap_unordered(func, iterable[, chunksize])#(应该是无序返回)
    close()#阻止后续任务被提交至进城池。不会中断正在运行的worker
    terminate()#立即中断进城池所有worker 
    join()#等待worker 进程结束，必须在close()或terminate()后使用


class multiprocessing.pool.AsyncResult
    #Pool.apply_async()和Pool.map_async()返回的结果类
    get([timeout])
    #当结果到达时返回结果，当指定timeout 而结果未及时到达则引发超时异常multiprocessing.TimeoutError
    wait([timeout])#在消息到达或超时前阻塞
    ready()#返回调用是否完成(不清楚是任务调用还是回调函数调用)
    successful()#返回调用是否报错，如果未结束则引发 AssertionError


#例子
from multiprocessing import Pool
import time

def f(x):
    return x*x

if __name__ == '__main__':
    pool = Pool(processes=4) # 开启4个worker进程

    result = pool.apply_async(f, (10,))# 在一个进程中异步计算 "f(10)"
    print result.get(timeout=1)# 超时前打印 "100"

    print pool.map(f, range(10))# 打印 "[0, 1, 4,..., 81]"

    it = pool.imap(f, range(10))
    print it.next() # prints "0"
    print it.next() # prints "1"
    print it.next(timeout=1) # prints "4"

    result = pool.apply_async(time.sleep, (10,))
    print result.get(timeout=1) # 引发multiprocessing.TimeoutError
```
>注意pool的所有方法只能被创建他的进程调用，且无法在交互环境下使用
## Process 类
```python
from multiprocessing import Process

def f(name):
    print 'hello', name

if __name__ == '__main__':
    p = Process(target=f, args=('bob',))
    p.start()
    p.join()
```
>os.getppid()可查看父进程id，仅限unix
>os.getpid()查看当前进程id，所有系统支持

```python
class multiprocessing.Process(group=None, target=None, name=None, args=(), kwargs={})
    # group 为None
    # 如需在子类中重写构造函数，需要在进程启动前调用基类构造函数Process.__init__()
    run()#进程启动后执行的函数，也可由构造函数中target传入
    start()#启动进程
    join([timeout])#阻塞调用他的进程，直到本进程结束或超时
    name
    is_alive()
    daemon#守护标志，需在start()前声明,初始值继承于创建他的进程
    #当一个进程退出时，它试图终止它的所有守护进程子进程，标为守护则表示会随着主进程结束而结束

    #与threading.Thread不同的API
    pid
    exitcode #exit code,没结束时为None ,若为负(-N)表示被其正值信号(N)终止
    authkey#认证码，一个字节串,主进程初始化时随机生成，子进程继承，也可以自定义
    terminate()#终止进程，unix会发送信号，win会调用TerminateProcess()
    #注意,将不执行退出处理程序，最后子句等
    #注意，他的子进程将不会被终止，成为孤儿进程
    #注意，会损坏进程正在使用的管道和队列，死锁已经获取的信号量
#以上方法只能被创建他的进程调用
```
```shell
>>> import multiprocessing, time, signal
>>> p = multiprocessing.Process(target=time.sleep, args=(1000,))
>>> print p, p.is_alive()
<Process(Process-1, initial)> False
>>> p.start()
>>> print p, p.is_alive()
<Process(Process-1, started)> True
>>> p.terminate()
>>> time.sleep(0.1)
>>> print p, p.is_alive()
<Process(Process-1, stopped[SIGTERM])> False
>>> p.exitcode == -signal.SIGTERM
True
```

## 进程间通信
>当使用多进程时，通常用消息传递来进行进程之间的通信，并且避免使用任何同步原语，如锁。
管道Pipe()用于两个进程间通信
队列queue 用于多进程通信

### 队列
>1. 类似线程队列
>1. Queue, multiprocessing.queues.SimpleQueue 和 JoinableQueue 是模仿标准库Queue.Queue的多生产者多消费者先进先出的队列;不同在于他没有task_done()和join()
>1. 如果使用JoinableQueue，则必须在每个任务结束后调用JoinableQueue.task_done()，否则计数信号量最终可能溢出
>1. manager 也可以创建队列
>1. 如果队列永远无法为空，则join 会形成无法被终止的死锁

```python
class multiprocessing.Queue([maxsize])
    #返回使用管道和几个锁/信号量实现的进程共享队列。
    #当一个进程首先将一个项放到队列上时，就会启动一个feed线程，它将对象从缓冲区传输到管道中。
    #Queue.Empty 和 Queue.Full需从Queue导入，否则会引发超时
    #本类有Queue.Queue除了task_done()和join()之外的所有方法 
    qsize()
    empty()
    full()
    put(obj[, block[, timeout]])
    put_nowait(obj)
    get([block[, timeout]])
    get_nowait()
    
    #外加方法
    close()#表示当前进程不会在向队列中发送内容,所有内容推送后，后台线程会退出
    join_thread()#Join 后台线程,只能在close()后使用,在后台线程退出前一直阻塞,以确保所有内容被推送到管道中
    #如果进程不是队列的创建者，在他退出是默认join, 调用cancel_join_thread()可以是他无效
    cancel_join_thread()#阻止join_thread()阻塞,在不关心丢失的数据时使用


class multiprocessing.queues.SimpleQueue
    #简化队列，很像带锁的管道
    empty()
    get()
    put(item)


class multiprocessing.JoinableQueue([maxsize])
    #带 task_done() 和 join()的队列子进程
    task_done()#如果调用次数多于队列中放置的项，则引发ValueError
    join()#Block until all items in the queue have been gotten and processed.
```

```python
from multiprocessing import Process, Queue

def f(q):
    q.put([42, None, 'hello'])

if __name__ == '__main__':
    q = Queue()
    p = Process(target=f, args=(q,))
    p.start()
    print q.get()    # prints "[42, None, 'hello']"
    p.join()
```
### 管道
>双向连接的对象

```python
multiprocessing.Pipe([duplex])
#返回(conn1, conn2)表示管道两端
#duplex为True表示双向，False表示单项，此时conn1只能收，conn2只能发
```

```python
from multiprocessing import Process, Pipe

def f(conn):
    conn.send([42, None, 'hello'])
    conn.close()

if __name__ == '__main__':
    parent_conn, child_conn = Pipe()
    p = Process(target=f, args=(child_conn,))
    p.start()
    print parent_conn.recv()   # prints "[42, None, 'hello']"
    p.join()
```
>Pipe()返回的两个连接对象表示管道的两端。每个连接对象都有send()和recv()方法等；如果两个进程（或线程）同时尝试读取或写入管道的同一端，则管道中的数据可能会损坏。当然，同时使用管道的不同端部的过程不存在损坏的风险。

## Listeners and Clients
>通常，进程间使用队列和管道返回的Connection对象传递消息，然而multiprocessing.connection提供额外的灵活性,他给出了Windows命名管道和sockets的高级API,还支持使用hmac 模块的摘要身份认证

```python
multiprocessing.connection.deliver_challenge(connection, authkey)
#发送随机生成的message 到连接的另一端并等待回应，如果应答与使用authkey作为密钥的消息摘要匹配，则向连接的另一端发送欢迎消息。否则将引发身份验证错误。AuthenticationError
multiprocessing.connection.answer_challenge(connection, authkey)
#接收消息，使用authkey作为密钥计算消息的摘要，然后发送回摘要。如果没有收到欢迎消息，则引发AuthenticationError
multiprocessing.connection.Client(address[, family[, authenticate[, authkey]]])
#尝试建立连接返回对象，连接类型由family 参数决定,也可以省略由地址判断
#如果authenticate为True或authkey为字符串，则使用摘要身份验证。
class multiprocessing.connection.Listener([address[, family[, backlog[, authenticate[, authkey]]]]])
#封装了socket 或 Windows 命名管道，监听连接；地址使用0.0.0.0则本机无法连接，本机连接应使用127.0.0.1
#family 为连接类型(sockets或命名管道),他的备选项AF_INET(TCP socket),AF_UNIX(Unix domain socket),AF_PIPE(Windows命名管道)其中只有第一种是保证可用的；
#AF_INET是元组(hostname, port)
#AF_UNIX是字符串，表示文件名
#AF_PIPE是字符串，r'\\.\pipe{PipeName}',如果是远程主机r'\ServerName\pipe{PipeName}'
accept()#在listener 后接受连接，返回Connection对象
close()#关闭listener 对象的socket连接或命名管道
#Listener 对象有以下只读属性address,last_accepted

#一些标准错误https://docs.python.org/2/library/multiprocessing.html#multiprocessing.connection.ProcessError
multiprocessing.connection.ProcessError
multiprocessing.connection.BufferTooShort
multiprocessing.connection.AuthenticationError
multiprocessing.connection.TimeoutError

#例子
from multiprocessing.connection import Listener
from array import array

address = ('localhost', 6000)     # family is deduced to be 'AF_INET'
listener = Listener(address, authkey='secret password')

conn = listener.accept()
print 'connection accepted from', listener.last_accepted

conn.send([2.25, None, 'junk', float])

conn.send_bytes('hello')

conn.send_bytes(array('i', [42, 1729]))

conn.close()
listener.close()
#客户端
from multiprocessing.connection import Client
from array import array

address = ('localhost', 6000)
conn = Client(address, authkey='secret password')

print conn.recv()                 # => [2.25, None, 'junk', float]

print conn.recv_bytes()            # => 'hello'

arr = array('i', [0, 0, 0, 0, 0])
print conn.recv_bytes_into(arr)     # => 8
print arr                         # => array('i', [42, 1729, 0, 0, 0])

conn.close()

```



## 进程锁
```python
from multiprocessing import Process, Lock

def f(l, i):
    l.acquire()
    print 'hello world', i
    l.release()

if __name__ == '__main__':
    lock = Lock()

    for num in range(10):
        Process(target=f, args=(lock, num)).start()
```
## 进程间共享状态
>通常应避免
```python
from multiprocessing import Process, Value, Array

def f(n, a):
    n.value = 3.1415927
    for i in range(len(a)):
        a[i] = -a[i]

if __name__ == '__main__':
    num = Value('d', 0.0)
    arr = Array('i', range(10))

    p = Process(target=f, args=(num, arr))
    p.start()
    p.join()

    print num.value
    print arr[:]
# 输出
3.1415927
[0, -1, -2, -3, -4, -5, -6, -7, -8, -9]
```
## manager对象
>Manager() 返回manager 对象，他控制一个服务进程，以承载python对象并允许其他进程通过代理操作这些python对象
持类型list, dict, Namespace, Lock, RLock, Semaphore, BoundedSemaphore, Condition, Event, Queue, Value , Array
使用manager比共享内存更加灵活，因为他支持任意对象，此外他还可以通过网络在主机之间实现共享，但他比共享内存慢
```python
from multiprocessing import Process, Manager

def f(d, l):
    d[1] = '1'
    d['2'] = 2
    d[0.25] = None
    l.reverse()

if __name__ == '__main__':
    manager = Manager()

    d = manager.dict()
    l = manager.list(range(10))

    p = Process(target=f, args=(d, l))
    p.start()
    p.join()

    print d
    print l

# 输出
{0.25: None, 1: '1', '2': 2}
[9, 8, 7, 6, 5, 4, 3, 2, 1, 0]
```

```python
multiprocessing.Manager()


class multiprocessing.managers.BaseManager([address[, authkey]])
    start([initializer[, initargs]])#创建manager子进程

    get_server()#返回一个Server 对象
    >>> from multiprocessing.managers import BaseManager
    >>> manager = BaseManager(address=('', 50000), authkey='abc')
    >>> server = manager.get_server()
    >>> server.serve_forever()
    
    connect()#连接本地manager对象和远程manager进程
    >>> from multiprocessing.managers import BaseManager
    >>> m = BaseManager(address=('127.0.0.1', 5000), authkey='abc')
    >>> m.connect()

    shutdown()#停止manager所使用的进程

    register(typeid[, callable[, proxytype[, exposed[, method_to_typeid[, create_method]]]]])
```
## Connection 对象
>该对象允许收发字符串和对象，可看作消息导向的socket连接
>https://docs.python.org/2/library/multiprocessing.html#connection-objects
```python
class Connection
    send(obj)
    recv()
    fileno()#返回连接的文件描述
    close()#关闭连接
    poll([timeout])#返回是否有数据待读取
    send_bytes(buffer[, offset[, size]])#发送字节数据
    recv_bytes([maxlength])
    recv_bytes_into(buffer[, offset])
```

## 同步原语
>在多进程程序中，同步原语并不像在多线程程序中那样必要
还可以通过使用manager对象创建同步原语
https://docs.python.org/2/library/multiprocessing.html#synchronization-primitives
```python
class multiprocessing.BoundedSemaphore([value])
    #有界信号量


class multiprocessing.Condition([lock])
    #条件变量


class multiprocessing.Event


class multiprocessing.Lock
    acquire(block=True, timeout=None)
    release()


class multiprocessing.RLock
    acquire(block=True, timeout=None)
    release()



class multiprocessing.Semaphore([value])


```
## 其他
```python
multiprocessing.active_children()#返回存活的子进程列表
multiprocessing.cpu_count()#返回cpu个数
multiprocessing.current_process()#返回正在运行的进程对象
multiprocessing.freeze_support()#https://docs.python.org/2/library/multiprocessing.html#multiprocessing.freeze_support
multiprocessing.set_executable()#设置启动子进程时要使用的Python解释器的路径
```

>更多高级用法见
https://docs.python.org/2/library/multiprocessing.html