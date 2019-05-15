## Servlet 客户端 HTTP 请求

### 常用头信息
头信息|描述
--|--
Accept|这个头信息指定浏览器或其他客户端可以处理的 MIME 类型。值 image/png 或 image/jpeg 是最常见的两种可能值。
Accept-Charset|这个头信息指定浏览器可以用来显示信息的字符集。例如 ISO-8859-1。
Accept-Encoding|这个头信息指定浏览器知道如何处理的编码类型。值 gzip 或 compress 是最常见的两种可能值。
Accept-Language|这个头信息指定客户端的首选语言，在这种情况下，Servlet 会产生多种语言的结果。例如，en、en-us、ru 等。
Authorization|这个头信息用于客户端在访问受密码保护的网页时识别自己的身份。
Connection|这个头信息指示客户端是否可以处理持久 HTTP 连接。持久连接允许客户端或其他浏览器通过单个请求来检索多个文件。值 Keep-Alive 意味着使用了持续连接。
Content-Length|这个头信息只适用于 POST 请求，并给出 POST 数据的大小（以字节为单位）。
Cookie|这个头信息把之前发送到浏览器的 cookies 返回到服务器。
Host|这个头信息指定原始的 URL 中的主机和端口。
If-Modified-Since|这个头信息表示只有当页面在指定的日期后已更改时，客户端想要的页面。如果没有新的结果可以使用，服务器会发送一个 304 代码，表示 Not Modified 头信息。
If-Unmodified-Since|这个头信息是 If-Modified-Since 的对立面，它指定只有当文档早于指定日期时，操作才会成功。
Referer|这个头信息指示所指向的 Web 页的 URL。例如，如果您在网页 1，点击一个链接到网页 2，当浏览器请求网页 2 时，网页 1 的 URL 就会包含在 Referer 头信息中。
User-Agent|这个头信息识别发出请求的浏览器或其他客户端，并可以向不同类型的浏览器返回不同的内容。

