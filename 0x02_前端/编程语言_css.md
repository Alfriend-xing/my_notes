# css
    CSS 指层叠样式表

    样式表定义如何显示 HTML 元素，就像 HTML 中的字体标签和颜色属性所起的作用那样。 样式通常保存在外部的 .css 文件中。 我们只需要编辑一个简单的 CSS 文档就可以改变所有页面的布局和外观。
## CSS 语法
```css
CSS 规则由两个主要的部分构成：选择器，以及一条或多条声明
选择器通常是您需要改变样式的 HTML 元素。
每条声明由一个属性和一个值组成。

CSS声明总是以分号(;)结束，声明组以大括号({})括起来
p {color:red;text-align:center;}

CSS注释以 "/*" 开始, 以 "*/" 结束
/*这是个注释*/
p
{
text-align:center;
/*这是另一个注释*/
color:black;
font-family:arial;
}
```

## id 和 class 选择器
```css
id 选择器
HTML元素以id属性来设置id选择器,CSS 中 id 选择器以 "#" 来定义
#para1
{
    text-align:center;
    color:red;
}

class 选择器
class 选择器用于描述一组元素的样式，class 选择器有别于id选择器，class可以在多个元素中使用
class 选择器在HTML中以class属性表示, 在 CSS 中，类选择器以一个点"."号显示
.center {text-align:center;}
```
## 插入CSS
    外部样式表(External style sheet)
    当样式需要应用于很多页面时，外部样式表将是理想的选择。
    在使用外部样式表的情况下，你可以通过改变一个文件来改变整个站点的外观。
```css
<head>
<link rel="stylesheet" type="text/css" href="mystyle.css">
</head>
```
    内部样式表(Internal style sheet)
    当单个文档需要特殊的样式时，就应该使用内部样式表
```css
<head>
<style>
hr {color:sienna;}
p {margin-left:20px;}
body {background-image:url("images/back40.gif");}
</style>
</head>
```
    内联样式(Inline style)
    请慎用这种方法，例如当样式仅需要在一个元素上应用一次时。
```css
<p style="color:sienna;margin-left:20px">这是一个段落。</p>
```
    多重样式
    如果某些属性在不同的样式表中被同样的选择器定义，那么属性值将从更具体的样式表中被继承过来

    多重样式优先级
    内联样式> 内部样式 >外部样式 > 浏览器默认样式

## CSS 背景
```css
background-color 背景颜色
body {background-color:#b0c4de;}
颜色值通常以以下方式定义:
十六进制 - 如："#ff0000"
RGB - 如："rgb(255,0,0)"
颜色名称 - 如："red"

background-image 背景图像
body {background-image:url('paper.gif');}

水平或垂直平铺
body
{
background-image:url('gradient2.png');
background-repeat:repeat-x;
}
不平铺
body
{
background-image:url('img_tree.png');
background-repeat:no-repeat;
}

设置定位
body
{
background-image:url('img_tree.png');
background-repeat:no-repeat;
background-position:right top;
}

简写属性
body {background:#ffffff url('img_tree.png') no-repeat right top;}

当使用简写属性时，属性值的顺序为：:
background-color
background-image
background-repeat
background-attachment
background-position
以上属性无需全部使用
```

## CSS 文本格式
```css
文本颜色
body {color:red;}
h1 {color:#00ff00;}
h2 {color:rgb(255,0,0);}

对齐方式
h1 {text-align:center;}
p.date {text-align:right;}
p.main {text-align:justify;}

文本修饰
主要是用来删除链接的下划线
a {text-decoration:none;}
h1 {text-decoration:overline;}
h2 {text-decoration:line-through;}
h3 {text-decoration:underline;}

文本转换
大写、小写、首字母大写
p.uppercase {text-transform:uppercase;}
p.lowercase {text-transform:lowercase;}
p.capitalize {text-transform:capitalize;}

文本缩进
文本缩进属性是用来指定文本的第一行的缩进
p {text-indent:50px;}
```

