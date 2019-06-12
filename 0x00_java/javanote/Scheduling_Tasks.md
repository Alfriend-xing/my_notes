# Scheduling Tasks
使用`Spring`的`@Scheduled`注解执行计划任务。此技术适用于任何类型的应用程序。

## 项目路径和配置文件
`mkdir -p src/main/java/hello`

`pom.xml`
```java
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.springframework</groupId>
    <artifactId>gs-scheduling-tasks</artifactId>
    <version>0.1.0</version>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.4.RELEASE</version>
    </parent>

    <properties>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>
    </dependencies>

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
其中
- `Spring Boot Maven plugin`插件用于
  - 将classpath中所有jar文件整合为一个jar文件
  - 查找`public static void main()`方法并标记可执行类
  - 解析springboot版本，匹配依赖

## 创建计划任务
`src/main/java/hello/ScheduledTasks.java`
```java
package hello;

import java.text.SimpleDateFormat;
import java.util.Date;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.scheduling.annotation.Scheduled;
import org.springframework.stereotype.Component;

@Component
public class ScheduledTasks {

    private static final Logger log = LoggerFactory.getLogger(ScheduledTasks.class);

    private static final SimpleDateFormat dateFormat = new SimpleDateFormat("HH:mm:ss");

    @Scheduled(fixedRate = 5000)
    public void reportCurrentTime() {
        log.info("The time is now {}", dateFormat.format(new Date()));
    }
}
```
其中
- `Scheduled`注解规定了指定的方法合适执行，本例使用`fixedRate`，其他参数查看文档


## 启用计划
尽管计划任务可嵌入到web应用中，更简单的方法是创建jar文件使用main()方法启动
`src/main/java/hello/Application.java`
```java
package hello;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.scheduling.annotation.EnableScheduling;

@SpringBootApplication
@EnableScheduling
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class);
    }
}
```
其中
- `@SpringBootApplication`注解和之前的作用一样
- `@EnableScheduling`创建后台任务执行程序。不使用该注解则无法执行任何计划任务

## 创建JAR文件
`./mvnw spring-boot:run`
`./mvnw clean package`
`java -jar target/gs-scheduling-tasks-0.1.0.jar`


## 效果
启动后台线程，每5秒使用log打印当前时间



















