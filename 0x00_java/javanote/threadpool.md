## 线程池
Java Platform SE 8
```java
// 传统线程使用方法
new Thread(new Runnable() {

    @Override
    public void run() {
        // TODO Auto-generated method stub
        }
    }
).start();
```
缺点
- 每次new Thread新建对象性能差。 
- 线程缺乏统一管理，可能无限制新建线程，相互之间竞争，及可能占用过多系统资源导致死机或oom。 
- 缺乏更多功能，如定时执行、定期执行、线程中断。

相比new Thread，Java提供的四种线程池的好处在于：

- 重用存在的线程，减少对象创建、消亡的开销，性能佳。 
- 可有效控制最大并发线程数，提高系统资源的使用率，同时避免过多资源竞争，避免堵塞。 
- 提供定时执行、定期执行、单线程、并发数控制等功能。

Java通过Executors提供四种线程池，分别为
- newCachedThreadPool创建一个可缓存线程池，如果线程池长度超过处理需要，可灵活回收空闲线程，若无可回收，则新建线程。
- newFixedThreadPool 创建一个定长线程池，可控制线程最大并发数，超出的线程会在队列中等待。
- newScheduledThreadPool 创建一个定长线程池，支持定时及周期性任务执行。
- newSingleThreadExecutor 创建一个单线程化的线程池，它只会用唯一的工作线程来执行任务，保证所有任务按照指定顺序(FIFO, LIFO, 优先级)执行。

### newCachedThreadPool
线程池为无限大，当执行第二个任务时第一个任务已经完成，会复用执行第一个任务的线程，而不用每次新建线程。
```java
ExecutorService cachedThreadPool = Executors.newCachedThreadPool();
cachedThreadPool.execute(new Runnable() {
                        @Override
                        public void run() {
                            System.out.println(index);
                            }
                        });
```

### newFixedThreadPool
创建一个定长线程池，可控制线程最大并发数，超出的线程会在队列中等待
```java
ExecutorService fixedThreadPool = Executors.newFixedThreadPool(3);
```

### newScheduledThreadPool
创建一个定长线程池，支持定时及周期性任务执行
```java
ScheduledExecutorService scheduledThreadPool = Executors.newScheduledThreadPool(5);

// 延时执行
 scheduledThreadPool.schedule(new Runnable() , 3, TimeUnit.SECONDS);

// 延时1秒后，每3秒执行一次
scheduledThreadPool.scheduleAtFixedRate(new Runnable() , 1, 3, TimeUnit.SECONDS);
```

### newSingleThreadExecutor
创建一个单线程化的线程池，它只会用唯一的工作线程来执行任务，保证所有任务按照指定顺序(FIFO, LIFO, 优先级)执行
```java
ExecutorService singleThreadExecutor = Executors.newSingleThreadExecutor();
singleThreadExecutor.execute(new Runnable())
```

### 实例
```java
package app.executors;  

import java.util.concurrent.Executors;  
import java.util.concurrent.ExecutorService;  


public class Test {  
    public static void main(String[] args) {  
        // 创建一个可重用固定线程数的线程池  
        ExecutorService pool = Executors.newFixedThreadPool(2);  
        // 创建线程  
        Thread t1 = new MyThread();  
        Thread t2 = new MyThread();  
        Thread t3 = new MyThread();  
        Thread t4 = new MyThread();  
        Thread t5 = new MyThread();  
        // 将线程放入池中进行执行  
        pool.execute(t1);  
        pool.execute(t2);  
        pool.execute(t3);  
        pool.execute(t4);  
        pool.execute(t5);  
        // 关闭线程池  
        pool.shutdown();  
    }  
}  

class MyThread extends Thread {  
    @Override  
    public void run() {  
        System.out.println(Thread.currentThread().getName() + "正在执行。。。");  
    }  
}  

```
## ThreadPoolExecutor
Java Platform SE 7
实现Executor, ExecutorService接口
子类ScheduledThreadPoolExecutor

