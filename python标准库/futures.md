# futures
    futures目前是concurrent包中唯一的模块
    它提供了一个异步执行函数的高级接口

## Executor类
    提供异步执行函数的方法，被进程和线程类所继承，不应被直接调用
```python
class concurrent.futures.Executor
    submit(fn, *args, **kwargs)
    #注册可执行的函数fn，*args, **kwargs为其参数
    #返回Future对象

    map(func, *iterables, timeout=None, chunksize=1)
    #与内置map函数相似，除了
    #1.迭代器会被立即收集 2.函数会被并发调用异步执行
    #timeout限制返回的迭代器__next__()方法的执行时间
    #如果内部执行的函数报错，会在被迭代器检索到时raise
    #使用ProcessPoolExecutor时，此方法会将任务分块交给进程池
    #chunksize用于指定块大小，对于ThreadPoolExecutor无效

    shutdown(wait=True)
    #发出信号，在当前futures执行完毕后释放资源
    #调用该方法后再调用Executor.submit()和Executor.map()会报运行错误
    #wait=True表示该方法会在挂起的futures执行完毕后再返回
    #设为False则立刻返回并释放资源
    #使用with语句相当于调用shutdown(wait=True)
```
## ThreadPoolExecutor类
    异步执行函数的线程池，Executor的子类
    在一个线程中调用另一个线程所返回Future对象的方法可能导致死锁
    多用于io密集型操作

```python
class concurrent.futures.ThreadPoolExecutor(max_workers=None, 
thread_name_prefix='', initializer=None, initargs=())
#max_workers线程数量，initializer为可调函数，每个线程开始时会执行一次
#initargs元组为initializer参数
```
## ProcessPoolExecutor类
    进程池，可绕过全局解释器锁，无法在交互解释器中使用
    在子进程执行的函数中调用Executor 和 Future的方法将导致死锁

```python
class concurrent.futures.ProcessPoolExecutor(max_workers=None, 
mp_context=None, initializer=None, initargs=())
#max_workers表示进程数
#mp_context表示进程上下文，用于设置进程
#initializer为子进程初始化函数，initargs元组为其参数

with concurrent.futures.ProcessPoolExecutor() as executor:
    for number, prime in zip(PRIMES, executor.map(is_prime, PRIMES)):
        print('%d is prime: %s' % (number, prime))
```
## Future类
    表示一个任务，有Executor.submit()创建，不应由用户创建
```python
class concurrent.futures.Future

    cancel()
    #取消任务，如果任务正在执行则不会被取消且返回False

    cancelled()
    #如果任务被取消返回True

    running()
    #如果任务正在执行且无法取消 返回True

    done()
    #任务成功结束返回True

    result(timeout=None)
    #返回执行函数的返回值，如果任务未结束则会等待至超时时间
    #如果任务取消会raise CancelledError
    #如果任务报错，此方法会raise同样的错误

    exception(timeout=None)
    #返回执行函数报错信息，正常执行返回None

    add_done_callback(fn)
    #当任务函数结束或取消时调用fn，参数为future对象
    #如果执行函数已经结束或被取消，则fn会立即执行

```
## 模块函数

```python
concurrent.futures.wait(fs, timeout=None, return_when=ALL_COMPLETED)
#等待所有Future实例执行结束，返回集合，包含两个命名元组
#第一个集合叫done，包含结束的future
#第二个叫not_done，包含未结束的
#timeout表示超时时间
#return_when表示返回时机
#备选项FIRST_COMPLETED，FIRST_EXCEPTION，ALL_COMPLETED

concurrent.futures.as_completed(fs, timeout=None)
#返回fs所创建的future对象中结束的部分所组成的迭代器

```
## Exception类
```python
exception concurrent.futures.CancelledError

exception concurrent.futures.TimeoutError

exception concurrent.futures.BrokenExecutor

exception concurrent.futures.thread.BrokenThreadPool
#初始化函数失败时报错

exception concurrent.futures.process.BrokenProcessPool

```

