## URL 处理
URL 可以分为如下几个部分
`protocol://host:port/path?query#fragment`

`https://www.twle.cn/index.html?language=cn#j2se`
- 协议为(protocol)： http
- 主机为(host:port) ：`www.twle.cn`
- 端口号为(port): 80 ，这个 URL 并未指定端口，因为 HTTP 协议默认的端口号为 80
- 文件路径为(path)： /index.html
- 请求参数(query) ：language=cn
- 定位位置(fragment)： j2se，定位到网页中 id 属性为 j2se 的 HTML 元素位置

### URL 类
java.net.URL 类提供了丰富的 URL 构建方式，并可以通过 java.net.URL 来获取资源
```java
// 构造

public URL(String protocol, String host, int port, String file) throws MalformedURLException
// 通过给定的参数(协议、主机名、端口号、文件名)创建URL

public URL(String protocol, String host, String file) throws MalformedURLException
// 使用指定的协议、主机名、文件名创建URL，端口使用协议的默认端口

public URL(String url) throws MalformedURLException
// 通过给定的URL字符串创建URL

public URL(URL context, String url) throws MalformedURLException
// 使用基地址和相对URL创建
```
```java
// 访问
public String getPath() //返回URL路径部分
public String getQuery() //返回URL查询部分
public String getAuthority() //获取此 URL 的授权部分
public int getPort() //返回 URL 端口部分
public int getDefaultPort() //返回协议的默认端口号
public String getProtocol() //返回 URL 的协议
public String getHost() //返回 URL 的主机
public String getFile() //返回 URL 文件名部分
public String getRef() //获取此 URL 的锚点 ( 也称为"引用" )
public URLConnection openConnection() throws IOException 
//打开一个 URL 连接，并运行客户端访问资源
```
### URLConnections 类
URL 类的 openConnection() 会返回一个 java.net.URLConnection
- 如果连接 HTTP 协议的 URL, openConnection() 方法返回 HttpURLConnection 对象
- 如果连接的 URL 为一个 JAR 文件, openConnection() 方法将返回 JarURLConnection 对象

```java
// 常用方法

Object getContent() 
// 检索 URL 链接内容

Object getContent(Class[] classes) 
// 检索URL链接内容

String getContentEncoding() 
// 返回头部 content-encoding 字段值

int getContentLength() 
// 返回头部 content-length字段值

String getContentType()
// 返回头部 content-type 字段值

int getLastModified() 
// 返回头部 last-modified 字段值

long getExpiration() 
// 返回头部 expires 字段值

long getIfModifiedSince() 
// 返回对象的 ifModifiedSince 字段值

public void setDoInput(boolean input) 
// URL 连接可用于输入和/或输出。如果打算使用 URL 连接进行输入，
// 则将 DoInput 标志设置为 true；如果不打算使用，则设置为 false。默认值为 true

public void setDoOutput(boolean output) 
// URL 连接可用于输入和/或输出。如果打算使用 URL 连接进行输出，
// 则将 DoOutput 标志设置为 true；如果不打算使用，则设置为 false。默认值为 false

public InputStream getInputStream() throws IOException 
// 返回URL的输入流，用于读取资源

public OutputStream getOutputStream() throws IOException 
// 返回URL的输出流, 用于写入资源

public URL getURL() 
// 返回 URLConnection 对象连接的 URL
```
```java
// 实例

import java.net.*;
import java.io.*;
public class URLConnDemo
{
   public static void main(String [] args)
   {
      try
      {
         URL url = new URL("http://www.twle.cn");
         URLConnection urlConnection = url.openConnection();
         HttpURLConnection connection = null;
         if(urlConnection instanceof HttpURLConnection)
         {
            connection = (HttpURLConnection) urlConnection;
         }
         else
         {
            System.out.println("请输入 URL 地址");
            return;
         }
         BufferedReader in = new BufferedReader(
         new InputStreamReader(connection.getInputStream()));
         String urlString = "";
         String current;
         while((current = in.readLine()) != null)
         {
            urlString += current;
         }
         System.out.println(urlString);
      }catch(IOException e)
      {
         e.printStackTrace();
      }
   }
}
```






