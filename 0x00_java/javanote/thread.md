## 线程概念
- 一个多线程程序包含两个或多个能并发运行的部分
- 程序的每一部分都称作一个线程，并且每个线程定义了一个独立的执行路径
- 多线程是多任务的一种特别的形式，但多线程使用了更小的资源开销
- 多线程能满足程序员编写高效率的程序来达到充分利用 CPU 的目的
- Java 给多线程编程提供了内置的支持


### 通过实现 Runnable 接口来创建线程
- 创建一个实现 Runnable 接口的类
- 重写run()方法
- 在类中实例化一个线程对象
- 调用线程对象的 start() 方法使其运行
```java
class RunnableDemo implements Runnable {
   private Thread t;
   private String threadName;

   RunnableDemo( String name) {
      threadName = name;
      System.out.println("Creating " +  threadName );
   }

   public void run() {
      System.out.println("Running " +  threadName );
      try {
         for(int i = 4; i > 0; i--) {
            System.out.println("Thread: " + threadName + ", " + i);
            // 让线程睡眠一会
            Thread.sleep(50);
         }
      }catch (InterruptedException e) {
         System.out.println("Thread " +  threadName + " interrupted.");
      }
      System.out.println("Thread " +  threadName + " exiting.");
   }

   public void start () {
      System.out.println("Starting " +  threadName );
      if (t == null) {
         t = new Thread (this, threadName);
         t.start ();
      }
   }
}

public class TestThread {

   public static void main(String args[]) {
      RunnableDemo R1 = new RunnableDemo( "Thread-1");
      R1.start();

      RunnableDemo R2 = new RunnableDemo( "Thread-2");
      R2.start();
   }   
}
```

### 通过继承 Thread 来创建线程
- 创建一个新的类，该类继承 Thread 类
- 创建一个该类的实例
- 继承类必须重写 run() 方法
- 也必须调用 start() 方法才能执行
```java
class ThreadDemo extends Thread {
   private Thread t;
   private String threadName;

   ThreadDemo( String name) {
      threadName = name;
      System.out.println("Creating " +  threadName );
   }

   public void run() {
      System.out.println("Running " +  threadName );
      try {
         for(int i = 4; i > 0; i--) {
            System.out.println("Thread: " + threadName + ", " + i);
            // 让线程睡醒一会
            Thread.sleep(50);
         }
      }catch (InterruptedException e) {
         System.out.println("Thread " +  threadName + " interrupted.");
      }
      System.out.println("Thread " +  threadName + " exiting.");
   }

   public void start () {
      System.out.println("Starting " +  threadName );
      if (t == null) {
         t = new Thread (this, threadName);
         t.start ();
      }
   }
}

public class TestThread {

   public static void main(String args[]) {
      ThreadDemo T1 = new ThreadDemo( "Thread-1");
      T1.start();

      ThreadDemo T2 = new ThreadDemo( "Thread-2");
      T2.start();
   }   
}
```

Thread 类的方法

Thread 类的一些重要方法

方法|描述
--|--
public void start()|使该线程开始执行；Java虚拟机调用该线程的 run 方法
public void run()|如果该线程是使用独立的 Runnable 运行对象构造的，则调用该 Runnable 对象的 run 方法；否则，该方法不执行任何操作并返回
public final void setName(String name)|改变线程名称，使之与参数 name 相同
public final void setPriority(int priority)|更改线程的优先级
public final void setDaemon(boolean on)|将该线程标记为守护线程或用户线程
public final void join(long millisec)|等待该线程终止的时间最长为 millis 毫秒
public void interrupt()|中断线程
public final boolean isAlive()|测试线程是否处于活动状态

Thread 类的静态方法

方法描述|
--|
public static void yield()|
暂停当前正在执行的线程对象，并执行其他线程|
public static void sleep(long millisec)|
在指定的毫秒数内让当前正在执行的线程休眠（暂停执行），此操作受到系统计时器和调度程序精度和准确性的影响|
public static boolean holdsLock(Object x)|
当且仅当当前线程在指定的对象上保持监视器锁时，才返回 true|
public static Thread currentThread()|
返回对当前正在执行的线程对象的引用|
public static void dumpStack()|
将当前线程的堆栈跟踪打印至标准错误流|


### 通过 Callable 和 Future 创建线程
- 创建 Callable 接口的实现类，并实现 call() 方法，该 call() 方法将作为线程执行体，并且有返回值
- 创建 Callable 实现类的实例，使用 FutureTask 类来包装 Callable 对象，该 FutureTask 对象封装了该 Callable 对象的 call() 方法的返回值
- 使用 FutureTask 对象作为 Thread 对象的 target 创建并启动新线程
- 调用 FutureTask 对象的 get() 方法来获得子线程执行结束后的返回值
```java
public class CallableThreadTest implements Callable<Integer> {
    public static void main(String[] args)  
    {  
        CallableThreadTest ctt = new CallableThreadTest();  
        FutureTask<Integer> ft = new FutureTask<>(ctt);  
        for(int i = 0;i < 100;i++)  
        {  
            System.out.println(Thread.currentThread().getName()+" 的循环变量i的值"+i);  
            if(i==20)  
            {  
                new Thread(ft,"有返回值的线程").start();  
            }  
        }  
        try  
        {  
            System.out.println("子线程的返回值："+ft.get());  
        } catch (InterruptedException e)  
        {  
            e.printStackTrace();  
        } catch (ExecutionException e)  
        {  
            e.printStackTrace();  
        }  

    }
    @Override  
    public Integer call() throws Exception  
    {  
        int i = 0;  
        for(;i<100;i++)  
        {  
            System.out.println(Thread.currentThread().getName()+" "+i);  
        }  
        return i;  
    }  
}
```

### 创建线程的三种方式的对比
- 采用实现 Runnable、Callable 接口的方式创见多线程时，线程类只是实现了 Runnable 接口或 Callable 接口，还可以继承其他类
- 使用继承 Thread 类的方式创建多线程时，编写简单，如果需要访问当前线程，则无需使用 Thread.currentThread() 方法，直接使用 this 即可获得当前线程

注意
- 通过对多线程的使用，可以编写出非常高效的程序
- 不过请注意，如果创建太多的线程，程序执行的效率实际上是降低了，而不是提升了
- 请记住，上下文的切换开销也很重要，如果你创建了太多的线程，CPU 花费在上下文的切换的时间将多于执行程序的时间