## CSS 字体
```css
字体样式
p.normal {font-style:normal;}
p.italic {font-style:italic;}
p.oblique {font-style:oblique;}

字体大小
h1 {font-size:40px;}
h2 {font-size:30px;}
p {font-size:14px;}

用em来设置字体大小
1em的默认大小是16px
h1 {font-size:2.5em;} /* 40px/16=2.5em */
h2 {font-size:1.875em;} /* 30px/16=1.875em */
p {font-size:0.875em;} /* 14px/16=0.875em */

使用百分比和EM组合
body {font-size:100%;}
h1 {font-size:2.5em;}
h2 {font-size:1.875em;}
p {font-size:0.875em;}
```

## CSS 链接
```css
链接样式
a:link {color:#000000;}      /* 未访问链接*/
a:visited {color:#00FF00;}  /* 已访问链接 */
a:hover {color:#FF00FF;}  /* 鼠标移动到链接上 */
a:active {color:#0000FF;}  /* 鼠标点击时 */

常见的链接样式
文本修饰
text-decoration 属性主要用于删除链接中的下划线
a:link {text-decoration:none;}
a:visited {text-decoration:none;}
a:hover {text-decoration:underline;}
a:active {text-decoration:underline;}
背景颜色
a:link {background-color:#B2FF99;}
a:visited {background-color:#FFFF85;}
a:hover {background-color:#FF704D;}
a:active {background-color:#FF704D;}
```

## CSS 列表
```css
表项标记
list-style-type
ul.a {list-style-type:circle;}
ul.b {list-style-type:square;}
ol.c {list-style-type:upper-roman;}
ol.d {list-style-type:lower-alpha;}

列表项标记的图像
ul
{
    list-style-image: url('sqpurple.gif');
}

list-style  简写属性。用于把所有用于列表的属性设置于一个声明中
list-style-image    将图象设置为列表项标志。
list-style-position 设置列表中列表项标志的位置。
list-style-type     设置列表项标志的类型。
```

## CSS 表格
```css
表格边框
指定一个表格的Th和TD元素的黑色边框
table, th, td
{
    border: 1px solid black;
}

折叠边框
设置表格的边框是否被折叠成一个单一的边框或隔开
table
{
    border-collapse:collapse;
}
table,th, td
{
    border: 1px solid black;
}

表格宽度和高度
    width:100%;
    height:50px;

表格文字对齐
水平
text-align:right;
垂直
height:50px;
vertical-align:bottom;

表格填充
padding:15px;

表格颜色
边框
border:1px solid green;
文本和背景
background-color:green;
color:white;
```

## CSS 盒子模型
- Margin(外边距) - 清除边框外的区域，外边距是透明的。
- Border(边框) - 围绕在内边距和内容外的边框。
- Padding(内边距) - 清除内容周围的区域，内边距是透明的。
- Content(内容) - 盒子的内容，显示文本和图像。
![](box-model.gif)
```css
div {
    width: 300px;
    border: 25px solid green;
    padding: 25px;
    margin: 25px;
}
```

## CSS 边框
```css
边框样式
border-style
dotted: 定义一个点线边框
dashed: 定义一个虚线边框
solid: 定义实线边框
double: 定义两个边框。 两个边框的宽度和 border-width 的值相同
groove: 定义3D沟槽边框。效果取决于边框的颜色值
ridge: 定义3D脊边框。效果取决于边框的颜色值
inset:定义一个3D的嵌入边框。效果取决于边框的颜色值
outset: 定义一个3D突出边框。 效果取决于边框的颜色值

边框宽度
border-width:5px;

边框颜色
border-color

单独设置各边
p
{
    border-top-style:dotted;
    border-right-style:solid;
    border-bottom-style:dotted;
    border-left-style:solid;
}
```

## CSS 轮廓
```css
outline     在一个声明中设置所有的轮廓属性
outline-color
outline-style
outline-width
inherit

outline-color       设置轮廓的颜色
color-name
hex-number
rgb-number
invert
inherit

outline-style       设置轮廓的样式
none
dotted
dashed
solid
double
groove
ridge
inset
outset
inherit

outline-width       设置轮廓的宽度
thin
medium
thick
length
inherit
```