### 构造方法
```java
public ThreadPoolExecutor(int corePoolSize,int maximumPoolSize,long keepAliveTime,TimeUnit unit,
BlockingQueue<Runnable> workQueue);

public ThreadPoolExecutor(int corePoolSize,int maximumPoolSize,long keepAliveTime,TimeUnit unit,
BlockingQueue<Runnable> workQueue,ThreadFactory threadFactory);

public ThreadPoolExecutor(int corePoolSize,int maximumPoolSize,long keepAliveTime,TimeUnit unit,
BlockingQueue<Runnable> workQueue,RejectedExecutionHandler handler);

public ThreadPoolExecutor(int corePoolSize,int maximumPoolSize,long keepAliveTime,TimeUnit unit,
BlockingQueue<Runnable> workQueue,ThreadFactory threadFactory,RejectedExecutionHandler handler);
```
参数说明
- corePoolSize：核心池的大小，预创建线程
- maximumPoolSize：线程池最大线程数
- keepAliveTime：表示线程没有任务执行时最多保持多久时间会终止
- unit：参数keepAliveTime的时间单位
```java
TimeUnit.DAYS;               //天
TimeUnit.HOURS;             //小时
TimeUnit.MINUTES;           //分钟
TimeUnit.SECONDS;           //秒
TimeUnit.MILLISECONDS;      //毫秒
TimeUnit.MICROSECONDS;      //微妙
TimeUnit.NANOSECONDS;       //纳秒
```
- workQueue：一个阻塞队列，用来存储等待执行的任务
```java
ArrayBlockingQueue;
LinkedBlockingQueue;
SynchronousQueue;
```
- threadFactory：线程工厂，主要用来创建线程；
- handler：表示当拒绝处理任务时的策略，有以下四种取值：
```java
ThreadPoolExecutor.AbortPolicy:丢弃任务并抛出RejectedExecutionException异常。 
ThreadPoolExecutor.DiscardPolicy：也是丢弃任务，但是不抛出异常。 
ThreadPoolExecutor.DiscardOldestPolicy：丢弃队列最前面的任务，然后重新尝试执行任务（重复此过程）
ThreadPoolExecutor.CallerRunsPolicy：由调用线程处理该任务 
```
方法
```java
afterExecute(Runnable r, Throwable t)   //给线程添加回调
awaitTermination(long timeout, TimeUnit unit)   //在所有任务完成前阻塞
beforeExecute(Thread t, Runnable r)     //线程执行前调用
execute(Runnable command)   //异步执行线程
finalize()  //当线程池不活跃且无任务时shutdown
getActiveCount()
getCompletedTaskCount()
getCorePoolSize()
getKeepAliveTime(TimeUnit unit)
getLargestPoolSize()    //获取线程池任务数历史峰值
getMaximumPoolSize()
getPoolSize()   //返回池中当前线程数
getQueue()      //返回使用的任务队列
getTaskCount()  //返回线程池执行的任务总数
getThreadFactory()  //返回创建线程的线程工厂
isShutdown()
isTerminated()
isTerminating() //返回是否正在终止(shutdown() or shutdownNow()后未终止时)
prestartAllCoreThreads()    //启动所有核心线程，等待任务
prestartCoreThread()    //启动一个核心线程等待任务
purge()//尝试从工作队列中删除已取消的所有Future任务
remove(Runnable task)//尚未启动时删除任务
setCorePoolSize(int corePoolSize)
setKeepAliveTime(long time, TimeUnit unit)
setMaximumPoolSize(int maximumPoolSize)
setThreadFactory(ThreadFactory threadFactory)
shutdown()//不在接收新任务，等待现有任务结束
shutdownNow()//尝试停止所有正在执行的任务，返回未执行的任务列表
toString()//返回线程池字符串和状态
```
https://docs.oracle.com/javase/7/docs/api/java/util/concurrent/ThreadPoolExecutor.html
https://docs.oracle.com/javase/7/docs/api/java/util/concurrent/ExecutorService.html
https://docs.oracle.com/javase/7/docs/api/java/util/concurrent/Executors.html#newFixedThreadPool(int)
[参考链接](https://blog.csdn.net/u011974987/article/details/51027795)