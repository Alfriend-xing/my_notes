# 连接池
传统JDBC的缺点
- 用户每次请求都需要向数据库获得链接，而数据库创建连接通常需要消耗相对较大的资源，创建时间也较长。
- 假设网站一天10万访问量，数据库服务器就需要创建10万次连接，极大的浪费数据库的资源，并且极易造成数据库服务器内存溢出、拓机。

连接池原理
- 在服务器端一次性创建多个连接，将多个连接保存在一个连接池对象中，当应用程序的请求需要操作数据库时，不会为请求创建新的连接，而是直接从连接池中获得一个连接，操作数据库结束之后，并不需要真正关闭连接，而是将连接放回到连接池中。
- 节省创建连接、释放连接 资源

## 一.自定义连接池
编写连接池需实现javax.sql.DataSource接口。DataSource接口中定义了两个重载的getConnection方法：
```java
Connection.getConnection() 
Connection.getConnection(String username, String password)
```
自定义一个类，实现`DataSource`接口，并实现连接池功能的步骤：
- 在自定义类的构造函数中批量创建`Connection`，并把创建的连接保存到一个集合对象中`(LinkedList)`。
- 在自定义类中实现`Connection.getConnection`方法，让 `getConnection` 方法每次调用时，从集合对象中取出一个 `Connection` 返回给用户。
- 当用户使用完`Connection`，不能调用`Connection.close()`方法，而要使用连接池提供关闭方法，即将`Connection`放回到连接池之中(把`Connection`存入集合对象中)。`Connection`对象应保证将自己返回到连接池的集合对象中，而不要把`Connection`还给数据库。
- 如果用户习惯调用`Connection.close()`方法，则可以使用动态代理来增强原有方法。

demo
```java
public class MyDataSource implements DataSource {

    // 链表 --- 实现 栈结构 、队列 结构
    private LinkedList<Connection> dataSources = new LinkedList<Connection>();

    public MyDataSource() {
        // 一次性创建10个连接
        for (int i = 0; i < 10; i++) {
            try {
                Connection conn = JDBCUtils.getConnection();
                // 将连接加入连接池中
                dataSources.add(conn);
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }

    @Override
    public Connection getConnection() throws SQLException {
        // 取出连接池中一个连接
        final Connection conn = dataSources.removeFirst(); // 删除第一个连接返回
        System.out.println("取出一个连接剩余 " + dataSources.size() + "个连接！");
        // 将目标Connection对象进行增强
        Connection connProxy = (Connection) Proxy.newProxyInstance(conn
                .getClass().getClassLoader(), conn.getClass().getInterfaces(),
                new InvocationHandler() {
                    // 执行代理对象任何方法 都将执行 invoke
                    @Override
                    public Object invoke(Object proxy, Method method,
                            Object[] args) throws Throwable {
                        if (method.getName().equals("close")) {
                            // 需要加强的方法
                            // 不将连接真正关闭，将连接放回连接池
                            releaseConnection(conn);
                            return null;
                        } else {
                            // 不需要加强的方法
                            return method.invoke(conn, args); // 调用真实对象方法
                        }
                    }
                });
        return connProxy;
    }

    // 将连接放回连接池
    public void releaseConnection(Connection conn) {
        dataSources.add(conn);
        System.out.println("将连接 放回到连接池中 数量:" + dataSources.size());
    }

    @Override
    public Connection getConnection(String username, String password)
            throws SQLException {
        return null;
    }

    @Override
    public PrintWriter getLogWriter() throws SQLException {
        // TODO Auto-generated method stub
        return null;
    }

    @Override
    public int getLoginTimeout() throws SQLException {
        // TODO Auto-generated method stub
        return 0;
    }

    @Override
    public void setLogWriter(PrintWriter out) throws SQLException {
        // TODO Auto-generated method stub

    }

    @Override
    public void setLoginTimeout(int seconds) throws SQLException {
        // TODO Auto-generated method stub

    }

    @Override
    public boolean isWrapperFor(Class<?> iface) throws SQLException {
        // TODO Auto-generated method stub
        return false;
    }

    @Override
    public <T> T unwrap(Class<T> iface) throws SQLException {
        // TODO Auto-generated method stub
        return null;
    }

}
```
## 二.开源数据库连接池
- 现在很多WEB服务器(`Weblogic`, `WebSphere`, `Tomcat`)都提供了`DataSoruce`的实现，即连接池的实现。通常我们把`DataSource`的实现，按其英文含义称之为数据源，数据源中都包含了数据库连接池的实现。
- 实际应用时不需要编写连接数据库代码，直接从数据源获得数据库的连接。程序员编程时也应尽量使用这些数据源的实现，以提升程序的数据库访问性能。
- 原来由`jdbcUtil`创建连接，现在由`dataSource`创建连接，为实现不和具体数据绑定，因此`datasource`也应采用配置文件的方法获得连接。
- 在`Apache`官网下载时，注意这些开源连接池的版本，要与本地`JDK`与`JDBC`版本对应。比如说：`Apache Commons DBCP 2.1.1 for JDBC 4.1 (Java 7.0+)`,下载低版本的两个jar包即可

