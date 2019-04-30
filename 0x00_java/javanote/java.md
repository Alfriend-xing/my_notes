# java


## 编译和运行
```java
// 创建文件HelloWorld.java(文件名需与类名一致)
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello World");
    }
}

// javac 后面跟着的是java文件的文件名，该命令用于将 java 源文件编译为 class 字节码文件
$ javac HelloWorld.java

// java 后面跟着的是java文件中的类名,例如 HelloWorld 就是类名
$ java HelloWorld
```

## 基础

### 语法
- 大小写敏感：Java 是大小写敏感的，这就意味着标识符 Hello 与 hello 是不同的。
- 类名：对于所有的类来说，类名的首字母应该大写。如果类名由若干单词组成，那么每个单词的首字母应该大写，例如 MyFirstJavaClass 。
- 方法名：所有的方法名都应该以小写字母开头。如果方法名含有若干单词，则后面的每个单词首字母大写。
- 源文件名：源文件名必须和类名相同。当保存文件的时候，你应该使用类名作为文件名保存（切记 Java 是大小写敏感的），文件名的后缀为 .java。（如果文件名和类名不相同则会导致编译错误）。
- 主方法入口：所有的 Java 程序由 public static void main(String []args) 方法开始执行。

### 标识符
类名、变量名以及方法名都被称为标识符
- 所有的标识符都应该以字母 ,美元符 、或者下划线开始
- 首字符之后可以是字母 ,美元符 、下划线、数字的任何字符组合
### 变量
- 局部变量
- 类变量（静态变量）
- 成员变量（非静态变量）
```java
public class Variable{
    static int allClicks=0;    // 类变量

    String str="hello world";  // 实例变量

    public void method(){

        int i =0;  // 局部变量

    }
}
```
1. 局部变量
    - 局部变量声明在方法、构造方法或者语句块中
    - 局部变量在方法、构造方法、或者语句块被执行的时候创建，当它们执行完成后，变量将会被销毁
    - 访问修饰符不能用于局部变量
    - 局部变量只在声明它的方法、构造方法或者语句块中可见
    - 局部变量是在栈上分配的
    - 局部变量没有默认值，所以局部变量被声明后，必须经过初始化，才可以使用
1. 实例变量
    - 实例变量声明在一个类中，但在方法、构造方法和语句块之外
    - 当一个对象被实例化之后，每个实例变量的值就跟着确定
    - 实例变量在对象创建的时候创建，在对象被销毁的时候销毁
    - 实例变量的值应该至少被一个方法、构造方法或者语句块引用， 使得外部能够通过这些方式获取实例变量信息
    - 实例变量可以声明在使用前或者使用后
    - 访问修饰符可以修饰实例变量
    - 实例变量对于类中的方法、构造方法或者语句块是可见的。一般情况下应该把实例变量设为私有。通过使用访问修饰符可以使实例变量对子类可见
    - 实例变量具有默认值。数值型变量的默认值是 0，布尔型变量的默认值是 false，引用类型变量的默认值是 null。变量的值可以在声明时指定，也可以在构造方法中指定
    - 实例变量可以直接通过变量名访问。但在静态方法以及其他类中，就应该使用完全限定名：ObejectReference.VariableName
1. 类变量 ( 静态变量 )
    - 类变量也称为静态变量，在类中以 static 关键字声明，但必须在方法构造方法和语句块之外
    - 无论一个类创建了多少个对象，类只拥有类变量的一份拷贝
    - 静态变量除了被声明为常量外很少使用。常量是指声明为 public/private，final和static类型的变量。常量初始化后不可改变
    - 静态变量储存在静态存储区。经常被声明为常量，很少单独使用 static 声明变量
    - 静态变量在程序开始时创建，在程序结束时销毁
    - 与实例变量具有相似的可见性。但为了对类的使用者可见，大多数静态变量声明为 public 类型
    - 默认值和实例变量相似。数值型变量默认值是0，布尔型默认值是false，引用类型默认值是null。变量的值可以在声明的时候指定，也可以在构造方法中指定。此外，静态变量还可以在静态语句块中初始化
    - 静态变量可以通过： ClassName.VariableName 的方式访问
    - 类变量被声明为 public static final 类型时，类变量名称一般建议使用大写字母。如果静态变量不是 public 和 final 类型，其命名方式与实例变量以及局部变量的命名方式一致
### 数组
可以保存多个同类型变量

