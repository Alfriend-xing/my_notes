## HashMap
HashMap实现了Map接口，并继承 AbstractMap 抽象类，其中 Map 接口定义了键值映射规则。和 AbstractCollection抽象类在 Collection 族的作用类似， AbstractMap 抽象类提供了 Map 接口的骨干实现，以最大限度地减少实现Map接口所需的工作。

### 构造函数
```java
HashMap()
// 该构造函数意在构造一个具有> 默认初始容量 (16) 和 默认负载因子(0.75) 的空 HashMap

HashMap(int initialCapacity, float loadFactor)
// 该构造函数意在构造一个 指定初始容量 和 指定负载因子的空 HashMap

HashMap(int initialCapacity)
该构造函数意在构造一个指定初始容量和默认负载因子 (0.75)的空 HashMap

HashMap(Map<? extends K, ? extends V> m)
// 该构造函数意在构造一个与指定 Map 具有相同映射的 HashMap，
// 其初始容量不小于 16 (具体依赖于指定Map的大小)，负载因子是 0.75
```
### 初始容量 和 负载因子，
这两个参数是影响HashMap性能的重要参数。

- 容量表示哈希表中桶的数量 (table 数组的大小)，初始容量是创建哈希表时桶的数量；
- 负载因子是哈希表在其容量自动增加之前可以达到多满的一种尺度，它衡量的是一个散列表的空间的使用程度，负载因子越大表示散列表的装填程度越高，反之愈小。

### 存取
```java
// 键值对的存储
public V put(K key, V value)

// 读取实现
public V get(Object key)

// 扩容
// 为了保证HashMap的效率，系统必须要在某个临界点进行扩容处理，
// 该临界点就是HashMap中元素的数量在数值上等于threshold（table数组长度*加载因子）
// 扩容是一个非常耗时的过程
void resize(int newCapacity)

// 重哈希
// 重新计算原HashMap中的元素在新table数组中的位置并进行复制处理
void transfer(Entry[] newTable)
```


[参考链接](https://blog.csdn.net/justloveyou_/article/details/62893086)

