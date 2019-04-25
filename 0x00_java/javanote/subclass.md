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