### 枚举
限制变量只能是预先设定好的值
- 枚举可以单独声明或者声明在类里面
- 方法、变量、构造函数也可以在枚举中定义
```java
class FreshJuice {
   enum FreshJuiceSize{ SMALL, MEDIUM , LARGE }
   FreshJuiceSize size;
}

public class FreshJuiceTest {
   public static void main(String []args){
      FreshJuice juice = new FreshJuice();
      juice.size = FreshJuice.FreshJuiceSize.MEDIUM  ;
   }
}
```
### 关键字
保留字不能用于常量、变量、和任何标识符的名称
关键字|描述
--|--
abstract|抽象方法，抽象类的修饰符
assert|断言条件是否满足
boolean|布尔数据类型
break|跳出循环或者label代码段
byte|8-bit 有符号数据类型
case|switch语句的一个条件
catch|和try搭配捕捉异常信息
char|16-bit Unicode字符数据类型
class|定义类
const|未使用
continue|不执行循环体剩余部分
default|switch语句中的默认分支
do|循环语句，循环体至少会执行一次
double|64-bit双精度浮点数
else|if条件不成立时执行的分支
enum|枚举类型
extends|表示一个类是另一个类的子类
final|表示一个值在初始化之后就不能再改变了表示方法不能被重写，或者一个类不能有子类
finally|为了完成执行的代码而设计的，主要是为了程序的健壮性和完整性，无论有没有异常发生都执行代码。
float|32-bit单精度浮点数
for|for循环语句
goto|未使用
if|条件语句
implements|表示一个类实现了接口
import|导入类
instanceof|测试一个对象是否是某个类的实例
int|32位整型数
interface|接口，一种抽象的类型，仅有方法和常量的定义
long|64位整型数
native|表示方法用非java代码实现
new|分配新的类实例
package|一系列相关类组成一个包
private|表示私有字段，或者方法等，只能从类内部访问
protected|表示字段只能通过类或者其子类访问子类或者在同一个包内的其他类
public|表示共有属性或者方法
return|方法返回值
short|16位数字
static|表示在类级别定义，所有实例共享的
strictfp|浮点数比较使用严格的规则
super|表示基类
switch|选择语句
synchronized|表示同一时间只能由一个线程访问的代码块
this|表示调用当前实例或者调用另一个构造函数
throw|抛出异常
throws|定义方法可能抛出的异常
transient|修饰不要序列化的字段
try|表示代码块要做异常处理或者和finally配合表示是否抛出异常都执行finally中的代码
void|标记方法不返回任何值
volatile|标记字段可能会被多个线程同时访问，而不做同步
while|while循环
### 修饰符
使用修饰符来修饰类中方法和属性
- 访问控制修饰符 : default, public , protected, private
- 非访问控制修饰符 : final, abstract, static, synchronized
### 注释
```java
/* 多
行
注
释*/

// 单行注释
```
## 对象和类
- 对象具有属性和方法，方法操作对象内部状态的改变，对象的相互调用也是通过方法来完成
- 类可以看成是创建 Java 对象的模板
### 构造方法
- 每个类都有构造方法
- 如果没有显式地为类定义构造方法，Java 编译器将会为该类提供一个默认构造方法
- 构造方法的名称必须与类同名，一个类可以有多个构造方法
```java
public class Puppy {

    public Puppy(){
    }

    public Puppy(String name){
        // 这个构造器仅有一个参数：name
    }
}
```
### 创建对象
创建对象需要以下三步
- 声明 ：声明一个对象，包括对象名称和对象类型
- 实例化 ：使用关键字new来创建一个对象
- 初始化 ：使用new创建对象时，会调用构造方法初始化对象
```java
// 下面的语句将创建一个Puppy对象
Puppy myPuppy = new Puppy( "tommy" );
```
### 访问对象的变量和方法
```java
/* 实例化对象 */
ObjectReference = new Constructor();
/* 访问类中的变量 */
ObjectReference.variableName;
/* 访问类中的方法 */
ObjectReference.MethodName();
```
## 基本数据类型
Java 语言提供了八种基本类型：
- 六种数字类型 ( 四个整数型，两个浮点型 )
- 一种字符类型
- 一种布尔型
```java
byte a = 100;
```
- byte 数据类型是 8 位、有符号的，以二进制补码表示的整数
- 最小值是 -128 ( -2^7 )
- 最大值是 127 ( 2^7-1 )
- 默认值是 0
- byte 类型用在大型数组中节约空间，主要代替整数，因为 byte 变量占用的空间只有 int 类型的四分之一
```java
short s = 1000;
```
- short 数据类型是 16 位、有符号的以二进制补码表示的整数
- 最小值是 -32768 ( -2^15 )
- 最大值是 32767 ( 2^15 - 1 )
- short 数据类型也可以像 byte 那样节省空间，一个 short 变量是 int 型变量所占空间的二分之一
- 默认值是 0
```java
int a = 100000;
int d = 3, e = 4, f = 5;// 声明三个整数并赋予初值
```
- int 数据类型是 32 位、有符号的以二进制补码表示的整数
- 最小值是 -2,147,483,648 ( -2^31 )
- 最大值是 2,147,483,647 ( 2^31 - 1 )
- 一般地整型变量默认为 int 类型
- 默认值是 0
```java
long a = 100000L;
```
- long 数据类型是 64 位、有符号的以二进制补码表示的整数
- 最小值是 -9,223,372,036,854,775,808 ( -2^63 )
- 最大值是 9,223,372,036,854,775,807 ( 2^63 -1 )
- long 类型主要使用在需要比较大整数的系统上
- 默认值是 0L
- "L" 理论上不分大小写，但是若写成"l"容易与数字"1"混淆，不容易分辩，所以最好大写
```java
float f1 = 234.5f;
```
- float 数据类型是单精度、32 位、符合 IEEE 754 标准的浮点数
- float 在储存大型浮点数组的时候可节省内存空间
- 默认值是 0.0f
- 浮点数不能用来表示精确的值，如货币
```java
double d1 = 123.4d;
```
- double 数据类型是双精度、64 位、符合 IEEE 754 标准的浮点数
- 浮点数的默认类型为 double 类型
- double 类型同样不能表示精确的值，如货币
- 默认值是 0.0d
```java
boolean one = true;
```
- boolean 数据类型表示一位的信息
- 只有两个取值：true 和 false
- 这种类型只作为一种标志来记录 true/false 情况
- 默认值是 false
```java
char letter = 'A';
```
- char 类型是一个单一的 16 位 Unicode 字符
- 最小值是 \u0000 ( 即为0 )
- 最大值是 \uffff ( 即为 65,535 )
- char 数据类型可以储存任何字符
```java
// 以上数据类型有属性表示其存储空间和最值
Double.SIZE
Double.MIN_VALUE
Double.MAX_VALUE
```
### 引用类型
引用类型指向一个对象，指向对象的变量是引用变量，变量一旦声明后，类型就不能被改变了
```java
Site site = new Site("Twle");
```
### 常量
- 常量在程序运行时，不会被修改
- 使用 final 关键字来修饰常量，声明方式和变量类似
- 为了便于识别，通常使用大写字母表示常量
```java
final double PI = 3.1415927;
```
byte、int、long、和 short 都可以用十进制、16 进制以及 8 进制的方式来表示
```java
// 前缀 0 表示 8 进制，而前缀 0x 代表 16 进制
int decimal = 100;
int octal = 0144;
int hexa =  0x64;
```
### 类型转换
- 不能对boolean类型进行类型转换
- 在把容量大的类型转换为容量小的类型时必须使用强制类型转换
- 转换过程中可能导致溢出或损失精度
```java
// 强制类型转换
int i =128;   //int(32位)
byte b = (byte)i;//byte(8位)

// 自动类型转换 小容量转大容量类型
char c1='a';//char(16位)
int i1 = c1;//char自动类型转换为int(32位)
```
## 访问控制符
使用 访问控制符 来保护对类、变量、方法和构造方法的访问
1. default ( 即缺省，什么也不写 )
    - 在同一包内可见，不使用任何控制符
    - 使用对象：类、接口、变量、方法

