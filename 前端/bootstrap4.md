# bootstrap4

---

[TOC]

---

## 添加方式
```html
在head标签中添加
<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">
保为所有设备正确渲染和触摸缩放
<meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

在</body>前添加
<script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/umd/popper.min.js" integrity="sha384-UO2eT0CpHqdSJQ6hJty5KVphtPhzWj9WO1clHTMGa3JDZwrnQq4sF86dIHNDz0W1" crossorigin="anonymous"></script>
<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js" integrity="sha384-JjSmVgyd0p3pXB1rRibZUAYoIIy6OrQ6VrjIEaFf/nJGzIxFDsf4x0xIM+B07jRM" crossorigin="anonymous"></script>
```

## 布局

### 网格
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
```
- 断点
>共有五种断点使网格变得可响应

 .| 极小 | 小 | 中 | 大 | 极大
--|--|--|--|--|--|
响应范围 | 0-576px | ≥576 | ≥768 | ≥992 | ≥1200 |
最大容器宽度 | None (auto) | 540px | 720px | 960px | 1140px |
类前缀 | .col- | .col-sm- | .col-md- | .col-lg- | .col-xl- |
>使用 `.col `和` .col-*` 可匹配所有断点

- 设置列宽
```html
范围1-12，表示一行最多容纳12列，如果一行中放入col-6和col-10大于12，自动断行
<div class="col-6"></div>
<div class="col-5"></div>
<div class="col-sm-8">col-sm-8</div>
<div class="col-sm-4">col-sm-4</div>
```
- 可变宽度
```html
使用col-{breakpoint}-auto类
<div class="col-md-auto">
    Variable width content
</div>
```
- 垂直对齐
```html
上中下对齐
<div class="row align-items-start"></div>
<div class="row align-items-center"></div>
<div class="row align-items-end"></div>
```
- 水平对齐
```html
左中右对齐
<div class="row justify-content-start"></div>
<div class="row justify-content-center"></div>
<div class="row justify-content-end"></div>

各行之前、之间、之后都留有空白
<div class="row justify-content-around"></div>

各行之间留有空白
<div class="row justify-content-between"></div>
```
- 排序
```html
调整列顺序

放最后
<div class="col order-12"></div>
<div class="col order-last"></div>

放最前
<div class="col order-1"></div>
<div class="col order-first"></div>
```
- 偏移
```html
横向移动若干列
<div class="col-md-4">.col-md-4</div>
<div class="col-md-4 offset-md-4">.col-md-4 .offset-md-4</div>
```
- 嵌套
```html
<div class="container">
  <div class="row">
    <div class="col-sm-9">
      Level 1: .col-sm-9
      <!-- 在某一列中插入新行 -->
      <div class="row">
        <div class="col-8 col-sm-6">
          Level 2: .col-8 .col-sm-6
        </div>
        <div class="col-4 col-sm-6">
          Level 2: .col-4 .col-sm-6
        </div>
      </div>
    </div>
  </div>
</div>
```
## 排版

- 标题
```html
<h1>h1. Bootstrap heading</h1>
<h2>h2. Bootstrap heading</h2>
<h3>h3. Bootstrap heading</h3>
<h4>h4. Bootstrap heading</h4>
<h5>h5. Bootstrap heading</h5>
<h6>h6. Bootstrap heading</h6>
或
<p class="h1">h1. Bootstrap heading</p>
<p class="h2">h2. Bootstrap heading</p>
<p class="h3">h3. Bootstrap heading</p>
<p class="h4">h4. Bootstrap heading</p>
<p class="h5">h5. Bootstrap heading</p>
<p class="h6">h6. Bootstrap heading</p>

副标题
<h3>
  Fancy display heading
  <small class="text-muted">With faded secondary text</small>
</h3>

另一种方案
<h1 class="display-1">Display 1</h1>
<h1 class="display-2">Display 2</h1>
<h1 class="display-3">Display 3</h1>
<h1 class="display-4">Display 4</h1>
```
- 段落
```html
<p class="lead">
  Vivamus sagittis lacus vel augue laoreet rutrum faucibus dolor auctor. Duis mollis, est non commodo luctus.
</p>
```
- 内联文本元素
```html
高亮
<p>You can use the mark tag to <mark>highlight</mark> text.</p>

删除
<p><del>This line of text is meant to be treated as deleted text.</del></p>

