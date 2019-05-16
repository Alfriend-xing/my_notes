# jsp
JSP通过在标准的HTML页面中插入Java代码，其静态的部分无须Java程序控制，只有那些需要从数据库读取并根据程序动态生成信息时，才使用Java脚本控制。每个JSP页面就是一个Servlet实例——JSP页面由系统编译成Servlet，Servlet再负责响应用户请求。

简单的JSP页面 test1.jsp
```html
<!-- 表明此为一个JSP页面 -->
<%@ page contentType="text/html; charset=gb2312" language="java" %>
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN">
<HTML>
    <HEAD>
          <TITLE>第一个JSP页面</TITLE>
    </HEAD>
    
    <BODY>
    <!-- 下面是Java脚本-->
        <%for(int i = 0 ; i < 10; i++)
          {
            out.println(i);
        %>
       <br>
        <%}
        %>
    </BODY>
</HTML>
```
启动Tomcat之后会生成test1_jsp.java和test1_jsp.class。

- JSP文件必须在JSP服务器内运行。
- JSP文件必须生成Servlet才能执行。
- 每个JSP页面的第一个访问者速度很慢，因为必须等待JSP编译成Servlet。
- JSP页面的访问者无须安装任何客户端，甚至不需要可以运行Java的运行环境，因为JSP页面输送到客户端的是标准HTML页面。

JSP和Servlet转换
- JSP页面的静态内容、JSP脚本都会转换成Servlet的xxxService()方法，类似于自行创建Servlet时service()方法。
- JSP声明部分，转换成Servlet的成员部分。所有JSP声明部分可以使用private,protected,public,static等修饰符，其他地方则不行。
- JSP的输出表达式(<%= ..%>部分)，输出表达式会转换成Servlet的xxxService()方法里的输出语句。
- 九个内置对象要么是xxxService()方法的形参，要么是该方法的局部变量，所以九个内置对象只能在JSP脚本和输出表达式中使用。

## Jsp和Servlet的区别
- Servlet在Java代码中通过HttpServletResponse对象动态输出HTML内容
- JSP在静态HTML内容中嵌入Java代码，Java代码被动态执行后生成HTML内容

jsp就是在html里面写java代码，servlet就是在java里面写html代码…其实jsp经过容器解释之后就是servlet.只是我们自己写代码的时候尽量能让它们各司其职，jsp更注重前端显示，servlet更注重模型和业务逻辑。不要写出万能的jsp或servlet来即可。


## 各自的特点
- Servlet能够很好地组织业务逻辑代码，但是在Java源文件中通过字符串拼接的方式生成动态HTML内容会导致代码维护困难、可读性差
- JSP虽然规避了Servlet在生成HTML内容方面的劣势，但是在HTML中混入大量、复杂的业务逻辑同样也是不可取的

既然JSP和Servlet都有自身的适用环境，那么能否扬长避短，让它们发挥各自的优势呢？答案是肯定的——MVC(Model-View-Controller)模式非常适合解决这一问题。

## jsp优缺点
优点:
- vs. Active Server Pages (ASP) - 因为JSP使用JAVA编写，最终会被Tomcat(Jetty)等容器转化为Servlet，由于JAVA的跨平台型，JSP可以被运行在几乎所有操作系统上，但是ASP只能运行在微软自家的平台
- vs. Pure(纯) Servlets相比 - 更加便捷，在Servlets中你需要拼凑HTML代码，极不方便，开发效率低，不容易调试，
- vs. JavaScript - 你无法在客服端与服务器进行更复杂的交互，比如你无法操作执行数据库操作，无法操作服务器文件系统
- vs. HTML - 无法生成动态网页

缺点
- 增大了服务器压力
- 前端后段开发未分离，影响开发效率
- 过度依赖于JAVA环境
- 复用率较低

## EL表达式和jstl技术
大量java代码夹杂在jsp页面中让页面显得十分臃肿，不利于维护，这样的代码严重背离了MVC的设计模式,与web各式框架的理念相悖，这就导致出现了EL表达式代替java获取页面参数，jstl提供代替java代码的逻辑语句(if、for循环等)。