1. private
    - 在同一类内可见
    - 使用对象：变量、方法
    - 注意：不能修饰类 ( 外部类 )

1. public
    - 对所有类可见
    - 使用对象：类、接口、变量、方法

1. protected
    - 对同一包内的类和所有子类可见
    - 使用对象：变量、方法

访问控制和继承
- 父类中声明为 public 的方法在子类中也必须为 public
- 父类中声明为 protected 的方法在子类中要么声明为 protected，要么声明为     public，不能声明为 private
- 父类中声明为 private 的方法，不能够被继承
## 非访问修饰符
1. static 修饰符，用来修饰类方法和类变量

1. final 修饰符，用来修饰类、方法和变量
    - final 修饰的类不能够被继承，修饰的方法不能被继承类重新定义，修饰的变量为常量，是不可修改的
1. abstract 修饰符，用来创建抽象类和抽象方法
1. synchronized 和 volatile 修饰符，主要用于多线程的编程
### static 修饰符
静态变量
- static 关键字用于声明独立于对象的静态变量
- 无论一个类实例化多少对象，它的静态变量只有一份拷贝
- 静态变量也被称为类变量
- 局部变量不能被声明为 static 变量

静态方法
- static 关键字可以用于声明独立于对象的静态方法
- 静态方法不能使用类的非静态变量
- 静态方法从参数列表得到数据，然后计算这些数据

类的静态变量和静态方法可以直接使用 classname.variablename 和 classname.methodname 的方式访问

### final 修饰符
final 变量
- final 变量能被显式地初始化并且只能初始化一次
- 被声明为 final 的对象的引用不能指向不同的对象
- 但是 final 对象里的数据可以被改变
- 也就是说 final 对象的引用不能改变，但是里面的值可以改变
- final 修饰符通常和 static 修饰符一起使用来创建类常量
>对于一个final变量，如果是基本数据类型的变量，则其数值一旦在初始化之后便不能更改；如果是引用类型的变量(`final StringBuilder sb = new StringBuilder("haha");`)，则在对其初始化之后便不能再让其指向另一个对象，但是里面的值可以改变。

final 方法
- 类中的 final 方法可以被子类继承，但是不能被子类修改
- 声明 final 方法的主要目的是防止该方法的内容被修改

final 类
- final 类不能被继承，没有类能够继承 final 类的任何特性

### abstract 修饰符
抽象类
- 抽象类不能用来实例化对象，声明抽象类的唯一目的是为了将来对该类进行扩充
- 一个类不能同时被 abstract 和 final 修饰
- 如果一个类包含抽象方法，那么该类一定要声明为抽象类，否则将出现编译错误
- 抽象类可以包含抽象方法和非抽象方法

抽象方法
- 抽象方法是一种没有任何实现的方法，该方法的的具体实现由子类提供
- 抽象方法不能被声明成 final 和 static
- 任何继承抽象类的子类必须实现父类的所有抽象方法，除非该子类也是抽象类
- 如果一个类包含若干个抽象方法，那么该类必须声明为抽象类
- 抽象类可以不包含抽象方法
- 抽象方法的声明以分号结尾，如如： public abstract sample();

### synchronized 修饰符
- synchronized 关键字声明的方法同一时间只能被一个线程访问
- synchronized 修饰符可以和四个访问控制符一起使用
```java
public synchronized void showDetails(){
    // ....
}
```
### transient 修饰符
- 序列化的对象包含 transient 修饰的实例变量时，java 虚拟机 ( JVM ) 会跳过该特定的变量
- 该修饰符包含在定义变量的语句中，用来预处理类和变量的数据类型
```java
public transient int limit = 55;   // 不会持久化
public int b;                      // 持久化
```
### volatile 修饰符
- volatile 修饰的成员变量在每次被线程访问时，都强制从共享内存中重新读取该成员变量的值
- 而且，当成员变量发生变化时，会强制线程将变化值回写到共享内存
- 这样在任何时刻，两个不同的线程总是看到某个成员变量的同一个值

## 运算符
算术运算符
```java
+、 -、 *、 /、 %、 
++、 -- //前缀++a --a，先自增或者自减运算，再进行表达式运算；后缀相反
```
关系运算符
```java
== 、!=、 >、 <、 >=、 <=
```
位运算符
```java
& 位与 60&13=12 (0000 1100)
| 位或 60&13=61 (0011 1101)
^ 位异或 对应位值不同得1
~|按位补运算符翻转操作数的每一位 ( ~60 ) 得到 -61，即1100 0011
<<|按位左移运算符 60 << 2 得到 240，即 1111 0000
>>|按位右移运算符 左操作数按位右移右操作数指定的位数
>>>|按位右移补零操作符 左操作数的值按右操作数指定的位数右移，移动得到的空位以零填充
```
逻辑运算符
```java
&& //逻辑与
|| //逻辑或
! //逻辑非
```
短路逻辑运算符
>&&当得到第一个操作为 false 时，其结果就必定是 false，这时候就不会再判断第二个操作了