表示不再准确
<p><s>This line of text is meant to be treated as no longer accurate.</s></p>

表示文档补充
<p><ins>This line of text is meant to be treated as an addition to the document.</ins></p>

表示下划线
<p><u>This line of text will render as underlined</u></p>

缩小
<p><small>This line of text is meant to be treated as fine print.</small></p>

加粗
<p><strong>This line rendered as bold text.</strong></p>

斜体
<p><em>This line rendered as italicized text.</em></p>
```
- 缩略语
```html
悬停时显示完整版本
<p><abbr title="attribute">attr</abbr></p>
<p><abbr title="HyperText Markup Language" class="initialism">HTML</abbr></p>
```
- 引用文字
```html
<blockquote class="blockquote">
  <p class="mb-0">Lorem ipsum dolor sit amet, consectetur adipiscing elit. Integer posuere erat a ante.</p>
</blockquote>
```
- 删除列表样式
```html
<ul class="list-unstyled">
  <li>Lorem ipsum dolor sit amet</li>
  <li>Consectetur adipiscing elit</li>
  <li>Integer molestie lorem at massa</li>
  <li>Facilisis in pretium nisl aliquet</li>
  <li>Nulla volutpat aliquam velit
</ul>
```
- 在一行中放入列
```html
<ul class="list-inline">
  <li class="list-inline-item">Lorem ipsum</li>
  <li class="list-inline-item">Phasellus iaculis</li>
  <li class="list-inline-item">Nulla volutpat</li>
</ul>
```
- 术语列表
```html
<dl class="row">
  <dt class="col-sm-3">Description lists</dt>
  <dd class="col-sm-9">A description list is perfect for defining terms.</dd>

  <dt class="col-sm-3">Euismod</dt>
  <dd class="col-sm-9">
    <p>Vestibulum id ligula porta felis euismod semper eget lacinia odio sem nec elit.</p>
    <p>Donec id elit non mi porta gravida at eget metus.</p>
  </dd>
</dl>
```
- 代码
```html
行内代码，&lt;section&gt;表示<section>
For example, <code>&lt;section&gt;</code> should be wrapped as inline.

代码块
<pre><code>&lt;p&gt;Sample text here...&lt;/p&gt;
&lt;p&gt;And another line of sample text here...&lt;/p&gt;
</code></pre>

变量
<var>y</var> = <var>m</var><var>x</var> + <var>b</var>

键盘输入(用户输入
To switch directories, type <kbd>cd</kbd> followed by the name of the directory.<br>
To edit settings, press <kbd><kbd>ctrl</kbd> + <kbd>,</kbd></kbd>)

程序输出，表示代码的执行结果
<samp>This text is meant to be treated as sample output from a computer program.</samp>
```
- 图片
```html
使图片随父元素缩放
<img src="..." class="img-fluid" alt="Responsive image">

缩略图
<img src="..." alt="..." class="img-thumbnail">

图片对齐
<img src="..." class="rounded float-left" alt="...">
<img src="..." class="rounded float-right" alt="...">
<img src="..." class="rounded mx-auto d-block" alt="...">
<div class="text-center">
  <img src="..." class="rounded" alt="...">
</div>
```
- 表格
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
  </tbody>
</table>

黑色表格
<table class="table table-dark">

表头
<thead class="thead-dark">
浅色
<thead class="thead-light">

条纹行
<table class="table table-striped">
深色
<table class="table table-striped table-dark">

边框表格
<table class="table table-bordered">
<table class="table table-bordered table-dark">

无边框表格
<table class="table table-borderless">
<table class="table table-borderless table-dark">

行悬停，高亮标注鼠标悬停的那一行
<table class="table table-hover">
<table class="table table-hover table-dark">

小表，空白填充减半，使表格紧凑
<table class="table table-sm">
<table class="table table-sm table-dark">

为表行或单元格上色
<tr class="table-active">...</tr>
<td class="table-active">...</td>
<th class="table-active">...</th>
table-active
table-primary
table-secondary
table-success
table-danger
table-warning
table-info
table-light
table-dark
使用文本或背景工具可以达到相似的效果
<tr class="bg-primary">...</tr>
<tr class="bg-success">...</tr>
<tr class="bg-warning">...</tr>
<tr class="bg-danger">...</tr>
<tr class="bg-info">...</tr>

表格标题
<table class="table">
  <caption>List of users</caption>
  <thead>
  </thead>

响应式表格，允许滚动，实际上使用了overflow-y: hidden
<div class="table-responsive">
  <table class="table">
    ...
  </table>
</div>
为响应式表格指定断点  .table-responsive{-sm|-md|-lg|-xl}
<div class="table-responsive-sm">
  <table class="table">
    ...
  </table>
</div>
当缩小网页时，指定断点xl的元素最先响应为可滚动元素(出现滚动条)
```
- 带注释的图像
```html
任何时候你需要显示一段内容 - 如带有可选标题的图像，请考虑使用<figure>
<figure class="figure">
  <img src="..." class="figure-img img-fluid rounded" alt="...">
  <figcaption class="figure-caption">A caption for the above image.</figcaption>
</figure>

文字靠右
<figure class="figure">
  <img src="..." class="figure-img img-fluid rounded" alt="...">
  <figcaption class="figure-caption text-right">A caption for the above image.</figcaption>
</figure>
```

