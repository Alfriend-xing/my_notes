## collection
来源于Java.util包，是非常实用常用的数据结构

```java
// 主要方法

boolean add(Object o)
// 添加对象到集合
boolean remove(Object o)
// 删除指定的对象
int size()
// 返回当前集合中元素的数量
boolean contains(Object o)
// 查找集合中是否有指定的对象
boolean isEmpty()
// 判断集合是否为空
Iterator iterator()
// 返回一个迭代器
boolean containsAll(Collection c)
// 查找集合中是否有集合c中的元素
boolean addAll(Collection c)
// 将集合c中所有的元素添加给该集合
void clear()
// 删除集合中所有元素
void removeAll(Collection c)
// 从集合中删除c集合中也有的元素
void retainAll(Collection c)
// 从集合中删除集合c中不包含的元素
```
子接口对象
```java
// List(抽象接口，可重复有序)主要方法

void add(int index,Object element)
// 在指定位置上添加一个对象
boolean addAll(int index,Collection c)
// 将集合c的元素添加到指定的位置
Object get(int index)
// 返回List中指定位置的元素
int indexOf(Object o)
// 返回第一个出现元素o的位置.
Object remove(int index)
// 删除指定位置的元素
Object set(int index,Object element)
// 用元素element取代位置index上的元素,返回被取代的元素
void sort()

// List主要子接口对象
LinkedList// 没有同步方法
ArrayList// 非同步的（unsynchronized）
Vector// (同步) 非常类似ArrayList，但是Vector是同步的 
Stack// 记住 push和pop方法，还有peek方法得到栈顶的元素，empty方法测试堆栈是否为空，search方法检测一个元素在堆栈中的位置。注意：Stack刚创建后是空栈。
```
```java
// Set(不包含重复的元素)主要方法

// Set主要子接口对象
HashSet
SortSet
TreeSet
```
- 如果涉及到堆栈，队列（先进后出）等操作，应该考虑用List，对于需要快速插入，删除元素，应该使用LinkedList，如果需要快速随机访问元素，应该使用ArrayList。
- 如果程序在单线程环境中，或者访问仅仅在一个线程中进行，考虑非同步的类，其效率较高，如果多个线程可能同时操作一个类，应该使用同步的类。
- 要特别注意对哈希表的操作，作为key的对象要正确复写equals和hashCode方法。
- 尽量返回接口而非实际的类型，如返回List而非ArrayList，这样如果以后需要将ArrayList换成LinkedList时，客户端代码不用改变。这就是针对抽象编程。
- ArrayList、HashSet/LinkedHashSet、PriorityQueue、LinkedList是线程不安全的，



[参考链接](https://www.cnblogs.com/taiwan/p/6954135.html)


![](http://www.runoob.com/wp-content/uploads/2014/01/2243690-9cd9c896e0d512ed.gif)
![](http://img.blog.csdn.net/20160706172512559?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)