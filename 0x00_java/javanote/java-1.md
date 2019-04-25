## 继承
- 提高代码重用性
```java
class Parent {
}

class Child extends Parent {
}
```
继承的特性
- 子类拥有父类非 private 的属性，方法
- 子类可以拥有自己的属性和方法，即子类可以对父类进行扩展
- 子类可以用自己的方式实现父类的方法
- Java的继承是单继承，但是可以多重继承，单继承就是一个子类只能继承一个父类，多重继承就是，例如 A类继承 B 类，B 类继承 C 类，所以按照关系就是 C 类是 B类的父类， B 类是 A 类的父类
- 提高了类之间的耦合性 ( 继承的缺点，耦合度高就会造成代码之间的联系 )

### extends 
Java 中，类的继承是单一继承，也就是说，一个子类只能拥有一个父类，所以 extends 只能继承一个类
### implements 
用于类继承接口，可以变相的使 java 具有多继承的特性，可以同时继承多个接口
```java
public interface A {
    public void eat();
    public void sleep();
}

public interface B {
    public void show();
}

public class C implements A,B {
}
```
### super 
用于对父类成员的访问，用来引用当前对象的父类

### this 
this 关键字则指向自己的引用

### final 
- 当用于修饰类时可以把类定义为不能继承的，即最终类
- 当用于修饰方法时，该方法不能被子类重写


## 重写 ( Override )
重写是子类对父类的允许访问的方法的实现过程进行重新编写, 返回值和形参都不能改变，即外壳不变，核心重写

重写规则
- 参数列表必须完全与被重写方法的相同
- 返回类型必须完全与被重写方法的返回类型相同
- 访问权限不能比父类中被重写的方法的访问权限更低。例如：如果父类的一个方法被声明为public，那么在子类中重写该方法就不能声明为protected
- 父类的成员方法只能被它的子类重写
- 声明为final的方法不能被重写
- 声明为static的方法不能被重写，但是能够被再次声明
- 子类和父类在同一个包中，那么子类可以重写父类所有方法，除了声明为private和final的方法
- 子类和父类不在同一个包中，那么子类只能够重写父类的声明为public和protected的非final方法
- 重写的方法能够抛出任何非强制异常，无论被重写的方法是否抛出异常。但是，重写的方法不能抛出新的强制性异常，或者比被重写方法声明的更广泛的强制性异常，反之则可以
- 构造方法不能被重写
- 如果不能继承一个方法，则不能重写这个方法

## 重载 ( Overload )
重载 ( overloading ) 是在一个类里面，方法名字相同，而参数不同，返回类型可以相同也可以不同
每个重载的方法 ( 或者构造函数 ) 都必须有一个独一无二的参数类型列表

重载规则
- 被重载的方法必须改变参数列表 (参数个数或类型或顺序不一样)
- 被重载的方法可以改变返回类型
- 被重载的方法可以改变访问修饰符
- 被重载的方法可以声明新的或更广的检查异常
- 方法能够在同一个类中或者在一个子类中被重载
- 无法以返回值类型作为重载函数的区分标准

重写与重载之间的区别
区别点|重载方法|重写方法
--|--|--
参数列表|必须修改|一定不能修改
返回类型|可以修改|一定不能修改
异常|可以修改|可以减少或删除，一定不能抛出新的或者更广的异常
访问|可以修改|一定不能做更严格的限制 ( 可以降低限制 )

## 多态
多态是同一个行为具有多个不同表现形式或形态的能力
多态就是同一个接口，使用不同的实例而执行不同操作
可以使程序有良好的扩展，并可以对所有类的对象进行通用处理

多态的优点
- 消除类型之间的耦合关系
- 可替换性
- 可扩充性
- 接口性
- 灵活性
- 简化性

多态存在的三个必要条件
- 继承
- 重写
- 父类引用指向子类对象