### 读取 HTTP 头的方法
下面的方法可用在 Servlet 程序中读取 HTTP 头。这些方法通过 HttpServletRequest 对象可用
```java
Cookie[] getCookies()
// 返回一个数组，包含客户端发送该请求的所有的 Cookie 对象。
Enumeration getAttributeNames()
// 返回一个枚举，包含提供给该请求可用的属性名称。
Enumeration getHeaderNames()
// 返回一个枚举，包含在该请求中包含的所有的头名。
Enumeration getParameterNames()
// 返回一个 String 对象的枚举，包含在该请求中包含的参数的名称。
HttpSession getSession()
// 返回与该请求关联的当前 session 会话，或者如果请求没有 session 会话，则创建一个。
HttpSession getSession(boolean create)
// 返回与该请求关联的当前 HttpSession，或者如果没有当前会话，且创建是真的，则返回一个新的 session 会话。
Locale getLocale()
// 基于 Accept-Language 头，返回客户端接受内容的首选的区域设置。
Object getAttribute(String name)
// 以对象形式返回已命名属性的值，如果没有给定名称的属性存在，则返回 null。
ServletInputStream getInputStream()
// 使用 ServletInputStream，以二进制数据形式检索请求的主体。
String getAuthType()
// 返回用于保护 Servlet 的身份验证方案的名称，例如，"BASIC" 或 "SSL"，如果JSP没有受到保护则返回 null。
String getCharacterEncoding()
// 返回请求主体中使用的字符编码的名称。
String getContentType()
// 返回请求主体的 MIME 类型，如果不知道类型则返回 null。
String getContextPath()
// 返回指示请求上下文的请求 URI 部分。
String getHeader(String name)
// 以字符串形式返回指定的请求头的值。
String getMethod()
// 返回请求的 HTTP 方法的名称，例如，GET、POST 或 PUT。
String getParameter(String name)
// 以字符串形式返回请求参数的值，或者如果参数不存在则返回 null。
String getPathInfo()
// 当请求发出时，返回与客户端发送的 URL 相关的任何额外的路径信息。
String getProtocol()
// 返回请求协议的名称和版本。
String getQueryString()
// 返回包含在路径后的请求 URL 中的查询字符串。
String getRemoteAddr()
// 返回发送请求的客户端的互联网协议（IP）地址。
String getRemoteHost()
// 返回发送请求的客户端的完全限定名称。
String getRemoteUser()
// 如果用户已通过身份验证，则返回发出请求的登录用户，或者如果用户未通过身份验证，则返回 null。
String getRequestURI()
// 从协议名称直到 HTTP 请求的第一行的查询字符串中，返回该请求的 URL 的一部分。
String getRequestedSessionId()
// 返回由客户端指定的 session 会话 ID。
String getServletPath()
// 返回调用 JSP 的请求的 URL 的一部分。
String[] getParameterValues(String name)
// 返回一个字符串对象的数组，包含所有给定的请求参数的值，如果参数不存在则返回 null。
boolean isSecure()
// 返回一个布尔值，指示请求是否使用安全通道，如 HTTPS。
int getContentLength()
// 以字节为单位返回请求主体的长度，并提供输入流，或者如果长度未知则返回 -1。
int getIntHeader(String name)
// 返回指定的请求头的值为一个 int 值。
int getServerPort()
// 返回接收到这个请求的端口号。
int getParameterMap()
// 将参数封装成 Map 类型。
```
### 实例
```java
//导入必需的 java 库
import java.io.IOException;
import java.io.PrintWriter;
import java.util.Enumeration;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/DisplayHeader")

//扩展 HttpServlet 类
public class DisplayHeader extends HttpServlet {

    // 处理 GET 方法请求的方法
    public void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException
    {
        // 设置响应内容类型
        response.setContentType("text/html;charset=UTF-8");

        PrintWriter out = response.getWriter();
        String title = "HTTP Header 请求实例 - 菜鸟教程实例";
        String docType =
            "<!DOCTYPE html> \n";
            out.println(docType +
            "<html>\n" +
            "<head><meta charset=\"utf-8\"><title>" + title + "</title></head>\n"+
            "<body bgcolor=\"#f0f0f0\">\n" +
            "<h1 align=\"center\">" + title + "</h1>\n" +
            "<table width=\"100%\" border=\"1\" align=\"center\">\n" +
            "<tr bgcolor=\"#949494\">\n" +
            "<th>Header 名称</th><th>Header 值</th>\n"+
            "</tr>\n");

        Enumeration headerNames = request.getHeaderNames();

        while(headerNames.hasMoreElements()) {
            String paramName = (String)headerNames.nextElement();
            out.print("<tr><td>" + paramName + "</td>\n");
            String paramValue = request.getHeader(paramName);
            out.println("<td> " + paramValue + "</td></tr>\n");
        }
        out.println("</table>\n</body></html>");
    }
    // 处理 POST 方法请求的方法
    public void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }
}
```
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<web-app>  
  <servlet>  
    <!-- 类名 -->  
    <servlet-name>DisplayHeader</servlet-name>  
    <!-- 所在的包 -->  
    <servlet-class>com.runoob.test.DisplayHeader</servlet-class>  
  </servlet>  
  <servlet-mapping>  
    <servlet-name>DisplayHeader</servlet-name>  
    <!-- 访问的网址 -->  
    <url-pattern>/TomcatTest/DisplayHeader</url-pattern>  
  </servlet-mapping>  
