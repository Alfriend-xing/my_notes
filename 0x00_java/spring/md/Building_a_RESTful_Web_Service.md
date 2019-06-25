# 创建RESTful风格的接口

## 最终效果
- 访问 `http://localhost:8080/greeting`
- 返回json格式的数据 `{"id":1,"content":"Hello, World!"}`
- 添加参数 `http://localhost:8080/greeting?name=User`
- 返回 `{"id":1,"content":"Hello, User!"}`

## 项目路径
`mkdir -p src/main/java/hello`

## 配置文件

`pom.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.springframework</groupId>
    <artifactId>gs-rest-service</artifactId>
    <version>0.1.0</version>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.6.RELEASE</version>
    </parent>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>com.jayway.jsonpath</groupId>
            <artifactId>json-path</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <properties>
        <java.version>1.8</java.version>
    </properties>


    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>
```
> `Spring Boot Maven plugin` 插件提供了许多方便的功能
> - 收集class路径下所有jar包，构建为一个jar包
> - 搜索 `public static void main()` 方法，标记为可运行类
> - 内置依赖解析器，设置版本号，匹配依赖

## 创建资源表示类
包含id和content变量，构造方法，访问方法

`src/main/java/hello/Greeting.java`

```java
package hello;

public class Greeting {

    private final long id;
    private final String content;

    public Greeting(long id, String content) {
        this.id = id;
        this.content = content;
    }

    public long getId() {
        return id;
    }

    public String getContent() {
        return content;
    }
}
```

## 资源控制器
控制器(controller)用于处理http请求，这个组件使用 `@RestController` 注解标志。

下面的 `GreetingController` 方法用于处理 `/greeting` 路径的get请求，处理方式为返回 `Greeting` 类型的实例。

`src/main/java/hello/GreetingController.java`
```java
package hello;

import java.util.concurrent.atomic.AtomicLong;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class GreetingController {

    private static final String template = "Hello, %s!";
    private final AtomicLong counter = new AtomicLong();

    @RequestMapping("/greeting")
    public Greeting greeting(@RequestParam(value="name", defaultValue="World") String name) {
        return new Greeting(counter.incrementAndGet(),
                            String.format(template, name));
    }
}
```
-  `@RestController` 表示该类是一个控制器
-  ` @RequestMapping` 表示将 `/greeting` 地址的http请求映射到 `greeting()` 方法，可以使用 `@RequestMapping(method=GET)` 来限制请求的方法
-  `@RequestParam` 注解将请求中的 `name` 参数绑定到 `greeting()` 方法中的 `name` 参数，如果请求中没有附加 `name` 参数，则使用默认值(defaultValue)  `World` 。
-  返回的 `Greeting` 实例需要传入 `counter` 生成的id和 `template` 格式的字符串
-  `MVC` 控制器和 `RESTful` 控制器之间一个关键不同是返回数据的生成方法。前者在服务端将数据渲染为html代码，后者返回一个 `Greeting` 对象，对象的数据将作为json直接写入http响应中，spring会自动将对象转为json格式。

## 启动
传统方法是打包为war包部署至外部服务器，简单的方法是创建一个jar包独立运行。

要独立运行，需要写一个main方法，这样就可以使用spring内嵌的tomcat servlet容器

`src/main/java/hello/Application.java`
```java
package hello;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```
- `@SpringBootApplication` 注解实现类以下功能
  - `@Configuration` 为应用上下文标记该类为bean
  - `@EnableAutoConfiguration` 
  - 识别 classpath 中的注解，将应用标记为web应用
  - `@ComponentScan` 查找同一个包中的组件、配置、服务、控制器
- `main()` 方法中调用了 `SpringApplication.run()` 方法来启动应用，无需其他xml配置。

使用Maven在命令行中启动，或者将所有用到的资源打包为可执行jar包

- 直接运行 `./mvnw spring-boot:run` 
- 打包 `./mvnw clean package`
- 运行jar包 `java -jar target/gs-rest-service-0.1.0.jar`