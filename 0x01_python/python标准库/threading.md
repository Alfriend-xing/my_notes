# threading
    线程模块
---
## 模块函数
```python
threading.active_count()
#返回当前存活的线程数量

threading.current_thread()
#返回当前线程对象，相当于调用方的控制线程，如果它不是线程模块创建的，则返回一个功能有限的虚拟线程对象

threading.get_ident()
#返回线程id，是一个非零整数

threading.enumerate()
#返回当前存活的所有线程组成的列表

threading.main_thread()
#返回主线程对象

threading.settrace(func)
#为线程模块启动的所有线程设置跟踪函数

threading.setprofile(func)
#为从线程模块启动的所有线程设置profile函数(不懂)

threading.stack_size([size])
#返回创建线程分配的内存大小，设置之后创建时分配的内存大小
#参数必须是0或大于32,768 (32 KiB)的正数

threading.TIMEOUT_MAX
#设置超时时间，适用于线程锁和其他阻塞(New in version 3.2.)

```
## 模块类
### Thread-Local Data
    用于保存线程中声明的数据
```python
mydata = threading.local()
mydata.x = 1
```
### Thread对象
    表示在单独线程中执行的任务，两种制定方式，重写run函数或在构造函数中传入

```python
class threading.Thread(group=None, target=None, name=None, args=(), kwargs={}, *, daemon=None)
#group目前必须为None，target为线程中执行的函数，name为线程名称
    start()
    #开始线程活动，每个线程对象只能调用一次

    run()
    #线程执行的任务函数，如果未重写，则会调用传入的target

    join(timeout=None)
    #在线程结束前阻塞调用线程，在子线程内调用自己的join会死锁

    name
    #线程名称

    getName()
    setName()
    #修改名称的旧api，内部直接操作name属性

    ident
    #线程标识符，线程未启动则为None

    is_alive()
    #返回线程存活状态

    daemon
    #表示线程守护的布尔值，需在start()调用前设置

    isDaemon()
    setDaemon()
    #线程守护的旧api
```
### Lock对象
```python
class threading.Lock
#Lock实际上是一个工厂函数，它返回锁实例

    acquire(blocking=True, timeout=-1)
    #获取锁，指定是否阻塞和超时时间，成功获取锁返回Ture，超时返回False

    release()
    #释放锁，可被任何线程调用，不只是获取锁的线程
    #释放一个未上锁的锁会抛出RuntimeError
```
### RLock对象
    可重入锁，可被同一个线程获取多次，具有 拥有线程和递归级别的概念;必须由获取它的线程来释放
```python
class threading.RLock
#RLock为工厂函数

    acquire(blocking=True, timeout=-1)

    release()
```
### Condition对象
    条件锁
```python
class threading.Condition(lock=None)
#lock可以是None，Lock 或RLock，如果是None，会在内部生成RLock

    acquire(*args)
    #获取基础锁

    release()
    #释放锁，调用基础锁的释放方法

    wait(timeout=None)
    #在被通知或超时前阻塞，调用其他线程获取的锁的wait方法会抛出RuntimeError 
    #此方法会释放基础锁，在其他线程调用相同条件变量的notify() or notify_all()方法前阻塞
    #一旦被唤醒或超时，该方法会重新获取锁并返回

    wait_for(predicate, timeout=None)
    #在条件变量为true之前等待

    notify(n=1)
    #默认唤醒一个等待这种情况的线程，需在获取锁后调用，否则抛出RuntimeError
    #无法确定当前等待条件的线程数，所以设置n为其他数字是不安全的

    notify_all()
    #唤醒所有等待这种情况的线程，需在获取锁后调用，否则抛出RuntimeError
```
### Semaphore对象
    信号量，最古老的同步原语之一,内部管理一个计数器
```python
class threading.Semaphore(value=1)

    acquire(blocking=True, timeout=None)
    #计数器减一
    #内部计数器大于零时会立即返回True，否则会阻塞

    release()
    #计数器加一

class threading.BoundedSemaphore(value=1)
#有界信号量，value值为上限，获取次数不能超过上限
```

### Event对象
    线程间最简单的通信机制，对象管理一个内部的标志
```python
class threading.Event

    is_set()
    #如果内部标志为True ，返回True

    set()
    #设置flag为True

    clear()
    #重置flag为false

    wait(timeout=None)
    #在flag为True之前阻塞
```
### Timer对象
    在指定时间后运行，Thread的一个子类
```python
class threading.Timer(interval, function, args=None, kwargs=None)
#interval 表示等待时间，秒

    与父类相同的属性方法

    cancel()
    #在计时器处于等待状态时取消计时
```
### Barrier Objects
    屏障对象
    New in version 3.2. 简单的同步原语
```python
class threading.Barrier(parties, action=None, timeout=None)
#parties表示使用的线程数，action表示被释放的线程调用的函数

    wait(timeout=None)
    #当所有参与的线程都调用了这个函数时，它们都被同时释放
    #它返回0至parties-1的整数，每个线程返回都不同

    reset()
    #重置屏障对象为空，等待它的线程会抛出BrokenBarrierError exception

    abort()
    #打破屏障，未来等待的线程会抛出BrokenBarrierError，避免死锁，最好使用超时时间创建屏障

    parties
    #需要穿过屏障的线程数

    n_waiting
    #当前等待屏障的线程数

    broken
    #表示屏障是否破损的bool值
```
## 使用with语句自动获取和释放锁
```python
使用
with some_lock:
    # do something...

而不是
some_lock.acquire()
try:
    # do something...
finally:
    some_lock.release()
```
Lock, RLock, Condition, Semaphore, 和BoundedSemaphore可以用上下文管理器