</web-app>
```
## Servlet 服务器 HTTP 响应
当一个 Web 服务器响应一个 HTTP 请求时，响应通常包括一个状态行、一些响应报头、一个空行和文档。

### 常用 HTTP 1.1 响应报头
头信息|描述
--|--
Allow|这个头信息指定服务器支持的请求方法（GET、POST 等）。
Cache-Control|这个头信息指定响应文档在何种情况下可以安全地缓存。可能的值有：public、private 或 no-cache 等。Public 意味着文档是可缓存，Private 意味着文档是单个用户私用文档，且只能存储在私有（非共享）缓存中，no-cache 意味着文档不应被缓存。
Connection|这个头信息指示浏览器是否使用持久 HTTP 连接。值 close 指示浏览器不使用持久 HTTP 连接，值 keep-alive 意味着使用持久连接。
Content-Disposition|这个头信息可以让您请求浏览器要求用户以给定名称的文件把响应保存到磁盘。
Content-Encoding|在传输过程中，这个头信息指定页面的编码方式。
Content-Language|这个头信息表示文档编写所使用的语言。例如，en、en-us、ru 等。
Content-Length|这个头信息指示响应中的字节数。只有当浏览器使用持久（keep-alive）HTTP 连接时才需要这些信息。
Content-Type|这个头信息提供了响应文档的 MIME（Multipurpose Internet Mail Extension）类型。
Expires|这个头信息指定内容过期的时间，在这之后内容不再被缓存。
Last-Modified|这个头信息指示文档的最后修改时间。然后，客户端可以缓存文件，并在以后的请求中通过 If-Modified-Since 请求头信息提供一个日期。
Location|这个头信息应被包含在所有的带有状态码的响应中。在 300s 内，这会通知浏览器文档的地址。浏览器会自动重新连接到这个位置，并获取新的文档。
Refresh|这个头信息指定浏览器应该如何尽快请求更新的页面。您可以指定页面刷新的秒数。
Retry-After|这个头信息可以与 503（Service Unavailable 服务不可用）响应配合使用，这会告诉客户端多久就可以重复它的请求。
Set-Cookie|这个头信息指定一个与页面关联的 cookie。

### 设置 HTTP 响应报头的方法
```java
String encodeRedirectURL(String url)
// 为 sendRedirect 方法中使用的指定的 URL 进行编码，或者如果编码不是必需的，则返回 URL 未改变。
String encodeURL(String url)
// 对包含 session 会话 ID 的指定 URL 进行编码，或者如果编码不是必需的，则返回 URL 未改变。
boolean containsHeader(String name)
// 返回一个布尔值，指示是否已经设置已命名的响应报头。
boolean isCommitted()
// 返回一个布尔值，指示响应是否已经提交。
void addCookie(Cookie cookie)
// 把指定的 cookie 添加到响应。
void addDateHeader(String name, long date)
// 添加一个带有给定的名称和日期值的响应报头。
void addHeader(String name, String value)
// 添加一个带有给定的名称和值的响应报头。
void addIntHeader(String name, int value)
// 添加一个带有给定的名称和整数值的响应报头。
void flushBuffer()
// 强制任何在缓冲区中的内容被写入到客户端。
void reset()
// 清除缓冲区中存在的任何数据，包括状态码和头。
void resetBuffer()
// 清除响应中基础缓冲区的内容，不清除状态码和头。
void sendError(int sc)
// 使用指定的状态码发送错误响应到客户端，并清除缓冲区。
void sendError(int sc, String msg)
// 使用指定的状态发送错误响应到客户端。
void sendRedirect(String location)
// 使用指定的重定向位置 URL 发送临时重定向响应到客户端。
void setBufferSize(int size)
// 为响应主体设置首选的缓冲区大小。
void setCharacterEncoding(String charset)
// 设置被发送到客户端的响应的字符编码（MIME 字符集）例如，UTF-8。
void setContentLength(int len)
// 设置在 HTTP Servlet 响应中的内容主体的长度，该方法设置 HTTP Content-Length 头。
void setContentType(String type)
// 如果响应还未被提交，设置被发送到客户端的响应的内容类型。
void setDateHeader(String name, long date)
// 设置一个带有给定的名称和日期值的响应报头。
void setHeader(String name, String value)
// 设置一个带有给定的名称和值的响应报头。
void setIntHeader(String name, int value)
// 设置一个带有给定的名称和整数值的响应报头。
void setLocale(Locale loc)
// 如果响应还未被提交，设置响应的区域。
void setStatus(int sc)
// 为该响应设置状态码。
```
### 实例
```java
//导入必需的 java 库
import java.io.IOException;
import java.io.PrintWriter;
import java.text.SimpleDateFormat;
import java.util.Calendar;
import java.util.Date;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/Refresh")

