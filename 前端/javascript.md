# javascript

## JavaScript位置

```html
<script> js代码; </script>

<script src="外部js文件.js"></script>
```
## JavaScript 输出
```javascript
弹出警告框
window.alert()

写入到 HTML 文档中
document.write()

写入到 HTML 元素
document.getElementById("demo").innerHTML = "段落已修改。";

写入到浏览器的控制台
console.log()
```
## JavaScript 语法
```js
使用关键字 var 来定义变量， 使用等号来为变量赋值
如果重新声明 JavaScript 变量，该变量的值不会丢失
未使用值来声明的变量，其值实际上是 undefined
如果把值赋给尚未声明的变量，该变量将被自动作为 window 的一个属性
var x, length
x = 5
length = 6

语句用分号分隔
x = 5 + 6;
y = x * 10;

JavaScript 关键字必须以字母、下划线（_）或美元符（$）开始
后续的字符可以是字母、数字、下划线或美元符

注释
双斜杠 // 后的内容将会被浏览器忽略

多行注释
多行注释以 /* 开始，以 */ 结尾

数据类型
JavaScript 拥有动态类型。这意味着相同的变量可用作不同的类型
基本类型：字符串（String）、数字(Number)、布尔(Boolean)、对空（Null）、未定义（Undefined）、Symbol
引用数据类型：对象(Object)、数组(Array)、函数(Function)
字符串可以使用单引号或双引号,可以使用索引位置来访问字符串中的每个字符
获取字符串长度txt.length
JavaScript 只有一种数字类型。数字可以带小数点，也可以不带
布尔（逻辑）只能有两个值：true 或 false
可以使用 typeof 操作符来检测变量的数据类型

类型转换
Number() 转换为数字， String() 转换为字符串， Boolean() 转化为布尔值

创建名为 cars 的数组：
var cars=new Array();
cars[0]="Saab";
cars[1]="Volvo";
cars[2]="BMW";
或var cars=new Array("Saab","Volvo","BMW");
或var cars=["Saab","Volvo","BMW"];

JavaScript 对象
对象由花括号分隔。对象的属性(name : value)
var person={firstname:"John", lastname:"Doe", id:5566};
对象属性有两种寻址方式
name=person.lastname;
name=person["lastname"];

对象方法
创建对象方法
methodName : function() { code lines }
访问对象方法
objectName.methodName()

局部变量&全局变量
在函数内部声明的变量是局部变量，所以只能在函数内部访问它
在函数外声明的变量是全局变量，网页上的所有脚本和函数都能访问它
局部变量会在函数运行以后被删除
全局变量会在页面关闭后被删除

函数
function myFunction(a, b) {
    return a * b;   // 返回 a 乘以 b 的结果
}

JavaScript 对大小写是敏感的

JavaScript 使用 Unicode 字符集

JavaScript 会忽略多余的空格。您可以向脚本添加空格，来提高其可读性

代码折行
document.write("你好 \
世界!");
```
## JavaScript 语句标识符
```js
语句            描述
break           用于跳出循环。
catch           语句块，在 try 语句块执行出错时执行 catch 语句块。
continue        跳过循环中的一个迭代。
do ... while    执行一个语句块，在条件语句为 true 时继续执行该语句块。
for             在条件语句为 true 时，可以将代码块执行指定的次数。
for ... in      用于遍历数组或者对象的属性（对数组或者对象的属性进行循环操作）。
function        定义一个函数
if ... else     用于基于不同的条件来执行不同的动作。
return          退出函数
switch          用于基于不同的条件来执行不同的动作。
throw           抛出（生成）错误 。
try             实现错误处理，与 catch 一同使用。
var             声明一个变量。
while           当条件语句为 true 时，执行语句块。
```

## HTML 事件
    当在HTML页面中使用 avaScript时， 这些事件可以触发JavaScript

```html
<button onclick="getElementById('demo').innerHTML=Date()">现在的时间是?</button>
```
常见的HTML事件:
```
onchange    HTML 元素改变
onclick     用户点击 HTML 元素
onmouseover 用户在一个HTML元素上移动鼠标
onmouseout  用户从一个HTML元素上移开鼠标
onkeydown   用户按下键盘按键
onload      浏览器已完成页面的加载
```
## 内置对象

### 字符串
    可以使用索引位置来访问字符串中的每个字符
