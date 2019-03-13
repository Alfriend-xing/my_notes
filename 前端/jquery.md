# jquery

## 基础

### 网页中添加 jQuery
- 从 jquery.com 下载 jQuery 库
    - Production version - 用于实际的网站中，已被精简和压缩。
    - Development version - 用于测试和开发（未压缩，是可读的代码）
    - `<script src="jquery-1.10.2.min.js"></script>`
- 从 CDN 中载入 jQuery, 如从 Google 中加载 jQuery
    - `<head> <script src="https://apps.bdimg.com/libs/jquery/2.1.4/jquery.min.js"> </script> </head>`

### jQuery 语法
    `$(selector).action()`
### 文档就绪事件
    为了防止文档在完全加载（就绪）之前运行 jQuery 代码
```js
$(document).ready(function(){
// 开始写 jQuery 代码...
});

简洁写法（与以上写法效果相同）:
$(function(){
// 开始写 jQuery 代码...
});
```
### jQuery 选择器
    基于元素的 id、类、类型、属性、属性值等"查找"（或选择）HTML 元素
```js
元素选择器
在页面中选取所有 <p> 元素:
$("p")

#id 选择器
$("#test")

.class 选择器
$(".test")

$("*")      选取所有元素
$(this)     选取当前 HTML 元素
$("p.intro")        选取 class 为 intro 的 <p> 元素
$("p:first")        选取第一个 <p> 元素
$("ul li:first")    选取第一个 <ul> 元素的第一个 <li> 元素
$("ul li:first-child")      选取每个 <ul> 元素的第一个 <li> 元素
$("[href]") 选取带有 href 属性的元素
$("a[target='_blank']")     选取所有 target 属性值等于 "_blank" 的 <a> 元素
$("a[target!='_blank']")    选取所有 target 属性值不等于 "_blank" 的 <a> 元素
$(":button")        选取所有 type="button" 的 <input> 元素 和 <button> 元素
$("tr:even")        选取偶数位置的 <tr> 元素
$("tr:odd") 选取奇数位置的 <tr> 元素


如果您的网站包含许多页面，并且您希望您的 jQuery 函数易于维护，那么请把您的 jQuery 函数放到独立的 .js 文件中
<head>
<script src="http://cdn.static.runoob.com/libs/jquery/1.10.2/jquery.min.js">
</script>
<script src="my_jquery_functions.js"></script>
</head>
```
### jQuery 事件
常见 DOM 事件:

鼠标事件 | 键盘事件 | 表单事件 | 文档/窗口事件
--------|--------|--------|--------
click | keypress | submit | load
dblclick | keydown | change | resize
mouseenter | keyup | focus | scroll
mouseleave | hover | blur | unload

