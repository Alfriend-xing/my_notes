# Java Reflection(反射机制)
Java反射机制可以让我们在编译期`(Compile Time)`之外的运行期`(Runtime)`获得任何一个类的字节码。包括接口、变量、方法等信息。还可以让我们在运行期实例化对象，通过调用`get/set`方法获取变量的值。

例子
```java
package Reflect;
 
/**
 * 通过一个对象获得完整的包名和类名
 * */
class Demo{
    //other codes...
}
 
class hello{
    public static void main(String[] args) {
        Demo demo=new Demo();
        System.out.println(demo.getClass().getName());
    }
}
// 运行结果：Reflect.Demo
```
>所有类的对象其实都是Class的实例。

这个例子通过调用类的class属性获取对应的class对象，通过这个 Class 类的对象获取 MyObject 类中的方法集合
```java
Class myObjectClass = MyObject.class;
```
使用 Java 反射机制可以在运行时期检查 Java 类的信息，检查 Java 类的信息往往是你在使用 Java 反射机制的时候所做的第一件事情，通过获取类的信息你可以获取以下相关的内容：
## 1.Class 对象
在你想检查一个类的信息之前，你首先需要获取类的 Class 对象。Java 中的所有类型包括基本类型(int, long, float等等)，即使是数组都有与之关联的 Class 类的对象。如果你在编译期知道一个类的名字的话，那么你可以使用如下的方式获取一个类的 Class 对象。
```java
String className = null;//在运行期获取的类名字符串
Class class = Class.forName("Reflect.Demo");
```
## 2.类名
如果你在编译期不知道类的名字，但是你可以在运行期获得到类名的字符串,你可以从 Class 对象中获取两个版本的类名。

使用 `getName()` 方法返回类的全限定类名(包含包名)：
```java
Class aClass = ... //获取Class对象，具体方式可见Class对象小节
String className = aClass.getName();
```
使用 `getSimpleName()` 方法想获取类的名字(不包含包名):
```java
Class aClass = ... //获取Class对象，具体方式可见Class对象小节
String simpleClassName = aClass.getSimpleName();
```
这个方法对读别人(庞大复杂的app)的代码特别有用，就拿安卓来说吧，你可以在`Activity` 或者是自己定义的`BaseActivity`中打印该`className or simpleClassName`
```java
Log.i(simpleClassName,"Someting");
//这样你就知道自己每次打开的是具体哪个activity了，对阅读捋清App功能逻辑很有帮助
```
## 3.修饰符
可以通过 Class 对象来访问一个类的修饰符， 即public,private,static 等等的关键字
```java
Class  aClass = ... //获取Class对象，具体方式可见Class对象小节
int modifiers = aClass.getModifiers();
```
修饰符都被包装成一个int类型的数字，这样每个修饰符都是一个位标识(flag bit)，这个位标识可以设置和清除修饰符的类型。 可以使用 java.lang.reflect.Modifier 类中的方法来检查修饰符的类型：
```java
Modifier.isAbstract(int modifiers);
Modifier.isFinal(int modifiers);
Modifier.isInterface(int modifiers);
Modifier.isNative(int modifiers);
Modifier.isPrivate(int modifiers);
Modifier.isProtected(int modifiers);
Modifier.isPublic(int modifiers);
Modifier.isStatic(int modifiers);
Modifier.isStrict(int modifiers);
Modifier.isSynchronized(int modifiers);
Modifier.isTransient(int modifiers);
Modifier.isVolatile(int modifiers);
```
## 4.包信息
```java
Class  aClass = ... //获取Class对象，具体方式可见Class对象小节
Package package = aClass.getPackage();
```
## 5.父类
```java
Class superclass = aClass.getSuperclass();
// 可以看到 superclass 对象其实就是一个 Class 类的实例，
// 所以你可以继续在这个对象上进行反射操作。
```
## 6.实现的接口
```java
Class  aClass = ... //获取Class对象，具体方式可见Class对象小节
Class[] interfaces = aClass.getInterfaces();
```
由于一个类可以实现多个接口，因此 getInterfaces(); 方法返回一个 Class 数组，在 Java 中接口同样有对应的 Class 对象。 注意：getInterfaces() 方法仅仅只返回当前类所实现的接口。当前类的父类如果实现了接口，这些接口是不会在返回的 Class 集合中的，尽管实际上当前类其实已经实现了父类接口。
## 7.构造器
```java
Class aClass = ...//获取Class对象
Constructor[] constructors = aClass.getConstructors();
```
返回的 Constructor 数组包含每一个声明为公有的（Public）构造方法。 如果你知道你要访问的构造方法的方法参数类型，你可以用下面的方法获取指定的构造方法
```java
Class aClass = ...//获取Class对象
Constructor constructor = aClass.getConstructor(new Class[]{String.class});
// int.class
```
如果没有指定的构造方法能满足匹配的方法参数则会抛出： `NoSuchMethodException` 。
你可以通过如下方式获取指定构造方法的方法参数信息
```java
Constructor constructor = ... //获取Constructor对象
Class[] parameterTypes = constructor.getParameterTypes();
```
你可以通过如下方法实例化一个类：
```java
Constructor constructor = MyObject.class.getConstructor(String.class);
MyObject myObject = (MyObject) constructor.newInstance("constructor-arg1");
```
constructor.newInstance()方法的方法参数是一个可变参数列表，但是当你调用构造方法的时候你必须提供精确的参数，即形参与实参必须一一对应。在这个例子中构造方法需要一个 String 类型的参数，那我们在调用 newInstance 方法的时候就必须传入一个 String 类型的参数。

