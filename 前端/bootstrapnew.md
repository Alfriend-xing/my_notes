# bootstrap4
---

[布局](#布局)
[排版](#排版)
[组件](#组件)
[实用工具](#实用工具)

## 布局
容器(Containers)
>最基础的布局元素，使用grid的前提
```html
<div class="container"></div>
<!--默认不占满全屏，在不同分辨率下有不同的最大宽度-->

<div class="container-fluid"></div>
<!--在所有分辨率下都为100%宽度 fluid(流体)-->
```
响应式断点(breakpoints)
>构成响应式的基础，允许我们在视口更改时放大元素, 
> .col-{value} 为预设值，即随最大值而变化，但占比不变

断点名称 | 类名 | 屏幕尺寸 | 容器最大宽度 | 
--|--|--|--|
极小 | col-xs-(官方文档中此处是col-) | 小于576px | None |
小 | col-sm- | 大于576px | 540px |
中 | col-md- | 大于768px | 720px |
大 | col-lg- | 大于992px | 960px |
极大 | col-xl- | 大于1200px | 1140px |

网格(grid)
>使用容器，行，列，响应断点来布局和对齐元素
```html
<div class="container">
  <div class="row">
    <div class="col-sm">
      One of three columns
    </div>
    <div class="col-sm">
      One of three columns
    </div>
    <div class="col-sm">
      One of three columns
    </div>
  </div>
</div>

<div class="w-100"></div>
<!--用在两列中，将两列从同一行中断为两行，现阶段认为没什么卵用-->
<!--加了满宽0高的一行-->

<div class="col-6"></div>
<!--指定宽度6列-->

<div class="col-md-auto"></div>
<!--列宽随内容宽度变化-->

<div class="row justify-content-md-center"></div>
<!--该行居中 content-around(前中后有间隔) content-between(列与列间隔) -->

<div class="row align-items-start">
<!--该行垂直置顶，需指定父元素高度 否则无效-->

<div class="col-12 col-md-8">.col-12 .col-md-8</div>
<!--针对不同情况使用不同宽度-->

<div class="col-6">.col-6</div>
<!--总是使用50%宽度，无论电脑还是手机-->

<div class="container px-lg-5"></div>
<div class="row mx-lg-n5"></div>
<!--增加和减少内外边距 negative(负)-->
```
>- container 封装了row，row封装了column
>- 每个column都有可见的水平填充(padding)用于控制间距
>- 在网格布局中，所有元素必须放在列元素中(column)，
>- 列元素必须是row的直接子元素，嵌套列元素需先创建row
>- 上例中.col-sm(或.col)未指定使用的列数，则它与同一行中的其他列等宽
>- .col-4表示使用当前分辨率的4列作为宽度，可选1-12，是父元素宽度的一定比例
>- 要去掉内外边距，在row中使用no-gutters类
>- 每行列数和大于12时 自动换行
>- 加入order-12的元素会放至行尾 order-1放至行首 不受代码中的位置影响
>- 还有order-last order-first
>- 偏移 offset-md-3

用于布局的实用程序
- display改变元素的可见性
- Flexbox 使用.d-flex或.d-sm-flex实现display: flex
- 内外边距 .mr-3(.mr-md-3)表示margin-right: 1rem

## 排版
标题
```html
<p class="h1">h1. Bootstrap heading</p>
<p class="h6">h6. Bootstrap heading</p>
<h1 class="display-1">Display 1</h1>
<h1 class="display-4">Display 4</h1>
```
段落
```html
<p class="lead">aa</p>
```
[文字修饰](https://getbootstrap.com/docs/4.3/content/typography/#inline-text-elements)

列表
```html
<!--去掉列表修饰符-->
<ul class="list-unstyled"></ul>

单行显示列表
<ul class="list-inline">
  <li class="list-inline-item">Lorem ipsum</li>
  <li class="list-inline-item">Phasellus iaculis</li>
  <li class="list-inline-item">Nulla volutpat</li>
</ul>
```
显示代码
```html
行内代码
<code>code</code>

多行代码
<pre><code>code
code
</code></pre>

按键
<kbd>ctrl</kbd>
```
图片
```html
<img src="..." class="img-fluid" alt="Responsive image">
<!--.img-fluid 表示 max-width: 100%; and height: auto; -->

<img class="img-thumbnail">
<!--为图片加1px边框表示缩略图-->

<img src="..." class="rounded float-left" alt="...">
<!--设置图片位置-->

<img src="..." class="rounded mx-auto d-block" alt="...">
<!--单行居中显示图片-->
```
表格
```html
<table class="table">
  <thead>
    <tr>
      <th scope="col">#</th>
      <th scope="col">First</th>
      <th scope="col">Last</th>
      <th scope="col">Handle</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">1</th>
      <td>Mark</td>
      <td>Otto</td>
      <td>@mdo</td>
    </tr>
    <tr>
      <th scope="row">2</th>
      <td>Jacob</td>
      <td>Thornton</td>
      <td>@fat</td>
    </tr>
    <tr>
      <th scope="row">3</th>
      <td>Larry</td>
      <td>the Bird</td>
      <td>@twitter</td>
    </tr>
  </tbody>
</table>
```
```html
<!--可选参数-->

<!--黑色表格-->
<table class="table table-dark"></table>

<!--表头颜色-->
<thead class="thead-dark"></thead>
<thead class="thead-light"></thead>

<!--条纹行-->
<table class="table table-striped"></table>

<!--添加竖线-->
<table class="table table-bordered"></table>

<!--去掉横竖分割线(无线表格)-->
<table class="table table-borderless"></table>

<!--响应鼠标轨迹(高亮鼠标当前所在行)-->
<table class="table table-hover"></table>

<!--紧凑型表格(内边距减半)-->
<table class="table table-sm"></table>

<!--上色(使用上下文类，对黑色表格无效，可用text和bg上色)-->
<tr class="table-active">...</tr>
<td class="table-active">...</td>

<!--响应式表格(根据显示器设置表格最大宽度576px,768px,992px,1120px
超出宽度变为水平滚动表)-->
<table class="table-responsive-{breakpoint}"></table>
<!--总是响应-->
<div class="table-responsive">
  <table class="table">
  </table>
</div>

<!--表格题目-->
<table class="table">
  <caption>List of users</caption>
  <thead></thead></table>
```
## 组件

### 导航
```html
<ul class="nav">
  <li class="nav-item">
    <a class="nav-link" href="#">Active</a>
  </li>
</ul>

<!--简单版-->
<nav class="nav">
  <a class="nav-link" href="#">Active</a>
</nav>

<!--激活和禁用(tabindex表示使用tab键切换的顺序，-1无法切换
aria-disabled="true"表示此区域禁用交互)-->
<a class="nav-link active" href="#">Active</a>
<a class="nav-link disabled" href="#" tabindex="-1" aria-disabled="true">Disabled</a>

<!--水平居中(start end)-->
<ul class="nav justify-content-center"></nav>

<!--垂直导航栏-->
<ul class="nav flex-column">
  <li class="nav-item">
    <a class="nav-link" href="#">Active</a>
  </li>
  <li>...</li>
</ul>
<nav class="nav flex-column">
  <a class="nav-link" href="#">Active</a>
  <a>...</a>
</nav>

<!--选项卡(需要js)-->
<ul class="nav nav-tabs">
  <li class="nav-item">
    <a class="nav-link active" href="#">Active</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" href="#">Link</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" href="#">Link</a>
  </li>
  <li class="nav-item">
    <a class="nav-link disabled" href="#" tabindex="-1" aria-disabled="true">Disabled</a>
  </li>
</ul>

<!--胶囊样式-->
<ul class="nav nav-pills"></ul>

<!--均匀分布(<nav>内的标签需要加nav-item)-->
<ul class="nav nav-pills nav-fill"></ul>
<nav class="nav nav-pills nav-fill">
  <a class="nav-item nav-link" href="#">Active</a>
</nav>

<!--等宽(胶囊样式比较明显)-->
<ul class="nav nav-pills nav-justified"></ul>

<!--响应式(小屏幕时分多行显示)-->
<nav class="nav nav-pills flex-column flex-sm-row">
  <a class="flex-sm-fill text-sm-center nav-link" href="#">
  Longer nav link</a>
</nav>

<!--下拉菜单-->
<ul class="nav nav-tabs">
  <li class="nav-item dropdown">
    <a class="nav-link dropdown-toggle" data-toggle="dropdown" href="#" role="button" aria-haspopup="true" aria-expanded="false">Dropdown</a>
    <div class="dropdown-menu">
      <a class="dropdown-item" href="#">Action</a>
      <div class="dropdown-divider"></div>
      <a class="dropdown-item" href="#">Separated link</a>
    </div>
  </li>
</ul>

<!--选项卡(适用于胶囊和垂直样式)-->
<ul class="nav nav-tabs" id="myTab" role="tablist">
  <li class="nav-item">
    <a class="nav-link active" id="home-tab" data-toggle="tab" href="#home" role="tab" aria-controls="home" aria-selected="true">Home</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" id="profile-tab" data-toggle="tab" href="#profile" role="tab" aria-controls="profile" aria-selected="false">Profile</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" id="contact-tab" data-toggle="tab" href="#contact" role="tab" aria-controls="contact" aria-selected="false">Contact</a>
  </li>
</ul>
<div class="tab-content" id="myTabContent">
  <div class="tab-pane fade show active" id="home" role="tabpanel" aria-labelledby="home-tab">...</div>
  <div class="tab-pane fade" id="profile" role="tabpanel" aria-labelledby="profile-tab">...</div>
  <div class="tab-pane fade" id="contact" role="tabpanel" aria-labelledby="contact-tab">...</div>
</div>

<nav>
  <div class="nav nav-tabs" id="nav-tab" role="tablist">
    <a class="nav-item nav-link active" id="nav-home-tab" data-toggle="tab" href="#nav-home" role="tab" aria-controls="nav-home" aria-selected="true">Home</a>
    <a class="nav-item nav-link" id="nav-profile-tab" data-toggle="tab" href="#nav-profile" role="tab" aria-controls="nav-profile" aria-selected="false">Profile</a>
    <a class="nav-item nav-link" id="nav-contact-tab" data-toggle="tab" href="#nav-contact" role="tab" aria-controls="nav-contact" aria-selected="false">Contact</a>
  </div>
</nav>
<div class="tab-content" id="nav-tabContent">
  <div class="tab-pane fade show active" id="nav-home" role="tabpanel" aria-labelledby="nav-home-tab">...</div>
  <div class="tab-pane fade" id="nav-profile" role="tabpanel" aria-labelledby="nav-profile-tab">...</div>
  <div class="tab-pane fade" id="nav-contact" role="tabpanel" aria-labelledby="nav-contact-tab">...</div>
</div>
```
### 列表组
```html
<ul class="list-group">
  <li class="list-group-item">Cras justo odio</li>
  <li class="list-group-item">Dapibus ac facilisis in</li>
  <li class="list-group-item">Morbi leo risus</li>
  <li class="list-group-item">Porta ac consectetur ac</li>
  <li class="list-group-item">Vestibulum at eros</li>
</ul>

<!--激活和禁用-->
<li class="list-group-item active">Cras justo odio</li>
<li class="list-group-item disabled" aria-disabled="true">
Cras justo odio</li>

<!--链接和按钮-->
<a href="#" class="list-group-item list-group-item-action active">
Cras justo odio
</a>
<button type="button" class="list-group-item list-group-item-action active">
Cras justo odio
</button>

<!--去掉边框-->
<ul class="list-group list-group-flush"></ul>

<!--水平列表-->
<ul class="list-group list-group-horizontal"></ul>

<!--上色-->
<li class="list-group-item list-group-item-primary"></li>

<!--鼠标高亮响应-->
<a href="#" class="list-group-item list-group-item-action"></a>

<!--列表标签-->
<div class="row">
  <div class="col-4">
    <div class="list-group" id="list-tab" role="tablist">
      <a class="list-group-item list-group-item-action active" id="list-home-list" data-toggle="list" href="#list-home" role="tab" aria-controls="home">Home</a>
      <a class="list-group-item list-group-item-action" id="list-profile-list" data-toggle="list" href="#list-profile" role="tab" aria-controls="profile">Profile</a>
      <a class="list-group-item list-group-item-action" id="list-messages-list" data-toggle="list" href="#list-messages" role="tab" aria-controls="messages">Messages</a>
      <a class="list-group-item list-group-item-action" id="list-settings-list" data-toggle="list" href="#list-settings" role="tab" aria-controls="settings">Settings</a>
    </div>
  </div>
  <div class="col-8">
    <div class="tab-content" id="nav-tabContent">
      <div class="tab-pane fade show active" id="list-home" role="tabpanel" aria-labelledby="list-home-list">...</div>
      <div class="tab-pane fade" id="list-profile" role="tabpanel" aria-labelledby="list-profile-list">...</div>
      <div class="tab-pane fade" id="list-messages" role="tabpanel" aria-labelledby="list-messages-list">...</div>
      <div class="tab-pane fade" id="list-settings" role="tabpanel" aria-labelledby="list-settings-list">...</div>
    </div>
  </div>
</div>
```
### 按钮
```html
<button type="button" class="btn btn-primary">Primary</button>
<button type="button" class="btn btn-outline-primary">Primary</button>
<a class="btn btn-primary" href="#" role="button">Link</a>

<!--按钮尺寸-->
<button type="button" class="btn btn-primary btn-lg">Large button</button>
<button type="button" class="btn btn-primary btn-sm">Small button</button>
<button type="button" class="btn btn-primary btn-lg btn-block">Block level button</button>

<!--激活和禁用-->
<a href="#" class="btn btn-primary btn-lg active" role="button" aria-pressed="true">Primary link</a>
<button type="button" class="btn btn-lg btn-primary" disabled>Primary button</button>
<!--<a>标签不接受disabled属性 需要放置在类中-->
```
### 分页
```html
<nav aria-label="...">
  <ul class="pagination">
    <li class="page-item disabled">
      <span class="page-link">Previous</span>
    </li>
    <li class="page-item"><a class="page-link" href="#">1</a></li>
    <li class="page-item active" aria-current="page">
      <span class="page-link">
        2
        <span class="sr-only">(current)</span>
      </span>
    </li>
    <li class="page-item"><a class="page-link" href="#">3</a></li>
    <li class="page-item">
      <a class="page-link" href="#">Next</a>
    </li>
  </ul>
</nav>
<!--激活和禁用，禁用的项可以放在span中
Previous符号&laquo; Next符号&raquo;-->

<!--位置-->
<nav aria-label="Page navigation example">
  <ul class="pagination justify-content-end">
  </ul>
</nav>
```
### 进度条
```html
<div class="progress">
  <div class="progress-bar" role="progressbar" style="width: 25%" aria-valuenow="25" aria-valuemin="0" aria-valuemax="100"></div>
</div>
```
### 注释Tooltips
### 提示框Toasts
### 等待动画Spinners


## 实用工具
### 边框
```html
<span class="border"></span>
<span class="border border-primary"></span>
```
### 清除浮动
```html
<div class="bg-info clearfix">
  <button type="button" class="btn btn-secondary float-left">Example Button floated left</button>
  <button type="button" class="btn btn-secondary float-right">Example Button floated right</button>
</div>
<!--如果父元素没有clearfix，将不显示父元素-->
```
### 颜色
```html
<p class="text-primary">.text-primary</p>
<p class="text-secondary">.text-secondary</p>
<p class="text-success">.text-success</p>
<p class="text-danger">.text-danger</p>
<p class="text-warning">.text-warning</p>
<p class="text-info">.text-info</p>
<p class="text-light bg-dark">.text-light</p>
<p class="text-dark">.text-dark</p>
<p class="text-body">.text-body</p>
<p class="text-muted">.text-muted</p>
<p class="text-white bg-dark">.text-white</p>
<p class="text-black-50">.text-black-50</p>
<p class="text-white-50 bg-dark">.text-white-50</p>

<div class="p-3 mb-2 bg-primary text-white">.bg-primary</div>
<div class="p-3 mb-2 bg-secondary text-white">.bg-secondary</div>
<div class="p-3 mb-2 bg-success text-white">.bg-success</div>
<div class="p-3 mb-2 bg-danger text-white">.bg-danger</div>
<div class="p-3 mb-2 bg-warning text-dark">.bg-warning</div>
<div class="p-3 mb-2 bg-info text-white">.bg-info</div>
<div class="p-3 mb-2 bg-light text-dark">.bg-light</div>
<div class="p-3 mb-2 bg-dark text-white">.bg-dark</div>
<div class="p-3 mb-2 bg-white text-dark">.bg-white</div>
<div class="p-3 mb-2 bg-transparent text-dark">.bg-transparent</div>
<!--mb-2表示外边框(margin)底部(bottom)2rem宽
p-3表示内边框(padding)3rem宽
1rem=html根节点字体像素值
-->
```
### 弹性容器flex
```html
<!--创建弹性容器(块级元素弹性容器，内联元素弹性容器)-->
<div class="d-flex p-2 bd-highlight">I'm a flexbox container!</div>
<div class="d-inline-flex p-2 bd-highlight">I'm an inline flexbox container!</div>

<!--单行排列内部元素，无视其是否有块级属性-->
<div class="d-flex flex-row"></div>
<!--从右排列-->
<div class="d-flex flex-row-reverse"></div>

<!--按列排列内部元素，无视其是否有块级属性-->
<div class="d-flex flex-column"></div>
<div class="d-flex flex-column-reverse"></div>

<!--水平分布内部元素-->
<div class="d-flex justify-content-start">...</div>
<div class="d-flex justify-content-end">...</div>
<div class="d-flex justify-content-center">...</div>
<div class="d-flex justify-content-between">...</div>
<div class="d-flex justify-content-around">...</div>

<!--垂直分布内部元素-->
<div class="d-flex align-items-start">...</div>
<div class="d-flex align-items-end">...</div>
<div class="d-flex align-items-center">...</div>
<div class="d-flex align-items-baseline">...</div>
<div class="d-flex align-items-stretch">...</div>

<!--扩展元素宽度填满一行-->
<div class="d-flex bd-highlight">
  <div class="p-2 flex-fill bd-highlight">Flex item with a lot of content</div>
  <div class="p-2 flex-fill bd-highlight">Flex item</div>
  <div class="p-2 flex-fill bd-highlight">Flex item</div>
</div>

```

### 浮动布局float
```html
<!--靠左靠右 无浮动-->
<div class="float-left">Float left on all viewport sizes</div><br>
<div class="float-right">Float right on all viewport sizes</div><br>
<div class="float-none">Don't float on all viewport sizes</div>
```

### 固定位置fixed
```html
<div class="fixed-top">...</div>
<div class="fixed-bottom">...</div>
```
### 粘贴
```html
<div class="sticky-top">...</div>
```
### 内容溢出
```html
<div class="overflow-auto">...</div>
<div class="overflow-hidden">...</div>
```
### 尺寸
```html
<!--宽-->
<div class="w-25 p-3">Width 25%</div>
<div class="w-50 p-3">Width 50%</div>
<div class="w-75 p-3">Width 75%</div>
<div class="w-100 p-3">Width 100%</div>
<div class="w-auto p-3">Width auto</div>

<!--高-->
<div style="height: 100px; background-color: rgba(255,0,0,0.1);">
  <div class="h-25 d-inline-block" style="width: 120px; background-color: rgba(0,0,255,.1)">Height 25%</div>
</div>

<!--最大值-->
mw-100
mh-100
```
### 间距
```html
{property}{sides}-{size} 
{property}{sides}-{breakpoint}-{size}

property:
m- margin
p- padding

sides:
t- top
b- bottom
l- left
r- right
x- both *-left and *-right
y- both *-top and *-bottom
blank空白- 所有 4 sides

size:
0-  0
1- * .25
2- * .5
3- 1
4- * 1.5
5- * 3
auto- set the margin to auto
```
### 文字处理
[link](https://getbootstrap.com/docs/4.2/utilities/text/)

### 垂直分布
[link](https://getbootstrap.com/docs/4.2/utilities/vertical-align/)

---
经验
- 使用fixed之后，元素脱离之前的父元素，变为窗口的子元素，设置长宽可以按全屏取百分比