```java
Parent p = new Child();
// 当使用多态方式调用方法时，首先检查父类中是否有该方法，
// 如果没有，则编译错误；如果有，再去调用子类的同名方法
```
```java
public class Test {
    public static void main(String[] args) {
      show(new Cat());  // 以 Cat 对象调用 show 方法
      show(new Dog());  // 以 Dog 对象调用 show 方法

      Animal a = new Cat();  // 向上转型  
      a.eat();               // 调用的是 Cat 的 eat
      Cat c = (Cat)a;        // 向下转型  
      c.work();        // 调用的是 Cat 的 work
  }  

    public static void show(Animal a)  {
      a.eat();  
        // 类型判断
        if (a instanceof Cat)  {  // 猫做的事情 
            Cat c = (Cat)a;  
            c.work();  
        } else if (a instanceof Dog) { // 狗做的事情 
            Dog c = (Dog)a;  
            c.work();  
        }  
    }  
}

abstract class Animal {  
    abstract void eat();  
}  

class Cat extends Animal {  
    public void eat() {  
        System.out.println("吃鱼");  
    }  
    public void work() {  
        System.out.println("抓老鼠");  
    }  
}  

class Dog extends Animal {  
    public void eat() {  
        System.out.println("吃骨头");  
    }  
    public void work() {  
        System.out.println("看家");  
    }  
}
```
## 抽象类
Java 使用 abstract class 语句来定义抽象类
抽象类不能被实例化
```java
// 定义抽象类
public abstract class Employee
{
    private String name;
    private String address;
    private int number;

    public abstract double computePay();
}

// 继承抽象类
public class Salary extends Employee
{
    public double computePay()
   {
      System.out.println("Computing salary pay for " + getName());
      return salary/52;
   }
}
```
抽象类的几点约束
- 抽象类不能被实例化 ( 初学者很容易犯的错 )，如果被实例化，就会报错，编译无法通过，只有抽象类的非抽象子类可以创建对象
- 抽象类中不一定包含抽象方法，但是有抽象方法的类必定是抽象类
- 抽象类中的抽象方法只是声明，不包含方法体，就是不给出方法的具体实现也就是方法的具体功能
- 构造方法，类方法 ( 用 static 修饰的方法 ) 不能声明为抽象方法
- 抽象类的子类必须给出抽象类中的抽象方法的具体实现，除非该子类也是抽象类

## 封装
- 封装是面向对象编程最重要的一个特性，是指一种将抽象性函式接口的实现细节部份包装、隐藏起来的方法
- 封装可以被认为是一个保护屏障，防止该类的代码和数据被外部类定义的代码随机访问
- 要访问该类的代码和数据，必须通过严格的接口控制
- 封装最主要的功能在于我们能修改自己的实现代码，而不用修改那些调用我们代码的程序片段
- 适当的封装可以让程式码更容易理解与维护，也加强了程式码的安全性

>将属性设为私有，公开修改的方法，为什么这样设置，因为如果其他子类可以直接随意更改属性，后期很难管理，比如加入判断语句或线程锁

封装的优点
- 良好的封装能够减少耦合
- 类内部的结构可以自由修改
- 可以对成员变量进行更精确的控制
- 隐藏信息，实现细节

```java
// 修改属性的可见性来限制对属性的访问 ( 一般限制为 private )
public class Person {
    private String name;
    private int age;
}

// 对每个值属性提供对外的公共方法访问,这些方法被称为 getter 和 setter 方法
public class Person{
    private String name;
    public String getName(){
      return name;
    }
    public void setName(String name){
      this.name = name;
    }
}
// 采用 this 关键字是为了解决实例变量 (private String name ) 
// 和局部变量 ( setName(String name)中的name变量）之间发生的同名的冲突
```
## 接口
- 接口通常以 interface 来声明
- 一个类通过继承接口的方式，从而来继承接口的抽象方法
- 接口并不是类，编写接口的方式和类很相似，但是它们属于不同的概念
   - 类描述对象的属性和方法
   - 接口则包含类要实现的方法
- 除非实现接口的类是抽象类，否则该类要定义接口中的所有方法
- 接口无法被实例化，但是可以被实现
- 一个实现接口的类，必须实现接口内所描述的所有方法，否则就必须声明为抽象类
- Java 中，接口类型可用来声明一个变量，他们可以成为一个空指针，或是被绑定在一个以此接口实现的对象

接口与类相似点
- 一个接口可以有多个方法
- 接口文件保存在 .java 结尾的文件中，文件名使用接口名
- 接口的字节码文件保存在 .class 结尾的文件中
- 接口相应的字节码文件必须在与包名称相匹配的目录结构中