## 8.方法
```java
Class aClass = ...//获取Class对象
Method[] methods = aClass.getMethods();
```
返回的 Method 对象数组包含了指定类中声明为公有的(public)的所有变量集合。
获取指定的方法，方法对象名称是“doSomething”，方法参数是 String 类型
```java
Class  aClass = ...//获取Class对象
Method method = aClass.getMethod("doSomething", new Class[]{String.class});
// 要获取的方法没有参数
Method method = aClass.getMethod("doSomething", null);
```
如果根据给定的方法名称以及参数类型无法匹配到相应的方法，则会抛出 NoSuchMethodException。
获取指定方法的方法参数
```java
Method method = ... //获取Class对象
Class[] parameterTypes = method.getParameterTypes();
```
获取指定方法的返回类型
```java
Method method = ... //获取Class对象
Class returnType = method.getReturnType();
```
通过 Method 对象调用方法
```java
//获取一个方法名为doSomesthing，参数类型为String的方法
Method method = MyObject.class.getMethod("doSomething", String.class);
Object returnValue = method.invoke(null, "parameter-value1");
```
传入的 null 参数是你要调用方法的对象，如果是一个静态方法调用的话则可以用 null 代替指定对象作为 invoke()的参数，在上面这个例子中，如果 doSomething 不是静态方法的话，你就要传入有效的 MyObject 实例而不是 null。 Method.invoke(Object target, Object … parameters)方法的第二个参数是一个可变参数列表，但是你必须要传入与你要调用方法的形参一一对应的实参。就像上个例子那样，方法需要 String 类型的参数，那我们必须要传入一个字符串。

## 9.变量
访问一个类的公有成员变量
```java
Field[] method = aClass.getFields();
// Class.getField(String name)
```
访问私有变量
```java
Class.getDeclaredField(String name)
Class.getDeclaredFields()


public class PrivateObject {

  private String privateString = null;

  public PrivateObject(String privateString) {
    this.privateString = privateString;
  }
}
PrivateObject privateObject = new PrivateObject("The Private Value");

Field privateStringField = PrivateObject.class.
            getDeclaredField("privateString");

privateStringField.setAccessible(true);
// 关闭指定类 Field 实例的反射访问检查，这行代码执行之后不论是私有的、受保护的以及包访问的作用域，
// 你都可以在任何地方访问，即使你不在他的访问权限作用域之内

String fieldValue = (String) privateStringField.get(privateObject);
System.out.println("fieldValue = " + fieldValue);
// 输出fieldValue = The Private Value
```
在通常的观点中从对象的外部访问私有变量以及方法是不允许的，但是 Java 反射机制可以做到这一点。注意：这个功能只有在代码运行在单机 Java 应用(standalone Java application)中才会有效,就像你做单元测试或者一些常规的应用程序一样。如果你在 Java Applet 中使用这个功能，那么你就要想办法去应付 SecurityManager 对你限制了。

访问私有方法
```java
Class.getDeclaredMethod(String name, Class[] parameterTypes)
Class.getDeclaredMethods()

public class PrivateObject {

  private String privateString = null;

  public PrivateObject(String privateString) {
    this.privateString = privateString;
  }

  private String getPrivateString(){
    return this.privateString;
  }
}
PrivateObject privateObject = new PrivateObject("The Private Value");

Method privateStringMethod = PrivateObject.class.
        getDeclaredMethod("getPrivateString", null);

privateStringMethod.setAccessible(true);

String returnValue = (String)
        privateStringMethod.invoke(privateObject, null);

System.out.println("returnValue = " + returnValue);
```
## 10.泛型
下面是两个典型的使用泛型的场景：
1. 声明一个需要被参数化（parameterizable）的类/接口。
2. 使用一个参数化类。

当你声明一个类或者接口的时候你可以指明这个类或接口可以被参数化， java.util.List 接口就是典型的例子。你可以运用泛型机制创建一个标明存储的是 String 类型 list，这样比你创建一个 Object 的l ist 要更好。
```java
public class MyClass {

protected List<String> stringList = ...;

public List<String> getStringList(){
return this.stringList;
}
}
```
```java
Method method = MyClass.class.getMethod("getStringList", null);

Type returnType = method.getGenericReturnType();

if(returnType instanceof ParameterizedType){
    ParameterizedType type = (ParameterizedType) returnType;
    Type[] typeArguments = type.getActualTypeArguments();
    for(Type typeArgument : typeArguments){
        Class typeArgClass = (Class) typeArgument;
        System.out.println("typeArgClass = " + typeArgClass);
    }
}
```

泛型方法参数类型
```java
public class MyClass {
  protected List<String> stringList = ...;

  public void setStringList(List<String> list){
    this.stringList = list;
  }
}
```

```java
method = Myclass.class.getMethod("setStringList", List.class);

Type[] genericParameterTypes = method.getGenericParameterTypes();

for(Type genericParameterType : genericParameterTypes){
    if(genericParameterType instanceof ParameterizedType){
        ParameterizedType aType = (ParameterizedType) genericParameterType;
        Type[] parameterArgTypes = aType.getActualTypeArguments();
        for(Type parameterArgType : parameterArgTypes){
            Class parameterArgClass = (Class) parameterArgType;
            System.out.println("parameterArgClass = " + parameterArgClass);
        }
    }
}
```


[参考链接](https://www.cnblogs.com/rollenholt/archive/2011/09/02/2163758.html)
[参考链接](https://www.jianshu.com/p/2315dda64ad2)