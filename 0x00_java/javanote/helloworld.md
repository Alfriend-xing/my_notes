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