```js
指定一个点击事件
$("p").click();

触发事件
$("p").click(function(){
    // 动作触发后执行的代码!!
});

常用事件方法
$(document).ready()在文档完全加载完后执行函数
click()点击事件
dblclick()双击元素
mouseenter()鼠标指针穿过元素
mouseleave()鼠标指针离开元素
mousedown()移动到元素上方并按下鼠标按键
mouseup()在元素上松开鼠标按钮

hover()模拟光标悬停事件
$("#p1").hover(
    function(){
        alert("你进入了 p1!");
    },
    function(){
        alert("拜拜! 现在你离开了 p1!");
    }
);

focus()元素获得焦点时，发生 focus 事件
$("input").focus(function(){
$(this).css("background-color","#cccccc");
});

blur()元素失去焦点时，发生 blur 事件
$("input").blur(function(){
$(this).css("background-color","#ffffff");
});
```
## 效果
### 隐藏和显示
```js
$("#hide").click(function(){
$("p").hide();
});
$("#show").click(function(){
$("p").show();
});

$(selector).hide(speed,callback);
$(selector).show(speed,callback);
可选的 speed 参数规定隐藏/显示的速度，可以取以下值："slow"、"fast" 或毫秒。
可选的 callback 参数是隐藏或显示完成后所执行的函数名称。

切换 hide() 和 show() 方法
$("button").click(function(){
$("p").toggle();
});
$(selector).toggle(speed,callback);
```
### 淡入淡出
```js
淡入已隐藏的元素
$(selector).fadeIn(speed,callback);

淡出可见元素
$(selector).fadeOut(speed,callback);

在 fadeIn() 与 fadeOut() 方法之间进行切换
$(selector).fadeToggle(speed,callback);

允许渐变为给定的不透明度（值介于 0 与 1 之间）
$(selector).fadeTo(speed,opacity,callback);
$("button").click(function(){
$("#div1").fadeTo("slow",0.15);
$("#div2").fadeTo("slow",0.4);
$("#div3").fadeTo("slow",0.7);
});
```
### 滑动
```js
向下滑动元素
$(selector).slideDown(speed,callback);
"slow"、"fast" 或毫秒

向上滑动元素
$(selector).slideUp(speed,callback);

在 slideDown() 与 slideUp() 方法之间进行切换
$(selector).slideToggle(speed,callback);
```
### 动画
```js
创建自定义动画
$(selector).animate({params},speed,callback);
必需的 params 参数定义形成动画的 CSS 属性。
可选的 speed 参数规定效果的时长。它可以取以下值："slow"、"fast" 或毫秒。
可选的 callback 参数是动画完成后所执行的函数名称。
把 <div> 元素往右边移动 250 像素
$("button").click(function(){
$("div").animate({left:'250px'});
});

操作多个属性
$("button").click(function(){
$("div").animate({
    left:'250px',
    opacity:'0.5',
    height:'150px',
    width:'150px'
});
});

使用 animate() 时，必须使用 Camel 标记法书写所有的属性名，
比如，必须使用 paddingLeft 而不是 padding-left，
使用 marginRight 而不是 margin-right
要生成颜色动画，需要下载颜色动画插件

使用相对值(该值相对于元素的当前值)
$("button").click(function(){
$("div").animate({
    left:'250px',
    height:'+=150px',
    width:'+=150px'
});
});

使用预定义的值
$("button").click(function(){
$("div").animate({
    height:'toggle'
});
});

使用队列功能
$("button").click(function(){
var div=$("div");
div.animate({height:'300px',opacity:'0.4'},"slow");
div.animate({width:'300px',opacity:'0.8'},"slow");
div.animate({height:'100px',opacity:'0.4'},"slow");
div.animate({width:'100px',opacity:'0.8'},"slow");
});

停止动画
用于在动画完成之前停止动画
可选的 stopAll 参数规定是否应该清除动画队列。默认是 false，即仅停止活动的动画，允许任何排入队列的动画向后执行。
可选的 goToEnd 参数规定是否立即完成当前动画。默认是 false。
$(selector).stop(stopAll,goToEnd);
```
### Callback 方法
```js
callback
$("button").click(function(){
$("p").hide("slow",function(){
    alert("段落现在被隐藏了");
});
});

没有callback则会立即执行后面的函数
$("button").click(function(){
$("p").hide(1000);
alert("段落现在被隐藏了");
});
```
### 链(Chaining)
    允许我们在一条语句中对一个元素运行多个 jQuery 方法
```js
$("#p1").css("color","red").slideUp(2000).slideDown(2000);

建议格式
$("#p1").css("color","red")
.slideUp(2000)
.slideDown(2000);
```
## jQuery HTML

