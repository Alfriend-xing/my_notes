# bootstrap4
---
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