## CSS 分组和嵌套选择器
```css
分组选择器
h1,h2,p
{
    color:green;
}

嵌套选择器
p{ }: 为所有 p 元素指定一个样式。
.marked{ }: 为所有 class="marked" 的元素指定一个样式。
.marked p{ }: 为所有 class="marked" 元素内的 p 元素指定一个样式。
p.marked{ }: 为所有 class="marked" 的 p 元素指定一个样式。
```
## CSS 尺寸
```css
height      设置元素的高度。
line-height 设置行高。
max-height  设置元素的最大高度。
max-width   设置元素的最大宽度。
min-height  设置元素的最小高度。
min-width   设置元素的最小宽度。
width       设置元素的宽度。
```
## CSS Display(显示) 与 Visibility（可见性）
```css
隐藏元素
h1.hidden {visibility:hidden;}
visibility:hidden可以隐藏某个元素，但隐藏的元素仍需占用与未隐藏之前一样的空间
h1.hidden {display:none;}
display:none可以隐藏某个元素，且隐藏的元素不会占用任何空间
```
## CSS Display - 块和内联元素
```css
块元素是一个元素，占用了全部宽度，在前后都是换行符。
<h1>
<p>
<div>
内联元素只需要必要的宽度，不强制换行。
<span>
<a>
改变一个元素显示
li {display:inline;}
span {display:block;}
```
## CSS Position(定位)
```css
static 定位
HTML 元素的默认值，即没有定位，遵循正常的文档流对象
div.static {
    position: static;
    border: 3px solid #73AD21;
}

fixed 定位
元素的位置相对于浏览器窗口是固定位置。
即使窗口是滚动的它也不会移动
p.pos_fixed
{
    position:fixed;
    top:30px;
    right:5px;
}

relative 定位
相对定位元素的定位是相对其正常位置
h2.pos_left
{
    position:relative;
    left:-20px;
}
h2.pos_right
{
    position:relative;
    left:20px;
}

absolute 定位
绝对定位的元素的位置相对于最近的已定位父元素，如果元素没有已定位的父元素，那么它的位置相对于<html>
h2
{
    position:absolute;
    left:100px;
    top:150px;
}
absolute 定位使元素的位置与文档流无关，因此不占据空间。
absolute 定位的元素和其他元素重叠

sticky 定位
当页面滚动超出目标区域时，它的表现就像 position:fixed;，它会固定在目标位置
指定 top, right, bottom 或 left 四个阈值其中之一
div.sticky {
    position: -webkit-sticky; /* Safari */
    position: sticky;
    top: 0;
    background-color: green;
    border: 2px solid #4CAF50;
}

重叠的元素
具有更高堆叠顺序的元素总是在较低的堆叠顺序元素的前面
如果两个定位元素重叠，没有指定z - index，最后定位在HTML代码中的元素将被显示在最前面
img
{
    position:absolute;
    left:0px;
    top:0px;
    z-index:-1;
}
```
## CSS 布局 - Overflow
    用于控制内容溢出元素框时显示的方式
```css
CSS Overflow
控制内容溢出元素框时在对应的元素区间内添加滚动条
overflow:
visible     默认值。内容不会被修剪，会呈现在元素框之外。
hidden      内容会被修剪，并且其余内容是不可见的。
scroll      内容会被修剪，但是浏览器会显示滚动条以便查看其余的内容。
auto        如果内容被修剪，则浏览器会显示滚动条以便查看其余的内容。
inherit     规定应该从父元素继承 overflow 属性的值。
```
## CSS Float(浮动)
    会使元素向左或向右移动，其周围的元素也会重新排列
```css
如果图像是右浮动，下面的文本流将环绕在它左边
img
{
    float:right;
}

几个浮动的元素放到一起，如果有空间的话，它们将彼此相邻
.thumbnail
{
    float:left;
    width:110px;
    height:90px;
    margin:5px;
}

清除浮动
.text_line
{
    clear:both;
}
```
## CSS 布局 - 水平 & 垂直对齐
```css
元素居中对齐
margin: auto;

文本居中对齐
text-align: center;

左右对齐
position: absolute;
float: right;

可以在父元素上添加 overflow: auto; 来解决子元素溢出的问题:
overflow: auto;

垂直居中对齐
padding: 70px 0;

垂直居中
line-height: 200px;
```

## CSS 组合选择符
```css
后代选择器(以空格分隔)
div p
{
background-color:yellow;
}

子元素选择器(以大于号分隔）
div>p
{
background-color:yellow;
}

相邻兄弟选择器（以加号分隔）
可选择紧接在另一元素后的元素，且二者有相同父元素
div+p
{
background-color:yellow;
}

普通兄弟选择器（以波浪线分隔）
取所有指定元素之后的相邻兄弟元素
div~p
{
background-color:yellow;
}
```
## CSS 伪类
    selector:pseudo-class {property:value;}
