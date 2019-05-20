## 日期时间
java.util.Date 类提供了操作日期与时间的一些方法
```java
// 生成Date对象
// 没有任何参数，使用的是当前日期和时间来初始化对象
Date()
// 指定毫秒时间戳
Date(long millisec)
```
Date 类方法

方法|描述
--|--
boolean after(Date date)|若当调用此方法的 Date 对象在指定日期之后返回 true,否则返回 false
boolean before(Date date)|若当调用此方法的 Date 对象在指定日期之前返回 true,否则返回 false
Object clone()|返回此对象的副本
int compareTo(Date date)|比较当调用此方法的 Date 对象和指定日期。两者相等时候返回0。调用对象在指定日期之前则返回负数。调用对象在指定日期之后则返回正数
int compareTo(Object obj)|若 obj 是 Date 类型则操作等同于 compareTo(Date) 。否则它抛出 ClassCastException
boolean equals(Object date)|当调用此方法的 Date对象和指定日期相等时候返回 true,否则返回 false
long getTime()|返回自 1970 年 1 月 1 日 00:00:00 GMT 以来此 Date 对象表示的毫秒数
int hashCode( )|返回此对象的哈希码值
void setTime(long time)|用自1970年1月1日00:00:00 GMT以后time毫秒数设置时间和日期
String toString()|转换 Date 对象为 String 表示形式，并返回该字符串
### 休眠( sleep )
Thread.sleep() 方法可以使当前线程进入停滞状态 ( 阻塞当前线程 )，让出 CPU 的使用权
```java
// 单位毫秒
Thread.sleep(1000*3);
```
### 测量时间
System.currentTimeMillis() 方法返回当前程序运行的时间
```java
long start = System.currentTimeMillis( );
System.out.println(new Date( ) + "\n");
Thread.sleep(3000);
System.out.println(new Date( ) + "\n");
long end = System.currentTimeMillis( );
long diff = end - start;
System.out.println("Difference is : " + diff);
// 输出
// Tue Jan 23 08:12:43 CST 2018
// Tue Jan 23 08:12:46 CST 2018
// Difference is : 3044
```