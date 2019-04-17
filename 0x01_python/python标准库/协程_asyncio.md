# asyncio
一个python库，用async/await写并发代码；asyncio意为异步io

```python
import asyncio
import time

# 定义协程函数，执行后会返回协程对象
async def say_after(delay, what):
    await asyncio.sleep(delay)
    print(what)

async def main():
    # 创建任务，用于在协程函数中执行
    # 任务执行时，会使用get_running_loop()寻找当前事件循环
    # 再将任务加入事件循环中执行，没有事件循环会报错
    task1 = asyncio.create_task(
        say_after(1, 'hello'))

    task2 = asyncio.create_task(
        say_after(2, 'world'))

    print(f"started at {time.strftime('%X')}")

    # 阻塞当前协程，允许其他协程执行
    await asyncio.sleep(1)

    # Wait until both tasks are completed (should take
    # around 2 seconds.)
    await task1
    await task2

    # 并发运行任务
    await asyncio.gather(
        say_after(1, 'hello'),
        say_after(2, 'world'),
        say_after(3, 'world'),
    )

    print(f"finished at {time.strftime('%X')}")

# 运行传入的协程对象，管理协程事件循环，一个线程只能运行一个事件循环
# 它应当被用作 asyncio 程序的主入口点，理想情况下应当只被调用一次。
asyncio.run(main())

# 预计输出
# started at 17:14:32
# hello
# world
# finished at 17:14:34
```
```python
res = await shield(something())
# 防止协程对象或任务对象被取消

await asyncio.wait_for(eternity(), timeout=1.0)
# 设置超时时间，超时报错

done, pending = await asyncio.wait(aws)
# 阻塞线程，直到return_when 条件(FIRST_COMPLETED,FIRST_EXCEPTION,ALL_COMPLETED)满足，
# 返回Tasks/Futures集合(done, pending)

asyncio.run_coroutine_threadsafe(coro, loop)
# 从其他线程向事件循环插入协程任务，返回Future对象

asyncio.current_task(loop=None)
asyncio.all_tasks(loop=None)
# 返回当前/所有运行的task对象
# 如果 loop 为 None 则会使用 get_running_loop() 获取当前事件循环。

```
## Task对象
由`asyncio.create_task()``loop.create_task() `或` ensure_future()`创建
```python
class asyncio.Task(coro, *, loop=None)

cancel()
# 请求取消 Task 对象,在下一轮事件循环中抛出一个 CancelledError 异常

cancelled()
# 如果 Task 对象 被取消 则返回 True

done()
# 如果 Task 对象 已完成 则返回 True

result()
# 返回 Task 的结果,
# task取消则引发CancelledError 异常
# task对象的结果还不可用，引发InvalidStateError 异常

exception()
# 返回 Task 对象的异常

add_done_callback(callback, *, context=None)
# 添加一个回调，将在 Task 对象 完成 时被运行

remove_done_callback(callback)
# 从回调列表中移除 callback 指定的回调

get_stack(*, limit=None)
# 返回此 Task 对象的栈框架列表

print_stack(*, limit=None, file=None)
# 打印此 Task 对象的栈或回溯

```
## 协程socket

## 协程锁
### 常用锁
```python
lock = asyncio.Lock()

# ... later
async with lock:
    # access shared state
```
相当于
```python
lock = asyncio.Lock()

# ... later
await lock.acquire()
try:
    # access shared state
finally:
    lock.release()
```
```python
lock = asyncio.Lock()
lock.locked()
# 如果已被上锁，返回True
```
### 事件锁
用于提醒多个协程任务，某件事已经发生了
```python
# 例子
async def waiter(event):
    print('waiting for it ...')
    await event.wait()
    print('... got it!')

async def main():
    # Create an Event object.
    event = asyncio.Event()

    # Spawn a Task to wait until 'event' is set.
    waiter_task = asyncio.create_task(waiter(event))

    # Sleep for 1 second and set the event.
    await asyncio.sleep(1)
    event.set()

    # Wait until the waiter task is finished.
    await waiter_task

asyncio.run(main())
```
```python
class asyncio.Event(*, loop=None)

coroutine wait()
# 在事件被set前阻塞

set()
# 设置事件

clear()
# 清除未set的事件

is_set()
#  如果事件被set返回True
```
### 条件锁
```python
cond = asyncio.Condition()

# ... later
async with cond:
    await cond.wait()

# 等效于

cond = asyncio.Condition()

# ... later
await lock.acquire()
try:
    await cond.wait()
finally:
    lock.release()
```
```python
class asyncio.Condition(lock=None, *, loop=None)

coroutine acquire()
# 获取锁

notify(n=1)
# 最多通知n个等待的协程，调用前需获取锁

locked()
# 如果锁已被获取，返回True

notify_all()
# 唤醒所有等待的协程任务，调用前需获取锁

release()
# 释放锁，调用前需获取锁

coroutine wait()
# 在notify前持续等待，调用前需获取锁，被唤醒后重新调用返回True

coroutine wait_for(predicate)
# 在predicate返回True前阻塞，predicate需为可调用对象，且返回的值能布尔化
```
### 信号量锁Semaphore

## 子进程

## 队列

## 异常类