```css
anchor伪类
a:link {color:#FF0000;} /* 未访问的链接 */
a:visited {color:#00FF00;} /* 已访问的链接 */
a:hover {color:#FF00FF;} /* 鼠标划过链接 */
a:active {color:#0000FF;} /* 已选中的链接 */

first-child 伪类
选择父元素的第一个子元素
在IE8的之前版本必须声明<!DOCTYPE> ，这样 :first-child 才能生效

lang 伪类
为不同的语言定义特殊的规则
```

## CSS 伪元素
    selector:pseudo-element {property:value;}
```css
first-line 伪元素
用于向文本的首行设置特殊样式
p:first-line
{
    color:#ff0000;
    font-variant:small-caps;
}

first-letter 伪元素

before 伪元素
在元素的内容前面插入新内容
h1:before
{
    content:url(smiley.gif);
}

after 伪元素
```

## CSS 导航栏
```css
链接列表
<ul>
<li><a href="#home">主页</a></li>
<li><a href="#news">新闻</a></li>
<li><a href="#contact">联系</a></li>
<li><a href="#about">关于</a></li>
</ul>

删除边距和填充
ul {
    list-style-type: none;
    margin: 0;
    padding: 0;
    width: 200px;
    background-color: #f1f1f1;
}

垂直导航栏
li a {
    display: block;
    color: #000;
    padding: 8px 16px;
    text-decoration: none;
}

鼠标移动到选项上修改背景颜色
li a:hover {
    background-color: #555;
    color: white;
}

激活/当前导航条实例
在点击了选项后，我们可以添加 "active" 类来标准哪个选项被选中
li a.active {
    background-color: #4CAF50;
    color: white;
}
未选中项在移至上方时的前景背景色
li a:hover:not(.active) {
    background-color: #555;
    color: white;
}

创建链接并添加边框
ul {
    border: 1px solid #555;
}

li {
    text-align: center;
    border-bottom: 1px solid #555;
}

li:last-child {
    border-bottom: none;
}

全屏高度的固定导航条
ul {
    list-style-type: none;
    margin: 0;
    padding: 0;
    width: 25%;
    background-color: #f1f1f1;
    height: 100%; /* 全屏高度 */
    position: fixed;
    overflow: auto; /* 如果导航栏选项多，允许滚动 */
}

水平导航栏
有两种方法创建横向导航栏。使用内联(inline)或浮动(float)的列表项
这两种方法都很好，但如果你想链接到具有相同的大小，你必须使用浮动的方法。

内联列表项
li
{
    display:inline;
}
默认情况下，<li>元素是块元素。在这里，我们删除换行符之前和之后每个列表项，以显示一行

浮动列表项
li
{
    float:left;
}
a
{
    display:block;
    width:60px;
}

水平导航条实例
ul {
    list-style-type: none;
    margin: 0;
    padding: 0;
    overflow: hidden;
    background-color: #333;
}
li {
    float: left;
}
li a {
    display: block;
    color: white;
    text-align: center;
    padding: 14px 16px;
    text-decoration: none;
}
/*鼠标移动到选项上修改背景颜色 */
li a:hover {
    background-color: #111;
}

激活/当前导航条实例
.active {
    background-color: #4CAF50;
}

链接右对齐
将导航条最右边的选项设置右对齐 (float:right;)
<ul>
<li><a href="#home">主页</a></li>
<li><a href="#news">新闻</a></li>
<li><a href="#contact">联系</a></li>
<li style="float:right"><a class="active" href="#about">关于</a></li>
</ul>

添加分割线
<li> 通过 border-right 样式来添加分割线
/* 除了最后一个选项(last-child) 其他的都添加分割线 */
li {
    border-right: 1px solid #bbb;
}

li:last-child {
    border-right: none;
}

固定导航条
固定在头部
ul {
    position: fixed;
    top: 0;
    width: 100%;
}

灰色水平导航条
ul {
    border: 1px solid #e7e7e7;
    background-color: #f3f3f3;
}

li a {
    color: #666;
}
```