赋值运算符
```java
= += -= *= /= %=
<<= >>= &= ^= |=
```
条件运算符
```java
variable x = (expression) ? value if true : value if false
```
instanceof 运算符
>用于检查某个对象是否是一个特定类型 ( 类类型或接口类型 )
```java
( Object reference variable ) instanceof  (class/interface type)

String name = "James";
// 由于 name 是 String 类型，所以返回真
boolean result = name instanceof String; 
```
## 条件判断
```java
if( boolean_expression )
{
   /* 如果布尔表达式 boolean_expression 为真将执行的语句 */
}

if(布尔表达式){
   //如果布尔表达式的值为true
}else{
   //如果布尔表达式的值为false
}

if(布尔表达式 1){
   //如果布尔表达式 1的值为true执行代码
}else if(布尔表达式 2){
   //如果布尔表达式 2的值为true执行代码
}else if(布尔表达式 3){
   //如果布尔表达式 3的值为true执行代码
}else {
   //如果以上布尔表达式都不为true执行代码
}
```
```java
switch(expression){
    case value :
       //语句
       break; //可选
    case value :
       //语句
       break; //可选
    //你可以有任意数量的case语句
    default : //可选
       //语句
}
```
- switch 语句中的变量类型可以是： byte、short、int 或者 char从 Java SE 7 开始，switch 支持字符串类型了
- 当变量的值与 case 语句的值相等时，那么 case 语句之后的语句开始执行，直到 break 语句出现才会跳出 switch 语句
- 当遇到 break 语句时，switch 语句终止,如果没有 break 语句出现，程序会继续执行下一条 case 语句，直到出现 break 语句
- switch 语句可以包含一个 default 分支，该分支必须是 switch 语句的最后一个分支
default 在没有 case 语句的值和变量值相等的时候执行
default 分支不需要 break 语句

## 循环语句
```java
while( 布尔表达式 ) {
  // 循环体
}

do {
    //代码语句
}while(布尔表达式);

for(初始化; 布尔表达式; 更新) {
    // 代码语句
}
for(int x = 10; x < 20; x = x+1) {
         System.out.print("value of x : " + x );
         System.out.print("\n");
      }

// 主要用于数组的增强型 for 循环
for(声明语句 : 表达式)
{
   // 代码句子
}
int [] numbers = {10, 20, 30, 40, 50};
for(int x : numbers ){
    System.out.print( x );
    System.out.print(",");
}
```
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
xxxValue()|将 Number 对象转换为xxx数据类型的值并返回
compareTo()|将 number 对象与参数比较
equals()|判断 number 对象是否与参数相等
valueOf()|返回一个 Number 对象指定的内置数据类型
toString()|以字符串形式返回值
parseInt()|将字符串解析为 int 类型

## Character 类
Character 是内置数据类型 char 的一个包装类 
```java
// 使用 Character 的构造方法创建一个 Character 类对象
Character ch = new Character('a');
// 原始字符 'a' 装箱到 Character 对象 ch 中
Character ch = 'a';
// 原始字符 'x' 用 test 方法装箱
// 返回拆箱的值到 'c'
char c = test('x');
```
类方法
方法|描述
--|--
isLetter()|是否是一个字母
isDigit()|是否是一个数字字符
isWhitespace()|是否是一个空格
isUpperCase()|是否是大写字母
isLowerCase()|是否是小写字母
toUpperCase()|指定字母的大写形式
toLowerCase()|指定字母的小写形式
toString()|返回字符的字符串形式，字符串的长度仅为1

