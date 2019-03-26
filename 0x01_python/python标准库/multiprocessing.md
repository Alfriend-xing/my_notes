# multiprocessing
    多进程模块，有效绕过全局解释器锁

## 模块函数

```python
multiprocessing.active_children()
#返回存活子进程列表

multiprocessing.cpu_count()
#返回cpu线程数

multiprocessing.current_process()
#返回当前进程的Process对象

multiprocessing.freeze_support()
#使打包exe后的程序支持多进程
if __name__ == '__main__':
    freeze_support()
#打包的exe不调用该方法会抛出RuntimeError

multiprocessing.get_all_start_methods()
#返回受支持的方法列表，可能出现'fork', 'spawn' ,'forkserver'
#Windows 只能用 'spawn' ; Unix还有 'fork' and 'spawn'
#默认'fork'

multiprocessing.get_context(method=None)
#返回context 对象
#method可以是'fork', 'spawn', 'forkserver'

multiprocessing.get_start_method(allow_none=False)
#返回start 方法使用的程序名
#返回值可能是 'fork', 'spawn', 'forkserver' or None
#allow_none表示未设置时是否返回None

multiprocessing.set_executable()
#设置子进程所使用的python解释器路径

multiprocessing.set_start_method(method)
#设置子进程启动的方法，可选项'fork', 'spawn' or 'forkserver'
```
>spawn
> - 父进程启动一个新的python解释器进程。子进程将只继承运行进程对象run（）方法所需的资源

> fork
> - 父进程的所有资源都由子进程继承，但无法创建孙进程

> forkserver
> - 子进程会从独立的服务器进程那里继承，不会继承不必要的资源

## 模块类

### Process类
```python
class multiprocessing.Process(group=None, target=None, name=None, args=(), kwargs={}, *, daemon=None)
#代表执行任务的子进程，参考threading.Thread

    run()

    start()

    join([timeout])

    name

    is_alive()

    daemon

    #与线程不同的部分

    pid
    #返回进程id，运行之前为None

    exitcode
    #子进程的退出代码，未结束时为None，负值-n表示子进程被信号n终止

    authkey
    #进程的认证密钥，字节字符串；进程初始化产生的随机值，进程创建时用于验证身份

    sentinel
    #系统对象的数字句柄

    terminate()
    #终止进程

    kill()
    #终止进程，与terminate()效果相同 ，但使用的信号不同

    close()
    #关闭子进程，释放资源；如果关闭一个正在运行的子进程，会抛出ValueError，成功关闭后调用该进程对象的方法和属性也会抛出ValueError


```
>注意`start()`, `join()`, `is_alive()`, `terminate()` 和`exitcode`只能由创建该子进程的进程调用
### Pipes and Queues管道和队列
    当使用多个进程时，通常使用消息传递在进程之间进行通信，并避免使用任何同步原语（如锁）。
    Queue, SimpleQueue 和 JoinableQueue是多对多先进先出的，且继承于队列模块，于queue.Queue不同的是进程队列没有task_done() 和 join()方法

```python
multiprocessing.Pipe([duplex])
#返回一组 Connection对象(conn1, conn2)
#duplex 默认True，表示双向管道

class multiprocessing.Queue([maxsize])
#返回一个进程共享的队列，空和满异常表示为超时
#queue实现queue.queue的所有方法，但task_done和join除外

    qsize()
    #返回队列的大致大小，但结果不可靠

    empty()
    #返回True表示队列空

    full()
    #返回True表示队列满

    put(obj[, block[, timeout]])
    #将对象放入队列，block表示队列满时阻塞还是报错，timeout表示超时

    put_nowait(obj)
    #等于put(obj, False)

    get([block[, timeout]])
    #取出对象 并从队列中移除

    get_nowait()
    #等于get(False)

    #与queue.Queue不同的部分

    close()
    #表示当前进程不会再将数据放入此队列

    join_thread()
    #只能在close()之后调用，保证后台线程将所有待写入数据传入队列后再退出

    cancel_join_thread()
    #与join_thread()相反，直接退出后台向队列中写入数据的线程，如果有未来得及写入的数据则直接丢弃

class multiprocessing.SimpleQueue
#简单队列，像极了带锁的管道

    empty()
    get()
    put(item)

class multiprocessing.JoinableQueue([maxsize])
#有task_done() and join() 方法的队列
    task_done()
    #表示任务执行完毕，由获取项目的进程调用，一个get对应一个task_done

    join()
    #阻塞 ，直到队列中的所有项目都被获取和处理
```

