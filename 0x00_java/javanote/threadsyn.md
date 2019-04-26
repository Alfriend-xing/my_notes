## 线程同步
java允许多线程并发控制，当多个线程同时操作一个可共享的资源变量时（如数据的增删改查），将会导致数据不准确，相互之间产生冲突，因此加入同步锁以避免在该线程没有完成操作之前，被其他线程的调用， 从而保证了该变量的唯一性和准确性。







### 同步方法 

即有synchronized关键字修饰的方法。 
由于java的每个对象都有一个内置锁，当用此关键字修饰方法时， 内置锁会保护整个方法。在调用该方法前，需要获得内置锁，否则就处于阻塞状态。

```java
public synchronized void save(){}


// 用同步方法实现
public synchronized void save(int money) {
    account += money;
}
```

>synchronized关键字也可以修饰静态方法，此时如果调用该静态方法，将会锁住整个类

### 同步代码块 
即有synchronized关键字修饰的语句块。 
被该关键字修饰的语句块会自动被加上内置锁，从而实现同步

```java
synchronized(object){ 
}


// 用同步代码块实现
public void save1(int money) {
    synchronized (this) {
        account += money;
    }
}
```

>同步是一种高开销的操作，因此应该尽量减少同步的内容。 
通常没有必要同步整个方法，使用synchronized代码块同步关键代码即可。

### 使用特殊域变量(volatile)实现线程同步
- volatile关键字为域变量的访问提供了一种免锁机制， 
- 使用volatile修饰域相当于告诉虚拟机该域可能会被其他线程更新， 
- 因此每次使用该域就要重新计算，而不是使用寄存器中的值 
- volatile不会提供任何原子操作，它也不能用来修饰final类型的变量 
```java
class Bank {
    //需要同步的变量加上volatile
    private volatile int account = 100;

    public int getAccount() {
        return account;
    }
    //这里不再需要synchronized 
    public void save(int money) {
        account += money;
    }
｝
```

### 使用重入锁实现线程同步
在JavaSE5.0中新增了一个java.util.concurrent包来支持同步。 
ReentrantLock类是可重入、互斥、实现了Lock接口的锁， 
它与使用synchronized方法和快具有相同的基本行为和语义，并且扩展了其能力

```java
// ReenreantLock类的常用方法有：

ReentrantLock() //创建一个ReentrantLock实例 
lock() //获得锁 
unlock()  // 释放锁 

private Lock lock = new ReentrantLock();
lock.lock();
lock.unlock();
```
关于Lock对象和synchronized关键字的选择： 
- 最好两个都不用，使用一种java.util.concurrent包提供的机制， 
    能够帮助用户处理所有与锁相关的代码。 
- 如果synchronized关键字能满足用户的需求，就用synchronized，因为它能简化代码 
- 如果需要更高级的功能，就用ReentrantLock类，此时要注意及时释放锁，否则会出现死锁，通常在finally代码释放锁 
### 使用局部变量实现线程同步 
如果使用ThreadLocal管理变量，则每一个使用该变量的线程都获得该变量的副本，副本之间相互独立，这样每一个线程都可以随意修改自己的变量副本，而不会对其他线程产生影响。
```java
// ThreadLocal 类的常用方法

ThreadLocal()// T创建一个线程本地变量 
get()// T返回此线程局部变量的当前线程副本中的值 
initialValue()// T返回此线程局部变量的当前线程的"初始值" 
set(T value) // T将此线程局部变量的当前线程副本中的值设置为value

public class Bank{
    //使用ThreadLocal类管理共享变量account
    private static ThreadLocal<Integer> account = new ThreadLocal<Integer>(){
        @Override
        protected Integer initialValue(){
            return 100;
        }
    };
    public void save(int money){
        account.set(account.get()+money);
    }
    public int getAccount(){
        return account.get();
    }
}
```
ThreadLocal与同步机制 
- ThreadLocal与同步机制都是为了解决多线程中相同变量的访问冲突问题。 
- 前者采用以"空间换时间"的方法，后者采用以"时间换空间"的方式

### 使用阻塞队列实现线程同步
前面5种同步方式都是在底层实现的线程同步，但是我们在实际开发当中，应当尽量远离底层结构。 
```java
// LinkedBlockingQueue 类常用方法 
LinkedBlockingQueue() // 创建一个容量为Integer.MAX_VALUE的LinkedBlockingQueue 
put(E e)// 在队尾添加一个元素，如果队列满则阻塞 
size() //  返回队列中的元素个数 
take() //  移除并返回队头元素，如果队列空则阻塞 

private LinkedBlockingQueue<Integer> queue = new LinkedBlockingQueue<Integer>();

```
### 使用原子变量实现线程同步
- 需要使用线程同步的根本原因在于对普通变量的操作不是原子的。
原子操作就是指将读取变量值、修改变量值、保存变量值看成一个整体来操作
即-这几种行为要么同时完成，要么都不完成。
- 在java的util.concurrent.atomic包中提供了创建了原子类型变量的工具类，
使用该类可以简化线程同步。
- 其中AtomicInteger 表可以用原子方式更新int的值，可用在应用程序中(如以原子方式增加的计数器)，
但不能用于替换Integer；可扩展Number，允许那些处理机遇数字类的工具和实用工具进行统一访问。

```java
// AtomicInteger类常用方法：

AtomicInteger(int initialValue) //  创建具有给定初始值的新的AtomicInteger
addAddGet(int dalta) //  以原子方式将给定值与当前值相加
get() // 获取当前值

class Bank {
    private AtomicInteger account = new AtomicInteger(100);

    public AtomicInteger getAccount() {
        return account;
        }

    public void save(int money) {
        account.addAndGet(money);
        }
    } 
```
[参考链接](https://www.cnblogs.com/XHJT/p/3897440.html)