```js
属性
constructor 返回创建字符串属性的函数
length      返回字符串的长度
prototype   允许您向对象添加属性和方法

方法
charAt()    返回指定索引位置的字符
charCodeAt()        返回指定索引位置字符的 Unicode 值
concat()    连接两个或多个字符串，返回连接后的字符串
fromCharCode()      将 Unicode 转换为字符串
indexOf()   返回字符串中检索指定字符第一次出现的位置
lastIndexOf()       返回字符串中检索指定字符最后一次出现的位置
localeCompare()     用本地特定的顺序来比较两个字符串
match()     找到一个或多个正则表达式的匹配
replace()   替换与正则表达式匹配的子串
search()    检索与正则表达式相匹配的值
slice()     提取字符串的片断，并在新的字符串中返回被提取的部分
split()     把字符串分割为子字符串数组
substr()    从起始索引号提取字符串中指定数目的字符
substring() 提取字符串中两个指定的索引号之间的字符
toLocaleLowerCase() 根据主机的语言环境把字符串转换为小写，只有几种语言（如土耳其语）具有地方特有的大小写映射
toLocaleUpperCase() 根据主机的语言环境把字符串转换为大写，只有几种语言（如土耳其语）具有地方特有的大小写映射
toLowerCase()       把字符串转换为小写
toString()  返回字符串对象值
toUpperCase()       把字符串转换为大写
trim()      移除字符串首尾空白
valueOf()   返回某个字符串对象的原始值
```
## 运算符
```
算术运算符
+   加法    字符加数字等于字符
-   减法
*   乘法
/   除法
%   取模（余数）
++  自增
--  自减

赋值运算符
= += -= *= /= %=

比较运算符
==  等于
=== 绝对等于（值和类型均相等
!=   不等
!==  不绝对等于（值和类型有一个不相等，或两个都不相等）
>    大于
<    小于
>=   大于或等于
<=   小于或等于

逻辑运算符
&&  and
||  or
!   not

条件运算符
variablename=(condition)?value1:value2
voteable=(age<18)?"年龄太小":"年龄已达到";
```

## if...Else 语句
```js
if 语句
if (condition)
{
    当条件为 true 时执行的代码
}

if...else 语句
if (condition)
{
    当条件为 true 时执行的代码
}
else
{
    当条件不为 true 时执行的代码
}

if...else if...else 语句
if (condition1)
{
    当条件 1 为 true 时执行的代码
}
else if (condition2)
{
    当条件 2 为 true 时执行的代码
}
else
{
当条件 1 和 条件 2 都不为 true 时执行的代码
}
```
## switch 语句
    switch 语句会使用恒等计算符(===)进行比较