### Connection 类
    由pipe创建

```python
class multiprocessing.connection.Connection

    send(obj)
    #向管道另一端发送项目，大于32 MiB的项目可能引发异常

    recv()
    #接收项目，管道为空会阻塞，如果从已关闭的管道接收项目会抛出EOFError

    fileno()
    #返回连接使用的文件描述符或句柄

    close()
    #关闭连接

    poll([timeout])
    #返回管道中是否有数据

    send_bytes(buffer[, offset[, size]])
    #发送字节数据，大于32mb可能引发异常

    recv_bytes([maxlength])

    recv_bytes_into(buffer[, offset])
    #将管道内字节数据完整写入变量
```
### 同步原语
    通常进程之间没必要使用同步原语
```python
class multiprocessing.Barrier(parties[, action[, timeout]])
#屏障

class multiprocessing.BoundedSemaphore([value])
#有界信号量

class multiprocessing.Condition([lock])
#条件锁

class multiprocessing.Event
#事件锁

class multiprocessing.Lock
#基本锁

class multiprocessing.RLock
#重入锁

class multiprocessing.Semaphore([value])
#信号量

```
### 共享的ctypes对象
    子进程可以使用父进程创建的对象
```python
multiprocessing.Value(typecode_or_type, *args, lock=True)
#返回一个与子进程共享内存的ctypes对象
#lock 为Ture会为数据上锁，为False则无法保证进程安全
#自增不是原子操作，若想实现原子自增：
# with counter.get_lock():
#     counter.value += 1

multiprocessing.Array(typecode_or_type, size_or_initializer, *, lock=True)
#返回共享内存的ctypes 数组

multiprocessing.sharedctypes.RawArray(typecode_or_type, size_or_initializer)
#返回ctypes 数组

multiprocessing.sharedctypes.RawValue(typecode_or_type, *args)


multiprocessing.sharedctypes.Array(typecode_or_type, size_or_initializer, *, lock=True)

multiprocessing.sharedctypes.Value(typecode_or_type, *args, lock=True)

multiprocessing.sharedctypes.copy(obj)

multiprocessing.sharedctypes.synchronized(obj[, lock])


```

### Managers
    管理器提供了一种创建可以在不同进程之间共享的数据的方法

```python
multiprocessing.Manager()
#返回一个已经启动的SyncManager对象

class multiprocessing.managers.BaseManager([address[, authkey]])

    start([initializer[, initargs]])
    #在子进程中启动管理器

    get_server()
    #返回Server 对象

    connect()
    #连接本地管理器对象到远程管理器进程

    shutdown()

    register(typeid[, callable[, proxytype[, exposed[, method_to_typeid[, create_method]]]]])

    address

class multiprocessing.managers.SyncManager

    Barrier(parties[, action[, timeout]])

    BoundedSemaphore([value])

    Condition([lock])

    Event()

    Lock()

    Namespace()
    
    Queue([maxsize])
    
    RLock()
    
    Semaphore([value])
    
    Array(typecode, sequence)
    
    Value(typecode, value)
    
    dict()
    dict(mapping)
    dict(sequence)
    
    list()
    list(sequence)
```
### Pool类
    进程池

```python
class multiprocessing.pool.Pool([processes[, initializer[, initargs[, maxtasksperchild[, context]]]]])

```
[待续](https://docs.python.org/3/library/multiprocessing.html#module-multiprocessing.pool)












### 异常类
```python
exception multiprocessing.ProcessError
#异常基类

exception multiprocessing.BufferTooShort

exception multiprocessing.AuthenticationError

exception multiprocessing.TimeoutError
```












