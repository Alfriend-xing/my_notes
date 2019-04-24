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


