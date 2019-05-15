# XML
用途
- 两个程序间进行数据通信
- 程序配置文件

常见应用
- QQ之间的数据传送
- 弹幕文件
- Tomcat服务器的server.xml，web.xml配置文件
- 充当小型的数据库

## XML语法

### 文档声明
```xml
<?xml version="1.0" encoding="utf-8" standalone="yes" ?>
```
XML声明放在XML文档的第一行
version –文档符合XML1.0规范，我们学习1.0 
encoding –文档字符编码，比如”GB2312”或者”UTF-8” ,要与文件编码格式一致
standalone –文档定义是否独立使用 
standalone=”no”为默认值。yes代表是独立使用，而no代表不是独立使用

### 元素(或者叫标记、节点)
每个XML文档必须有且只有一个根元素
- 根元素是一个完全包括文档中其他所有元素的元素
- 根元素的起始标记要放在所有其他元素的起始标记之前
- 跟元素的结束标记要放在所有其他元素的结束标记之后

XML元素指的是XML文件中出现的标签，一个标签分为开始标签和结束标签，一个标签有如下几种书写方式
```xml
<!-- 包含标签体 -->
<a>www.sohu.com</a>
<!-- 不含标签体 -->
<a></a>
<!-- 简写为 -->
<a/>
```
一个标签中也可以嵌套若干子标签。但所有标签必须合理地嵌套，绝对不允许交叉嵌套
```xml
<a>welcome to <b> www.sohu.com </b></a>
```
对于XML标签中出现的所有空格和换行，XML解析程序都会当做标签内容进行处理
```xml
<stu>xiaoming</stu>
<!-- 和 -->
<stu>
    xiaoming
</stu>
<!-- 两段内容的意义是不一样的 -->
```
由于在XML中，空格和换行都作为原始内容被处理，所以，在编写XML文件时，要特别注意。

命名规范：一个XML元素可以包含字母、数字以及其它一些可见字符，但必须遵守以下规范
- 区分大小写，例如，元素P和元素p是两个不同的元素
- 不能以数字或下划线”_”开头
- 元素内不能包含空格
- 名称中间不能包含冒号（:）
- 可以使用中文，但一般不这么用

### 属性
```xml
<student id="100">
    <name>Tom</name>
</student>
```
属性值用双引号 `"` 或单引号 `'` 分隔,如果属性值中有单引号，则用双引号分隔；如果有双引号，则用单引号分隔。那么如果属性值中既有单引号还有双引号怎么办？这种要使用实体（转义字符，类似于html中的空格符），XML有5个预定义的实体字符，如下

实体字符|代表符号|符号名
--|--|--
`&lt;`|<|less than
`&gt;`|>|greater than
`&amp;`|&|ampersand
`&apos;`|'|apostrophe
`&quot;`|"|quotation mark
>在 XML 中，只有字符 "<" 和 "&" 确实是非法的。大于号是合法的，但是用实体引用来代替它是一个好习惯

一个元素可以有多个属性，它的基本格式为
```xml
<元素名 属性名1="属性值1" 属性名2="属性值2">
```
特定的属性名称在同一个元素标记中只能出现一次

属性值不能包括<,>,&，如果一定要包含，也要使用实体

### 注释

```xml
<!--这是一个注释-->
```
- 注释内容不要出现`--`
- 不要把注释放在标记中间
- 注释不能嵌套 
- 可以在除标记以外的任何地方放注释

### CDATA节
有些内容可能不想让解析引擎解析执行，而是当做原始内容处理，用于把整段文本解释为纯字符数据而不是标记。
```xml
<![CDATA[
    ......
]]>
<!-- 例子 -->
<stu id="001">
    <name>杨过</name> 
    <sex>男</sex>
    <age>20</age>
    <intro><![CDATA[ad<<&$^#*k]]></intro>
</stu>
```
CDATA节中可以输入任意字符（除]]>外），但是不能嵌套

### 处理指令
处理指令，简称PI（processing instruction）。处理指令用来指示解析引擎如何解析XML文件
```css
/* my.css文件 */
name{
    font-size:80px;
    font-weight:bold;
    color:red;
}

sex{
    font-size:60px;
    font-weight:bold;
    color:blue;
}

sex{
    font-size:40px;
    font-weight:bold;
    color:green;
}
```
```xml
<!-- xml文件中使用处理指令引入这个css文件 -->
<?xml version="1.0" encoding="gb2312"?>
<?xml-stylesheet href="my.css" type="text/css"?>
<class>
    <stu id="001">
        <name>杨过</name> 
        <sex>男</sex>
        <age>20</age>
    </stu>  
    <stu id="002">
        <name>小龙女</name>    
        <sex>女</sex>
        <age>21</age>
    </stu>
</class>
```
用浏览器打开这个xml文件，会发现浏览器解析出一个带样式的视图
>XML的处理指令很少使用

## 小结
- XML声明语句
- 必须有一个根元素
- 标记大小写敏感
- 属性值用引号
- 标记成对
- 空标记关闭
- 元素正确嵌套






[原文地址](https://blog.csdn.net/gavin_john/article/details/51511180)