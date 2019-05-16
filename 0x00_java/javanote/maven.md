# Maven
。Maven是Java项目构建工具，可以用于管理Java依赖，还可以用于编译、打包以及发布Java项目

标准目录结构
目录|目的
--|--
${basedir}|存放pom.xml和所有的子目录
${basedir}/src/main/java|项目的java源代码
${basedir}/src/main/resources|项目的资源，比如说property文件，springmvc.xml
${basedir}/src/test/java|项目的测试类，比如说Junit代码
${basedir}/src/test/resources|测试用的资源
${basedir}/src/main/webapp/WEB-INF|web应用文件目录，web项目的信息，比如存放web.xml、本地图片、jsp视图页面
${basedir}/target|打包输出目录
${basedir}/target/classes|编译输出目录
${basedir}/target/test-classes|测试编译输出目录
Test.java|Maven只会自动运行符合该命名规则的测试类
~/.m2/repository|Maven默认的本地仓库目录位置


## 安装

>安装jdk
- Maven 3.3 要求 JDK 1.7 或以上
- Maven 3.2 要求 JDK 1.6 或以上
- Maven 3.0/3.1 要求 JDK 1.5 或以上

>Maven 下载

`http://maven.apache.org/download.cgi`下载二进制版(Binary)

>环境变量
```shell
# windows
# 新建系统变量MAVEN_HOME
# 设置解压路径E:\Maven\apache-maven-3.3.9

# linux
wget http://mirrors.hust.edu.cn/apache/maven/maven-3/3.3.9/binaries/apache-maven-3.3.9-bin.tar.gz
tar -xvf  apache-maven-3.3.9-bin.tar.gz
sudo mv -f apache-maven-3.3.9 /usr/local/
sudo vim /etc/profile
# 文件末尾添加
# export MAVEN_HOME=/usr/local/apache-maven-3.3.9
# export PATH=${PATH}:${MAVEN_HOME}/bin
source /etc/profile
# 查看安装成功
mvn -v
```


## 配置文件
Maven的配置文件为pom.xml
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" 
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
        <modelVersion>4.0.0</modelVersion>
        <!-- 坐标 -->
        <groupId>com.fundebug</groupId>
        <artifactId>hello</artifactId>
        <version>1.0</version>
        <!-- 依赖 -->
        <dependencies>
                <!-- https://mvnrepository.com/artifact/org.json/json -->
                <dependency>
                        <groupId>org.json</groupId>
                        <artifactId>json</artifactId>
                        <version>20180813</version>
                </dependency>
        </dependencies>
</project>
```
`<project></project>`为最外层的标签；`<modelVersion>4.0.0</modelVersion>`定义了所使用的POM版本。这2个标签基本上是不变的。

groupId、artifactId与version一起则定义了模块的坐标(Coordinates)，每个公共模块的坐标应该是唯一的：
- groupId：组织名称，通常是把域名反过来，例如com.fundebug
- artifactId：模块名称，例如fundebug-java
- version：模块版本，例如0.2.0

`<dependencies></dependencies>`定义了当前项目所依赖的模块。Maven可以根据`<dependency></dependency>`中定义的坐标，自动下载所依赖的模块

## 常用命令
- mvn compile 编译Java源代码 
- mvn package 打包Java项目
- mvn deploy 将Java项目发布到Maven仓库 
- mvn clean 删除构建目录，清理的过程中会删除target目录下编译的内容。

生命周期|阶段描述
--|--
mvn validate|验证项目是否正确，以及所有为了完整构建必要的信息是否可用
mvn generate-sources|生成所有需要包含在编译过程中的源代码
mvn process-sources|处理源代码，比如过滤一些值
mvn generate-resources|生成所有需要包含在打包过程中的资源文件
mvn process-resources|复制并处理资源文件至目标目录，准备打包
mvn compile|编译项目的源代码
mvn process-classes|后处理编译生成的文件，例如对Java类进行字节码增强（bytecode enhancement）
mvn generate-test-sources|生成所有包含在测试编译过程中的测试源码
mvn process-test-sources|处理测试源码，比如过滤一些值
mvn generate-test-resources|生成测试需要的资源文件
mvn process-test-resources|复制并处理测试资源文件至测试目标目录
mvn test-compile|编译测试源码至测试目标目录
mvn test|使用合适的单元测试框架运行测试。这些测试应该不需要代码被打包或发布
mvn prepare-package|在真正的打包之前，执行一些准备打包必要的操作。这通常会产生一个包的展开的处理过的版本（将会在Maven 2.1+中实现）
mvn package|将编译好的代码打包成可分发的格式，如JAR，WAR，或者EAR
mvn pre-integration-test|执行一些在集成测试运行之前需要的动作。如建立集成测试需要的环境
mvn integration-test|如果有必要的话，处理包并发布至集成测试可以运行的环境
mvn post-integration-test|执行一些在集成测试运行之后需要的动作。如清理集成测试环境。
mvn verify|执行所有检查，验证包是有效的，符合质量规范
mvn install|安装包至本地仓库，以备本地的其它项目作为依赖使用
mvn deploy|复制最终的包至远程仓库，共享给其它开发人员和项目（通常和一次正式的发布相关）
### 创建项目
Maven 使用 archetype(原型) 来创建自定义的项目结构，形成 Maven 项目模板。
```shell
mvn archetype:generate