接口与类的区别
- 接口不能用于实例化对象
- 接口没有构造方法
- 接口中所有的方法必须是抽象方法
- 接口不能包含成员变量，除了 static 和 final 变量
- 接口不是被类继承了，而是要被类实现
- 接口支持多继承

接口特性
- 接口中每一个方法也是隐式抽象的,接口中的方法会被隐式的指定为 public abstract ( 只能是 public abstract，其他修饰符都会报错 )
- 接口中可以含有变量，但是接口中的变量会被隐式的指定为 public static final 变量（并且只能是 public，用 private 修饰会报编译错误 )
- 接口中的方法是不能在接口中实现的，只能由实现接口的类来实现接口中的方法

抽象类和接口的区别
- 抽象类中的方法可以有方法体，就是能实现方法的具体功能，但是接口中的方法不行
- 抽象类中的成员变量可以是各种类型的，而接口中的成员变量只能是 public static final 类型的
- 接口中不能含有静态代码块以及静态方法(用 static 修饰的方法)，而抽象类是可以有静态代码块和静态方法
- 一个类只能继承一个抽象类，而一个类却可以实现多个接口

```java
[可见度] interface 接口名称 [extends 其他的类名] {
        // 声明变量
        // 抽象方法
}

// 例
import java.lang.*;

public interface NameOfInterface
{
   //任何类型 final, static 字段
   //抽象方法
}
```

接口有以下特性
- 接口是隐式抽象的，当声明一个接口的时候，不必使用 abstract 关键字
- 接口中每一个方法也是隐式抽象的，声明时同样不需要 abstract 关键字
- 接口中的方法都是公有的

### 实现接口
类使用 implements 关键字实现接口
在类声明中，implements关键字放在 class 声明后面
当类实现接口的时候，类要实现接口中所有的方法，否则，类必须声明为抽象的类
>...implements 接口名称[, 其他接口, 其他接口..., ...] ...
```java
public class MammalInt implements Animal{}
```

重写接口中声明的方法时，需要注意以下规则
- 类在实现接口的方法时，不能抛出强制性异常，只能在接口中，或者继承接口的抽象类中抛出该强制性异常
- 类在重写方法时要保持一致的方法名，并且应该保持相同或者相兼容的返回值类型
- 如果实现接口的类是抽象类，那么就没必要实现该接口的方法

在实现接口的时候，也要注意一些规则
- 一个类可以同时实现多个接口
- 一个类只能继承一个类，但是能实现多个接口
- 一个接口能继承另一个接口，这和类之间的继承比较相似

### 接口的继承
一个接口能继承另一个接口，和类之间的继承方式比较相似
接口的继承使用 extends 关键字，子接口继承父接口的方法
```java
// 文件名: Sports.java
public interface Sports
{
   public void setHomeTeam(String name);
   public void setVisitingTeam(String name);
}

// 文件名: Football.java
public interface Football extends Sports
{
   public void homeTeamScored(int points);
   public void visitingTeamScored(int points);
   public void endOfQuarter(int quarter);
}

// 文件名: Hockey.java
public interface Hockey extends Sports
{
   public void homeGoalScored();
   public void visitingGoalScored();
   public void endOfPeriod(int period);
   public void overtimePeriod(int ot);
}
```
### 接口的多继承
Java中，类的多继承是不合法，但接口允许多继承
```java
public interface Hockey extends Sports, Event{}
```
### 空接口
- 空接口就是没有包含任何方法的接口
- 空接口是没有任何方法和属性的接口，它仅仅表明它的类属于一个特定的类型,供其他代码来测试允许做一些事情
- 空接口作用：简单形象的说就是给某个对象打个标 (盖个戳 )，使对象拥有某个或某些特权

## Java 数据结构

### 枚举 ( Enumeration )
Enumeration 接口中定义了一些方法，通过这些方法可以枚举对象集合中的元素
>这种传统接口已被迭代器取代，虽然Enumeration 还未被遗弃，但在现代代码中已经被很少使用了