## 组件

### 提醒框
>添加关闭按钮 需使用alerts jQuery plugin.
```html
<div class="alert alert-primary" role="alert">
  A simple primary alert—check it out!
</div>
alert-secondary
success
danger
warning
info
light
dark
```
>在提醒框中加入相同颜色的超链接
```html
<div class="alert alert-primary" role="alert">
  A simple primary alert with <a href="#" class="alert-link">an example link</a>. Give it a click if you like.
</div>
```
>在提醒框中加入其他html元素
```html
<div class="alert alert-success" role="alert">
  <h4 class="alert-heading">Well done!</h4>
  <p>Aww yeah, you successfully read this important alert message. This example text is going to run a bit longer so that you can see how spacing within an alert works with this kind of content.</p>
  <hr>
  <p class="mb-0">Whenever you need to, be sure to use margin utilities to keep things nice and tidy.</p>
</div>
```
>关闭按钮
```html
<div class="alert alert-warning alert-dismissible fade show" role="alert">
  <strong>Holy guacamole!</strong> You should check in on some of those fields below.
  <button type="button" class="close" data-dismiss="alert" aria-label="Close">
    <span aria-hidden="true">&times;</span>
  </button>
</div>
```
>使用js代码控制提醒框
```js
触发
$('.alert').alert()

关闭
$('.alert').alert('close')

删除
$('.alert').alert('dispose')
```
### 徽章
```html
<h1>Example heading <span class="badge badge-secondary">New</span></h1>

在按钮中
<button type="button" class="btn btn-primary">
  Notifications <span class="badge badge-light">4</span>
</button>
加入隐藏文本 ，介绍徽章含义
<button type="button" class="btn btn-primary">
  Profile <span class="badge badge-light">9</span>
  <span class="sr-only">unread messages</span>
</button>

其他外观
<span class="badge badge-primary">Primary</span>
Secondary Success Danger Warning Info Light Dark

胶囊徽章
<span class="badge badge-pill badge-primary">Primary</span>

可点击的徽章
<a href="#" class="badge badge-primary">Primary</a>
```
### 面包屑
>展示导航层次
```html
<nav aria-label="breadcrumb">
  <ol class="breadcrumb">
    <li class="breadcrumb-item"><a href="#">Home</a></li>
    <li class="breadcrumb-item"><a href="#">Library</a></li>
    <li class="breadcrumb-item active" aria-current="page">Data</li>
  </ol>
</nav>

更换分隔符
$breadcrumb-divider: quote(">");
```
### 按钮
>尽管.btn 类是为`<button> `元素设计的，但依然可用在`<a>`和`<input>`
```html
<button type="button" class="btn btn-primary">Primary</button>
Secondary Success Danger Warning Info Light Dark
<button type="button" class="btn btn-link">Link</button>

<a class="btn btn-primary" href="#" role="button">Link</a>
<button class="btn btn-primary" type="submit">Button</button>
<input class="btn btn-primary" type="button" value="Input">

只有轮廓无背景色的按钮
<button type="button" class="btn btn-outline-primary">Primary</button>

按钮尺寸
<button type="button" class="btn btn-primary btn-lg">Large button</button>
<button type="button" class="btn btn-primary btn-sm">Small button</button>
独占一行
<button type="button" class="btn btn-primary btn-lg btn-block">Block level button</button>

激活状态，表示刚被按下
<a href="#" class="btn btn-primary btn-lg active" role="button" aria-pressed="true">Primary link</a>
<a href="#" class="btn btn-secondary btn-lg active" role="button" aria-pressed="true">Link</a>

禁用状态，需添加disabled 布尔对象
<button type="button" class="btn btn-lg btn-primary" disabled>Primary button</button>
<button type="button" class="btn btn-secondary btn-lg" disabled>Button</button>
<a>标签不支持disabled对象，所以加.disabled类
<a href="#" class="btn btn-primary btn-lg disabled" tabindex="-1" role="button" aria-disabled="true">Primary link</a>
<a href="#" class="btn btn-secondary btn-lg disabled" tabindex="-1" role="button" aria-disabled="true">Link</a>

复选框和单选按钮
<div class="btn-group-toggle" data-toggle="buttons">
  <label class="btn btn-secondary active">
    <input type="checkbox" checked autocomplete="off"> Checked
  </label>
</div>

按钮组
<div class="btn-group" role="group" aria-label="Basic example">
  <button type="button" class="btn btn-secondary">Left</button>
  <button type="button" class="btn btn-secondary">Middle</button>
  <button type="button" class="btn btn-secondary">Right</button>
</div>

大小
<div class="btn-group btn-group-lg" role="group" aria-label="...">...</div>
<div class="btn-group" role="group" aria-label="...">...</div>
<div class="btn-group btn-group-sm" role="group" aria-label="...">...</div>

嵌套
<div class="btn-group" role="group" aria-label="Button group with nested dropdown">
  <button type="button" class="btn btn-secondary">1</button>
  <button type="button" class="btn btn-secondary">2</button>

  <div class="btn-group" role="group">
    <button id="btnGroupDrop1" type="button" class="btn btn-secondary dropdown-toggle" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
      Dropdown
    </button>
    <div class="dropdown-menu" aria-labelledby="btnGroupDrop1">
      <a class="dropdown-item" href="#">Dropdown link</a>
      <a class="dropdown-item" href="#">Dropdown link</a>
    </div>
  </div>
</div>

垂直按钮组
<div class="btn-group-vertical">
  ...
</div>
```
### 卡片
>可扩展容器
```html
<div class="card" style="width: 18rem;">
  <img src="..." class="card-img-top" alt="...">
  <div class="card-body">
    <h5 class="card-title">Card title</h5>
    <p class="card-text">Some quick example text to build on the card title and make up the bulk of the card's content.</p>
    <a href="#" class="btn btn-primary">Go somewhere</a>
  </div>
</div>

<div class="card-header">
<div class="card-body">
<h5 class="card-title">Card title</h5>
<a href="#" class="card-link">Card link</a>
<img class="card-img-top" src="..." alt="Card image">
<p class="card-text">
```
### 轮播
```html
<div id="carouselExampleIndicators" class="carousel slide" data-ride="carousel">
  <ol class="carousel-indicators">
    <li data-target="#carouselExampleIndicators" data-slide-to="0" class="active"></li>
    <li data-target="#carouselExampleIndicators" data-slide-to="1"></li>
    <li data-target="#carouselExampleIndicators" data-slide-to="2"></li>
  </ol>
  <div class="carousel-inner">
    <div class="carousel-item active">
      <img class="d-block w-100" src=".../800x400?auto=yes&bg=777&fg=555&text=First slide" alt="First slide">
    </div>
    <div class="carousel-item">
      <img class="d-block w-100" src=".../800x400?auto=yes&bg=666&fg=444&text=Second slide" alt="Second slide">
    </div>
    <div class="carousel-item">
      <img class="d-block w-100" src=".../800x400?auto=yes&bg=555&fg=333&text=Third slide" alt="Third slide">
    </div>
  </div>
  <a class="carousel-control-prev" href="#carouselExampleIndicators" role="button" data-slide="prev">
    <span class="carousel-control-prev-icon" aria-hidden="true"></span>
    <span class="sr-only">Previous</span>
  </a>
  <a class="carousel-control-next" href="#carouselExampleIndicators" role="button" data-slide="next">
    <span class="carousel-control-next-icon" aria-hidden="true"></span>
    <span class="sr-only">Next</span>
  </a>
</div>
```
### 折叠
```html
<p>
  <a class="btn btn-primary" data-toggle="collapse" href="#collapseExample" role="button" aria-expanded="false" aria-controls="collapseExample">
    Link with href
  </a>
  <button class="btn btn-primary" type="button" data-toggle="collapse" data-target="#collapseExample" aria-expanded="false" aria-controls="collapseExample">
    Button with data-target
  </button>
</p>
<div class="collapse" id="collapseExample">
  <div class="card card-body">
    Anim pariatur cliche reprehenderit, enim eiusmod high life accusamus terry richardson ad squid. Nihil anim keffiyeh helvetica, craft beer labore wes anderson cred nesciunt sapiente ea proident.
  </div>
</div>
```
### 下拉菜单
```html
<div class="dropdown">
  <button class="btn btn-secondary dropdown-toggle" type="button" id="dropdownMenuButton" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
    Dropdown button
  </button>
  <div class="dropdown-menu" aria-labelledby="dropdownMenuButton">
    <a class="dropdown-item" href="#">Action</a>
    <a class="dropdown-item" href="#">Another action</a>
    <a class="dropdown-item" href="#">Something else here</a>
  </div>
</div>
```
### 表单
```html
<form>
  <div class="form-group">
    <label for="exampleInputEmail1">Email address</label>
    <input type="email" class="form-control" id="exampleInputEmail1" aria-describedby="emailHelp" placeholder="Enter email">
    <small id="emailHelp" class="form-text text-muted">We'll never share your email with anyone else.</small>
  </div>
  <div class="form-group">
    <label for="exampleInputPassword1">Password</label>
    <input type="password" class="form-control" id="exampleInputPassword1" placeholder="Password">
  </div>
  <div class="form-group form-check">
    <input type="checkbox" class="form-check-input" id="exampleCheck1">
    <label class="form-check-label" for="exampleCheck1">Check me out</label>
  </div>
  <button type="submit" class="btn btn-primary">Submit</button>
</form>
```
### 列表组
```html
<ul class="list-group">
  <li class="list-group-item active">Cras justo odio</li>
  <li class="list-group-item">Dapibus ac facilisis in</li>
  <li class="list-group-item">Morbi leo risus</li>
  <li class="list-group-item">Porta ac consectetur ac</li>
  <li class="list-group-item">Vestibulum at eros</li>
</ul>

禁用
<li class="list-group-item disabled">Cras justo odio</li>

链接和按钮
<a href="#" class="list-group-item list-group-item-action">Dapibus ac facilisis in</a>
<button type="button" class="list-group-item list-group-item-action">Dapibus ac facilisis in</button>

移除边框和圆角
<ul class="list-group list-group-flush">
  <li class="list-group-item">Cras justo odio</li>
  <li class="list-group-item">Dapibus ac facilisis in</li>
</ul>

情景颜色
<li class="list-group-item list-group-item-primary">A simple primary list group item</li>

随意组合其他元素，徽章，其他HTML元素
```
### 对话框
```html
<!-- Button trigger modal -->
<button type="button" class="btn btn-primary" data-toggle="modal" data-target="#exampleModal">
  Launch demo modal
</button>

<!-- Modal -->
<div class="modal fade" id="exampleModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
  <div class="modal-dialog" role="document">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="exampleModalLabel">Modal title</h5>
        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
          <span aria-hidden="true">&times;</span>
        </button>
      </div>
      <div class="modal-body">
        ...
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
        <button type="button" class="btn btn-primary">Save changes</button>
      </div>
    </div>
  </div>
</div>
```
### 导航
```html
<ul class="nav justify-content-center">
  <li class="nav-item">
    <a class="nav-link" href="#">Active</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" href="#">Link</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" href="#">Link</a>
  </li>
  <li class="nav-item">
    <a class="nav-link disabled" href="#">Disabled</a>
  </li>
</ul>
```
### 分页
```html
禁用和活动状态,用 <span> 替换的活动或禁用的链接
<nav aria-label="...">
  <ul class="pagination">
    <li class="page-item disabled">
      <span class="page-link">Previous</span>
    </li>
    <li class="page-item"><a class="page-link" href="#">1</a></li>
    <li class="page-item active">
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
```
### 进度条
```html
<div class="progress">
  <div class="progress-bar" role="progressbar" style="width: 100%" aria-valuenow="100" aria-valuemin="0" aria-valuemax="100"></div>
</div>

加标签
<div class="progress">
  <div class="progress-bar" role="progressbar" style="width: 25%;" aria-valuenow="25" aria-valuemin="0" aria-valuemax="100">25%</div>
</div>

高度
<div class="progress" style="height: 1px;">
  <div class="progress-bar" role="progressbar" style="width: 25%;" aria-valuenow="25" aria-valuemin="0" aria-valuemax="100"></div>
</div>
<div class="progress" style="height: 20px;"></div>

背景
<div class="progress">
  <div class="progress-bar bg-danger" role="progressbar" style="width: 100%" aria-valuenow="100" aria-valuemin="0" aria-valuemax="100"></div>
</div>

动态条纹
<div class="progress">
  <div class="progress-bar progress-bar-striped progress-bar-animated" role="progressbar" aria-valuenow="75" aria-valuemin="0" aria-valuemax="100" style="width: 75%"></div>
</div>
```
### 滚动条

