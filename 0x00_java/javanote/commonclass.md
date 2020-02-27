# Random
## java.lang.Math.Random
- 调用这个Math.Random()函数能够返回带正号的double值，
- 该值大于等于0.0且小于1.0，即取值范围是[0.0,1.0)的左闭右开区间

```java
package IO;
import java.util.Random;

public class TestRandom {
    
    public static void main(String[] args) {
        // 案例1
        System.out.println("Math.random()=" + Math.random());// 结果是个double类型的值，区间为[0.0,1.0）
        int num = (int) (Math.random() * 3); // 注意不要写成(int)Math.random()*3，这个结果为0，因为先执行了强制转换
        System.out.println("num=" + num);
        /**
         * 输出结果为：
         * 
         * Math.random()=0.02909671613289655
         * num=0
         * 
         */
　　}
}
```
## java.util.Random

```java
// 两种构造方法：

// ：创建一个新的随机数生成器。
Random()

// ：使用单个 long 种子创建一个新的随机数生成器。
Random(long seed)

//设置种子，在没带参数构造函数生成的Random对象的种子缺省是当前系统时间的毫秒数
Random rand =new Random(25);
int i;
//产生的随机数为0-100的整数,不包括100
i=rand.nextInt(100);

protected int next(int bits)
//生成下一个伪随机数。
boolean nextBoolean()
//返回下一个伪随机数，它是取自此随机数生成器序列的均匀分布的boolean值。
void nextBytes(byte[] bytes)
//生成随机字节并将其置于用户提供的 byte 数组中。
double nextDouble()
//返回下一个伪随机数，它是取自此随机数生成器序列的、在0.0和1.0之间均匀分布的 double值。
float nextFloat()
//返回下一个伪随机数，它是取自此随机数生成器序列的、在0.0和1.0之间均匀分布float值。
double nextGaussian()
//返回下一个伪随机数，它是取自此随机数生成器序列的、呈高斯（“正态”）分布的double值，其平均值是0.0标准差是1.0。
int nextInt()
//返回下一个伪随机数，它是此随机数生成器的序列中均匀分布的 int 值。
int nextInt(int n)
//返回一个伪随机数，它是取自此随机数生成器序列的、在（包括和指定值（不包括）之间均匀分布的int值。
long nextLong()
//返回下一个伪随机数，它是取自此随机数生成器序列的均匀分布的 long 值。
void setSeed(long seed)
//使用单个 long 种子设置此随机数生成器的种子。
```
```java
// 例子
// 生成[0,1.0)区间的小数
double d1 = r.nextDouble();
// 生成[0,5.0)区间的小数：
double d2 = r.nextDouble() * 5;
// 生成[1,2.5)区间的小数：
double d3 = r.nextDouble() * 1.5 + 1;
// 生成-231到231-1之间的整数：
int n = r.nextInt();
// 生成[0,10)区间的整数：
int n2 = r.nextInt(10);//方法一
n2 = Math.abs(r.nextInt() % 10);//方法二
```
[参考链接](https://www.cnblogs.com/ningvsban/p/3590722.html)
# ArrayList
ArrayList是一个其容量能够动态增长的动态数组。它继承了AbstractList，实现了List、RandomAccess, Cloneable, java.io.Serializable。 
## 构造函数
```java
// 默认构造函数
ArrayList()

// capacity是ArrayList的默认容量大小。
// 当由于增加数据导致容量不足时，容量会添加上一次容量大小的一半。
ArrayList(int capacity)

// 创建一个包含collection的ArrayList
ArrayList(Collection<? extends E> collection)
```
## API
```java
// Collection中定义的API
boolean             add(E object)
boolean             addAll(Collection<? extends E> collection)
void                clear()
boolean             contains(Object object)
boolean             containsAll(Collection<?> collection)
boolean             equals(Object object)
int                 hashCode()
boolean             isEmpty()
Iterator<E>         iterator()
boolean             remove(Object object)
boolean             removeAll(Collection<?> collection)
boolean             retainAll(Collection<?> collection)
int                 size()
<T> T[]             toArray(T[] array)
Object[]            toArray()
// AbstractCollection中定义的API
void                add(int location, E object)
boolean             addAll(int location, Collection<? extends E> collection)
E                   get(int location)
int                 indexOf(Object object)
int                 lastIndexOf(Object object)
ListIterator<E>     listIterator(int location)
ListIterator<E>     listIterator()
E                   remove(int location)
E                   set(int location, E object)
List<E>             subList(int start, int end)
// ArrayList新增的API
Object               clone()
void                 ensureCapacity(int minimumCapacity)
void                 trimToSize()
void                 removeRange(int fromIndex, int toIndex)
```
## 遍历方式
```java
// 迭代器遍历
Iterator<Integer> it = arrayList.iterator();
while(it.hasNext()){
    System.out.print(it.next() + " ");
}

// 索引值遍历
for(int i = 0; i < arrayList.size(); i++){
   System.out.print(arrayList.get(i) + " ");
}

// for循环遍历
for(Integer number : arrayList){
   System.out.print(number + " ");
}

```
>需要说明的是，遍历ArrayList时，通过索引值遍历效率最高，for循环遍历次之，迭代器遍历最低。

```java
// 实例

// 创建ArrayList
ArrayList list = new ArrayList();

// 添加
list.add("1");
list.add("2");
list.add("3");
list.add("4");
// 将下面的元素添加到第1个位置
list.add(0, "5");

// 获取第1个元素
System.out.println("the first element is: "+ list.get(0));
// 删除“3”
list.remove("3");
// 获取ArrayList的大小
System.out.println("Arraylist size=: "+ list.size());
// 判断list中是否包含"3"
System.out.println("ArrayList contains 3 is: "+ list.contains(3));
// 设置第2个元素为10
list.set(1, "10");

// 通过Iterator遍历ArrayList
for(Iterator iter = list.iterator(); iter.hasNext(); ) {
    System.out.println("next is: "+ iter.next());
}

// 将ArrayList转换为数组
String[] arr = (String[])list.toArray(new String[0]);
for (String str:arr)
    System.out.println("str: "+ str);

// 清空ArrayList
list.clear();
// 判断ArrayList是否为空
System.out.println("ArrayList is empty: "+ list.isEmpty());
```
[参考链接](https://www.cnblogs.com/skywang12345/p/3308556.html)



# String
```java
// 创建
String greeting = "Hello World";

// 查找
// 首次出现位置
int size = greeting.indexOf("o");
// 末次出现位置
int size = greeting.lastIndexOf("o");

//获取字符串
// 获取指定索引位置的字符
char mychar =  greeting.charAt(5);
// 获取子字符串
String substr = greeting.substring(3); //获取3至最后
String substr = greeting.substring(0,3);//获取0至3
// 去除首尾空格trim()
String substr = greeting.trim()

// 字符串替换
String newstr = greeting.replace("a", "A");

// 判断字符串的开始与结尾，返回boolean
startsWith(String prefix) 
endsWith(String suffix) 
// 判断字符串是否相等
equals(String otherstr)//区分大小写
equalsIgnoreCase(String otherstr)//不区分

// 按字典顺序比较两个字符串
s1.compareTo(String s2);//s1的unicode大于s2返回正整数，小于返回负数

// 所有字母大小写转换
str.toLowerCase();
str.toUpperCase();

// 字符串分割
// 按指定的分隔字符或字符串对内容进行分割，并将分割后的结果存放在字符数组中
str.split(String sign);
str.split(String sign, in limit); //指定多个分割符
// “,|=”表示分割符分别为“，”和“=”，limit限定拆分的次数

```
[参考链接](https://www.cnblogs.com/freeabyss/archive/2013/05/15/3187057.html)
## Java 数组
存储一个固定大小的相同类型元素的顺序集合
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

// 计算所有元素的总和
double total = 0;
for (int i = 0; i < myList.length; i++)
{
    total += myList[i];
}

// 查找最大元素
double max = myList[0];
for (int i = 1; i < myList.length; i++)
{
    if (myList[i] > max) max = myList[i];
}

// 使用 foreach 循环来遍历数组
for ( double element: myList )
{
    System.out.println(element);
}

// 作为函数的参数
// 作为函数的返回值
public static int[] reverse(int[] list) {
    int[] result = new int[list.length];
    for (int i = 0, j = result.length - 1; i < list.length; i++, j--)
    {
        result[j] = list[i];
    }
    return result;
}

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
```
类方法

方法|说明
--|--
public static int binarySearch(Object[] a, Object key)|用二分查找算法在给定数组中搜索给定值的对象(Byte,Int,double等)。数组在调用前必须排序好的。如果查找值包含在数组中，则返回搜索键的索引；否则返回 ( -(插入点) - 1)
public static boolean equals(long[] a, long[] a2)|如果两个指定的 long 型数组彼此相等，则返回 true。如果两个数组包含相同数量的元素，并且两个数组中的所有相应元素对都是相等的，则认为这两个数组是相等的。换句话说，如果两个数组以相同顺序包含相同的元素，则两个数组是相等的。同样的方法适用于所有的其他基本数据类型 ( Byte，short，Int等 )
public static void fill(int[] a, int val)|将指定的 int 值分配给指定 int 型数组指定范围中的每个元素。同样的方法适用于所有的其他基本数据类型 ( Byte，short，Int 等 )
public static void sort(Object[] a)|对指定对象数组根据其元素的自然顺序进行升序排列。同样的方法适用于所有的其他基本数据类型 ( Byte，short，Int等 )
## Number 类
- 所有的包装类 Integer、Long、Byte、Double、Float、Short 都是抽象类 Number 的子类
- 当内置数据类型被当作对象使用的时候，编译器会把内置类型装箱为包装类
类似的，编译器也可以把一个对象拆箱为内置类型
```java
//当 x 被赋为整型值时，由于 x 是一个对象，所以编译器要对 x 进行装箱
Integer x = 5;
// 为了使 x 能进行加运算，所以要对 x 进行拆箱
x =  x + 10;
```
类方法

方法|描述
--|--
xxxValue()|将 Number 对象转换为xxx数据类型的值并返回 Number.intValue()
compareTo()|将 number 对象与参数比较，指定的数大于参数返回 1
equals()|判断 number 对象是否与参数相等，方法的参数类型与数值都相等返回 True
valueOf()|返回一个 Number 对象指定的内置数据类型
toString()|以字符串形式返回值
parseInt()|将字符串解析为 int 类型

## Math
```java
abs()
// 返回参数的绝对值。
ceil()
// 返回大于等于( >= )给定参数的的最小整数，类型为双精度浮点型。向上取整
floor()
// 返回小于等于（<=）给定参数的最大整数 。向下取整
rint()
// 返回与参数最接近的整数。返回类型为double。
round()
// 它表示四舍五入，算法为 Math.floor(x+0.5)，即将原来的数字加上 0.5 后再向下取整，所以，Math.round(11.5) 的结果为12，Math.round(-11.5) 的结果为-11。
min()
// 返回两个参数中的最小值。
max()
// 返回两个参数中的最大值。
exp()
// 返回自然数底数e的参数次方。
log()
// 返回参数的自然数底数的对数值。
pow()
// 返回第一个参数的第二个参数次方。
sqrt()
// 求参数的算术平方根。
sin()
// 求指定double类型参数的正弦值。
cos()
// 求指定double类型参数的余弦值。
tan()
// 求指定double类型参数的正切值。
asin()
// 求指定double类型参数的反正弦值。
acos()
// 求指定double类型参数的反余弦值。
atan()
// 求指定double类型参数的反正切值。
tan2()
// 将笛卡尔坐标转换为极坐标，并返回极坐标的角度值。
toDegrees()
// 将参数转化为角度。
toRadians()
// 将角度转换为弧度。
random()
// 返回一个随机数。
```

## 队列（Queue）

队列是一种特殊的线性表，它只允许在表的前端进行删除操作，而在表的后端进行插入操作。

LinkedList类实现了Queue接口，因此我们可以把LinkedList当成Queue来用。

```java
//声明和初始化
//add()和remove()方法在失败的时候会抛出异常(不推荐)
Queue<String> queue = new LinkedList<String>();

```
```
offer，add 区别：
一些队列有大小限制，因此如果想在一个满的队列中加入一个新项，多出的项就会被拒绝。
这时新的 offer 方法就可以起作用了。它不是对调用 add() 方法抛出一个 unchecked 异常，而只是得到由 offer() 返回的 false。

poll，remove 区别：
remove() 和 poll() 方法都是从队列中删除第一个元素。remove() 的行为与 Collection 接口的版本相似， 
但是新的 poll() 方法在用空集合调用时不是抛出异常，只是返回 null。因此新的方法更适合容易出现异常条件的情况。

peek，element区别：
element() 和 peek() 用于在队列的头部查询元素。与 remove() 方法类似，在队列为空时， element() 抛出一个异常，而 peek() 返回 null。
```



















