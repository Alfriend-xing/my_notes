## set
Set继承于Collection接口，是一个不允许出现重复元素，并且无序的集合，主要有HashSet和TreeSet两大实现类。

### set
- 与List接口一样，Set接口也提供了集合操作的基本方法。\
- 但与List不同的是，Set还提供了equals(Object o)和hashCode()，供其子类重写，以实现对集合中插入重复元素的处理；
```java
A:添加功能
boolean add(E e);
boolean addAll(Collection<? extends E> c);

B:删除功能
boolean remove(Object o);
boolean removeAll(Collection<?> c);
void clear();

C:长度功能
int size();

D:判断功能
boolean isEmpty();
boolean contains(Object o);
boolean containsAll(Collection<?> c);
boolean retainAll(Collection<?> c); 

E:获取Set集合的迭代器：
Iterator<E> iterator();

F:把集合转换成数组
Object[] toArray();
<T> T[] toArray(T[] a);

//判断元素是否重复，为子类提高重写方法
boolean equals(Object o);
int hashCode();
```

### HashSet
- 不允许出现重复因素；
- 允许插入Null值；
- 元素无序（添加顺序和遍历顺序不一致）；
- 线程不安全，若2个线程同时操作HashSet，必须通过代码实现同步；
```java
//创建HashSet集合：
Set<String> hashSet = new HashSet<String>();
System.out.println("HashSet初始容量大小："+hashSet.size());

//元素添加：
hashSet.add("my");
hashSet.add("name");
hashSet.add("is");
hashSet.add("jiaboyan");
hashSet.add(",");
hashSet.add("hello");
hashSet.add("world");
hashSet.add("!");
System.out.println("HashSet容量大小："+hashSet.size());

//迭代器遍历：
Iterator<String> iterator = hashSet.iterator();
while (iterator.hasNext()){
    String str = iterator.next();
    System.out.println(str);
}
//增强for循环
for(String str:hashSet){
    if("jiaboyan".equals(str)){
        System.out.println("你就是我想要的元素:"+str);
    }
    System.out.println(str);
}

//元素删除：
hashSet.remove("jiaboyan");
System.out.println("HashSet元素大小：" + hashSet.size());
hashSet.clear();
System.out.println("HashSet元素大小：" + hashSet.size());

//集合判断：
boolean isEmpty = hashSet.isEmpty();
System.out.println("HashSet是否为空：" + isEmpty);
boolean isContains = hashSet.contains("hello");
System.out.println("HashSet是否为空：" + isContains);
```
### SortSet

### TreeSet
- 对插入的元素进行排序，是一个有序的集合（主要与HashSet的区别）;
- 底层使用红黑树结构，而不是哈希表结构；
- 允许插入Null值；
- 不允许插入重复元素；
- 线程不安全；
```java
TreeSet<String> treeSet = new TreeSet<String>();
System.out.println("TreeSet初始化容量大小："+treeSet.size());

//元素添加：
treeSet.add("my");
treeSet.add("name");
treeSet.add("jiaboyan");
treeSet.add("hello");
treeSet.add("world");
treeSet.add("1");
treeSet.add("2");
treeSet.add("3");
System.out.println("TreeSet容量大小：" + treeSet.size());
System.out.println("TreeSet元素顺序为：" + treeSet.toString());

//增加for循环遍历：
for(String str:treeSet){
    System.out.println("遍历元素："+str);
}

//迭代器遍历：升序
Iterator<String> iteratorAesc = treeSet.iterator();
while(iteratorAesc.hasNext()){
    String str = iteratorAesc.next();
    System.out.println("遍历元素升序："+str);
}

//迭代器遍历：降序
Iterator<String> iteratorDesc = treeSet.descendingIterator();
while(iteratorDesc.hasNext()){
    String str = iteratorDesc.next();
    System.out.println("遍历元素降序："+str);
}

//元素获取:实现NavigableSet接口
String firstEle = treeSet.first();//获取TreeSet头节点：
System.out.println("TreeSet头节点为：" + firstEle);

// 获取指定元素之前的所有元素集合：(不包含指定元素)
SortedSet<String> headSet = treeSet.headSet("jiaboyan");
System.out.println("jiaboyan节点之前的元素为："+headSet.toString());

//获取给定元素之间的集合：（包含头，不包含尾）
SortedSet subSet = treeSet.subSet("1","world");
System.out.println("1--jiaboan之间节点元素为："+subSet.toString());

//集合判断：
boolean isEmpty = treeSet.isEmpty();
System.out.println("TreeSet是否为空："+isEmpty);
boolean isContain = treeSet.contains("who");
System.out.println("TreeSet是否包含who元素："+isContain);

//元素删除：
boolean jiaboyanRemove = treeSet.remove("jiaboyan");
System.out.println("jiaboyan元素是否被删除"+jiaboyanRemove);

//集合中不存在的元素，删除返回false
boolean whoRemove = treeSet.remove("who");
System.out.println("who元素是否被删除"+whoRemove);

//删除并返回第一个元素：如果set集合不存在元素，则返回null
String pollFirst = treeSet.pollFirst();
System.out.println("删除的第一个元素："+pollFirst);

//删除并返回最后一个元素：如果set集合不存在元素，则返回null
String pollLast = treeSet.pollLast();
System.out.println("删除的最后一个元素："+pollLast);


treeSet.clear();//清空集合:
```



[参考链接](https://www.jianshu.com/p/b48c47a42916)


![](set.PNG)