//扩展 HttpServlet 类
public class Refresh extends HttpServlet {

    // 处理 GET 方法请求的方法
      public void doGet(HttpServletRequest request,
                        HttpServletResponse response)
                throws ServletException, IOException
      {
          // 设置刷新自动加载时间为 5 秒
          response.setIntHeader("Refresh", 5);
          // 设置响应内容类型
          response.setContentType("text/html;charset=UTF-8");
         
          //使用默认时区和语言环境获得一个日历  
          Calendar cale = Calendar.getInstance();  
          //将Calendar类型转换成Date类型  
          Date tasktime=cale.getTime();  
          //设置日期输出的格式  
          SimpleDateFormat df=new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");  
          //格式化输出  
          String nowTime = df.format(tasktime);
          PrintWriter out = response.getWriter();
          String title = "自动刷新 Header 设置 - 菜鸟教程实例";
          String docType =
          "<!DOCTYPE html>\n";
          out.println(docType +
            "<html>\n" +
            "<head><title>" + title + "</title></head>\n"+
            "<body bgcolor=\"#f0f0f0\">\n" +
            "<h1 align=\"center\">" + title + "</h1>\n" +
            "<p>当前时间是：" + nowTime + "</p>\n");
      }
      // 处理 POST 方法请求的方法
      public void doPost(HttpServletRequest request,
                         HttpServletResponse response)
          throws ServletException, IOException {
         doGet(request, response);
      }
}
```
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<web-app>  
  <servlet>  
     <!-- 类名 -->  
    <servlet-name>Refresh</servlet-name>  
    <!-- 所在的包 -->  
    <servlet-class>com.runoob.test.Refresh</servlet-class>  
  </servlet>  
  <servlet-mapping>  
    <servlet-name>Refresh</servlet-name>  
    <!-- 访问的网址 -->  
    <url-pattern>/TomcatTest/Refresh</url-pattern>  
    </servlet-mapping>  
</web-app>
```

## Servlet HTTP 状态码
`HTTP/1.1 200 OK`
状态行包括 HTTP 版本（在本例中为 HTTP/1.1）、一个状态码（在本例中为 200）和一个对应于状态码的短消息（在本例中为 OK）。


### 设置 HTTP 状态代码的方法
这些方法通过 HttpServletResponse 对象可用
```java
public void setStatus ( int statusCode )
// 该方法设置一个任意的状态码。setStatus 方法接受一个 int（状态码）作为参数。
// 如果您的响应包含了一个特殊的状态码和文档，请确保在使用 PrintWriter 实际返回任何内容之前调用 setStatus。
public void sendRedirect(String url)
// 该方法生成一个 302 响应，连同一个带有新文档 URL 的 Location 头。
public void sendError(int code, String message)
// 该方法发送一个状态码（通常为 404），连同一个在 HTML 文档内部自动格式化并发送到客户端的短消息。
```
### HTTP 状态码实例
```java
// 导入必需的 java 库
import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;
import java.util.*;
import javax.servlet.annotation.WebServlet;

@WebServlet("/showError")
// 扩展 HttpServlet 类
public class showError extends HttpServlet {
 
  // 处理 GET 方法请求的方法
  public void doGet(HttpServletRequest request,
                    HttpServletResponse response)
            throws ServletException, IOException
  {
      // 设置错误代码和原因
      response.sendError(407, "Need authentication!!!" );
  }
  // 处理 POST 方法请求的方法
  public void doPost(HttpServletRequest request,
                     HttpServletResponse response)
      throws ServletException, IOException {
     doGet(request, response);
  }
}
```
现在，调用上面的 Servlet 将显示以下结果
`HTTP Status 407 - Need authentication!!!`

