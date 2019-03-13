# Beautifulsoup
>是一个可以从HTML或XML文件中提取数据的Python库
---
>传入文档或字符串创建Beautifulsoup对象
```python
from bs4 import BeautifulSoup

soup = BeautifulSoup(open("index.html"))

soup = BeautifulSoup("<html>data</html>")
```
>对象的种类
```python
Tag#与XML或HTML原生文档中的tag相同
soup = BeautifulSoup('<b class="boldest">Extremely bold</b>')
tag = soup.b
type(tag)
# <class 'bs4.element.Tag'>

Name#每个tag都有自己的名字,通过 .name 来获取
tag.name
# u'b'

Attributes#一个tag可能有很多个属性. tag <b class="boldest"> 有一个 “class” 的属性,值为 “boldest” . 
#tag的属性的操作方法与字典相同,属性可以被添加,删除或修改
#多值属性的值以列表形式返回
tag['class']
# u'boldest'
#也可以直接”点”取属性, 比如: .attrs 
tag.attrs
# {u'class': u'boldest'}

```
## 节点操作
>直接指定节点名来操作文档树
```python
soup.head
# <head><title>The Dormouse's story</title></head>

soup.title
# <title>The Dormouse's story</title>

soup.body.b
# <b>The Dormouse's story</b>
```
>遍历子节点
```python
.contents#返回列表形式的所有子节点

.children#以生成器形式返回，区别在于无法直接指定某个子节点

.descendants#递归遍历子节点和孙节点

.string#获取只有一个 NavigableString 类型子节点,若包含多个节点返回None

.strings 和 stripped_strings
#以列表形式返回节点内多个字符串，后者自动去除换行符和空格

```
>遍历父节点
```python
.parent

.parents
```
>兄弟结点
```python
.next_sibling 和 .previous_sibling
#没有下一个或上一个则返回None，忽略节点中的字符串

.next_siblings 和 .previous_siblings
#返回兄弟节点的迭代对象

```
>回退和前进
```python
.next_element 和 .previous_element
#查看上一个和下一个节点,考虑节点中的字符串

.next_elements 和 .previous_elements
```
## 搜索文档树

>过滤器
```python
#可以被用在tag的name中,节点的属性中,字符串中或他们的混合中
#字符串
soup.find_all('b')
#正则表达式
soup.find_all(re.compile("^b"))
#列表
soup.find_all(["a", "b"])
#True可以匹配任何值
soup.find_all(True)
```
>方法
```python
find_all( name , attrs , recursive , string , **kwargs )
#遍历当前节点和所有子节点,并判断是否符合过滤器的条件

name #指定节点名
# soup.find_all("title")
# [<title>The Dormouse's story</title>]

keyword #指定参数和属性
# soup.find_all(id='link2')
# [<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>]
# soup.find_all(href=re.compile("elsie"))
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>]
# soup.find_all(id=True)
#查找包含id属性的节点，无论值是什么
#soup.find_all(href=re.compile("elsie"), id='link1')
# [<a class="sister" href="http://example.com/elsie" id="link1">three</a>]

#如果属性名称中有特殊符号-，则无法匹配，此时应用下列方法
data_soup.find_all(attrs={"data-foo": "value"})
# [<div data-foo="value">foo!</div>]

#4.1.1版本后可用class_搜索css类
soup.find_all("a", class_="sister")
#单独指定类名也可以
css_soup.find_all("p", class_="body")
# [<p class="body strikeout"></p>]

string#可以搜搜文档中的字符串内容
soup.find_all(string=re.compile("Dormouse"))
#[u"The Dormouse's story", u"The Dormouse's story"]

limit#限制了返回数量

recursive #find_all会遍历子孙节点，recursive=False将只遍历子节点
```

```python
find( name , attrs , recursive , string , **kwargs )
#返回一个结果，没有则返回None

#遍历当前节点的父节点
find_parents( name , attrs , recursive , string , **kwargs )
find_parent( name , attrs , recursive , string , **kwargs )

#遍历当前节点之后的兄弟结点
find_next_siblings( name , attrs , recursive , string , **kwargs )
find_next_sibling( name , attrs , recursive , string , **kwargs )

#遍历当前节点之前的兄弟结点
find_previous_siblings( name , attrs , recursive , string , **kwargs )
find_previous_sibling( name , attrs , recursive , string , **kwargs )

#遍历当前节点之后的节点和字符串，与.next_elements属性一样考虑字符串
find_all_next( name , attrs , recursive , string , **kwargs )
find_next( name , attrs , recursive , string , **kwargs )

#遍历当前节点之前的节点和字符串，与.next_elements属性一样考虑字符串
find_all_previous( name , attrs , recursive , string , **kwargs )
find_previous( name , attrs , recursive , string , **kwargs )
```
```python
#css选择器
soup.select("title")
# [<title>The Dormouse's story</title>]

#通过tag标签逐层查找
soup.select("body a")
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
#  <a class="sister" href="http://example.com/lacie"  id="link2">Lacie</a>,
#  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

soup.select("html head title")
# [<title>The Dormouse's story</title>]

#直接子标签
soup.select("head > title")
# [<title>The Dormouse's story</title>]

#兄弟节点标签
soup.select("#link1 ~ .sister")
# [<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
#  <a class="sister" href="http://example.com/tillie"  id="link3">Tillie</a>]

#通过CSS的类名查找
soup.select(".sister")
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
#  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
#  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

#通过tag的id查找
soup.select("#link1")
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>]

#同时用多种CSS选择器查询元素
soup.select("#link1,#link2")
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
#  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>]

#通过是否存在某个属性来查找
soup.select('a[href]')
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
#  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
#  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

#通过属性的值来查找
soup.select('a[href="http://example.com/elsie"]')
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>]

#返回查找到的元素的第一个
soup.select_one(".sister")
# <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>
```
## 修改文档树
[链接](https://beautifulsoup.readthedocs.io/zh_CN/v4.4.0/#id44)

##输出
```python
prettify() 
#文档树格式化后以Unicode编码输出,每个XML/HTML标签都独占一行
print(soup.prettify())
# <html>
#  <head>
#  </head>
#  <body>
#   <a href="http://example.com/">
#    I linked to
#    <i>
#     example.com
#    </i>
#   </a>
#  </body>
# </html>
```
```python
#单行输出
str(soup)
# '<html><head></head><body><a href="http://example.com/">I linked to <i>example.com</i></a></body></html>'

unicode(soup.a)
# u'<a href="http://example.com/">I linked to <i>example.com</i></a>'
```
```python
#将HTML中的特殊字符转换成Unicode,比如"&lquot;"
soup = BeautifulSoup("&ldquo;Dammit!&rdquo; he said.")
unicode(soup)
# u'<html><head></head><body>\u201cDammit!\u201d he said.</body></html>'
#如果将文档转换成字符串,Unicode编码会被编码成UTF-8.这样就无法正确显示HTML特殊字符了
str(soup)
# '<html><head></head><body>\xe2\x80\x9cDammit!\xe2\x80\x9d he said.</body></html>'

```
```python
#得到tag中包含的文本内容
get_text()

#指定tag的文本内容的分隔符
soup.get_text("|")
# u'\nI linked to |example.com|\n'

#去除获得文本内容的前后空白:
soup.get_text("|", strip=True)
# u'I linked to|example.com'
```
## 文档解析器
[链接](https://beautifulsoup.readthedocs.io/zh_CN/v4.4.0/#id52)
## 编码
[链接](https://beautifulsoup.readthedocs.io/zh_CN/v4.4.0/#id55)

