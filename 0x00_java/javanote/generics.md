## 泛型generics 
泛型提供了编译时类型安全检测机制，该机制允许程序员在编译时检测到非法的类型。

### 泛型方法
泛型方法是在调用时可以接收不同类型的参数，根据传递给泛型方法的参数类型，编译器适当地处理每一个方法调用

- 所有泛型方法声明都有一个类型参数声明部分 (由尖括号分隔)，该类型参数声明部分在方法返回类型之前 ( <E> )
- 每一个类型参数声明部分包含一个或多个类型参数，参数间用逗号隔开
一个泛型参数，也被称为一个类型变量，是用于指定一个泛型类型名称的标识符
- 类型参数能被用来声明返回值类型，并且能作为泛型方法得到的实际参数类型的占位符
- 泛型方法体的声明和其它方法一样
注意类型参数只能代表引用型类型，不能是原始类型 ( 像 int,double,char 等)

```java
public class GenericMethodTest
{
   // 泛型方法 printArray                         
   public static < E > void printArray( E[] inputArray )
   {
      // 输出数组元素            
         for ( E element : inputArray ){        
            System.out.printf( "%s ", element );
         }
         System.out.println();
    }

    public static void main( String args[] )
    {
        // 创建不同类型数组： Integer, Double 和 Character
        Integer[] intArray = { 1, 2, 3, 4, 5 };
        Double[] doubleArray = { 1.1, 2.2, 3.3, 4.4 };
        Character[] charArray = { 'H', 'E', 'L', 'L', 'O' };

        System.out.println( "整型数组元素为:" );
        printArray( intArray  ); // 传递一个整型数组

        System.out.println( "\n双精度型数组元素为:" );
        printArray( doubleArray ); // 传递一个双精度型数组

        System.out.println( "\n字符型数组元素为:" );
        printArray( charArray ); // 传递一个字符型数组
    } 
}
```

### 有界的类型参数
用于限制传递到一个类型参数的类型种类范围
要声明一个有界的类型参数，首先列出类型参数的名称，后跟 extends 关键字，最后紧跟它的上界
```java
public class MaximumTest
{
   // 比较三个值并返回最大值
   public static <T extends Comparable<T>> T maximum(T x, T y, T z)
   {                     
      T max = x; // 假设x是初始最大值
      if ( y.compareTo( max ) > 0 ){
         max = y; //y 更大
      }
      if ( z.compareTo( max ) > 0 ){
         max = z; // 现在 z 更大           
      }
      return max; // 返回最大对象
   }
   public static void main( String args[] )
   {
      System.out.printf( "%d, %d 和 %d 中最大的数为 %d\n\n",
                   3, 4, 5, maximum( 3, 4, 5 ) );

      System.out.printf( "%.1f, %.1f 和 %.1f 中最大的数为 %.1f\n\n",
                   6.6, 8.8, 7.7, maximum( 6.6, 8.8, 7.7 ) );

      System.out.printf( "%s, %s 和 %s 中最大的数为 %s\n","pear",
         "apple", "orange", maximum( "pear", "apple", "orange" ) );
   }
}
```
### 泛型类
泛型类的声明和非泛型类的声明类似，除了在类名后面添加了类型参数声明部分
和泛型方法一样，泛型类的类型参数声明部分也包含一个或多个类型参数，参数间用逗号隔开

```java
public class Box<T> {

  private T t;

  public void add(T t) {
    this.t = t;
  }

  public T get() {
    return t;
  }

  public static void main(String[] args) {
    Box<Integer> integerBox = new Box<Integer>();
    Box<String> stringBox = new Box<String>();

    integerBox.add(new Integer(10));
    stringBox.add(new String("简单编程"));

    System.out.printf("整型值为 :%d\n\n", integerBox.get());
    System.out.printf("字符串为 :%s\n", stringBox.get());
  }
}
```
### 类型通配符
类型通配符一般是使用 ? 代替具体的类型参数
例如 List<?> 在逻辑上是 List<String>,List<Integer> 等所有 List<具体类型实参> 的父类
```java
public class GenericTest {

    public static void main(String[] args) {
        List<String> name = new ArrayList<String>();
        List<Integer> age = new ArrayList<Integer>();
        List<Number> number = new ArrayList<Number>();

        name.add("icon");
        age.add(18);
        number.add(314);

        getData(name);
        getData(age);
        getData(number);

   }

   public static void getData(List<?> data) {
      System.out.println("data :" + data.get(0));
   }
}
// getDate() 方法的参数是 <List> 类型的，
// 所以 name，age，number 都可以作为这个方法的实参，这就是通配符的作用
```
### 类型通配符上限
类型通配符上限通过形如 List extends <Number>来定义
如此定义就是通配符泛型值接受Number及其下层子类类型

```java
public class GenericTest {

    public static void main(String[] args) {
        List<String> name = new ArrayList<String>();
        List<Integer> age = new ArrayList<Integer>();
        List<Number> number = new ArrayList<Number>();

        name.add("icon");
        age.add(18);
        number.add(314);

        //getUperNumber(name);
        //1会报错，getUperNumber方法中的参数已经限定了参数泛型上限为 
        // Number，String不在这个范围之内
        getUperNumber(age);//2
        getUperNumber(number);//3

   }

   public static void getData(List<?> data) {
      System.out.println("data :" + data.get(0));
   }

   public static void getUperNumber(List<? extends Number> data) {
          System.out.println("data :" + data.get(0));
       }
}
```
### 类型通配符下限
类型通配符下限通过形如 List<? super Number> 来定义
表示类型只能接受 Number 及其三层父类类型，如 Objec 类型的实例