### 获取内容和属性
```js
text() - 设置或返回所选元素的文本内容
html() - 设置或返回所选元素的内容（包括 HTML 标记）
val() - 设置或返回表单字段的值
$("#btn1").click(function(){
alert("Text: " + $("#test").text());
});
$("#btn2").click(function(){
alert("HTML: " + $("#test").html());
});
$("#btn1").click(function(){
alert("值为: " + $("#test").val());
});

获取属性    attr()
$("button").click(function(){
alert($("#runoob").attr("href"));
});
```
### 设置内容和属性
```js
text() - 设置或返回所选元素的文本内容
html() - 设置或返回所选元素的内容（包括 HTML 标记）
val() - 设置或返回表单字段的值
$("#btn1").click(function(){
    $("#test1").text("Hello world!");
});
$("#btn2").click(function(){
    $("#test2").html("<b>Hello world!</b>");
});
$("#btn3").click(function(){
    $("#test3").val("RUNOOB");
});

text()、html() 以及 val() 的回调函数
回调函数有两个参数：被选元素列表中当前元素的下标，以及原始（旧的）值
$("#btn1").click(function(){
    $("#test1").text(function(i,origText){
        return "旧文本: " + origText + " 新文本: Hello world! ";
    });
});

设置属性 - attr()
$("button").click(function(){
$("#runoob").attr("href","http://www.runoob.com/jquery");
});
$("button").click(function(){
    $("#runoob").attr({
        "href" : "http://www.runoob.com/jquery",
        "title" : "jQuery 教程"
    });
});

attr() 的回调函数
回调函数有两个参数：被选元素列表中当前元素的下标，以及原始（旧的）值
$("button").click(function(){
$("#runoob").attr("href", function(i,origValue){
    return origValue + "/jquery";
});
});
```
### 添加元素
```js
append() - 在被选元素的结尾插入内容 仍在该元素的内部
prepend() - 在被选元素的开头插入内容 仍在该元素的内部
after() - 在被选元素之后插入内容
before() - 在被选元素之前插入内容

$("p").append("追加文本");
$("p").prepend("在开头追加文本");
function appendText()
{
    var txt1="<p>文本。</p>";              // 使用 HTML 标签创建文本
    var txt2=$("<p></p>").text("文本。");  // 使用 jQuery 创建文本
    var txt3=document.createElement("p");
    txt3.innerHTML="文本。";               // 使用 DOM 创建文本 text with DOM
    $("body").append(txt1,txt2,txt3);        // 追加新元素
}

$("img").after("在后面添加文本");
$("img").before("在前面添加文本");
function afterText()
{
    var txt1="<b>I </b>";                    // 使用 HTML 创建元素
    var txt2=$("<i></i>").text("love ");     // 使用 jQuery 创建元素
    var txt3=document.createElement("big");  // 使用 DOM 创建元素
    txt3.innerHTML="jQuery!";
    $("img").after(txt1,txt2,txt3);          // 在图片后添加文本
}
```
### 删除元素
```js
删除被选元素及其子元素
$("#div1").remove();

删除被选元素的子元素
$("#div1").empty();

过滤被删除的元素
删除 class="italic" 的所有 <p> 元素
$("p").remove(".italic");
```
### 获取并设置 CSS 类
```js
addClass() - 向被选元素添加一个或多个类
$("button").click(function(){
$("h1,h2,p").addClass("blue");
$("div").addClass("important");
});
$("button").click(function(){
$("body div:first").addClass("important blue");
});

removeClass() - 从被选元素删除一个或多个类
$("button").click(function(){
$("h1,h2,p").removeClass("blue");
});

toggleClass() - 对被选元素进行添加/删除类的切换操作
$("button").click(function(){
$("h1,h2,p").toggleClass("blue");
});

css() - 设置或返回样式属性
返回首个匹配元素的 background-color 值：
$("p").css("background-color");
为所有匹配元素设置 background-color 值
$("p").css("background-color","yellow");

设置多个 CSS 属性
$("p").css({"background-color":"yellow","font-size":"200%"});
```
### jQuery 尺寸
```js
width() 和 height()
width() 方法设置或返回元素的宽度（不包括内边距、边框或外边距）
height() 方法设置或返回元素的高度（不包括内边距、边框或外边距）
$("#div1").width();

innerWidth() 和 innerHeight()
innerWidth() 方法返回元素的宽度（包括内边距）
innerHeight() 方法返回元素的高度（包括内边距）
$("#div1").innerWidth();

outerWidth() 和 outerHeight()
outerWidth() 方法返回元素的宽度（包括内边距和边框）
outerHeight() 方法返回元素的高度（包括内边距和边框）
$("#div1").outerWidth();
```
## jQuery 遍历
```js
上遍历 DOM 树
parent()返回被选元素的直接父元素
parents()返回被选元素的所有祖先元素,它一路向上直到文档的根元素 (<html>)
parentsUntil()返回介于两个给定元素之间的所有祖先元素
$(document).ready(function(){
$("span").parents();
});
$(document).ready(function(){
$("span").parents("ul");
});
$(document).ready(function(){
$("span").parentsUntil("div");
});

向下遍历 DOM 树
children()返回被选元素的所有直接子元素
find()返回被选元素的后代元素，一路向下直到最后一个后代
$(document).ready(function(){
$("div").children();
});
返回类名为 "1" 的所有 <p> 元素，并且它们是 <div> 的直接子元素
$(document).ready(function(){
$("div").children("p.1");
});

$(document).ready(function(){
$("div").find("span");
});
$(document).ready(function(){
$("div").find("*");
});

在 DOM 树中水平遍历
siblings()返回被选元素的所有同胞元素
next()返回被选元素的下一个同胞元素
nextAll()返回被选元素的所有跟随的同胞元素
nextUntil()返回介于两个给定参数之间的所有跟随的同胞元素
prev()方向相反
prevAll()方向相反
prevUntil()方向相反

缩小搜索元素的范围
first()返回被选元素的首个元素
$("div p").first();
返回被选元素的最后一个元素
last()
$("div p").last();
返回被选元素中带有指定索引号的元素
eq()
选取第二个 <p> 元素
$("p").eq(1);
规定一个标准,不匹配这个标准的元素会被从集合中删除，匹配的元素会被返回
filter()
返回带有类名 "url" 的所有 <p> 元素
$("p").filter(".url");
返回不匹配标准的所有元素
not()
返回不带有类名 "url" 的所有 <p> 元素
$("p").not(".url");
```
## jQuery Ajax
    AJAX = 异步 JavaScript 和 XML（Asynchronous JavaScript and XML）。 简短地说，在不重载整个网页的情况下，AJAX 通过后台加载数据，并在网页上进行显示。
