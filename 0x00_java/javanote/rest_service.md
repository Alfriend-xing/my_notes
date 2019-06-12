# Building a RESTful Web Service

本篇介绍RESTful风格API的开发
- 向`http://localhost:8080/greeting`发送get请求
- 返回json数据`{"id":1,"content":"Hello, World!"}`
- 发送`http://localhost:8080/greeting?name=User`
- 返回`{"id":1,"content":"Hello, User!"}`

## 项目路径
`mkdir -p src/main/java/hello`
```shell
└── src
    └── main
        └── java
            └── hello
```
## maven配置文件
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
        <version>2.1.4.RELEASE</version>
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

## 类文件
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
其中
- `@RequestMapping`注解：用于确保发往`/greeting`的http请求被映射到`greeting()`方法，但不鉴别请求类型`(GET PUT POST)`,限制请求方法使用 `@RequestMapping(method=GET)`
- `@RequestParam`注解：将http请求中的参数`name`与`greeting()`方法中的形参`name`绑定，如果请求中不包含`name`参数，则将默认值'World'传入方法
- 方法主体创建并返回了一个`Greeting`对象，其中id值为`counter`的下一个值，content值为使用`template`格式化后的`name`
- 传统MVC控制器和上面的RESTful Web服务控制器之间的关键区别在于创建HTTP响应主体的方式。RESTful Web服务控制器只是填充并返回一个Greeting对象，而不是依靠视图技术将Greeting对象渲染为HTML。 对象数据将作为JSON直接写入HTTP响应。
- `spring`的`MappingJackson2HttpMessageConverter`类会自动将`Greeting`对象转为json格式

## 启动服务
此时已经可以将代码打包为WAR文件，并部署至外部应用服务器了，但你可以使用`spring`内置的`Tomcat servlet`容器，以`main()`方式启动。

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
其中
- `@SpringBootApplication`注释包含以下注释
  - `@Configuration`标记该类作为应用程序上下文的bean定义的来源。
  - `@EnableAutoConfiguration`通知`Spring Boot`根据各种设置添加`beans`
  - `@EnableWebMvc`
  - `@ComponentScan`告诉`Spring`在`hello`包中寻找其他组件，配置和服务，允许它找到控制器。
- `main()`方法中调用`SpringApplication.run()`方法启动应用。这个应用只包含 `java` 代码，没有其他`xml`文件和`web.xml`配置

## 生成JAR包
你可以使用`Maven`在命令行中启动应用。也可以构建单独的可执行JAR文件，它包含所有可能用到的代码和资源。JAR文件使开发过程中跨平台发布项目，部署项目，控制版本都变得十分容易

使用`Maven`运行
```shell
./mvnw spring-boot:run
```
构建JAR文件
```shell
./mvnw clean package
```
运行JAR文件
```shell
java -jar target/gs-rest-service-0.1.0.jar
```