```java
import java.util.Vector;
import java.util.Enumeration;

public class EnumerationTester {

   public static void main(String args[]) {
      Enumeration<String> days;
      Vector<String> dayNames = new Vector<String>();
      dayNames.add("Sunday");
      dayNames.add("Monday");
      dayNames.add("Tuesday");
      dayNames.add("Wednesday");
      dayNames.add("Thursday");
      dayNames.add("Friday");
      dayNames.add("Saturday");
      days = dayNames.elements();
    //   boolean hasMoreElements()测试此枚举是否包含更多的元素
      while (days.hasMoreElements()){
//           Object nextElement()
// 如果此枚举对象至少还有一个可提供的元素，则返回此枚举的下一个元素
         System.out.println(days.nextElement()); 
      }
   }
}
```

### 位集合 ( BitSet )
Bitset 类创建一种特殊类型的数组来保存位值
BitSet 中数组大小会随需要增加
```java
void and(BitSet set)
// 对此目标位 set 和参数位 set 执行逻辑与操作
void andNot(BitSet set)
// 清除此 BitSet 中所有的位，其相应的位在指定的 BitSet 中已设置
int cardinality( )
// 返回此 BitSet 中设置为 true 的位数
void clear( )
// 将此 BitSet 中的所有位设置为 false
void clear(int index)
// 将索引指定处的位设置为 false
void clear(int startIndex, int endIndex)
// 将指定的 fromIndex（包括）到指定的 toIndex（不包括）范围内的位设置为 false
Object clone( )
// 复制此 BitSet，生成一个与之相等的新 BitSet
boolean equals(Object bitSet)
// 将此对象与指定的对象进行比较
void flip(int index)
// 将指定索引处的位设置为其当前值的补码
void flip(int startIndex, int endIndex)
// 将指定的 fromIndex（包括）到指定的 toIndex（不包括）范围内的每个位设置为其当前值的补码
boolean get(int index)
// 返回指定索引处的位值
BitSet get(int startIndex, int endIndex)
// 返回一个新的 BitSet，它由此 BitSet 中从 fromIndex（包括）到 toIndex（不包括）范围内的位组成
int hashCode( )
// 返回此位 set 的哈希码值
boolean intersects(BitSet bitSet)
// 如果指定的 BitSet 中有设置为 true 的位，并且在此 BitSet 中也将其设置为 true，则返回 ture
boolean isEmpty( )
// 如果此 BitSet 中没有包含任何设置为 true 的位，则返回 ture
int length( )
// 返回此 BitSet 的"逻辑大小"：BitSet 中最高设置位的索引加 1
int nextClearBit(int startIndex)
// 返回第一个设置为 false 的位的索引，这发生在指定的起始索引或之后的索引上
int nextSetBit(int startIndex)
// 返回第一个设置为 true 的位的索引，这发生在指定的起始索引或之后的索引上
void or(BitSet bitSet)
// 对此位 set 和位 set 参数执行逻辑或操作
void set(int index)
// 将指定索引处的位设置为 true
void set(int index, boolean v)
// 将指定索引处的位设置为指定的值
void set(int startIndex, int endIndex)
// 将指定的 fromIndex（包括）到指定的 toIndex（不包括）范围内的位设置为 true
void set(int startIndex, int endIndex, boolean v)
// 将指定的 fromIndex（包括）到指定的 toIndex（不包括）范围内的位设置为指定的值
int size( )
// 返回此 BitSet 表示位值时实际使用空间的位数
String toString( )
// 返回此位 set 的字符串表示形式
void xor(BitSet bitSet)
// 对此位 set 和位 set 参数执行逻辑异或操作
```