# 按下 Enter 选择默认选项 (6:maven-archetype-quickstart:1.1)

# Maven 将询问项目细节。按要求输入项目细节。
# 如果要使用默认值则直接按 Enter 键。你也可以输入自己的值。
# Maven 将要求确认项目细节，按 Enter 或按 Y
```
Maven 为项目自动生成 pom.xml文件, App.java,AppTest.java

### 编译(构建)
Eclipse中构建方式:在Elipse项目上右击 -> Run As 就能看到很多Maven操作。这些操作和maven命令是等效的
Maven命令构建方式:进入工程所在目录，输入maven命令就可以了。`mvn clean compile`

### 打包
项目打包有三种打包方式，pom打包，jar包和war包。打包方式在pom.xml文件中进行指定
- pom工程一般是聚合工程，代表父工程，负责管理jar包的版本、maven插件的版本等，主要做统一的依赖管理。
- jar包就是普通的打包方式，可以是pom工程的子工程。
- war包的都是web工程，是可以直接放到tomcat下运行的工程。
- 执行mvn package命令，即可将源码打包为.jar文件
- mvn package执行之后，项目中会新增一个tartget目录，编译的字节码文件位于target/classes目录，而jar包位于target/hello-1.0.jar
- 如果在pom.xml文件中指定打包方式,会默认打成war
    ```xml
    <artifactId>test</artifactId>
    <name>test</name>
    <packaging>war</packaging> 
    ```
    
- jar：即Java Archive，Java的包，Java编译好之后生成class文件，但如果直接发布这些class文件的话会很不方便，所以就把许多的class文件打包成一个jar，jar中除了class文件还可以包括一些资源和配置文件，通常一个jar包就是一个java程序或者一个java库。
- war：Web application Archive，与jar基本相同，但它通常表示这是一个Java的Web应用程序的包，tomcat这种Servlet容器会认出war包并自动部署。


### 运行
打包好的jar包，可以直接使用java命令运行，注意需要指定所依赖的jar包。
```shell
java -cp target/hello-1.0.jar:$HOME/.m2/repository/org/json/json/20180813/json-20180813.jar com.fundebug.Hello
{
    "name": "Fundebug",
    "url": "https://www.fundebug.com"
}
```
也可以使用mvn exec:java命令执行，不需要指定依赖的jar包，更加方便
```shell
mvn exec:java -Dexec.mainClass="com.fundebug.Hello"
```

### 添加外部依赖jar包
项目中如果不使用maven的话，项目中用到的jar包需要自己下载，然后放到项目的lib目录，比较麻烦

在Maven工程中添加依赖jar包，只要在POM文件中引入对应的`<dependency>`标签即可，编译时Maven会自动从设置的源地址下载jar包。
```xml
<dependencies>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>${junit.version}</version>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>1.2.12</version>
        <scope>compile</scope>
    </dependency>