### EL表达式
servlet
```java
package com.jstl;

import java.io.IOException;
import java.util.HashMap;
import java.util.Map;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class JstlELServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp)
            throws ServletException, IOException {
        //普通字符串
        req.setAttribute("hello", "Hello World");

        //结构
        Group group = new Group();//这个Group类你自己建就好了，里面只有一个私有的name属性，然后你给他生成get和set方法
        group.setName("组一");

        User user = new User();//这个User类也自己建吧，包括name，age，Group三个属性，还有各自的set和get方法
        user.setUsername("胡阳");
        user.setAge(23);
        user.setGroup(group);

        req.setAttribute("user", user);

        //map
        Map map = new HashMap();
        map.put("k1", "v1");
        map.put("k2", "v2");
        req.setAttribute("map", map);

        //字符串数组
        String[] strArray = new String[]{"a", "b", "c"};
        req.setAttribute("str_array", strArray);

        //对象数组
        User[] users = new User[9];
        for (int i = 0; i < users.length; i++) {
            users[i] = new User();
            users[i].setUsername("胡阳");
            users[i].setAge(23);
        }
        req.setAttribute("users", users);

        req.getRequestDispatcher("/jstl_el.jsp").forward(req, resp);
    }

}
```
web.xml文件
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:javaee="http://java.sun.com/xml/ns/javaee" xmlns:web="http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd">
  <servlet>
    <servlet-name>JstlELServlet</servlet-name>
    <servlet-class>com.jstl.JstlELServlet</servlet-class>
    <load-on-startup>5</load-on-startup>
  </servlet>
  <servlet-mapping>
    <servlet-name>JstlELServlet</servlet-name>
    <url-pattern>/servlet/JstlELServlet</url-pattern>
  </servlet-mapping>
</web-app>
```
两个jsp页面：index.jsp和jstl_el.jsp
```html
<%@ page language="java" contentType="text/html; charset=GB18030"
    pageEncoding="GB18030"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=GB18030">
<title>Insert title here</title>
</head>
<body>
<a href="servlet/JstlELServlet">测试EL</a>
</body>
</html>
```
```html
<%@ page language="java" contentType="text/html; charset=GB18030"
    pageEncoding="GB18030"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=GB18030">
<title>测试EL</title>
</head>
<body>
    <h1>测试EL表达式</h1>
    <hr>
    <li>普通字符串</li>
    hello(jsp脚本)：<%=request.getAttribute("hello") %><br>
    hello(el表达式，语法：$和{}):${hello}<br>
    hello(el表达式，el的内置对象pageScope,requestScope,sessionScope,applicationScope)<br>
    如果不指定范围，它的搜索顺序为pageScope~applicationScope):${requestScope.hello}
    hello(el表达式，指定范围从session取得):${sessionScope.hello }

    <p>
        <li>结构</li>
        姓名：${user.username }<br>
        年龄：${user.age }<br>
        所属组:${user.group.name }<br>
    </p>
    <li>map</li><br>
    map.k1:${map.k1 }<br>
    map.k2:${map.k2 }<br>

    <li>字符串数组</li>
    strArray[1]:${str_array[1] }<br>
    <p>
    <li>对象数组,采用[]下标</li><br>
    users[1].username:${users[1].username }<br>

    <p>
    <li>list</li>
    userList[1].username:${userList[1].username }<br>

    <p>
    <li>EL表达式对运算符的支持</li>
    1+1=${1+1 }<br>
    10/4=${10/4 }<br>
    10 div 5=${10 div 5 }<br>
    10 mod 3=${10 mod 3 }<br>

</body>
</html>
```

### JSTL的核心库
核心库中的表达式像是一个新的语言，包括基本的语句。

这里需要引入两个jar包，http://www.apache.org/dist/jakarta/taglibs/standard/binaries/，然后在使用JSTL核心库的JSP页面要声明一下，看下面具体的JSP代码


Servlet
```java
package com.jstl;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * 演示JSTL核心库
 * @author 胡阳
 *
 */
public class JstlCoreServlet extends HttpServlet {

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //普通字符串
        request.setAttribute("hello", "hello world");

        //HTML
        request.setAttribute("welcome", "<font color=red>the5fire的技术博客</font>");

        //条件控制
        request.setAttribute("v1", 10);
        request.setAttribute("v2", 20);

        //对象数组
        //对象数组
        User[] users = new User[9];
        for (int i = 0; i < users.length; i++) {
            users[i] = new User();
            users[i].setUsername("胡阳" + i);
            users[i].setAge(23 + i);
        }
        request.setAttribute("users", users);

        request.getRequestDispatcher("/jstl_core.jsp").forward(request, response);
    }


}
```
配置xml

JSP页面 jstl_core.jsp  第二行引入
```html
<%@ page language="java" contentType="text/html; charset=GB18030"
    pageEncoding="GB18030"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=GB18030">
