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
