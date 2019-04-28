## lock
从Java 5之后，在java.util.concurrent.locks包下提供了另外一种方式来实现同步访问，那就是Lock。它提供了更多种同步方案

```java
// Lock是一个接口
public interface Lock {
    // 获取锁
    // 如果锁已被其他线程获取，则进行等待
    void lock();
    // 尝试获取锁，如果获取成功，则返回true，如果获取失败则返回false
    boolean tryLock();
    // 在时间期限之内如果还拿不到锁，就返回false
    boolean tryLock(long time, TimeUnit unit) throws InterruptedException;

    // 释放锁
    void unlock();

    // 可中断锁
    void lockInterruptibly() throws InterruptedException;

    Condition newCondition();
}
```
```java
Lock lock = ...;
lock.lock();
try{
    //处理任务
}catch(Exception ex){
     
}finally{
    lock.unlock();   //释放锁
}

```
```java
if(lock.tryLock()) {
     try{
         //处理任务
     }catch(Exception ex){
         
     }finally{
         lock.unlock();   //释放锁
     } 
}else {
    //如果不能获取锁，则直接做其他事情
}

```
```java
public void method() throws InterruptedException {
    lock.lockInterruptibly();
    try {  
     //.....
    }
    finally {
        lock.unlock();
    }  
}
```
### ReentrantLock 可重入锁
可多次获取的锁，获取和释放数量必须一致
```java
// true表示为公平锁，为fasle为非公平锁(默认)
// 等待时间最久的线程（最先请求的线程）会获得该锁
ReentrantLock lock = new ReentrantLock(true);

getHoldCount()  //返回上锁次数

isFair()        //判断锁是否是公平锁

isLocked()    //判断锁是否被任何线程获取了

isHeldByCurrentThread()   //判断锁是否被当前线程获取了

hasQueuedThreads()   //判断是否有线程在等待该锁

hasQueuedThread(Thread  thread) //查询指定线程是否获取锁

getQueueLength()    //返回等待锁的线程数估计值

// 未列出部分返回线程和指定条件的方法
```

### ReadWriteLock 
一个接口

### ReentrantReadWriteLock 可重入的读写锁
实现了ReadWriteLock接口

```java
private ReentrantReadWriteLock rwl = new ReentrantReadWriteLock();
rwl.readLock().lock();
rwl.readLock().unlock();
```
```java
getOwner()  //返回拥有者线程对象
getQueuedReaderThreads()    //返回等待读锁线程对象集合
getQueuedThreads()    //返回等待读写锁线程对象集合
getQueuedWriterThreads()    //返回等待写锁线程对象集合
getQueueLength()    //返回等待读写锁线程对象数量
getReadHoldCount()  //当前线程重入读锁数
getReadLockCount()  //当前读锁等待数
getWaitingThreads(Condition condition)
getWaitQueueLength(Condition condition)
getWriteHoldCount()     //读锁重入数
hasQueuedThread(Thread thread)  //查询指定线程是否在等待该锁
hasQueuedThreads()  //查询是否有线程在等待该锁
hasWaiters(Condition condition)
isFair()    //查询是否公平锁
isWriteLocked() //查询写锁是否被获取
isWriteLockedByCurrentThread()  //查询写锁是否被当前线程获取
readLock()  //返回用于读锁的锁对象
toString()  //返回锁的id字符串，上锁状态
writeLock() //返回用于写锁的锁对象
```