```js
switch(n)
{
    case 1:
        执行代码块 1
        break;
    case 2:
        执行代码块 2
        break;
    default:
        与 case 1 和 case 2 不同时执行的代码
}
```
## for 循环
```js
For 循环
for (语句 1; 语句 2; 语句 3)
{
    被执行的代码块
}
语句 1 （代码块）开始前执行
语句 2 定义运行循环（代码块）的条件
语句 3 在循环（代码块）已被执行之后执行

For/In 循环
遍历对象的属性
var person={fname:"John",lname:"Doe",age:25};
for (x in person)  // x 为属性名
{
    txt=txt + person[x];
}
```
## while 循环
```js
while 循环
while (条件)
{
    需要执行的代码
}

do/while 循环
该循环会在检查条件是否为真之前执行一次代码块
do
{
    需要执行的代码
}
while (条件);
```
## Break 和 Continue 语句
```js
先看JavaScript 标签
如需标记代码，请在语句之前加上冒号：
label:
statements

break labelname;
continue labelname;
continue 语句（带有或不带标签引用）只能用在循环中。
break 语句（不带标签引用），只能用在循环或 switch 中。
通过标签引用，break 语句可用于跳出任何 JavaScript 代码块：

cars=["BMW","Volvo","Saab","Ford"];
list:
{
    document.write(cars[0] + "<br>");
    document.write(cars[1] + "<br>");
    document.write(cars[2] + "<br>");
    break list;
    document.write(cars[3] + "<br>");
    document.write(cars[4] + "<br>");
    document.write(cars[5] + "<br>");
}
```
## JavaScript 正则表达式
```js
/正则表达式主体/修饰符(可选)
var patt = /runoob/i
/runoob/i  是一个正则表达式
runoob  是一个正则表达式主体 (用于检索)
i  是一个修饰符 (搜索不区分大小写)
一个字符串也算正则表达式

search() 并返回匹配子串的起始位置。
var str = "Visit Runoob!";
var n = str.search(/Runoob/i);

replace() 替换字符串
var str = document.getElementById("demo").innerHTML;
var txt = str.replace(/microsoft/i,"Runoob");

test()用于检测一个字符串是否匹配某个模式
var patt = /e/;
patt.test("The best things in life are free!");
返回true

exec()用于检索字符串中的正则表达式的匹配
/e/.exec("The best things in life are free!");
输出e
```
## JavaScript 错误 - throw、try 和 catch
```js
try {
    ...    //异常的抛出
} catch(e) {
    ...    //异常的捕获与处理
} finally {
    ...    //结束处理
}

try 语句测试代码块的错误。
catch 语句处理错误。
throw 语句创建自定义错误。
finally 语句在 try 和 catch 语句之后，无论是否有触发异常，该语句都会执行

Throw 语句
创建或抛出异常（exception）
throw exception
if(x == "")  throw "值为空";
```
## JavaScript 调试
```js
浏览器启用调试工具一般是按下 F12 键，并在调试菜单中选择 "Console" 如果浏览器支持调试，你可以使用 console.log() 方法在调试窗口上打印 JavaScript 值
在调试窗口中，你可以设置 JavaScript 代码的断点 在每个断点上，都会停止执行 JavaScript 代码，以便于我们检查 JavaScript 变量的值 在检查完毕后，可以重新执行代码
```
## JavaScript HTML DOM
```js
通过 id 查找 HTML 元素
var x=document.getElementById("intro");

通过标签名查找 HTML 元素
var x=document.getElementById("main");
var y=x.getElementsByTagName("p");

通过类名找到 HTML 元素
var x=document.getElementsByClassName("intro");

改变 HTML 内容
最简单的方法是使用 innerHTML 属性
document.getElementById(id).innerHTML=新的 HTML

改变 HTML 属性
document.getElementById(id).attribute=新属性值
document.getElementById("image").src="landscape.jpg";

改变 HTML 样式
document.getElementById(id).style.property=新样式
document.getElementById("p2").style.color="blue";
document.getElementById("p2").style.fontFamily="Arial";
document.getElementById("p2").style.fontSize="larger";

HTML DOM 事件
向一个 HTML 事件属性添加 JavaScript 代码：
onclick=JavaScript
<h1 onclick="this.innerHTML='Ooops!'">点击文本!</h1>
分配事件
<script>
document.getElementById("myBtn").onclick=function(){displayDate()};
</script>
onload 和 onunload 事件会在用户进入或离开页面时被触发
onchange 当用户改变输入字段的内容时，会调用
onmouseover 和 onmouseout 事件可用于在用户的鼠标移至 HTML 元素上方或移出元素时触发函数
onmousedown、onmouseup 以及 onclick 事件，点击、释放、点击释放

使用 addEventListener() 方法添加事件
添加的事件句柄不会覆盖已存在的事件句柄
element.addEventListener(event, function, useCapture);
第一个参数是事件的类型 (如 "click" 或 "mousedown").
第二个参数是事件触发后调用的函数。
第三个参数是个布尔值用于描述事件是冒泡还是捕获。该参数是可选的。
在 冒泡 中，内部元素的事件会先被触发，然后再触发外部元素
在 捕获 中，外部元素的事件会先被触发，然后才会触发内部元素的事件
element.addEventListener("click", function(){ alert("Hello World!"); });
removeEventListener() 方法移除由 addEventListener() 方法添加的事件句柄

创建新的 HTML 元素 (节点)
创建 <p> 元素
var para = document.createElement("p");
创建文本节点
var node = document.createTextNode("这是一个新的段落。");
将文本节点添加到 <p> 元素尾部
para.appendChild(node);
将新元素添加到开始位置
var para = document.createElement("p");
var node = document.createTextNode("这是一个新的段落。");
para.appendChild(node);
var element = document.getElementById("div1");
var child = document.getElementById("p1");
element.insertBefore(para, child);

移除一个元素
var parent = document.getElementById("div1");
var child = document.getElementById("p1");
parent.removeChild(child);

替换 HTML 元素
var para = document.createElement("p");
var node = document.createTextNode("这是一个新的段落。");
para.appendChild(node);
var parent = document.getElementById("div1");
var child = document.getElementById("p1");
parent.replaceChild(para, child);
```
## JavaScript Window - 浏览器对象模型
```js
Window 尺寸
window.innerHeight - 浏览器窗口的内部高度(包括滚动条)
window.innerWidth - 浏览器窗口的内部宽度(包括滚动条)

window.open() - 打开新窗口
window.close() - 关闭当前窗口
window.moveTo() - 移动当前窗口
window.resizeTo() - 调整当前窗口的尺寸

弹窗
window.alert("sometext");

确认框
点击 "确认", 确认框返回 true， 如果点击 "取消", 确认框返回 false
window.confirm("sometext");

提示框
需要输入某个值，然后点击确认或取消按钮才能继续操纵
window.prompt("sometext","defaultvalue");

计时事件
间隔指定的毫秒数不停地执行指定的代码
window.setInterval("javascript function",milliseconds);
停止 setInterval() 方法执行的函数代码
window.clearInterval(intervalVariable)
等待指定的毫秒数执行一次指定的代码
myVar= window.setTimeout("javascript function", milliseconds);
停止执行setTimeout()方法的函数代码
window.clearTimeout(timeoutVariable)
```
## JavaScript Cookie
```js
创建Cookie
document.cookie="username=John Doe";
添加一个过期时间,默认情况下，cookie 在浏览器关闭时删除
document.cookie="username=John Doe; expires=Thu, 18 Dec 2043 12:00:00 GMT";
告诉浏览器 cookie 的路径。默认情况下，cookie 属于当前页面
document.cookie="username=John Doe; expires=Thu, 18 Dec 2043 12:00:00 GMT; path=/";

读取 Cookie
var x = document.cookie;
将以字符串的方式返回所有的 cookie，类型格式： cookie1=value; cookie2=value; cookie3=value;

修改 Cookie
修改 cookie 类似于创建 cookie
document.cookie="username=John Smith; expires=Thu, 18 Dec 2043 12:00:00 GMT; path=/";

删除 Cookie
只需要设置 expires 参数为以前的时间即可
document.cookie = "username=; expires=Thu, 01 Jan 1970 00:00:00 GMT";

设置 cookie 值的函数
function setCookie(cname,cvalue,exdays)
{
var d = new Date();
d.setTime(d.getTime()+(exdays*24*60*60*1000));
var expires = "expires="+d.toGMTString();
document.cookie = cname + "=" + cvalue + "; " + expires;
}

获取 cookie 值的函数
function getCookie(cname)
{
var name = cname + "=";
var ca = document.cookie.split(';');
for(var i=0; i<ca.length; i++)
{
    var c = ca[i].trim();
    if (c.indexOf(name)==0) return c.substring(name.length,c.length);
}
return "";
}

检测 cookie 值的函数
function checkCookie()
{
var username=getCookie("username");
if (username!="")
{
    alert("Welcome again " + username);
}
else
{
    username = prompt("Please enter your name:","");
    if (username!="" && username!=null)
    {
    setCookie("username",username,365);
    }
}
}
```
## JavaScript 库
```js
JavaScript 高级程序设计（特别是对浏览器差异的复杂处理），通常很困难也很耗时。
为了应对这些调整，许多的 JavaScript (helper) 库应运而生。
这些 JavaScript 库常被称为 JavaScript 框架。

JavaScript 库提免费的CDN：静态资源公共库
```
## 附录
    JSON 字符串转换为 JavaScript 对象