### 提示框
```html
鼠标悬停显示提示
<button type="button" class="btn btn-secondary" data-toggle="tooltip" data-placement="top" title="Tooltip on top">
  Tooltip on top
</button>
```
## 工具

### 边框
```html
加
<span class="border"></span>
<span class="border-top"></span>
<span class="border-right"></span>
<span class="border-bottom"></span>
<span class="border-left"></span>
减
<span class="border-0"></span>
<span class="border-top-0"></span>
<span class="border-right-0"></span>
<span class="border-bottom-0"></span>
<span class="border-left-0"></span>
颜色
<span class="border border-primary"></span>
Secondary Success Danger Warning Info Light Dark
圆角边框
<img src="..." alt="..." class="rounded">
<img src="..." alt="..." class="rounded-top">
```
### 清除浮动
>个人认为叫完全浮动才合适，用于使容器完全包裹内部的浮动元素
```html
<div class="bg-info clearfix">
  <button type="button" class="btn btn-secondary float-left">Example Button floated left</button>
  <button type="button" class="btn btn-secondary float-right">Example Button floated right</button>
</div>
```
### 关闭图表
>用于关闭提醒框(页面内部弹窗)和对话框(页面弹窗)
```html
<button type="button" class="close" aria-label="Close">
  <span aria-hidden="true">&times;</span>
</button>
```
### 颜色
```html
文字颜色
<p class="text-primary">.text-primary</p>
背景颜色
<div class="p-3 mb-2 bg-primary text-white">.bg-primary</div>
```
### 显示(display)属性
```html
.d-{value}
.d-{breakpoint}-{value}

value可选值：
隐藏元素
none
内联元素 
inline
内联块级元素
inline-block
块级元素
block
table
table-cell
table-row
flex
inline-flex
```
### 嵌入元素
```html
<div class="embed-responsive embed-responsive-16by9">
  <iframe class="embed-responsive-item" src="https://www.youtube.com/embed/zpOULjyy-n8?rel=0" allowfullscreen></iframe>
</div>
16by9为自定义宽高比(21by9,4by3,4by3)
```
### 弹性布局
```html
创建flex
.d-flex
.d-inline-flex
.d-{breakpoint}-flex
.d-{breakpoint}-inline-flex

方向
由左向右排列(.flex-row)
<div class="d-flex flex-row bd-highlight mb-3"></div>
右到左(.flex-row-reverse)
<div class="d-flex flex-row-reverse bd-highlight"></div>
上到下
flex-column
下到上
flex-column-reverse

位置
水平位置(.justify-content)
<div class="d-flex justify-content-start">...</div>
end center between(元素之间留白) around(元素前后和之间留白)
垂直位置(.flex-direction-)
flex-direction-{value}

对齐
垂直对齐方式(.align-items)
<div class="d-flex align-items-start">...</div>
align-items-start end center baseline stretch
自身对齐(.align-self-)
<div class="align-self-start">Aligned flex item</div>
end center baseline stretch

填充(.flex-fill)
<div class="d-flex bd-highlight">
  <div class="p-2 flex-fill bd-highlight">Flex item</div>
  <div class="p-2 flex-fill bd-highlight">Flex item</div>
  <div class="p-2 flex-fill bd-highlight">Flex item</div>
</div>

伸缩(.flex-grow- .flex-shrink-)
<div class="d-flex bd-highlight">
  <div class="p-2 flex-grow-1 bd-highlight">Flex item</div>
  <div class="p-2 bd-highlight">Flex item</div>
  <div class="p-2 bd-highlight">Third flex item</div>
</div>
<div class="d-flex bd-highlight">
  <div class="p-2 w-100 bd-highlight">Flex item</div>
  <div class="p-2 flex-shrink-1 bd-highlight">Flex item</div>
</div>

排序
<div class="d-flex flex-nowrap bd-highlight">
  <div class="order-3 p-2 bd-highlight">First flex item</div>
  <div class="order-2 p-2 bd-highlight">Second flex item</div>
  <div class="order-1 p-2 bd-highlight">Third flex item</div>
</div>

多行对齐
<div class="d-flex align-content-start flex-wrap">
  ...
</div>
```
### 浮动
```html
.float-{value}
.float-{breakpoint}-{value}

<div class="float-left">Float left on all viewport sizes</div><br>
<div class="float-right">Float right on all viewport sizes</div><br>
<div class="float-none">Don't float on all viewport sizes</div>
```
### 内容溢出
```html
<div class="overflow-auto">...</div>
<div class="overflow-hidden">...</div>
```
### 定位
```html
通用属性
<div class="position-static">...</div>
<div class="position-relative">...</div>
<div class="position-absolute">...</div>
<div class="position-fixed">...</div>
<div class="position-sticky">...</div>

固定在顶部
<div class="fixed-top">...</div>
固定在底部
<div class="fixed-bottom">...</div>

粘性置顶
.sticky-top
```
### 屏幕阅读器
```html
使用screenreader工具隐藏除屏幕阅读器之外的所有设备上的元素
.sr-only
```
### 阴影
```html
Bootstrap 中默认禁用组件上的阴影,需要通过启用 $enable-shadows
<div class="shadow-none p-3 mb-5 bg-light rounded">No shadow</div>
<div class="shadow-sm p-3 mb-5 bg-white rounded">Small shadow</div>
<div class="shadow p-3 mb-5 bg-white rounded">Regular shadow</div>
<div class="shadow-lg p-3 mb-5 bg-white rounded">Larger shadow</div>
```
### 尺寸
```html
宽和高
<div class="w-25 p-3" style="background-color: #eee;">Width 25%</div>
<div class="w-50 p-3" style="background-color: #eee;">Width 50%</div>
<div class="w-75 p-3" style="background-color: #eee;">Width 75%</div>
<div class="w-100 p-3" style="background-color: #eee;">Width 100%</div>
<div class="w-auto p-3" style="background-color: #eee;">Width auto</div>
<div style="height: 100px; background-color: rgba(255,0,0,0.1);">
  <div class="h-25 d-inline-block" style="width: 120px; background-color: rgba(0,0,255,.1)">Height 25%</div>
  <div class="h-50 d-inline-block" style="width: 120px; background-color: rgba(0,0,255,.1)">Height 50%</div>
  <div class="h-75 d-inline-block" style="width: 120px; background-color: rgba(0,0,255,.1)">Height 75%</div>
  <div class="h-100 d-inline-block" style="width: 120px; background-color: rgba(0,0,255,.1)">Height 100%</div>
  <div class="h-auto d-inline-block" style="width: 120px; background-color: rgba(0,0,255,.1)">Height auto</div>
</div>

设置最大值(max-width: 100%; and max-height: 100%;)
class="mw-100"
class="mh-100"
```
### 文本对齐
```html
text-justify
text-left(right,center)

让文本不换行
text-nowrap
截掉多余内容(块级元素)
text-truncate
截掉多余内容(行内元素)
d-inline-block text-truncate

其他
<p class="text-lowercase">Lowercased text.</p>
<p class="text-uppercase">Uppercased text.</p>
<p class="text-capitalize">CapiTaliZed text.</p>
<p class="font-weight-bold">Bold text.</p>
<p class="font-weight-normal">Normal weight text.</p>
<p class="font-weight-light">Light weight text.</p>
<p class="font-italic">Italic text.</p>
<p class="text-monospace">This is in monospace</p>
```
### 可见性
>与display的区别在于invisible时元素仍占据空间
```html
<div class="visible">...</div>
<div class="invisible">...</div>
```
### 图标
```html
<span class="glyphicon glyphicon-search"></span>
当作文字用
```
>bootstrap4默认不提供图标[link](https://getbootstrap.com/docs/4.3/migration/#components)，用其他图标还要引入新文件
也许打算卖图标盈利
[bootstrap3的图标](https://getbootstrap.com/docs/3.4/components/#glyphicons-glyphs)
而且bs3文档比bs4更易读