```js
load() 方法从服务器加载数据，并把返回的数据放入被选元素中
$(selector).load(URL,data,callback);
必需的 URL 参数规定您希望加载的 URL。
可选的 data 参数规定与请求一同发送的查询字符串键/值对集合。
可选的 callback 参数是 load() 方法完成后所执行的函数名称。

把 jQuery 选择器添加到 URL 参数
把 "demo_test.txt" 文件中 id="p1" 的元素的内容，加载到指定的 <div> 元素中
$("#div1").load("demo_test.txt #p1");

callback 参数
responseTxt - 包含调用成功时的结果内容
statusTXT - 包含调用的状态
xhr - 包含 XMLHttpRequest 对象
$("button").click(function(){
$("#div1").load("demo_test.txt",function(responseTxt,statusTxt,xhr){
    if(statusTxt=="success")
    alert("外部内容加载成功!");
    if(statusTxt=="error")
    alert("Error: "+xhr.status+": "+xhr.statusText);
});
});

get() 方法
$.get(URL,callback);
必需的 URL 参数规定您希望请求的 URL。
可选的 callback 参数是请求成功后所执行的函数名。
$("button").click(function(){
$.get("demo_test.php",function(data,status){
    alert("数据: " + data + "\n状态: " + status);
});
});

post() 方法
$.post(URL,data,callback);
必需的 URL 参数规定您希望请求的 URL。
可选的 data 参数规定连同请求发送的数据。
可选的 callback 参数是请求成功后所执行的函数名。
$("button").click(function(){
    $.post("/try/ajax/demo_test_post.php",
    {
        name:"菜鸟教程",
        url:"http://www.runoob.com"
    },
        function(data,status){
        alert("数据: \n" + data + "\n状态: " + status);
    });
});
```