```js
函数提升，变量提升
即可以先使用后定义

var text = '{ "sites" : [' +
'{ "name":"Runoob" , "url":"www.runoob.com" },' +
'{ "name":"Google" , "url":"www.google.com" },' +
'{ "name":"Taobao" , "url":"www.taobao.com" } ]}';
var obj = JSON.parse(text);

JSON.parse()        用于将一个 JSON 字符串转换为 JavaScript 对象。
JSON.stringify()    用于将 JavaScript 值转换为 JSON 字符串。


javascript:void(0)
void 是 JavaScript 中非常重要的关键字
该操作符指定要计算一个表达式但是不返回值。

自调用函数
如果表达式后面紧跟 () ，则会自动调用
(function () {
    var x = "Hello!!";      // 我将调用自己
})();
以上函数实际上是一个 匿名自我调用的函数 (没有函数名)

箭头函数
(参数1, 参数2, …, 参数N) => { 函数声明 }
(参数1, 参数2, …, 参数N) => 表达式(单一)
单一参数 => {函数声明}      //当只有一个参数时，圆括号是可选的
() => {函数声明}    //没有参数的函数应该写成一对圆括号
// ES5
var x = function(x, y) {
    return x * y;
}
// ES6
const x = (x, y) => x * y;

ES6 函数可以自带参数
function myFunction(x, y = 10) {
// y is 10 if not passed or undefined
return x + y;
}
```