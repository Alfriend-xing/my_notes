# 内部类
内部类主要定义在类的内部，定义内部类的作用，主要是因为不希望该类作为大家共同使用访问的类。

## 成员内部类
- 成员内部类就是作为外部类的成员，可以直接使用外部类的所有成员和方法，即使是private。
- 外部类要访问内部类的所有成员变量或方法，则需要通过内部类的对象来获取
- 注意：成员内部类不能含有 static 的变量和方法。
```java
成员内部类的定义如下：

public class 外部类{
public class 内部类{}
}

内部类的实例化：

外部类 对象 = new 外部类（）；
外部类.内部类 对象2=对象.new 内部类（）；
```
## 局部内部类
- 局部内部类是指内部类定义在方法和作用域内。通俗来说，就是在外部内的方法中定义的内部类就是局部内部类。
- 局部内部类由于是在方法中定义的，因此，其作用域也是在方法内部中，方法外执行到，则被JVM回收。局部内部类的实例化也只能在方法中进行。
- 局部内部类方法中想要使用局部变量，该变量必须声明为 final 类型；所以例子中未用 r 成员变量。
```java

public class Method{
    public static void main(String[] args){
        Method m =new Method();
        m.test();
    }
    public void test(){
        final double pi=3.14;
        int r=6;
        class Circle implements Type{
            public double area(){
                return pi*6*6;
            }
        }
        Circle c =new Circle();
        System.out.println("area="+c.area());
    }
}
interface Type{
    public double area();
}
```

## 静态内部类
- 静态内部类就是修饰为 static 的内部类。声明为 static 的内部类，不需要内部类对象和外部类对象之间的联系，就是说，用户可以直接引用“外部类.内部类”。参考静态方法

```java
外部类.内部类 对象 = new 外部类.内部类（）
```

## 匿名内部类
- 匿名内部类是不能有名称的内部类，所以没办法引用。必须在创建时，作为 new 语句的一部分来声明
- 匿名内部类可以当作方法的返回值。
```java
new <类或接口> <类的主体>
// 匿名内部类形式如下：
new 类或接口{
//方法主体
}

// 示例：
public class NiMing {
    public static void main(String[] args) {
        T t = new T(){
            @Override
            public void t() {
                System.out.println("t...");
            }
        };
        t.t();
    }
}
interface T{
    public void t();
}
```


特别注意：
在使用匿名内部类时，要记住以下几个原则。
- 匿名内部类不能有构造方法。
- 匿名内部类不能定义任何静态成员，方法和类。
- 匿名内部类不能使用public，protected，private，static。
- 只能创建匿名内部类的一个实例。
- 一个匿名内部类一定时在 new 后面，用其隐含实现一个接口或实现一个类。
- 因匿名内部类为局部内部类，所以，局部内部类的所有限制都对其有效。
- 内部类只能访问外部类的静态变量或静态方法。
- 内部类当中的 this 指的是匿名内部类本身，如果使用外部类中的 this，则“外部类.this”。


参考链接
https://www.imooc.com/article/13448