## Servlet 异常处理
- 当一个 Servlet 抛出一个异常时，Web 容器在使用了 exception-type 元素的 web.xml 中搜索与抛出异常类型相匹配的配置。
- 您必须在 web.xml 中使用 error-page 元素来指定对特定异常 或 HTTP 状态码 作出相应的 Servlet 调用。
- 假设，有一个 ErrorHandler 的 Servlet 在任何已定义的异常或错误出现时被调用。以下将是在 web.xml 中创建的项
```xml
<!-- servlet 定义 -->
<servlet>
        <servlet-name>ErrorHandler</servlet-name>
        <servlet-class>ErrorHandler</servlet-class>
</servlet>
<!-- servlet 映射 -->
<servlet-mapping>
        <servlet-name>ErrorHandler</servlet-name>
        <url-pattern>/ErrorHandler</url-pattern>
</servlet-mapping>

<!-- error-code 相关的错误页面 -->
<error-page>
    <error-code>404</error-code>
    <location>/ErrorHandler</location>
</error-page>
<error-page>
    <error-code>403</error-code>
    <location>/ErrorHandler</location>
</error-page>

<!-- exception-type 相关的错误页面 -->
<error-page>
    <exception-type>
          javax.servlet.ServletException
    </exception-type >
    <location>/ErrorHandler</location>
</error-page>

<error-page>
    <exception-type>java.io.IOException</exception-type >
    <location>/ErrorHandler</location>
</error-page>
```
如果您想对所有的异常有一个通用的错误处理程序，那么应该定义下面的 error-page，而不是为每个异常定义单独的 error-page 元素：
```xml
<error-page>
    <exception-type>java.lang.Throwable</exception-type >
    <location>/ErrorHandler</location>
</error-page>
```
- Servlet ErrorHandler 与其他的 Servlet 的定义方式一样，且在 web.xml 中进行配置。
- 如果有错误状态代码出现，不管为 404（Not Found 未找到）或 403（Forbidden 禁止），则会调用 ErrorHandler 的 Servlet。
- 如果 Web 应用程序抛出 ServletException 或 IOException，那么 Web 容器会调用 ErrorHandler 的 Servlet。
- 您可以定义不同的错误处理程序来处理不同类型的错误或异常。上面的实例是非常通用的，希望您能通过实例理解基本的概念。

### 请求属性 - 错误/异常
```java
javax.servlet.error.status_code
// 该属性给出状态码，状态码可被存储，并在存储为 java.lang.Integer 数据类型后可被分析。
javax.servlet.error.exception_type
// 该属性给出异常类型的信息，异常类型可被存储，并在存储为 java.lang.Class 数据类型后可被分析。
javax.servlet.error.message
// 该属性给出确切错误消息的信息，信息可被存储，并在存储为 java.lang.String 数据类型后可被分析。
javax.servlet.error.request_uri
// 该属性给出有关 URL 调用 Servlet 的信息，信息可被存储，并在存储为 java.lang.String 数据类型后可被分析。
javax.servlet.error.exception
// 该属性给出异常产生的信息，信息可被存储，并在存储为 java.lang.Throwable 数据类型后可被分析。
javax.servlet.error.servlet_name
// 该属性给出 Servlet 的名称，名称可被存储，并在存储为 java.lang.String 数据类型后可被分析。
```