## 三.DBCP连接池
`DBCP` 是 `Apache` 软件基金组织下的开源连接池实现，使用`DBCP`连接池，需要在`build path`中增加如下两个 `jar` 文件：
- `Commons-dbcp.jar`：连接池的实现
- `Commons-pool.jar`：连接池实现的依赖库

`Tomcat` 的连接池正是采用该连接池来实现的。该数据库连接池既可以与应用服务器整合使用，也可由应用程序独立使用。

#### 配置方法
使用DBCP连接池，需要有`driverclass`、`url`、`username`、`password`，有两种方法配置
1使用`BasicDataSource.setXXX(XXX)`方法手动设置四个参数
```java
@Test
public void demo1() throws SQLException {
    // 使用BasicDataSource 创建连接池
    BasicDataSource basicDataSource = new BasicDataSource();
    // 创建连接池 一次性创建多个连接池

    // 连接池 创建连接 ---需要四个参数
    basicDataSource.setDriverClassName("com.mysql.jdbc.Driver");
    basicDataSource.setUrl("jdbc:mysql:///day14");
    basicDataSource.setUsername("root");
    basicDataSource.setPassword("123");

    // 从连接池中获取连接
    Connection conn = basicDataSource.getConnection();
    String sql = "select * from account";
    PreparedStatement stmt = conn.prepareStatement(sql);
    ResultSet rs = stmt.executeQuery();

    while (rs.next()) {
        System.out.println(rs.getString("name"));
    }

    JDBCUtils.release(rs, stmt, conn);
}
```
2编写`properties`配置文件，在测试代码中创建`Properties`对象加载文件
```java
// dbcp.properties文件
driverClassName=com.mysql.jdbc.Driver
url=jdbc:mysql:///day14
username=root
password=123
initsize=1
// 连接池启动时创建的初始化连接数量（默认值为0）
maxactive=1
// 连接池中可同时连接的最大的连接数,默认值为8
maxwait=5000
// 最大等待时间(毫秒数)，当没有可用连接时，连接池等待连接释放的最大时间，
// 超过该时间限制会抛出异常，如果设置-1表示无限等待,默认为无限
maxidle=1
// 连接池中最大的空闲的连接数，超过的空闲连接将被释放，
// 如果设置为负数表示不限制,默认为8
minidle=1
// 连接池中最小的空闲的连接数，低于这个数量会被创建新的连接,默认为0

// 测试代码
@Test
public void demo2() throws Exception {
    // 读取dbcp.properties ---- Properties
    Properties properties = new Properties();
    properties.load(new FileInputStream(this.getClass().getResource(
            "/dbcp.properties").getFile()));

    DataSource basicDataSource = BasicDataSourceFactory
            .createDataSource(properties);

    // 从连接池中获取连接
    Connection conn = basicDataSource.getConnection();
    String sql = "select * from account";
    PreparedStatement stmt = conn.prepareStatement(sql);
    ResultSet rs = stmt.executeQuery();

    while (rs.next()) {
        System.out.println(rs.getString("name"));
    }

    JDBCUtils.release(rs, stmt, conn);

}
```
## 四.C3P0连接池
使用`C3P0`连接池，需要在`build path`中添加一个`jar`包：`c3p0-版本号.jar`
`Basic Pool Configuration` 基本属性:
- `acquireIncrement`  当连接池连接用完了，根据该属性决定一次性新建多少连接
- `initialPoolSize`  初始化一次性创建多少个连接
- `maxPoolSize` 最大连接数
- `maxIdleTime` 最大空闲时间，当连接池中连接经过一段时间没有使用，根据该数据进行释放，默认0，单位秒
- `minPoolSize` 最小连接池尺寸
>当创建连接池时，一次性创建initialPoolSize 个连接，当连接使用完一次性创建 acquireIncrement  个连接，连接最大数量 maxPoolSize ，当连接池连接数量大于 minPoolSize ，经过maxIdleTime 连接没有使用， 该连接将被释放。

