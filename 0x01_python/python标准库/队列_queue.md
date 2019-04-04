# queue
    线程安全的内部队列

## 队列种类
```python
class queue.Queue(maxsize=0)
#先进先出队列

class queue.LifoQueue(maxsize=0)
#后进先出队列

class queue.PriorityQueue(maxsize=0)
#优先级队列，需放入可比较的项,小的先出来

class queue.SimpleQueue
#功能简单的队列

```
## 异常类

```python
exception queue.Empty
exception queue.Full
#当相应的存取操作未设置block或超时则抛出
```

## 通用方法
    Queue, LifoQueue,  PriorityQueue都具有的方法
```python
qsize() #返回队列长度，不可靠
empty() #队列为空返回True
full()  #队列为满返回True
put(item, block=True, timeout=None) #放
put_nowait(item)    #无阻塞放
get(block=True, timeout=None)   #取
get_nowait()    #无阻塞取项目
task_done() #表示任务完成
join()  #队列为空前阻塞
```
    SimpleQueue具有的方法
```python
qsize()
empty()
put(item, block=True, timeout=None)
put_nowait(item)
get(block=True, timeout=None)
get_nowait()
```

## 可比较的对象
```python
cmp(x,y) 
#函数用于比较2个对象,如果x<y返回-1,如果x==y返回0,如果x>y返回1

class Job(object):
    def __init__(self, priority, description):
        self.priority = priority
        self.description = description
        print 'Job:',description
        return
    def __cmp__(self, other):
        return cmp(self.priority, other.priority)
```