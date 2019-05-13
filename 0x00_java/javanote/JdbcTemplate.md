# JdbcTemplate
`JDBC Template`是`Spring`中关于`JDBC`的一个辅助类，它封装了`JDBC`的操作，使用起来非常方便。
## 手动配置
```java
package com.lcw.spring.jdbc;

import org.junit.Test;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.datasource.DriverManagerDataSource;

public class JDBCTemplate {
    
    @Test
    public void demo(){
        DriverManagerDataSource dataSource=new DriverManagerDataSource();
        dataSource.setDriverClassName("com.mysql.jdbc.Driver");
        dataSource.setUrl("jdbc:mysql:///spring");
        dataSource.setUsername("root");
        dataSource.setPassword("");
        
        JdbcTemplate jdbcTemplate=new JdbcTemplate(dataSource);
        jdbcTemplate.execute("create table temp(id int primary key,name varchar(32))");
    
    }

}
```
## 文件配置
```xml
<!-- appliactionContext.xml -->

<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    
    <!--数据源的配置 -->
    <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
        <property name="url" value="jdbc:mysql:///spring"></property>
        <property name="username" value="root"></property>
        <property name="password" value=""></property>
    </bean>
    
    
　　
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
        <property name="dataSource" ref="dataSource"></property>
    </bean>
    
    
    <bean id="userDao" class="com.curd.spring.impl.UserDAOImpl">
        <property name="jdbcTemplate" ref="jdbcTemplate"></property>
    </bean>



</beans>
```
#### 接口
```java
// 接口：IUserDAO.java
package com.curd.spring.dao;

import java.util.List;

import com.curd.spring.vo.User;

public interface IUserDAO {

    public void addUser(User user);

    public void deleteUser(int id);

    public void updateUser(User user);

    public String searchUserName(int id);
    
    public User searchUser(int id);
    
    public List<User> findAll();

}
```
JdbcTemplate主要提供下列方法：
- execute方法：可以用于执行任何SQL语句，一般用于执行DDL语句；
- update方法及batchUpdate方法：update方法用于执行新增、修改、删除等语句；batchUpdate方法用于执行批处理相关语句；
- query方法及queryForXXX方法：用于执行查询相关语句；
- call方法：用于执行存储过程、函数相关语句。
#### 接口实现类
```java
// 接口实现类：UserDAOImpl.java
package com.curd.spring.impl;

import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.List;

import org.springframework.jdbc.core.RowMapper;
import org.springframework.jdbc.core.support.JdbcDaoSupport;
import com.curd.spring.dao.IUserDAO;
import com.curd.spring.vo.User;

public class UserDAOImpl extends JdbcDaoSupport implements IUserDAO {

    public void addUser(User user) {
        String sql = "insert into user values(?,?,?)";
        this.getJdbcTemplate().update(sql, user.getId(), user.getUsername(),
                user.getPassword());
    }

    public void deleteUser(int id) {
        String sql = "delete from user where id=?";
        this.getJdbcTemplate().update(sql, id);

    }

    public void updateUser(User user) {
        String sql = "update user set username=?,password=? where id=?";
        this.getJdbcTemplate().update(sql, user.getUsername(),
                user.getPassword(), user.getId());
    }

    public String searchUserName(int id) {// 简单查询，按照ID查询，返回字符串
        String sql = "select username from user where id=?";
        // 返回类型为String(String.class)
        return this.getJdbcTemplate().queryForObject(sql, String.class, id);

    }

    public List<User> findAll() {// 复杂查询返回List集合
        String sql = "select * from user";
        return this.getJdbcTemplate().query(sql, new UserRowMapper());

    }

    public User searchUser(int id) {
        String sql="select * from user where id=?";
        return this.getJdbcTemplate().queryForObject(sql, new UserRowMapper(), id);
    }

    class UserRowMapper implements RowMapper<User> {
　　　　　//rs为返回结果集，以每行为单位封装着
        public User mapRow(ResultSet rs, int rowNum) throws SQLException {
    
            User user = new User();
            user.setId(rs.getInt("id"));
            user.setUsername(rs.getString("username"));
            user.setPassword(rs.getString("password"));
            return user;
        }

    }

}
```
#### 测试类
```java
// 测试类：UserTest.java
package com.curd.spring.test;

import java.util.List;

import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import com.curd.spring.dao.IUserDAO;
import com.curd.spring.vo.User;

public class UserTest {
    
    @Test//增
    public void demo1(){
        User user=new User();
        user.setId(3);
        user.setUsername("admin");
        user.setPassword("123456");
        
        ApplicationContext applicationContext=new ClassPathXmlApplicationContext("applicationContext.xml");
        IUserDAO dao=(IUserDAO) applicationContext.getBean("userDao");
        dao.addUser(user);
        
    }
    
    @Test//改
    public void demo2(){
        User user=new User();
        user.setId(1);
        user.setUsername("admin");
        user.setPassword("admin");
        
        ApplicationContext applicationContext=new ClassPathXmlApplicationContext("applicationContext.xml");
        IUserDAO dao=(IUserDAO) applicationContext.getBean("userDao");
        dao.updateUser(user);
    }
    
    @Test//删
    public void demo3(){
        ApplicationContext applicationContext=new ClassPathXmlApplicationContext("applicationContext.xml");
        IUserDAO dao=(IUserDAO) applicationContext.getBean("userDao");
        dao.deleteUser(3);
    }
    
    @Test//查（简单查询，返回字符串）
    public void demo4(){
        ApplicationContext applicationContext=new ClassPathXmlApplicationContext("applicationContext.xml");
        IUserDAO dao=(IUserDAO) applicationContext.getBean("userDao");
        String name=dao.searchUserName(1);
        System.out.println(name);
    }
    
    @Test//查（简单查询，返回对象）
    public void demo5(){
        ApplicationContext applicationContext=new ClassPathXmlApplicationContext("applicationContext.xml");
        IUserDAO dao=(IUserDAO) applicationContext.getBean("userDao");
        User user=dao.searchUser(1);
        System.out.println(user.getUsername());
    }
    
    @Test//查（复杂查询，返回对象集合）
    public void demo6(){
        ApplicationContext applicationContext=new ClassPathXmlApplicationContext("applicationContext.xml");
        IUserDAO dao=(IUserDAO) applicationContext.getBean("userDao");
        List<User> users=dao.findAll();
        System.out.println(users.size());
    }
    
    

}
```

[参考链接](https://www.cnblogs.com/lichenwei/p/3902294.html)