## String 类
用于存储或操作一个或多个字节的序列
一旦创建了 String 对象，那它的值就无法改变了
```java
String greeting = "Hello World";

char[] helloArray = { 't', 'w', 'l', 'e'};
String helloString = new String(helloArray); 

// 连接字符串
string1.concat( string2 );
string1 + string2

// 格式化字符串
System.out.printf("浮点型变量的值为 " +
                  "%f, 整型变量的值为 " +
                  " %d, 字符串变量的值为 " +
                  "is %s", floatVar, intVar, stringVar);
String fs;
fs = String.format("浮点型变量的值为 " +
                   "%f, 整型变量的值为 " +
                   " %d, 字符串变量的值为 " +
                   " %s", floatVar, intVar, stringVar);
// String 类的静态方法 format() 返回一个 String 对象
```
类方法
方法|描述
--|--
char charAt(int index)|返回指定索引处的 char 值
int compareTo(Object o)|把这个字符串和另一个对象比较
int compareTo(String anotherString)|按字典顺序比较两个字符串
int compareToIgnoreCase(String str)|按字典顺序比较两个字符串，不考虑大小写
String concat(String str)|将指定字符串连接到此字符串的结尾
boolean contentEquals(StringBuffer sb)|当且仅当字符串与指定的StringButter有相同顺序的字符时候返回真
static String copyValueOf(char[] data)|返回指定数组中表示该字符序列的 String
static String copyValueOf(char[] data, int offset, int count)|返回指定数组中表示该字符序列的 String
boolean endsWith(String suffix)|测试此字符串是否以指定的后缀结束
boolean equals(Object anObject)|将此字符串与指定的对象比较
boolean equalsIgnoreCase(String anotherString)|将此 String 与另一个 String 比较，不考虑大小写
byte[] getBytes()|使用平台的默认字符集将此 String 编码为 byte 序列，并将结果存储到一个新的 byte 数组中
byte[] getBytes(String charsetName)|使用指定的字符集将此 String 编码为 byte 序列，并将结果存储到一个新的 byte 数组中
void getChars(int srcBegin, int srcEnd, char[] dst, int dstBegin)|将字符从此字符串复制到目标字符数组
int hashCode()|返回此字符串的哈希码
int indexOf(int ch)|返回指定字符在此字符串中第一次出现处的索引
int indexOf(int ch, int fromIndex)|返回在此字符串中第一次出现指定字符处的索引，从指定的索引开始搜索
int indexOf(String str)|返回指定子字符串在此字符串中第一次出现处的索引
int indexOf(String str, int fromIndex)|返回指定子字符串在此字符串中第一次出现处的索引，从指定的索引开始
String intern()|返回字符串对象的规范化表示形式
int lastIndexOf(int ch)|返回指定字符在此字符串中最后一次出现处的索引
int lastIndexOf(int ch, int fromIndex)|返回指定字符在此字符串中最后一次出现处的索引，从指定的索引处开始进行反向搜索
int lastIndexOf(String str)|返回指定子字符串在此字符串中最右边出现处的索引
int lastIndexOf(String str, int fromIndex)|返回指定子字符串在此字符串中最后一次出现处的索引，从指定的索引开始反向搜索
int length()|返回此字符串的长度
boolean matches(String regex)|告知此字符串是否匹配给定的正则表达式
boolean regionMatches(boolean ignoreCase, int toffset, String other, int ooffset, int len)|测试两个字符串区域是否相等
boolean regionMatches(int toffset, String other, int ooffset, int len)|测试两个字符串区域是否相等
String replace(char oldChar, char newChar)|返回一个新的字符串，它是通过用 newChar 替换此字符串中出现的所有 oldChar 得到的
String replaceAll(String regex, String replacement|使用给定的 replacement 替换此字符串所有匹配给定的正则表达式的子字符串
String replaceFirst(String regex, String replacement)|使用给定的 replacement 替换此字符串匹配给定的正则表达式的第一个子字符串
String[] split(String regex)|根据给定正则表达式的匹配拆分此字符串
String[] split(String regex, int limit)|根据匹配给定的正则表达式来拆分此字符串
boolean startsWith(String prefix)|测试此字符串是否以指定的前缀开始
boolean startsWith(String prefix, int toffset)|测试此字符串从指定索引开始的子字符串是否以指定前缀开始
CharSequence subSequence(int beginIndex, int endIndex)|返回一个新的字符序列，它是此序列的一个子序列
String substring(int beginIndex)|返回一个新的字符串，它是此字符串的一个子字符串
String substring(int beginIndex, int endIndex)|返回一个新字符串，它是此字符串的一个子字符串
char[] toCharArray()|将此字符串转换为一个新的字符数组
String toLowerCase()|使用默认语言环境的规则将此 String 中的所有字符都转换为小写
String toLowerCase(Locale locale)|使用给定 Locale 的规则将此 String 中的所有字符都转换为小写
String toString()|返回此对象本身 ( 它已经是一个字符串 )
String toUpperCase()|使用默认语言环境的规则将此 String 中的所有字符都转换为大写
String toUpperCase(Locale locale)|使用给定 Locale 的规则将此 String 中的所有字符都转换为大写
String trim()|返回字符串的副本，忽略前导空白和尾部空白
static String valueOf(primitive data type x)|返回给定data type类型x参数的字符串表示形式

## Java StringBuffer 和 StringBuilder 类
- 和 String 类不同的是，StringBuffer 和 StringBuilder 类的对象能够被多次的修改，并且不产生新的未使用对象
- StringBuilder 和 StringBuffer 之间的最大不同在于 StringBuilder 的方法不是线程安全的 ( 不能同步访问 )
- 由于 StringBuilder 相较于 StringBuffer 有速度优势，所以多数情况下建议使用 StringBuilder 类
- 然而在应用程序要求线程安全的情况下，则必须使用 StringBuffer 类
```java
StringBuffer sBuffer = new StringBuffer("简单编程官网：");
sBuffer.append("www");
sBuffer.append(".twle");
sBuffer.append(".com");
System.out.println(sBuffer); 
```
StringBuffer 方法
方法|描述
--|--
public StringBuffer append(String s)|将指定的字符串追加到此字符序列
public StringBuffer reverse()|将此字符序列用其反转形式取代
public delete(int start, int end)|移除此序列的子字符串中的字符
public insert(int offset, int i)|将int参数的字符串表示形式插入此序列中
replace(int start, int end, String str)|使用给定String中的字符替换此序列的子字符串中的字符
其余方法则和 String 类类似

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
### 格式化日期
java.util.SimpleDateFormat 类是一个以语言环境敏感的方式来格式化和分析日期的类
```java
Date dNow = new Date( );
SimpleDateFormat ft = new SimpleDateFormat ("E yyyy.MM.dd 'at' hh:mm:ss a zzz");
System.out.println(ft.format(dNow));
// 输出
// Tue 2018.01.23 at 07:48:53 AM CST
```
格式化符
字母|描述|范例
--|--|--
G|纪元标记|AD
y|四位年份|2001
M|月份|July or 07
d|一个月的日期|10
h|A.M./P.M. (1~12)格式小时|12
H|一天中的小时 (0~23)|22
m|分钟数|30
s|秒数|55
S|毫秒数|234
E|星期几|Tuesday
D|一年中的日子|360
F|一个月中第几周的周几|2 (second Wed. in July)
w|一年中第几周|40
W|一个月中第几周|1
a|A.M./P.M. 标记|PM
k|一天中的小时(1~24)|24
K|A.M./P.M. (0~11)格式小时|10
z|时区|Eastern Standard Time
'|文字定界符|Delimiter
"|单引号|`

### 解析字符串为时间
SimpleDateFormat 类的 parse() 方法可以按照指定的 SimpleDateFormat 对象的格式化存储来解析字符串
```java
SimpleDateFormat ft = new SimpleDateFormat ("yyyy-MM-dd"); 
String input = args.length == 0 ? "1818-11-11" : args[0]; 
System.out.print(input + " Parses as "); 
Date t; 
try { 
    t = ft.parse(input); 
    System.out.println(t); 
} catch (ParseException e) { 
    System.out.println("Unparseable using " + ft); 
}
```
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
## 日历
- java.util.Calendar 类可以设置和获取日期数据的特定部分，在日期的这些部分加上或者减去值
- Calendar 类实现了公历日历
- GregorianCalendar 是 Calendar 类的一个具体实现
- Calendar 的 getInstance()方法返回一个默认用当前的语言环境和时区初始化的 GregorianCalendar 对象
```java
Calendar c = Calendar.getInstance();//默认是当前日期

//创建一个代表2009年6月12日的Calendar对象
// 月份从0开始，所以用6 - 1
Calendar c1 = Calendar.getInstance();
c1.set(2009, 6 - 1, 12);
// 只设定某个字段
c1.set(Calendar.DATE,10);

// 增减某个字段
Calendar c1 = Calendar.getInstance();
c1.add(Calendar.DATE, 10);
c1.add(Calendar.DATE, -10);

// 获取某个字段
// 获得年份
int year = c1.get(Calendar.YEAR);
// 获得月份(注意+1)
int month = c1.get(Calendar.MONTH) + 1;
```
常量|描述
--|--
Calendar.YEAR|年份
Calendar.MONTH|月份
Calendar.DATE|日期
Calendar.DAY_OF_MONTH|日期，和上面的字段意义完全相同
Calendar.HOUR|12小时制的小时
Calendar.HOUR_OF_DAY|24小时制的小时
Calendar.MINUTE|分钟
Calendar.SECOND|秒
Calendar.DAY_OF_WEEK|星期几

## 正则表达式
java.util.regex 包提供了操作正则表达式的一些方法
主要包括以下三个类
- Pattern 类
Pattern 类的实例对象是一个正则表达式的编译表示
- Matcher 类
Matcher 对象是对输入字符串进行解释和匹配操作的引擎
- PatternSyntaxException 是一个非强制异常类，它表示一个正则表达式模式中的语法错误
```java
// 匹配
String content = "I am Ys，from twle.cn";
String pattern = ".*twle.*";
boolean isMatch = Pattern.matches(pattern, content);

// 捕获，group(0)总是代表整个表达式
String line = "This order was placed for QT3000! OK?";
String pattern = "(\\D*)(\\d+)(.*)";
// 创建 Pattern 对象
Pattern r = Pattern.compile(pattern);
// 现在创建 matcher 对象
Matcher m = r.matcher(line);
if (m.find( )) {
    System.out.println("Found value: " + m.group(0) );
    System.out.println("Found value: " + m.group(1) );
    System.out.println("Found value: " + m.group(2) );
    System.out.println("Found value: " + m.group(3) ); 
} else {
    System.out.println("NO MATCH");
}
// 输出
// Found value: This order was placed for QT3000! OK?
// Found value: This order was placed for QT
// Found value: 3000
// Found value: ! OK?
```
### Matcher 类的方法
索引方法
索引方法提供了有用的索引值，精确表明输入字符串中在哪能找到匹配

方法|说明
--|--
public int start()|返回以前匹配的初始索引
public int start(int group)|返回在以前的匹配操作期间，由给定组所捕获的子序列的初始索引
public int end()|返回最后匹配字符之后的偏移量
public int end(int group)|返回在以前的匹配操作期间，由给定组所捕获子序列的最后字符之后的偏移量

匹配方法
匹配方法用来检查输入字符串并返回一个布尔值，表示是否找到该模式
方法|说明
--|--
public boolean lookingAt()|尝试将从区域开头开始的输入序列与该模式匹配
public boolean find()|尝试查找与该模式匹配的输入序列的下一个子序列
public boolean find(int start)|重置此匹配器，然后尝试查找匹配该模式、从指定索引开始的输入序列的下一个子序列
public boolean matches()|尝试将整个区域与模式匹配

替换方法
替换方法是替换输入字符串里文本的方法

方法及说明|
--|
public Matcher appendReplacement(StringBuffer sb, String replacement)|
实现非终端添加和替换步骤|
public StringBuffer appendTail(StringBuffer sb)|
实现终端添加和替换步骤|
public String replaceAll(String replacement)|
替换模式与给定替换字符串相匹配的输入序列的每个子序列|
public String replaceFirst(String replacement)|
替换模式与给定替换字符串匹配的输入序列的第一个子序列|
public static String quoteReplacement(String s)|
返回指定字符串的字面替换字符串。这个方法返回一个字符串，就像传递给Matcher类|的appendReplacement 方法一个字面字符串一样工作|

## 方法
```java
修饰符 返回值类型 方法名(参数类型 参数名){
    ...
    方法体
    ...
    return 返回值;
}
```

### 方法的重载
- 创建另一个有相同名字但参数不同的方法
- 就是说一个类的两个方法拥有相同的名字，但是有不同的参数列表
- Java编译器根据方法签名判断哪个方法应该被调用
- 方法重载可以让程序更清晰易读，执行密切相关任务的方法应该使用相同的名字
- 重载的方法必须拥有不同的参数列表。你不能仅仅依据修饰符或者返回类型的不同来重载方法

### 命令行参数的使用
运行一个程序时候传递给它消息
```java
public class CommandLine {
   public static void main(String args[]){ 
      for(int i=0; i<args.length; i++){
         System.out.println("args[" + i + "]: " +
                                           args[i]);
      }
   }
}
```
### 可变参数
- 支持传递同类型的可变参数给一个方法
- 一个方法中只能指定一个可变参数，它必须是方法的最后一个参数
```java
public static void main(String args[]) {
    // 调用可变参数的方法
    printMax(34, 3, 3, 2, 56.5);
    printMax(new double[]{1, 2, 3});
}

public static void printMax( double... numbers) {
    if (numbers.length == 0) {
        System.out.println("No argument passed");
        return;
    }

    double result = numbers[0];

    for (int i = 1; i <  numbers.length; i++){
        if (numbers[i] >  result) {
            result = numbers[i];
        }
    }
    System.out.println("The max value is " + result);
}
// 输出
// The max value is 56.5
// The max value is 3.0
```
## 流 ( Stream )
输入流表示从一个源读取数据，输出流表示向一个目标写数据
`Java.io` 包几乎包含了所有操作输入、输出需要的类
```java
// 读取控制台输入
BufferedReader br = new BufferedReader(new 
                      InputStreamReader(System.in));
// BufferedReader 对象创建后，我们就可以使用 read() 方法从控制台读取一个字符，
// 或者用 readLine() 方法读取一个字符串

// 从控制台读取多字符输入
// 使用 read() 方法从控制台不断读取字符直到用户输入 "q"
public static void main(String args[]) throws IOException {
char c;
// 使用 System.in 创建 BufferedReader 
BufferedReader br = new BufferedReader(new 
                InputStreamReader(System.in));
System.out.println("输入字符, 按下 'q' 键退出。");
// 读取字符
do {
    c = (char) br.read();
    System.out.println(c);
} while(c != 'q');
}

// 从控制台读取一行字符串
BufferedReader br = new BufferedReader(new
                            InputStreamReader(System.in));
do {
    str = br.readLine();
    System.out.println(str);
} while(!str.equals("end"));

// 控制台输出
System.out.write('word');
// write() 方法不经常使用，因为 print() 和 println() 方法用起来更为方便
```
## Scanner 类
java.util.Scanner 可以用来获取用户的输入
可以使用 next() 与 nextLine() 方法获取输入的字符串
还可以使用 hasNext() 与 hasNextLine() 两个方法判断是否还有输入的数据
```java
Scanner scan = new Scanner(System.in);

// 从键盘接收数据
// next方式接收字符串
System.out.println("next方式接收：");

// 判断是否还有输入
if (scan.hasNext()) {
    String str1 = scan.next();
    System.out.println("输入的数据为：" + str1);
}
scan.close();
```
## 读写文件
```java
// 使用字符串类型的文件名来创建一个输入流对象来读取文件
InputStream f = new FileInputStream("C:/java/hello");
```
方法及描述
- public void close() throws IOException{}
关闭此文件输入流并释放与此流有关的所有系统资源。抛出IOException异常
- protected void finalize()throws IOException {}
这个方法清除与该文件的连接。确保在不再引用文件输入流时调用其 close 方法。抛出 IOException 异常
- public int read(int r)throws IOException{}
这个方法从 InputStream 对象读取指定字节的数据。返回为整数值。返回下一字节数据，如果已经到结尾则返回 -1
- public int read(byte[] r) throws IOException{}
这个方法从输入流读取r.length长度的字节。返回读取的字节数。如果是文件结尾则返回 -1
- public int available() throws IOException{}
返回下一次对此输入流调用的方法可以不受阻塞地从此输入流读取的字节数。返回一个整数值
```java
// 使用字符串类型的文件名来创建一个输出流对象
OutputStream f = new FileOutputStream("/tmp/hello.txt")
```
- public void close() throws IOException{}
关闭此文件输入流并释放与此流有关的所有系统资源。抛出IOException异常
- protected void finalize() throws IOException {}
清除与该文件的连接。确保在不再引用文件输入流时调用其 close 方法。抛出IOException异常。
- public void write(int w) throws IOException{}
把指定的字节写到输出流中
- public void write(byte[] w)
把指定数组中 w.length 长度的字节写到 OutputStream 中

```java
// 创建文件 test.txt，并把给定的数字以二进制形式写进该文件，同时输出到控制台上
try{
    byte bWrite [] = {11,21,3,40,5};
    OutputStream os = new FileOutputStream("test.txt");
    for(int x=0; x < bWrite.length ; x++){
    os.write( bWrite[x] ); // writes the bytes
}
os.close();

InputStream is = new FileInputStream("test.txt");
int size = is.available();

for(int i=0; i< size; i++){
    System.out.print((char)is.read() + "  ");
}
    is.close();
}catch(IOException e){
    System.out.print("Exception");
}  
```
```java
// 解决乱码问题
File f = new File("a.txt");
FileOutputStream fop = new FileOutputStream(f);
// 构建FileOutputStream对象,文件不存在会自动新建

OutputStreamWriter writer = new OutputStreamWriter(fop, "UTF-8");
// 构建OutputStreamWriter对象,参数可以指定编码,默认为操作系统默认编码,windows上是gbk

writer.append("中文输入");
// 写入到缓冲区

writer.append("\r\n");
//换行

writer.append("English");
// 刷新缓存冲,写入到文件,如果下面已经没有写入的内容了,直接close也会写入

writer.close();
//关闭写入流,同时会把缓冲区内容写入文件,所以上面的注释掉

fop.close();
// 关闭输出流,释放系统资源

FileInputStream fip = new FileInputStream(f);
// 构建FileInputStream对象

InputStreamReader reader = new InputStreamReader(fip, "UTF-8");
// 构建InputStreamReader对象,编码与写入相同

StringBuffer sb = new StringBuffer();
while (reader.ready()) {
    sb.append((char) reader.read());
    // 转成char加到StringBuffer对象中
}
System.out.println(sb.toString());
reader.close();
// 关闭读取流

fip.close();
// 关闭输入流,释放系统资源
```
## 目录操作方法
### 创建目录
File类中有两个方法可以用来创建文件夹
方法|说明
--|--
mkdir()|创建一个文件夹，成功则返回 true，失败则返回 false。失败表明 File 对象指定的路径已经存在，或者由于整个路径还不存在，该文件夹不能被创建
mkdirs()|创建一个文件夹和它的所有父文件夹
```java
String dirname = "/tmp/user/java/bin";
File d = new File(dirname);
// 现在创建目录
d.mkdirs();
```
>Java 会自动转换目录路径分隔符，所以不管在 Windows 下还是 Linux 下，都可以使用 (/)
### 读取目录
- 一个目录其实就是一个 File 对象，包含其它文件和文件夹
- 如果创建一个 File 对象并且它是一个目录，那么调用 isDirectory() 方法会返回 true
- 可以通过调用该对象上的 list() 方法，来提取它包含的文件和文件夹的列表
```java
String dirname = "/tmp";
File f1 = new File(dirname);
if (f1.isDirectory()) {
    System.out.println( "目录 " + dirname);
    String s[] = f1.list();
    for (int i=0; i < s.length; i++) {
    File f = new File(dirname + "/" + s[i]);
    if (f.isDirectory()) {
        System.out.println(s[i] + " 是一个目录");
    } else {
        System.out.println(s[i] + " 是一个文件");
    }
    }
} else {
    System.out.println(dirname + " 不是一个目录");
}

```
### 删除目录或文件
java.io.File.delete() 方法
```java
public static void main(String args[]) {
    // 这里修改为自己的测试目录
    File folder = new File("/tmp/java/");
    deleteFolder(folder);
}

//删除文件及目录
public static void deleteFolder(File folder) {
    File[] files = folder.listFiles();
        if(files!=null) { 
            for(File f: files) {
                if(f.isDirectory()) {
                    deleteFolder(f);
                } else {
                    f.delete();
                }
            }
        }
    folder.delete();
}
```
## 异常处理
### 捕获
```java
try {
    // 程序代码
}catch(ExceptionName e1) {
    // Catch 块
}

try{
   // 程序代码
}catch(异常类型1 异常的变量名1){
  // 程序代码
}catch(异常类型2 异常的变量名2){
  // 程序代码
}catch(异常类型2 异常的变量名2){
  // 程序代码
}
```
### 抛出
```java
public void deposit(double amount) throws RemoteException
{

    // 方法实现
    throw new RemoteException();
}

// 一个方法可以声明抛出多个异常，多个异常之间用逗号隔开
public void withdraw(double amount) throws RemoteException,
                            InsufficientFundsException
{
    // 方法语句块
}

```
### finally 语句
无论是否发生异常，finally 代码块中的代码总会被执行
```java
try{
  // 程序代码
}catch(异常类型1 异常的变量名1){
  // 程序代码
}catch(异常类型2 异常的变量名2){
  // 程序代码
}finally{
  // 程序代码
}
```
- catch 不能独立于 try 存在
- 在 try/catch 后面添加 finally 块并非强制性要求的
- try 代码后不能既没 catch 块也没 finally 块
- try, catch, finally 块之间不能添加任何代码

### 自定义异常
- 所有异常都必须是 Throwable 的子类
- 如果希望写一个检查性异常类，则需要继承 Exception 类
- 如果你想写一个运行时异常类，那么需要继承 RuntimeException 类

## 源文件声明规则
- 一个源文件中只能有一个 public 类
- 一个源文件可以有多个非 public 类
- 源文件的名称应该和 public 类的类名保持一致
假如源文件中 public 类的类名是 Employee，那么源文件应该命名为 Employee.java
- 如果一个类定义在某个包中，那么 package 语句应该在源文件的首行
- 如果源文件包含 import 语句，那么应该放在 package 语句和类定义之间
如果没有 package 语句，那么 import 语句应该在源文件中最前面
- import 语句和 package 语句对源文件中定义的所有类都有效
在同一源文件中，不能给不同的类不同的包声明

## 包 ( package )
使用包 ( package ) 这种机制是为了防止命名冲突，访问控制，提供搜索和定位类 ( class)、接口、枚举 ( enumerations ) 和注释 ( annotation ) 等
通常使用小写的字母来命名避免与类、接口名字的冲突。
```java
// 使用 package 关键字定义一个包
package pkg1[．pkg2[．pkg3…]];
```
```java
// 例如,一个 Something.java 文件它的内容
package net.java.util
public class Something{
   //...
}
// 那么它的路径应该是 net/java/util/Something.java 这样保存的
```
包的作用
- 把功能相似或相关的类或接口组织在同一个包中，方便类的查找和使用
- 如同文件夹一样，包也采用了树形目录的存储方式。同一个包中的类名字是不同的，不同的包中的类的名字是可以相同的，当同时调用两个不同包中相同类名的类时，应该加上包名加以区别。因此，包可以避免名字冲突
- 包也限定了访问权限，拥有包访问权限的类才能访问某个包中的类

## import 关键字
为了能够使用某一个包的成员，我们需要在 Java 程序中明确导入该包
如果在一个包中，一个类想要使用本包中的另一个类，那么该包名可以省略
```java
// import 关键字的使用方法如下
import package1[.package2…].(classname|*);
```
```java
// 如果 Boss 类不在 payroll 包中
// Boss 类必须使用下面几种方法之一来引用其它包中的类

// 1 使用类全名描述
payroll.Employee

// 2 用 import 关键字引入，使用通配符 "*"
import payroll.*;

// 3 使用 import 关键字引入 Employee 类
import payroll.Employee;
```