<title>jstl核心库</title>
</head>
<body>
    <h1>jstl核心库</h1>
    <hr>
    <li>采用c:out标签</li>
    hello(使用标签):<c:out value="123"/><br>
    hello(使用标签):<c:out value="hello"/><br>
    hello(使用标签):<c:out value="${hello }"/><br>
    hello(使用EL表达式):${hello }<br>
    hello(default):${hello123 }<br>
    hello(使用缺省值):<c:out value="${hello123 }" default="没有纸"/><br>
    hello(使用缺省值):<c:out value="${hello123 }">没有纸</c:out><br>
    welcome(使用EL表达式):${welcome }<br>
    welcome(使用标签，escapeXml):<c:out value="${welcome }" escapeXml="true"/><br>
    welcome(使用标签，escapeXml):<c:out value="${welcome }" escapeXml="false"/><br>
    <p>
    <li>测试c:set,c:remove</li><br>
    <c:set value="root" var="userid"/>
    userid:${userid }<br>
    <c:remove var="userid"/>

    <p>
    <li>条件控制标签:c:if</li><br>
    <c:if test="${v1 lt v2 }">
        v1小于v2<br>
    </c:if>

    <p>
    <li>条件控制标签:c:choose,c:when,c:otherwise</li><br>
    <c:choose>
        <c:when test="${v1 gt v2 }">
            v1大于v2<br>
        </c:when>
    </c:choose>
    <c:choose>
        <c:when test="${empty userList }">
            没有符合条件的数据<br>
        </c:when>
        <c:otherwise>
            存在用户数据<br>
        </c:otherwise>
    </c:choose>
    <p>
    <li>演示循环控制标签:forEach</li><br>
    <h3>采用jsp脚本显示</h3>

        <table border="1">
            <tr>
                <td>用户名称</td>
                <td>年龄</td>
            </tr>
            <c:choose>
                <c:when test="${empty users }">
                    <tr>
                        <td colspan="3">没有符合条件的数据</td>
                    </tr>
                </c:when>
                <c:otherwise>   
                    <c:forEach items="${users }" var="user">
                        <tr>
                            <td>${user.username }</td>
                            <td>${user.age }</td>
                        </tr>
                    </c:forEach>

                </c:otherwise>
            </c:choose>
        </table>
    <h3>采用forEach标签，varstatus</h3>
    <table border="1">
            <tr>
                <td>用户名称</td>
                <td>年龄</td>
            </tr>
            <c:choose>
                <c:when test="${empty users }">
                    <tr>
                        <td colspan="3">没有符合条件的数据</td>
                    </tr>
                </c:when>
                <c:otherwise>   
                    <c:forEach items="${users }" var="user" varStatus="vs">
                        <c:choose>
                            <c:when test="${vs.count mod 2 == 0 }">
                                <tr bgcolor=red>
                            </c:when>
                            <c:otherwise>
                                <tr bgcolor=blue>
                            </c:otherwise>
                        </c:choose>
                            <td>${user.username }</td>
                            <td>${user.age }</td>
                        </tr>
                    </c:forEach>

                </c:otherwise>
            </c:choose>
        </table>
</body>
</html>
```

### JSTL中函数的简单使用
Servlet
```java
package com.jstl;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * 演示JSTL函数库的使用
 * @author 胡阳
 *
 */
public class JstlFnServlet extends HttpServlet {

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //简单测试字符串
        request.setAttribute("hello", "Hello-World");

        //页面转发
        request.getRequestDispatcher("/jstl_fn.jsp").forward(request, response);
    }

}
```
xml配置

建立jsp页面 jstl_fn.jsp
```html
<%@ page language="java" contentType="text/html; charset=GB18030"
    pageEncoding="GB18030"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/functions" prefix="fn" %> 
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>  
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=GB18030">
<title>Insert title here</title>
</head>
<body>
    <h2>测试JSTL方法：fn:contains这个方法的使用</h2>
    <li>判断返回字符串中是否存在World这个词：</li>
    <c:if test="${fn:contains(hello, 'World')}">
        存在<br>
    </c:if>

    <li>判断返回字符串中是否存在nihao这个词：</li>
    <c:if test="${fn:contains(hello, 'nihao')}">
        存在“nihao”<br>
    </c:if>
</body>
</html>
```





