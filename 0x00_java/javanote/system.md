## System
- System类代表系统，系统级的很多属性和控制方法都放置在该类的内部。该类位于java.lang包。
- 由于该类的构造方法是private的，所以无法创建该类的对象，也就是无法实例化该类。其内部的成员方法和成员变量都是static（静态）的，所以也可以很方便的调用他。
```java
System.arraycopy(a,b,c,d,e);

// 其中，a是被复制的数组，b是复制的起始位置，c是复制到的数组，
// d是复制到这个数组的起始位置，e是复制到这个数组的结束位置。
```
```java
System.currentTimeMillis();

// 返回毫秒数
```
```java
public static String getProperty(String key)
// 获取系统属性
```
属性名

属性|说明
--|--
`java.version`|Java 运行时环境版本
`java.homeJava`|安装目录
`os.name`|操作系统的名称
`os.version`|操作系统的版本
`user.name`|用户的账户名称
`user.home`|用户的主目录
`user.dir`|用户的当前工作目录

```java
public static void gc()

// 该方法的作用是请求系统进行垃圾回收
```
```java
public static void exit(int status)

// 该方法的作用是退出程序。其中status的值为0代表正常退出，非零代表异常退出。
// 使用该方法可以在图形界面编程中实现程序的退出功能等。
// 这是唯一一个能够退出程序并不执行finally的情况。
// 退出虚拟机会直接终止整个程序，这时的程序已经不是从代码的层面来终止程序，
// 所以finally不会被执行。
```