# jsp
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

