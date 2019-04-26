## Java 数组

### list
- 存储一个固定大小的相同类型元素的有序集合
- 在List集合中，我们常用到ArrayList和LinkedList这两个类
```java
// 声明数组变量
dataType[] arrayRefVar;
double[] myList; 

// 创建数组
arrayRefVar = new dataType[arraySize];

// 声明和创建
dataType[] arrayRefVar = new dataType[arraySize];

// 初始化
dataType[] arrayRefVar = {value0, value1, ..., valuek};

// 数组的元素是通过索引访问的
// 数组索引从 0 开始，所以索引值从 0 到 arrayRefVar.length-1
myList[0] = 5.6;


// 多维数组
// arraylenght1 为行数，arraylenght2 为列数，必须为正整数
type arrayName = new type[arraylenght1][arraylenght2];
String str[][] = new String[3][4];

String s[][] = new String[2][];
s[0] = new String[2];
s[1] = new String[3];
s[0][0] = new String("Good");
// s[0]=new String[2] 和 s[1]=new String[3] 是为最高维分配引用空间，
// 也就是为最高维限制其能保存数据的最长的长度，
// 然后再为其每个数组元素单独分配空间 s0=new String("Good") 等操作

A:添加功能
boolean add(E e):向集合中添加一个元素
void add(int index, E element):在指定位置添加元素
boolean addAll(Collection<? extends E> c)：向集合中添加一个集合的元素。
    
B:删除功能
void clear()：删除集合中的所有元素
E remove(int index)：根据指定索引删除元素，并把删除的元素返回
boolean remove(Object o)：从集合中删除指定的元素
boolean removeAll(Collection<?> c):从集合中删除一个指定的集合元素。

C:修改功能
E set(int index, E element):把指定索引位置的元素修改为指定的值，返回修改前的值。

D:获取功能
E get(int index)：获取指定位置的元素
Iterator iterator():就是用来获取集合中每一个元素。

E:判断功能
boolean isEmpty()：判断集合是否为空。
boolean contains(Object o)：判断集合中是否存在指定的元素。
boolean containsAll(Collection<?> c)：判断集合中是否存在指定的一个集合中的元素。

F:长度功能
int size():获取集合中的元素个数

G:把集合转换成数组
Object[] toArray():把集合变成数组。
```
类方法
方法|说明
--|--
public static int binarySearch(Object[] a, Object key)|用二分查找算法在给定数组中搜索给定值的对象(Byte,Int,double等)。数组在调用前必须排序好的。如果查找值包含在数组中，则返回搜索键的索引；否则返回 ( -(插入点) - 1)
public static boolean equals(long[] a, long[] a2)|如果两个指定的 long 型数组彼此相等，则返回 true。如果两个数组包含相同数量的元素，并且两个数组中的所有相应元素对都是相等的，则认为这两个数组是相等的。换句话说，如果两个数组以相同顺序包含相同的元素，则两个数组是相等的。同样的方法适用于所有的其他基本数据类型 ( Byte，short，Int等 )
public static void fill(int[] a, int val)|将指定的 int 值分配给指定 int 型数组指定范围中的每个元素。同样的方法适用于所有的其他基本数据类型 ( Byte，short，Int 等 )
public static void sort(Object[] a)|对指定对象数组根据其元素的自然顺序进行升序排列。同样的方法适用于所有的其他基本数据类型 ( Byte，short，Int等 )

### ArrayList
是Java集合框架中使用最多的一个类，是一个数组队列，线程不安全集合。
- 容量不固定，随着容量的增加而动态扩容（阈值基本不会达到）
- 有序集合（插入的顺序==输出的顺序）
- 插入的元素可以为null
- 增删改查效率更高（相对于LinkedList来说）
- 线程不安全
```java
List<String> list = new ArrayList<String>();
System.out.println("ArrayList集合初始化容量："+list.size());

//添加功能：
list.add("Hello");
list.add("world");
list.add(2,"!");
System.out.println("ArrayList当前容量："+list.size());

//修改功能：
list.set(0,"my");
list.set(1,"name");
System.out.println("ArrayList当前内容："+list.toString());

//获取功能：
String element = list.get(0);
System.out.println(element);

//迭代器遍历集合：(ArrayList实际的跌倒器是Itr对象)
Iterator<String> iterator =  list.iterator();
while(iterator.hasNext()){
    String next = iterator.next();
    System.out.println(next);
}

//for循环迭代集合：
for(String str:list){
    System.out.println(str);
}

//判断功能：
boolean isEmpty = list.isEmpty();
boolean isContain = list.contains("my");

//长度功能：
int size = list.size();

//把集合转换成数组：
String[] strArray = list.toArray(new String[]{});

//删除功能：
list.remove(0);
list.remove("world");
list.clear();
System.out.println("ArrayList当前容量："+list.size());
```
### LinkedList
是一个双向链表，每一个节点都拥有指向前后节点的引用。相比于ArrayList来说，LinkedList的随机访问效率更低。
- LinkedList实现List，得到了List集合框架基础功能；
- LinkedList实现Deque，Deque 是一个双向队列，也就是既可以先入先出，又可以先入后出，说简单些就是既可以在头部添加元素，也可以在尾部添加元素；
- LinkedList实现Cloneable，得到了clone()方法，可以实现克隆功能；
- LinkedList实现Serializable，表示可以被序列化，通过序列化去传输，典型的应用就是hessian协议。

```java
List<String> linkedList = new LinkedList<String>();
System.out.println("LinkedList初始容量："+linkedList.size());

//添加功能：
linkedList.add("my");
linkedList.add("name");
linkedList.add("is");
linkedList.add("jiaboyan");
System.out.println("LinkedList当前容量："+ linkedList.size());

//修改功能:
linkedList.set(0,"hello");
linkedList.set(1,"world");
System.out.println("LinkedList当前内容："+ linkedList.toString());

//获取功能：
String element = linkedList.get(0);
System.out.println(element);

//遍历集合：(LinkedList实际的跌倒器是ListItr对象)
Iterator<String> iterator =  linkedList.iterator();
while(iterator.hasNext()){
    String next = iterator.next();
    System.out.println(next);
}
//for循环迭代集合：
for(String str:linkedList){
    System.out.println(str);
}

//判断功能：
boolean isEmpty = linkedList.isEmpty();
boolean isContains = linkedList.contains("jiaboyan");

//长度功能：
int size = linkedList.size();

//删除功能：
linkedList.remove(0);
linkedList.remove("jiaboyan");
linkedList.clear();
System.out.println("LinkedList当前容量：" + linkedList.size());
```




[参考链接](https://www.jianshu.com/p/25aa92f8d681)