</dependencies>
```
`<dependency>`标签最常用的四个属性标签：
- groupId：项目组织唯一的标识符，实际对应JAVA的包的结构。
- artifactId：项目唯一的标识符，实际对应项目的名称，就是项目根目录的名称。
- version：jar包的版本号。可以直接填版本数字，也可以在properties标签中设置属性值。
- scope：jar包的作用范围。可以填写compile、runtime、test、system和provided。用来在编译、测试等场景下选择对应的classpath。

### 查询jar包
可以在`http://mvnrepository.com/`站点搜寻你想要的jar包版本

例如，想要使用log4j，可以找到需要的版本号，然后拷贝对应的maven标签信息，将其添加到pom .xml文件中。
```xml
<!-- https://mvnrepository.com/artifact/org.apache.logging.log4j/log4j-core -->
<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-core</artifactId>
    <version>2.11.2</version>
</dependency>

```

### 项目文档
```shell
# 进入项目文件夹
mvn site
# 打开target\site 文件夹。点击 index.html 就可以看到文档了
```


### Maven插件(Plugin)
要添加Maven插件，可以在pom.xml文件中添加`<plugin>`标签。
```xml
<build>
  <plugins>
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-compiler-plugin</artifactId>
      <version>3.3</version>
      <configuration>
        <source>1.7</source>
        <target>1.7</target>
      </configuration>
    </plugin>
  </plugins>
</build>
```
`<configuration>`标签用来配置插件的一些使用参数。

常用Maven插件
- maven-antrun-plugin 能让用户在Maven项目中运行Ant任务
- maven-archetype-plugin 生成一个很简单的项目骨架
- maven-assembly-plugin 制作项目分发包，该分发包可能包含了项目的可执行文件、源代码、readme、平台脚本等等。
- maven-dependency-plugin 最大的用途是帮助分析项目依赖
- maven-enforcer-plugin 允许你创建一系列规则强制大家遵守，包括设定Java版本、设定Maven版本、禁止某些依赖、禁止 SNAPSHOT依赖。
- maven-help-plugin 辅助工具 打印所有可用的环境变量和Java系统属性,打印项目的有效POM和有效settings
- maven-release-plugin 帮助自动化项目版本发布
- maven-resources-plugin 则用来处理资源文件
- maven-surefire-plugin
- build-helper-maven-plugin
- exec-maven-plugin
- jetty-maven-plugin 周期性地检查源文件，一旦发现变更后自动更新到内置的Jetty Web容器中,帮助开发者节省时间
- versions-maven-plugin 帮助你管理Maven项目的各种版本信息

## 使用国内镜像
maven最大的作用就是用于对项目中jar包依赖的统一管理。默认的远程仓库地址是国外的镜像，下载jar包的话比较慢，可以使用国内镜像提高下载效率。

在解压的文件夹中的conf目录下的settings.xml文件夹下就可以配置maven远程仓库和本地仓库的地址。配置了远程仓库的地址之后就可以从远程仓库下载jar包到本地仓库了。
```xml
//国内镜像
<mirror>
<id>CN</id>
<name>OSChina Central</name>
<url>http://maven.aliyun.com/nexus/content/groups/public/</url>
<mirrorOf>central</mirrorOf>
</mirror>

<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
  <!-- localRepository
   | The path to the local repository maven will use to store artifacts.
   |
   | Default: ${user.home}/.m2/repository
  <localRepository>/path/to/local/repo</localRepository>
  -->
```
默认的本地仓库地址是${user.home}/.m2/repository，如果是mac电脑的话默认地址就是/Users/本机用户名/.m2。也可以修改本地仓库地址为其他的地址。

## idea集成maven
以Intellij IDEA为例
- 首先新建项目的时候要构建成maven项目。如果是导入项目导入的类型也可以选择是maven项目，或者先倒入，等其他的都配置好了再把项目转成maven项目。
- 点击Build，Execution，Deployment中的maven，就可以对项目中使用到的maven进行配置。其中主要有三项需要配置:
  - Maven home direcroty：地址是下载的解压之后的maven压缩包。
  - User settings file:setting.xml所在的位置，通常是上面的Maven home direcroty的子目录。
  - Local repository:本地仓库的地址。





[参考链接](https://www.cnblogs.com/xdp-gacl/p/4240930.html)
[参考链接](https://www.cnblogs.com/jingmoxukong/p/5591368.html)