#### 配置方法
1手动配置(除了设置四个必须的参数，还可以设置 `Basic Pool Configuration` )
```java
@Test
public void demo1() throws Exception {
    // 创建一个连接池
    ComboPooledDataSource dataSource = new ComboPooledDataSource();
    // 手动设置四个参数
    dataSource.setDriverClass("com.mysql.jdbc.Driver");
    dataSource.setJdbcUrl("jdbc:mysql:///day14");
    dataSource.setUser("root");
    dataSource.setPassword("123");

    //dataSource.setMaxPoolSize(40); // 手动设置Basic Pool Configuration

    Connection conn = dataSource.getConnection();
    String sql = "select * from account";
    PreparedStatement stmt = conn.prepareStatement(sql);

    ResultSet rs = stmt.executeQuery();

    while (rs.next()) {
        System.out.println(rs.getString("name"));
    }

    JDBCUtils.release(rs, stmt, conn);
}
```
2使用`c3p0-config.xml`配置
```xml
<!-- c3p0-config.xml文件： -->
<?xml version="1.0" encoding="UTF-8"?>
  <c3p0-config>
      <default-config> <!-- 默认配置 -->
          <property name="driverClass">com.mysql.jdbc.Driver</property>
          <property name="jdbcUrl">jdbc:mysql:///day14</property>
          <property name="user">root</property>
          <property name="password">123</property>
          
          <property name="acquireIncrement">10</property>
          <property name="initialPoolSize">10</property>
          <property name="maxPoolSize">100</property>
          <property name="maxIdleTime">60</property>
          <property name="minPoolSize">5</property>
      </default-config>
      <named-config name="itcast">   <!-- 自定义配置 -->
          <property name="driverClass">com.mysql.jdbc.Driver</property>
          <property name="jdbcUrl">jdbc:mysql:///day14</property>
          <property name="user">root</property>
          <property name="password">123</property>
          
          <property name="acquireIncrement">10</property>
          <property name="initialPoolSize">10</property>
          <property name="maxPoolSize">100</property>
          <property name="maxIdleTime">60</property>
          <property name="minPoolSize">5</property>
      </named-config>
  </c3p0-config>

```
```java
// 测试代码：
@Test
public void demo2() throws SQLException {
    // 使用c3p0配置文件
    // 自动加载src/c3p0-config.xml，不需要像前文的dbcpconfig.properties那样加载配置文件！

    // ComboPooledDataSource dataSource = new ComboPooledDataSource(); // 使用默认配置
    ComboPooledDataSource dataSource = new ComboPooledDataSource("itcast"); // 使用自定义配置

    Connection conn = dataSource.getConnection();
    String sql = "select * from account";
    PreparedStatement stmt = conn.prepareStatement(sql);

    ResultSet rs = stmt.executeQuery();

    while (rs.next()) {
        System.out.println(rs.getString("name"));
    }

    JDBCUtils.release(rs, stmt, conn);
}
```
## 五.Tomcat内置连接池
- 因为`Tomcat`和 `dbcp` 都是`Apache`公司项目，`Tomcat`内部连接池就是dbcp。
- `Tomcat`支持`Servlet`、`JSP`等，类似于容器，但并不支持所有`JavaEE`规范，`JNDI`就是`JavaEE`规范之一。
- 开发者通过`JNDI`方式可以访问`Tomcat`内置连接池。

使用Tomcat内置连接池的前提
- 将web工程部署到Tomcat三种方式： 配置server.xml <Context> 元素、配置独立xml文件 <Context> 元素 、直接将网站目录复制Tomcat/webapps
虚拟目录 ---- <Context> 元素
- 若想使用Tomcat内置连接池，必须要在Context元素中添加Resource标签，具体代码如下：
```xml
<Context>
    <Resource name="jdbc/TestDB" auth="Container" type="javax.sql.DataSource"
                maxActive="100" maxIdle="30" maxWait="10000"
                username="root" password="123" driverClassName="com.mysql.jdbc.Driver"
                url="jdbc:mysql://localhost:3306/day14"/>
</Context>
```
- 有三个位置可以配置元素：
  1. tomcat安装目录/conf/context.xml 对当前Tomcat内部所有虚拟主机中任何工程都有效
  1. tomcat安装目录/conf/Catalina/虚拟主机目录/context.xml 对当前虚拟主机任何工程都有效
  1. 在web工程根目录/META-INF/context.xml 对当前工程有效 （常用！）

具体做法：MyEclipse中，在项目根目录的WebRoot/META-INF中，新建context.xml文件，配置代码如下：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- tomcat启动时，加载该配置文件，创建连接池，将连接池保存tomcat容器中 -->
<Context>
<Resource name="jdbc/TestDB" auth="Container" type="javax.sql.DataSource"
            maxActive="100" maxIdle="30" maxWait="10000"
            username="root" password="123" driverClassName="com.mysql.jdbc.Driver"
            url="jdbc:mysql://localhost:3306/day14"/>
</Context>
```


[参考链接](https://www.jianshu.com/p/ad0ff2961597)