### 向量 ( Vector )
向量 ( Vector ) 类和传统数组非常相似，但是 Vector 的大小能根据需要动态的变化
```java
void add(int index, Object element)
// 在此向量的指定位置插入指定的元素
boolean add(Object o)
// 将指定元素添加到此向量的末尾
boolean addAll(Collection c)
// 将指定 Collection 中的所有元素添加到此向量的末尾，按照指定 collection 的迭代器所返回的顺序添加这些元素
boolean addAll(int index, Collection c)
// 在指定位置将指定 Collection 中的所有元素插入到此向量中
void addElement(Object obj)
// 将指定的组件添加到此向量的末尾，将其大小增加 1
int capacity()
// 返回此向量的当前容量
void clear()
// 从此向量中移除所有元素
Object clone()
// 返回向量的一个副本
boolean contains(Object elem)
// 如果此向量包含指定的元素，则返回 true
boolean containsAll(Collection c)
// 如果此向量包含指定 Collection 中的所有元素，则返回 true
void copyInto(Object[] anArray)
// 将此向量的组件复制到指定的数组中
Object elementAt(int index)
// 返回指定索引处的组件
Enumeration elements()
// 返回此向量的组件的枚举
void ensureCapacity(int minCapacity)
// 增加此向量的容量（如有必要），以确保其至少能够保存最小容量参数指定的组件数
boolean equals(Object o)
// 比较指定对象与此向量的相等性
Object firstElement()
// 返回此向量的第一个组件（位于索引 0) 处的项）
Object get(int index)
// 返回向量中指定位置的元素
int hashCode()
// 返回此向量的哈希码值
int indexOf(Object elem)
// 返回此向量中第一次出现的指定元素的索引，如果此向量不包含该元素，则返回 -1
int indexOf(Object elem, int index)
// 返回此向量中第一次出现的指定元素的索引，从 index 处正向搜索，如果未找到该元素，则返回 -1
void insertElementAt(Object obj, int index)
// 将指定对象作为此向量中的组件插入到指定的 index 处
boolean isEmpty()
// 测试此向量是否不包含组件
Object lastElement()
// 返回此向量的最后一个组件
int lastIndexOf(Object elem)
// 返回此向量中最后一次出现的指定元素的索引；如果此向量不包含该元素，则返回 -1
int lastIndexOf(Object elem, int index)
// 返回此向量中最后一次出现的指定元素的索引，从 index 处逆向搜索，如果未找到该元素，则返回 -1
Object remove(int index)
// 移除此向量中指定位置的元素
boolean remove(Object o)
// 移除此向量中指定元素的第一个匹配项，如果向量不包含该元素，则元素保持不变
boolean removeAll(Collection c)
// 从此向量中移除包含在指定 Collection 中的所有元素
void removeAllElements()
// 从此向量中移除全部组件，并将其大小设置为零
boolean removeElement(Object obj)
// 从此向量中移除变量的第一个（索引最小的）匹配项
void removeElementAt(int index)
// 删除指定索引处的组件
protected void removeRange(int fromIndex, int toIndex)
// 从此 List 中移除其索引位于 fromIndex（包括）与 toIndex（不包括）之间的所有元素
boolean retainAll(Collection c)
// 在此向量中仅保留包含在指定 Collection 中的元素
Object set(int index, Object element)
// 用指定的元素替换此向量中指定位置处的元素
void setElementAt(Object obj, int index)
// 将此向量指定 index 处的组件设置为指定的对象
void setSize(int newSize)
// 设置此向量的大小
int size()
// 返回此向量中的组件数
List subList(int fromIndex, int toIndex)
// 返回此 List 的部分视图，元素范围为从 fromIndex（包括）到 toIndex（不包括）
Object[] toArray()
// 返回一个数组，包含此向量中以恰当顺序存放的所有元素
Object[] toArray(Object[] a)
// 返回一个数组，包含此向量中以恰当顺序存放的所有元素；返回数组的运行时类型为指定数组的类型
String toString()
// 返回此向量的字符串表示形式，其中包含每个元素的 String 表示形式
void trimToSize()
// 对此向量的容量进行微调，使其等于向量的当前大小
```
### 栈 ( Stack )
栈是 Vector 的一个子类，它实现了一个标准的后进先出的栈
堆栈只定义了默认构造函数，用来创建一个空栈
堆栈除了包括由 Vector 定义的所有方法，也定义了自己的一些方法
```java
boolean empty()
// 测试堆栈是否为空
Object peek()
// 查看堆栈顶部的对象，但不从堆栈中移除它
Object pop()
// 移除堆栈顶部的对象，并作为此函数的值返回该对象
Object push(Object element)
// 把项压入堆栈顶部
int search(Object element)
// 返回对象在堆栈中的位置，以 1 为基数
```
### 字典 ( Dictionary )
字典 ( Dictionary ) 类是一个抽象类，它定义了键映射到值的数据结构
当想要通过特定的键而不是整数索引来访问数据的时候，这时候应该使用 Dictionary
>Dictionary 类已经过时了，实际开发中，可以使用 实现 Map 接口 来获取键/值的存储功能
```java
Enumeration elements( )
// 返回此 dictionary 中值的枚举。
Object get(Object key)
// 返回此 dictionary 中该键所映射到的值。
boolean isEmpty( )
// 测试此 dictionary 是否不存在从键到值的映射。
Enumeration keys( )
// 返回此 dictionary 中的键的枚举。
Object put(Object key, Object value)
// 将指定 key 映射到此 dictionary 中指定 value。
Object remove(Object key)
// 从此 dictionary 中移除 key （及其相应的 value）。
int size( )
// 返回此 dictionary 中条目（不同键）的数量。
```
### 哈希表 ( Hashtable )
```java
// 默认构造方法
Hashtable()
// 创建指定大小的哈希表
Hashtable(int size)
// 创建了一个指定大小的哈希表，并且通过 fillRatio 指定填充比例
// 填充比例必须介于0.0和1.0之间，它决定了哈希表在重新调整大小之前的充满程度
Hashtable(int size,float fillRatio)
// 创建了一个以 M 中元素为初始化元素的哈希表
// 哈希表的容量被设置为 M 的两倍
Hashtable(Map m)
```
```java
// Hashtable 中除了从 Map 接口中定义的方法外，还定义了以下方法

void clear()
// 将此哈希表清空，使其不包含任何键
Object clone()
// 创建此哈希表的浅表副本
boolean contains(Object value)
// 测试此映射表中是否存在与指定值关联的键
boolean containsKey(Object key)
// 测试指定对象是否为此哈希表中的键
boolean containsValue(Object value)
// 如果此 Hashtable 将一个或多个键映射到此值，则返回 true
Enumeration elements()
// 返回此哈希表中的值的枚举
Object get(Object key)
// 返回指定键所映射到的值，如果此映射不包含此键的映射，则返回 null. 更确切地讲，如果此映射包含满足 (key.equals(k)) 的从键 k 到值 v 的映射，则此方法返回 v；否则，返回 null
boolean isEmpty()
// 测试此哈希表是否没有键映射到值
Enumeration keys()
// 返回此哈希表中的键的枚举
Object put(Object key, Object value)
// 将指定 key 映射到此哈希表中的指定 value
void rehash()
// 增加此哈希表的容量并在内部对其进行重组，以便更有效地容纳和访问其元素
Object remove(Object key)
// 从哈希表中移除该键及其相应的值
int size()
// 返回此哈希表中的键的数量
String toString()
// 返回此 Hashtable 对象的字符串表示形式，其形式为 ASCII 字符 ", " （逗号加空格）分隔开的、括在括号中的一组条目
```

### 属性 ( Properties )
Properties 继承于 Hashtable.Properties 类
Properties 表示了一个持久的属性集，属性列表中每个键及其对应值都是一个字符串
```java
// 第一个构造方法没有默认值
Properties()
// 第二个构造方法使用 propDefault 作为默认值
Properties(Properties propDefault)
```
```java
// 除了从 Hashtable 中所定义的方法，Properties 定义了以下方法

String getProperty(String key)
// 用指定的键在此属性列表中搜索属性
String getProperty(String key, String defaultProperty)
// 用指定的键在属性列表中搜索属性
void list(PrintStream streamOut)
// 将属性列表输出到指定的输出流
void list(PrintWriter streamOut)
// 将属性列表输出到指定的输出流
void load(InputStream streamIn) throws IOException
// 从输入流中读取属性列表 （ 键和元素对 )
Enumeration propertyNames() 
// 按简单的面向行的格式从输入字符流中读取属性列表 ( 键和元素对 )
Object setProperty(String key, String value)
// 调用 Hashtable 的方法 put
void store(OutputStream streamOut, String description)
// 以适合使用 load(InputStream)方法加载到 Properties 表中的格式，将此 Properties 表中的属性列表（键和元素对）写入输出流
```





















