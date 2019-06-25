# 使用maven构建java项目

使用Maven构建显示时间的应用

## 项目路径

`mkdir -p src/main/java/hello`

## 需要的类
`src/main/java/hello/HelloWorld.java`
```java
package hello;

public class HelloWorld {
    public static void main(String[] args) {
        Greeter greeter = new Greeter();
        System.out.println(greeter.sayHello());
    }
}
```
`src/main/java/hello/Greeter.java`
```java
package hello;

public class Greeter {
    public String sayHello() {
        return "Hello world!";
    }
}
```

## 下载Maven

从 `https://maven.apache.org/download.cgi.` 下载二进制文件

解压，将bin目录加入系统路径

执行 `mvn -v` 检查安装是否成功

## 项目配置

用于定义项目的名称，版本，依赖包

在项目根目录创建 `pom.xml` 文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.springframework</groupId>
    <artifactId>gs-maven</artifactId>
    <packaging>jar</packaging>
    <version>0.1.0</version>

    <properties>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
    </properties>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>2.1</version>
                <executions>
                    <execution>
                        <phase>package</phase>
                        <goals>
                            <goal>shade</goal>
                        </goals>
                        <configuration>
                            <transformers>
                                <transformer
                                    implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                                    <mainClass>hello.HelloWorld</mainClass>
                                </transformer>
                            </transformers>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
</project>
```
- `<modelVersion>` pom模块版本，总是4.0.0
- `<groupId>` 项目所属的组织名称，对于web项目，通常是域名倒序`com.baidu.www`
- `<artifactId>` 项目库名称
- `<version>` 项目版本
- `<packaging>` 项目打包方式，默认jar，可设war

## 编译代码

- `mvn compile` 执行完毕后将在 `target/classes` 路径下生成.class文件
- `mvn package` 执行完毕后 在 `target` 路径下生成名为'项目名称+版本'的jar文件
- `mvn install` 将当前项目的jar文件安装至本地依赖库，可被其他项目调用

## 声明依赖
本例中的代码不依赖其他库，但是大部分应用程序依赖于外部库来处理常见或复杂的功能

如果你想打印时间，你可以使用java自带的日期和时间库，但你可以通过Joda Time库使用更多功能。
`src/main/java/hello/HelloWorld.java`
```java
package hello;

import org.joda.time.LocalTime;

public class HelloWorld {
    public static void main(String[] args) {
        LocalTime currentTime = new LocalTime();
        System.out.println("The current local time is: " + currentTime);
        Greeter greeter = new Greeter();
        System.out.println(greeter.sayHello());
    }
}
```
>如果此时使用 `mvn compile` 构建，将会失败，因为没有声明 `Joda Time` 依赖

在 `pom.xml ` 文件的 `<project>` 中添加
```xml
<dependencies>
        <dependency>
            <groupId>joda-time</groupId>
            <artifactId>joda-time</artifactId>
            <version>2.9.2</version>
        </dependency>
</dependencies>
```
- `</dependencies>` 包含所有依赖
- `<dependency>` 包含一个依赖
- `<groupId>` 依赖包所属的组织名称
- `<artifactId>` 依赖包名称
- `<version>` 依赖版本
- `<scope>` 可以指定两个值
  - `provided` 编译运行代码的依赖
  - `test` 编译测试代码的依赖


## 测试用例

首先声明依赖

```xml
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
    <scope>test</scope>
</dependency>
```

写一个测试用例

`src/test/java/hello/GreeterTest.java`
```java
package hello;

import static org.hamcrest.CoreMatchers.containsString;
import static org.junit.Assert.*;

import org.junit.Test;

public class GreeterTest {

    private Greeter greeter = new Greeter();

    @Test
    public void greeterSaysHello() {
        assertThat(greeter.sayHello(), containsString("Hello"));
    }

}
```
- 运行 `mvn test` 时，maven使用surefire插件运行单元测试，默认搜集 `src/test/java` 路径下所有 *Test文件编译运行

最后附完整的配置文件

`pom.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.springframework</groupId>
    <artifactId>gs-maven</artifactId>
    <packaging>jar</packaging>
    <version>0.1.0</version>

    <properties>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
    </properties>

    <dependencies>
        <!-- tag::joda[] -->
        <dependency>
            <groupId>joda-time</groupId>
            <artifactId>joda-time</artifactId>
            <version>2.9.2</version>
        </dependency>
        <!-- end::joda[] -->
        <!-- tag::junit[] -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
        <!-- end::junit[] -->
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>2.1</version>
                <executions>
                    <execution>
                        <phase>package</phase>
                        <goals>
                            <goal>shade</goal>
                        </goals>
                        <configuration>
                            <transformers>
                                <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                                    <mainClass>hello.HelloWorld</mainClass>
                                </transformer>
                            </transformers>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>

</project>
```