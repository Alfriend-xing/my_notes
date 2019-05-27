# Java API

### 典型的应用目录结构
```shell
/my_application
  /bin
  /devlib
  /lib                <-- MyBatis *.jar 文件在这里。
  /src
    /org/myapp/
      /action
      /data           <-- MyBatis 配置文件在这里, 包括映射器类, XML 配置, XML 映射文件。
        /mybatis-config.xml
        /BlogMapper.java
        /BlogMapper.xml
      /model
      /service
      /view
    /properties       <-- 在你 XML 中配置的属性文件在这里。
  /test
    /org/myapp/
      /action
      /data
      /model
      /service
      /view
    /properties
  /web
    /WEB-INF
      /web.xml
```
### SqlSessions

#### SqlSessionFactoryBuilder
```java
// SqlSessionFactoryBuilder 有五个 build() 方法，
// 每一种都允许你从不同的资源中创建一个 SqlSession 实例。
SqlSessionFactory build(InputStream inputStream)
SqlSessionFactory build(InputStream inputStream, String environment)
SqlSessionFactory build(InputStream inputStream, Properties properties)
SqlSessionFactory build(InputStream inputStream, String env, Properties props)
SqlSessionFactory build(Configuration config)
```
#### SqlSessionFactory
```java
// SqlSessionFactory 有六个方法创建 SqlSession 实例。
SqlSession openSession()
SqlSession openSession(boolean autoCommit)
SqlSession openSession(Connection connection)
SqlSession openSession(TransactionIsolationLevel level)
SqlSession openSession(ExecutorType execType,TransactionIsolationLevel level)
SqlSession openSession(ExecutorType execType)
SqlSession openSession(ExecutorType execType, boolean autoCommit)
SqlSession openSession(ExecutorType execType, Connection connection)
Configuration getConfiguration();
```
#### SqlSession
SqlSession 实例在 MyBatis 中是非常强大的一个类。在这里你会看到所有执行语句、提交或回滚事务和获取映射器实例的方法。

在 SqlSession 类中有超过 20 个方法，所以将它们组合成易于理解的分组。

```java
// 执行语句方法
<T> T selectOne(String statement, Object parameter)
<E> List<E> selectList(String statement, Object parameter)
<T> Cursor<T> selectCursor(String statement, Object parameter)
<K,V> Map<K,V> selectMap(String statement, Object parameter, String mapKey)
int insert(String statement, Object parameter)
int update(String statement, Object parameter)
int delete(String statement, Object parameter)
```
```java
// 批量立即更新方法
List<BatchResult> flushStatements()
```
```java
// 事务控制方法
void commit()
void commit(boolean force)
void rollback()
void rollback(boolean force)
```
```java
// 本地缓存
void clearCache()
```
```java
// 确保 SqlSession 被关闭
void close()
```
```java
// 使用映射器
<T> T getMapper(Class<T> type)
```






















