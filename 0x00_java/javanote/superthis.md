### super 
用于对父类成员的访问，用来引用当前对象的父类
```java
// 普通的直接引用
// super相当于是指向当前对象的父类，这样就可以用super.xxx来引用父类的成员

// 引用构造函数
// super（参数）：调用父类中的某一个构造函数（应该为构造函数中的第一条语句）。
public class Chinese extends Person {
    Chinese() { 
        super(); // 调用父类构造方法（1） 
        prt("子类·调用父类”无参数构造方法“： "+"A chinese coder."); 
    } 

    Chinese(String name) { 
        super(name);// 调用父类具有相同形参的构造方法（2） 
        prt("子类·调用父类”含一个参数的构造方法“： "+"his name is " + name); 
    } 
}
```
### this 
this 关键字则指向自己的引用
```java
// 普通的直接引用
// this相当于是指向当前对象本身。

// 形参与成员名字重名，用this来区分
public int GetAge(int age){
    this.age = age;
    return this.age;
}

// 引用构造函数
// this（参数）：调用本类中另一种形式的构造函数（应该为构造函数中的第一条语句）。
public class Chinese extends Person {
    Chinese(String name, int age) { 
        this(name);// 调用具有相同形参的构造方法（3） 
        prt("子类：调用子类具有相同形参的构造方法：his age is " + age); 
    } 
}
```