## CSS 下拉菜单
```css
基本下拉菜单
<style>
.dropdown {
    position: relative;
    display: inline-block;
}

.dropdown-content {
    display: none;
    position: absolute;
    background-color: #f9f9f9;
    min-width: 160px;
    box-shadow: 0px 8px 16px 0px rgba(0,0,0,0.2);
    padding: 12px 16px;
    z-index: 1;
}

.dropdown:hover .dropdown-content {
    display: block;
}
</style>

<div class="dropdown">
<span>Mouse over me</span>
<div class="dropdown-content">
    <p>Hello World!</p>
</div>
</div>

下拉菜单
<style>
/* 下拉按钮样式 */
.dropbtn {
    background-color: #4CAF50;
    color: white;
    padding: 16px;
    font-size: 16px;
    border: none;
    cursor: pointer;
}

/* 容器 <div> - 需要定位下拉内容 */
.dropdown {
    position: relative;
    display: inline-block;
}

/* 下拉内容 (默认隐藏) */
.dropdown-content {
    display: none;
    position: absolute;
    background-color: #f9f9f9;
    min-width: 160px;
    box-shadow: 0px 8px 16px 0px rgba(0,0,0,0.2);
}

/* 下拉菜单的链接 */
.dropdown-content a {
    color: black;
    padding: 12px 16px;
    text-decoration: none;
    display: block;
}

/* 鼠标移上去后修改下拉菜单链接颜色 */
.dropdown-content a:hover {background-color: #f1f1f1}

/* 在鼠标移上去后显示下拉菜单 */
.dropdown:hover .dropdown-content {
    display: block;
}

/* 当下拉内容显示后修改下拉按钮的背景颜色 */
.dropdown:hover .dropbtn {
    background-color: #3e8e41;
}
</style>

<div class="dropdown">
<button class="dropbtn">下拉菜单</button>
<div class="dropdown-content">
    <a href="#">菜鸟教程 1</a>
    <a href="#">菜鸟教程 2</a>
    <a href="#">菜鸟教程 3</a>
</div>
</div>

下拉内容对齐方式
float:left;
float:right;
```

## CSS 提示工具(Tooltip)
```css
基础提示框(Tooltip)
<style>
/* Tooltip 容器 */
.tooltip {
    position: relative;
    display: inline-block;
    border-bottom: 1px dotted black; /* 悬停元素上显示点线 */
}

/* Tooltip 文本 */
.tooltip .tooltiptext {
    visibility: hidden;
    width: 120px;
    background-color: black;
    color: #fff;
    text-align: center;
    padding: 5px 0;
    border-radius: 6px;

    /* 定位 */
    position: absolute;
    z-index: 1;
}

/* 鼠标移动上去后显示提示框 */
.tooltip:hover .tooltiptext {
    visibility: visible;
}
</style>

<div class="tooltip">鼠标移动到这
<span class="tooltiptext">提示文本</span>
</div>


定位提示工具
显示在右侧：
.tooltip .tooltiptext {
    top: -5px;
    left: 105%;
}
显示在左侧：
.tooltip .tooltiptext {
    top: -5px;
    right: 105%;
}
显示在头部：
.tooltip .tooltiptext {
    width: 120px;
    bottom: 100%;
    left: 50%;
    margin-left: -60px; /* 使用一半宽度 (120/2 = 60) 来居中提示工具 */
}
显示在底部：
.tooltip .tooltiptext {
    width: 120px;
    top: 100%;
    left: 50%;
    margin-left: -60px; /* 使用一半宽度 (120/2 = 60) 来居中提示工具 */
}

添加箭头
顶部提示框/底部箭头：
.tooltip .tooltiptext::after {
    content: " ";
    position: absolute;
    top: 100%; /* 提示工具底部 */
    left: 50%;
    margin-left: -5px;
    border-width: 5px;
    border-style: solid;
    border-color: black transparent transparent transparent;
}

淡入效果
.tooltip .tooltiptext {
    opacity: 0;
    transition: opacity 1s;
}

.tooltip:hover .tooltiptext {
    opacity: 1;
}
```

## CSS 图片廊
## CSS 图像透明/不透明
## CSS 图像拼合技术
## CSS 媒体类型
## CSS 属性 选择器











