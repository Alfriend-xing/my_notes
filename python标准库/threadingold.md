# threading

---
## threading.active_count()
>返回当前活跃的线程数
## threading.Condition()
>工厂函数，返回一个Condition对象
## threading.current_thread()
>返回当前线程对象
## threading.enumerate()
>返回存活的线程对象列表
## threading.Event()
>工厂函数，返回一个event对象,(set,clear,wait,阻塞线程)
## class threading.local
>代表线程局部变量的类，管理线程内的数据，创建后存储就可以了
```python
mydata = threading.local()
mydata.x = 1
```
## threading.Lock()
>工厂函数，返回一个新的原始锁，当一个线程获取到时，其他线程获取时会阻塞，但任何线程都可以释放他
## threading.RLock()
>工厂函数，返回一个新的可重入锁，该锁只能有获取他的线程释放。一个线程获取他后，该线程可再次获取他（而不会阻塞），获取几次就要释放几次
## threading.Semaphore([value])
>工厂函数，返回一个新的semaphore 对象，他管理一个计数器，记录release()调用次数减去acquire()调用次数，加上初始值value。acquire()方法会在使计数器变负时阻塞，value默认为1
## threading.BoundedSemaphore([value])
>工厂函数，返回一个新的bounded semaphore对象（有界信号量），他检查当前值不超过初始值，否则会raised ValueError。通常用于保护容量有限的资源，默认值为1
可以确保release()方法的调用次数不能超过给定的初始信号量数值
## class threading.Thread
>代表线程的类
## class threading.Timer
>在特定时间后执行函数的线程
## threading.settrace(func)
>为所有线程设置追踪函数
## threading.setprofile(func)
>为所有线程设置profile 函数
## threading.stack_size([size])
>返回线程创建时的栈容量，size参数指定了之后创建线程时使用的栈容量，为0时使用系统默认大小，否则不能小于32,768 (32 KiB)，默认为0，如果数量超出能使用的量，则报错raised ThreadError ，如果输入值无效，raised ValueError
## exception threading.ThreadError
>针对与线程相关的报错
# 对象
>下面介绍详细的对象，对象的所有方法都是原子执行的
## Thread对象
>调用方式，将要执行的函数通过构造函数传入，或者重写__init__()和run()方法
is_alive()可以查看线程是否存活，join()会阻塞调用它的线程，name属性是线程的名称，可以读取更改；daemon标记守护线程，当只剩下守护程序线程时，整个Python程序都会退出；如果守护线程突然终止，那么他操作的资源可能丢失，如果你想让线程优雅的停止，请不要设置守护线程并使用event
```python
class threading.Thread(group=None, target=None, name=None, args=(), kwargs={})
group 应该为 None
target 是一个函数，或被 run()方法调用
name 是线程名
args和kwargs是参数
如果子类继承于该类，要保证子类的构造函数中先调用基类的构造函数Thread.__init__()

start() #表示开始线程
run()   #表示线程执行的内容，可以在子类中重写
join([timeout])     #在该线程结束之前，会阻塞调用的线程，可设置时间，可被多次调用
name #用于区别线程 的属性
getName()
setName()
is_alive()  #返回是否存活
daemon      #布尔值，需要在start()之前调用，他的初始值继承于创建他的线程，默认False
isDaemon()
setDaemon()
```
## Lock 对象
>他只有两种状态locked或unlocked
unlocked时 acquire() 会将它变为 locked
locked时 acquire() 会阻塞，直到其他线程调用 release()
locked时 acquire()会阻塞，而unlocked时release()会报错
release()只能在locked 时调用
当多个线程等待一个锁时，只有一个线程能获取到他，并且不确定是哪个线程
```python
Lock.acquire([blocking])
#获取锁，可以指定是否阻塞，默认阻塞blocking=True
Lock.release()
#释放锁
```
## RLock对象
>可在同一线程内多次获取，在原始的线程锁上增加了所属线程和递归调用的概念
```python
RLock.acquire([blocking=1])
# 获取锁，可设置是否阻塞，设为阻塞，调用时返回True，设为不阻塞，调用时返回False
RLock.release()
# 释放锁，减去一层递归，只能由所属线程调用，否则报错

```
## Condition 对象
>acquire()与release()调用相关联的锁的相应方法。在调用线程获得锁后，才可以使用wait() 方法notify()和notifyAll()方法。否则会报错
wait()方法会释放锁，阻塞会持续到其他线程在同样情况下调用notify()或notifyAll()时，一旦被唤醒，他会重新获得锁并返回，他可以指定超时时间
notify()方法会唤醒等待条件变量的线程中的一个线程；notifyAll()则会唤醒所有等待中的线程
注：notify() 和 notifyAll()并不是释放锁，
```python
# Consume one item
cv.acquire()
while not an_item_is_available():
    cv.wait()
get_an_available_item()
cv.release()

# Produce one item
cv.acquire()
make_an_item_available()
cv.notify()
cv.release()
```
```python

class threading.Condition([lock])
#传入Lock或RLock
    acquire(*args)
    release()
    wait([timeout])
    notify(n=1)
    notify_all()
```
## Semaphore 对象
>信号量同步基于内部计数器，每调用一次acquire()，计数器减1；每调用一次release()，计数器加1.当计数器为0时，acquire()调用被阻塞。
```python
class threading.Semaphore([value])
# value不能小于0，默认为1
    acquire([blocking])
    #获取时大于0，则返回，获取时为0，则阻塞
    release()
    #释放信号量
```
## Event 对象
>基于事件的同步是指：一个线程发送/传递事件，另外的线程等待事件的触发。
```python
class threading.Event
    is_set()    #如果内部标志为真，返回真
    set()   #设置内部标志为真，调用之后wait不阻塞
    clear() #设置内部标志为False，调用之后wait阻塞
    wait([timeout])     #内部标志为False，则阻塞至超时，除此之外永远返回True
```
## Timer 对象
>定时启动线程
```python
def hello():
    print "hello, world"

t = Timer(30.0, hello)
t.start()  # 30秒后打印hello, world

class threading.Timer(interval, function, args=[], kwargs={})
    cancel()
    #停止计时，只对等待中的计时器有效
```
>Lock, RLock, Condition, Semaphore, 和 BoundedSemaphore 对象 可以用with语句进行上下文管理，例如
```python
import threading

some_rlock = threading.RLock()

with some_rlock:
    print "some_rlock is locked while this executes"
```
## Queue 对象
>使用队列我们不用关心锁，队列会为我们处理锁的问题
```python
# 先进先出
class Queue.Queue(maxsize=0)
#每一个由put()调用入队的任务都有一个对应的task_done()调用
    task_done()#每一个get都应该对应一个task_done()，表示任务完成
    join()#会阻塞调用线程，直到所有任务被处理完
    put(item[, block[, timeout]])#入队，默认有阻塞，无超时
    put_nowait()#等同于put(item, False)
    get([block[, timeout]])#从队列中移除并返回一个数据
    get_nowait()#相当与get(False)
    full()#如果队列满了，返回True,反之False
    empty()#如果队列为空，返回True,反之返回False
    qsize()#返回队内元素个数

#后进先出
class Queue.LifoQueue(maxsize=0)

#优先级队列
class Queue.PriorityQueue(maxsize=0)
#放入可比较元素，小的先出来

import Queue
import threading

class Job(object):
    def __init__(self, priority, description):
        self.priority = priority
        self.description = description
        print 'Job:',description
        return
    def __cmp__(self, other):
        return cmp(self.priority, other.priority)

q = Queue.PriorityQueue()

q.put(Job(3, 'level 3 job'))
q.put(Job(10, 'level 10 job'))
q.put(Job(1, 'level 1 job'))

def process_job(q):
    while True:
        next_job = q.get()
        print 'for:', next_job.description
        q.task_done()

workers = [threading.Thread(target=process_job, args=(q,)),
        threading.Thread(target=process_job, args=(q,))
        ]

for w in workers:
    w.setDaemon(True)
    w.start()

q.join()
#结果

Job: level 3 job
Job: level 10 job
Job: level 1 job
for: level 1 job
for: level 3 job
for: job: level 10 job
```
## 
>
## 
>
## 
>
## 
>
## 
>
## 
>





http://yoyzhou.github.io/blog/2013/02/28/python-threads-synchronization-locks/
https://docs.python.org/2/library/threading.html#condition-objects
https://www.cnblogs.com/itogo/p/5635629.html