### Servlet 错误处理程序实例
```java
//导入必需的 java 库
import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;
import java.util.*;

//扩展 HttpServlet 类
public class ErrorHandler extends HttpServlet {

    // 处理 GET 方法请求的方法
    public void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException
    {
        Throwable throwable = (Throwable)
        request.getAttribute("javax.servlet.error.exception");
        Integer statusCode = (Integer)
        request.getAttribute("javax.servlet.error.status_code");
        String servletName = (String)
        request.getAttribute("javax.servlet.error.servlet_name");
        if (servletName == null){
            servletName = "Unknown";
        }
        String requestUri = (String)
        request.getAttribute("javax.servlet.error.request_uri");
        if (requestUri == null){
            requestUri = "Unknown";
        }
        // 设置响应内容类型
        response.setContentType("text/html;charset=UTF-8");
    
        PrintWriter out = response.getWriter();
        String title = "菜鸟教程 Error/Exception 信息";
       
        String docType = "<!DOCTYPE html>\n";
        out.println(docType +
            "<html>\n" +
             "<head><title>" + title + "</title></head>\n" +
             "<body bgcolor=\"#f0f0f0\">\n");
           out.println("<h1>菜鸟教程异常信息实例演示</h1>");
           if (throwable == null && statusCode == null){
              out.println("<h2>错误信息丢失</h2>");
              out.println("请返回 <a href=\"" + 
            response.encodeURL("http://localhost:8080/") + 
                "\">主页</a>。");
           }else if (statusCode != null) {
              out.println("错误代码 : " + statusCode);
        }else{
               out.println("<h2>错误信息</h2>");
              out.println("Servlet Name : " + servletName + 
                              "</br></br>");
              out.println("异常类型 : " + 
                              throwable.getClass( ).getName( ) + 
                              "</br></br>");
              out.println("请求 URI: " + requestUri + 
                              "<br><br>");
              out.println("异常信息: " + 
                                  throwable.getMessage( ));
           }
           out.println("</body>");
           out.println("</html>");
    }
    // 处理 POST 方法请求的方法
    public void doPost(HttpServletRequest request,
                      HttpServletResponse response)
       throws ServletException, IOException {
        doGet(request, response);
    }
}
```
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<web-app>  
<servlet>
        <servlet-name>ErrorHandler</servlet-name>
        <servlet-class>com.runoob.test.ErrorHandler</servlet-class>
</servlet>
<!-- servlet mappings -->
<servlet-mapping>
        <servlet-name>ErrorHandler</servlet-name>
        <url-pattern>/TomcatTest/ErrorHandler</url-pattern>
</servlet-mapping>
<error-page>
    <error-code>404</error-code>
    <location>/TomcatTest/ErrorHandler</location>
</error-page>
<error-page>
    <exception-type>java.lang.Throwable</exception-type >
    <location>/ErrorHandler</location>
</error-page>
</web-app>
```
## Servlet 编写过滤器
Servlet 过滤器可以动态地拦截请求和响应，以变换或使用包含在请求或响应中的信息。可以将一个或多个 Servlet 过滤器附加到一个 Servlet 或一组 Servlet。Servlet 过滤器也可以附加到 JavaServer Pages (JSP) 文件和 HTML 页面。调用 Servlet 前调用所有附加的 Servlet 过滤器。Servlet 过滤器是可用于 Servlet 编程的 Java 类，可以实现以下目的：
- 在客户端的请求访问后端资源之前，拦截这些请求。
- 在服务器的响应发送回客户端之前，处理这些响应。

根据规范建议的各种类型的过滤器：
- 身份验证过滤器（Authentication Filters）。
- 数据压缩过滤器（Data compression Filters）。
- 加密过滤器（Encryption Filters）。
- 触发资源访问事件过滤器。
- 图像转换过滤器（Image Conversion Filters）。
- 日志记录和审核过滤器（Logging and Auditing Filters）。
- MIME-TYPE 链过滤器（MIME-TYPE Chain Filters）。
- 标记化过滤器（Tokenizing Filters）。
- XSL/T 过滤器（XSL/T Filters），转换 XML 内容。

过滤器通过 Web 部署描述符（web.xml）中的 XML 标签来声明，然后映射到您的应用程序的部署描述符中的 Servlet 名称或 URL 模式。

当 Web 容器启动 Web 应用程序时，它会为您在部署描述符中声明的每一个过滤器创建一个实例。

Filter的执行顺序与在web.xml配置文件中的配置顺序一致，一般把Filter配置在所有的Servlet之前。

### Servlet 过滤器方法
过滤器是一个实现了 javax.servlet.Filter 接口的 Java 类。javax.servlet.Filter 接口定义了三个方法
```java
public void doFilter (ServletRequest, ServletResponse, FilterChain)
// 该方法完成实际的过滤操作，当客户端请求方法与过滤器设置匹配的URL时，Servlet容器将先调用过滤器的doFilter方法。FilterChain用户访问后续过滤器。
public void init(FilterConfig filterConfig)
// web 应用程序启动时，web 服务器将创建Filter 的实例对象，并调用其init方法，
// 读取web.xml配置，完成对象的初始化功能，从而为后续的用户请求作好拦截的准备工作（filter对象只会创建一次，init方法也只会执行一次）。
// 开发人员通过init方法的参数，可获得代表当前filter配置信息的FilterConfig对象。
public void destroy()
// Servlet容器在销毁过滤器实例前调用该方法，在该方法中释放Servlet过滤器占用的资源。
```
### FilterConfig 使用
Filter 的 init 方法中提供了一个 FilterConfig 对象。在 init 方法使用 FilterConfig 对象获取参数
```java
public void  init(FilterConfig config) throws ServletException {
    // 获取初始化参数
    String site = config.getInitParameter("Site"); 
    // 输出初始化参数
    System.out.println("网站名称: " + site); 
}
```
 web.xml 文件
```xml
<filter>
<filter-name>LogFilter</filter-name>
<filter-class>com.runoob.test.LogFilter</filter-class>
<init-param>
    <param-name>Site</param-name>
    <param-value>菜鸟教程</param-value>
</init-param>
</filter>
```
### Servlet 过滤器实例
实现接口
```java
package com.runoob.test;

//导入必需的 java 库
import javax.servlet.*;
import java.util.*;

//实现 Filter 类
public class LogFilter implements Filter  {
    public void  init(FilterConfig config) throws ServletException {
        // 获取初始化参数
        String site = config.getInitParameter("Site"); 

        // 输出初始化参数
        System.out.println("网站名称: " + site); 
    }
    public void  doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws java.io.IOException, ServletException {

        // 输出站点名称
        System.out.println("站点网址：http://www.runoob.com");

        // 把请求传回过滤链
        chain.doFilter(request,response);
    }
    public void destroy( ){
        /* 在 Filter 实例被 Web 容器从服务移除之前调用 */
    }
}
```
处理请求
```java
//导入必需的 java 库
import java.io.IOException;
import java.io.PrintWriter;
import java.util.Enumeration;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/DisplayHeader")

//扩展 HttpServlet 类
public class DisplayHeader extends HttpServlet {

    // 处理 GET 方法请求的方法
    public void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException
    {
        // 设置响应内容类型
        response.setContentType("text/html;charset=UTF-8");

        PrintWriter out = response.getWriter();
        String title = "HTTP Header 请求实例 - 菜鸟教程实例";
        String docType =
            "<!DOCTYPE html> \n";
            out.println(docType +
            "<html>\n" +
            "<head><meta charset=\"utf-8\"><title>" + title + "</title></head>\n"+
            "<body bgcolor=\"#f0f0f0\">\n" +
            "<h1 align=\"center\">" + title + "</h1>\n" +
            "<table width=\"100%\" border=\"1\" align=\"center\">\n" +
            "<tr bgcolor=\"#949494\">\n" +
            "<th>Header 名称</th><th>Header 值</th>\n"+
            "</tr>\n");

        Enumeration headerNames = request.getHeaderNames();

        while(headerNames.hasMoreElements()) {
            String paramName = (String)headerNames.nextElement();
            out.print("<tr><td>" + paramName + "</td>\n");
            String paramValue = request.getHeader(paramName);
            out.println("<td> " + paramValue + "</td></tr>\n");
        }
        out.println("</table>\n</body></html>");
    }
    // 处理 POST 方法请求的方法
    public void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }
}
```
Web.xml 中的 Servlet 过滤器映射
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<web-app>  
<filter>
  <filter-name>LogFilter</filter-name>
  <filter-class>com.runoob.test.LogFilter</filter-class>
  <init-param>
    <param-name>Site</param-name>
    <param-value>菜鸟教程</param-value>
  </init-param>
</filter>
<filter-mapping>
  <filter-name>LogFilter</filter-name>
  <url-pattern>/*</url-pattern>
</filter-mapping>
<servlet>  
  <!-- 类名 -->  
  <servlet-name>DisplayHeader</servlet-name>  
  <!-- 所在的包 -->  
  <servlet-class>com.runoob.test.DisplayHeader</servlet-class>  
</servlet>  
<servlet-mapping>  
  <servlet-name>DisplayHeader</servlet-name>  
  <!-- 访问的网址 -->  
  <url-pattern>/TomcatTest/DisplayHeader</url-pattern>  
</servlet-mapping>  
</web-app>
```
上述过滤器适用于所有的 Servlet，因为我们在配置中指定 /* 。如果您只想在少数的 Servlet 上应用过滤器，您可以指定一个特定的 Servlet 路径。

### 使用多个过滤器
```xml
<filter>
   <filter-name>LogFilter</filter-name>
   <filter-class>com.runoob.test.LogFilter</filter-class>
   <init-param>
      <param-name>test-param</param-name>
      <param-value>Initialization Paramter</param-value>
   </init-param>
</filter>

<filter>
   <filter-name>AuthenFilter</filter-name>
   <filter-class>com.runoob.test.AuthenFilter</filter-class>
   <init-param>
      <param-name>test-param</param-name>
      <param-value>Initialization Paramter</param-value>
   </init-param>
</filter>

<filter-mapping>
   <filter-name>LogFilter</filter-name>
   <url-pattern>/*</url-pattern>
</filter-mapping>

<filter-mapping>
   <filter-name>AuthenFilter</filter-name>
   <url-pattern>/*</url-pattern>
</filter-mapping>
```
实例将先应用 LogFilter，然后再应用 AuthenFilter

### web.xml配置各节点说明
- `<filter>`指定一个过滤器。
  - `<filter-name>`用于为过滤器指定一个名字，该元素的内容不能为空。
  - `<filter-class>`元素用于指定过滤器的完整的限定类名。
  - `<init-param>`元素用于为过滤器指定初始化参数，它的子元素`<param-name>`指定参数的名字，`<param-value>`指定参数的值。
  - 在过滤器中，可以使用FilterConfig接口对象来访问初始化参数。
- `<filter-mapping>`元素用于设置一个 Filter 所负责拦截的资源。一个Filter拦截的资源可通过两种方式来指定：Servlet 名称和资源访问的请求路径
  - `<filter-name>`子元素用于设置filter的注册名称。该值必须是在`<filter>`元素中声明过的过滤器的名字
  - `<url-pattern>`设置 filter 所拦截的请求路径(过滤器关联的URL样式)
- `<servlet-name>`指定过滤器所拦截的Servlet名称。
- `<dispatcher>`指定过滤器所拦截的资源被 Servlet 容器调用的方式，可以是REQUEST,INCLUDE,FORWARD和ERROR之一，默认REQUEST。用户可以设置多个`<dispatcher>`子元素用来指定 Filter 对资源的多种调用方式进行拦截。
- `<dispatcher>`子元素可以设置的值及其意义
  - REQUEST：当用户直接访问页面时，Web容器将会调用过滤器。如果目标资源是通过RequestDispatcher的include()或forward()方法访问时，那么该过滤器就不会被调用。
  - INCLUDE：如果目标资源是通过RequestDispatcher的include()方法访问时，那么该过滤器将被调用。除此之外，该过滤器不会被调用。
  - FORWARD：如果目标资源是通过RequestDispatcher的forward()方法访问时，那么该过滤器将被调用，除此之外，该过滤器不会被调用。
  - ERROR：如果目标资源是通过声明式异常处理机制调用时，那么该过滤器将被调用。除此之外，过滤器不会被调用。

[原文](https://www.runoob.com/servlet/servlet-writing-filters.html)