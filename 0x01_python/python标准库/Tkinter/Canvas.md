Tkinter
---
>1. 该模块是(Tk interface)的缩写
>1. 运行`python-m Tkinter`打开tkinter示例窗口
>1. 在 Python 3中更名为tkinter

## Canvas 
>结构化的图形工具,用于作图，分块,图形编辑器，实现各种自定义组件(如进度条)
```python
from Tkinter import *

master = Tk()

w = Canvas(master, width=200, height=100)
w.pack()

w.create_line(0, 0, 200, 100)
w.create_line(0, 100, 200, 0, fill="red", dash=(4, 4))

w.create_rectangle(50, 25, 150, 75, fill="blue")

mainloop()
```
>请注意，添加到画布的项将一直保留，直到您删除它们。如果要更改图形，可以使用`coords`、`itemconfig`和`move`等方法修改项目，也可以使用`delete`删除项目。
```python
i = w.create_line(xy, fill="red")

w.coords(i, new_xy) # change coordinates
w.itemconfig(i, fill="blue") # change color

w.delete(i) # remove

w.delete(ALL) # remove all items
```
>概念：画布上新的内容会覆盖旧的内容,画布上的内容可以持续控制，可以绑定函数

>画布组件
`arc (arc, chord, or pieslice)`： 弧（弧、弦或饼）
`bitmap (built-in or read from XBM file)`：位图（内置或从XBM文件读取）
`image (a BitmapImage or PhotoImage instance)`：图像（位图图像或照片图像实例）
`line`：线
`oval：(a circle or an ellipse)`椭圆（圆或椭圆）
`polygon`：多边形
`rectangle`：矩形
`text`：文本
`window`：窗口

>参考信息
```python
Canvas(master=None, **options) #class
    #结构化图形画布，master父级组件，**options组件选项，也可以用config方法设置
    #只有这一个类，且方法跟多，下述不在缩进

addtag(tag, method, *args)
#tag，标签；method,方式，备选( “above”, “all”, “below”, “closest”, “enclosed”, “overlapping” or “withtag”.)
#推荐使用下面更具体的方法

addtag_above(tag, item)
#为给定项目上方的项目添加标签，item(参考项目的id/标签)

addtag_all(tag)
#为canvas的所有项目添加标签

addtag_below(tag, item)
#为给定项目下方的项目添加标签

addtag_closest(tag, x, y, halo=None, start=None)
#为给定坐标最近的项目添加标签,xy(坐标),halo(范围半径)，start(可选开始项)

addtag_enclosed(tag, x1, y1, x2, y2)
#为给定区域内所有项目添加标签(左右上下界)

addtag_overlapped(tag, x1, y1, x2, y2)
#为与给定区域重合或被包含的项目添加标签

addtag_withtag(tag, item)
#为属于某种标签/id的项目添加另一种标签，item(标签/id)

bbox(item=None)
#返回匹配项目的边界框(4元组(4-tuple)),未指定则返回所有项目

canvasx(x, gridspacing=None) 
#将窗口坐标转换为画布坐标;x(屏幕坐标),gridspacing(可选网格间距。坐标四舍五入到最近的网格坐标)

canvasy(y, gridspacing=None)
#将窗口坐标转换为画布坐标;y(屏幕坐标),gridspacing(可选网格间距。坐标四舍五入到最近的网格坐标)

config(**options)
#修改一个或多个组件的选项，如果没指定则返回包含左右当先选项的字典
background=/bg=#画布背景颜色
borderwidth=/bd=#边框宽度，默认0，表示无边框
closeenough=#默认值为1
confine=#默认值为1
cursor=#鼠标在画布上移动时要使用的光标
height=#画布宽度。默认值为'7c'
highlightbackground=#画布失焦时高亮的颜色
highlightcolor=#画布聚焦时边框的高亮颜色
highlightthickness=#高亮的边框宽度,默认1到2个像素
insertbackground=#文本插入光标的颜色
insertborderwidth=#插入光标边框的宽度。如果设置为非零值，则使用凸起的边框样式绘制光标
insertofftime=/insertontime=#此选项与InsertOnTime一起控制光标闪烁。两个值均以毫秒为单位
insertwidth=#插入光标的宽度。通常是一个或两个像素
offset=#默认值‘0,0’
relief=#边框风格FLAT,SUNKEN, RAISED, GROOVE,和RIDGE
scrollregion=#画布滚动区域,没有默认值
selectbackground=#选中区域颜色
selectborderwidth=#选中区域边框颜色
selectforeground=#选中区域前景色，即文字颜色
state=#画布状态 NORMAL, DISABLED,或HIDDEN，作用于画布内所有组件,但是组件的state选项会覆盖画布的state选项
takefocus=#可以使用tab键移动至画布，默认可用，但需要组件绑定键盘(输入)
width=#画布宽度，默认10c
xscrollcommand=#连接画布和水平滚动条，需绑定滚动条的set方法
xscrollincrement=#默认值为0
yscrollcommand=#连接画布和垂直滚动条，需绑定滚动条的set方法
yscrollincrement=#默认值为0

coords(item, *coords) 
#返回项目的坐标；item(tag/id),*coords(可选参数，会替换原坐标)

create_arc(bbox, **options)
#在画布上绘制弧、饼或弦。新项目绘制在现有项目的顶部，返回项目id
#bbox(完整圆弧的边界框)
**options{
activedash=#激活虚线(?)
activefill=#将鼠标指针移到项上时的填充颜色
activeoutline=#将鼠标指针移到项上时的边界颜色
activeoutlinestipple=
activestipple=
activewidth=#默认0.0
dash=#虚线轮廓，
dashoffset=#默认0
disableddash=
disabledfill=#无效时的填充颜色
disabledoutline=#无效时的轮廓颜色
disabledoutlinestipple=
disabledstipple=
disabledwidth=#默认0
extent=#开始角度，默认90
fill=#填充颜色，空字符串表示透明
offset=#默认“0,0”
outline=#轮廓颜色，默认黑色
outlineoffset=#默认“0,0”
outlinestipple=#轮廓点画图案
start=#开始角度，默认0.0
state=#项目状态
stipple=#点画形式
style=#PIESLICE, CHORD, or ARC之一，默认PIESLICE(饼、弦或弧)
tags=#设置标签，多个标签以元组表示
width=#默认1.0
    }

create_bitmap(position, **options)
#在画布上绘制位图，返回项目id
position#位图位置，以两个坐标表示
**options{
activebackground=
activebitmap=
activeforeground=
anchor=#相对于给定位置放置位图的位置。默认为CENTER
background=#背景色，用于“关闭”的像素。使用空字符串使背景透明。默认值是透明的
bitmap=#位图描述符
disabledbackground=
disabledbitmap=
disabledforeground=
foreground=#前景色，用于“开”的像素。默认为“黑色”
state=
tags=
}

create_image(position, **options)
#在画布上绘制图片，返回项目id
position#位图位置，以两个坐标表示
**options{
activeimage=
anchor=#相对于给定位置放置图片的位置。默认为CENTER
disabledimage=
image=#图片对象
state=
tags=
}

create_line(coords, **options)
#在画布上绘制一条线，返回项目id
coords#坐标
**options{
activedash=
activefill=#在指针下的颜色
activestipple=
activewidth=#默认0.0
arrow=#默认None
arrowshape=#默认8 10 3
capstyle=#默认BUTT
dash=#虚线模式，作为段长度列表给出。只绘制奇数段
dashoffset=#默认0
disableddash=
disabledfill=
disabledstipple=
disabledwidth=#默认 0.0
fill=#线的颜色，默认黑色
joinstyle=#默认ROUND
offset=#默认“0,0”
smooth=#默认0
splinesteps=#默认12
state=#NORMAL, DISABLED, or HIDDEN
stipple=#点画图案
tags=
width=#默认1.0
}

create_oval(bbox, **options)
#在画布上绘制椭圆，返回项目id
bbox#椭圆坐标
**options{
activedash=
activefill=
activeoutline=
activeoutlinestipple=
activestipple=
activewidth=
dash=
dashoffset=
disableddash=
disabledfill=
disabledoutline=
disabledoutlinestipple=
disabledstipple=
disabledwidth=#默认0
fill=
offset=#默认“0,0”
outline=#轮廓线 默认黑色
outlineoffset=#默认 0.0
outlinestipple=
state=
stipple=
tags=
width=
}

create_polygon(coords, **options)
#在画布上绘制多边形，返回项目id
coords#坐标
**options{
activedash=
activefill=
activeoutline=
activeoutlinestipple=
activestipple=
activewidth=
dash=
dashoffset=#默认0
disableddash=
disabledfill=
disabledoutline=
disabledoutlinestipple=
disabledstipple=
disabledwidth=#默认0.0
fill=
joinstyle=
offset=
outline=
outlineoffset=
outlinestipple=
smooth=#默认0
splinesteps=#默认12
state=
stipple=
tags=
width=
}

create_rectangle(bbox, **options) 
#在画布上绘制矩形
#同上

create_text(position, **options) 
#在画布上绘制文本
position#位置，以两个坐标表示
**options{
activefill=
activestipple=
anchor=#文本相对于给定坐标的位置。默认为CENTER
disabledfill=
disabledstipple=
fill=
font=
justify=#默认LEFT
offset=
state=
stipple=
tags=
text=
width=#每行最大长度，大于则换行，设0不换行
}

create_window(position, **options)
#在画布上放置Tkinter组件,无法被覆盖
position#位置，以两个坐标表示
**options{
anchor=
height=#默认为组件需要的高度
state=
tags=
width=
window=
}

dchars(item, from, to=None)
#从可编辑项中删除文本
item#指定项目
from#开始位置
to#结束位置，不指定只删除1个字符

delete(item)
#删除匹配到的项目(tag/id)，未匹配到不会报错

dtag(item, tag=None)
#从匹配到的项目中删除标签，不指定标签则删除所有标签

find_above(item)
#返回指定项目上面的项目

find_all()
#返回画布上的所有项目，以元组形式，从下到上的顺序,最上面的项位于最后

find_below(item)
#返回指定项目下面的项目

find_closest(x, y, halo=None, start=None)
#返回距离给定画布坐标最近的项目,只要画布上有一个项目就会成功，(否则会失败?)
x,y#坐标
halo#可选半径距离
start#可选开始项目

find_enclosed(x1, y1, x2, y2)
#返回矩形区域内所有项目

find_overlapping(x1, y1, x2, y2) 
#返回所有与给定矩形重合或被包含的项目

find_withtag(item)
#返回指定的项目

focus(item=None)
#将焦点转到指定的项目上，之后该项目可以收到键盘输入
#最好先调用focus_set将焦点放到画布上，在将焦点放到画布中的项目上
#调用此方法并指定空字符串，移除项目上的焦点
#调用此方法不指定参数，返回当前焦点所在的项目

gettags(item)
#返回项目的标签

icursor(item, index) 
#将光标移动到指定位置，必须是可编辑的项目
item#指定项目
index#插入位置

index(item, index)
#获取与给定索引对应的数字光标索引；0在第一个字符的左边，len(text)在最后一个字符的右边。返回索引(一个正整数)
item#指定项目
index#可以使用数字，INSERT(当前插入光标)，END(文本长度)，SEL_FIRST 和
#SEL_LAS(选择区域开始结束位置),@x,y(画布坐标，会匹配最近的索引)

insert(item, index, text)
#向一个可编辑的项目中插入文本
item#指定项目
index#数字或符号常量
text

itemcget(item, option)
#获取指定项目参数值如果指定项目代表多个，则返回第一个项目的参数值
item#指定项目
option#项目参数

itemconfig(item, **options)
#改变所有匹配项目的多个参数值

lift(item, **options)
tag_raise(item)
tkraise(item, **options)
#将给定项移动到画布堆栈的顶部。如果多个项目匹配，则它们都将被移动，并保留其相对顺序
#此方法不适用于窗口项目。要更改顺序，请在小部件实例上使用lift

lower(item, **options)
tag_lower(item)
#将画布项移动到画布堆栈的底部。如果多个项目匹配，则它们都将被移动，并保留其相对顺序。
#此方法不适用于窗口项。要更改顺序，请在小部件实例上使用lower

move(item, dx, dy)
#根据偏移量移动项目

postscript(**options)
#为画布项目生成说明，不包括图片和嵌入的窗口部件

scale(self, xscale, yscale, xoffset, yoffset) 
#按比例缩放项目,先平移再缩放
xscale#x缩放比例
yscale#y缩放比例
xoffset#x偏移量
yoffset#y偏移量

scan_dragto(x, y)
#移动组件内容至锚点,使用scan_mark 设置锚点

scan_mark(x, y) 
#设置锚点，用于快速滚动

select_adjust(item, index) 
#调整所选内容，使其包含给定的索引。此方法还将选择锚定设置为此位置。这通常由鼠标绑定使用

select_clear()
#移除选择区域

select_from(item, index)
#设置选择锚点，使用select_adjust或select_to to延伸选择区域

select_item()
#返回选择文字所属的项目

select_to(item, index) 
#改变选择区域，以包括当前锚点和给定索引之间的区域,设置锚点使用select_from或select_adjust

tag_bind(item, event=None, callback, add=None)
#为同一标签下所有项目绑定一个事件，绑定后为新的项目添加此标签，新项目将不被绑定
item#tag/id
event#事件
callback#通常，新绑定会替换相同事件序列的任何现有绑定。如果此参数存在且设置为“+”，则新绑定将添加到任何现有绑定

tag_unbind(self, item, event)
#移除绑定事件

type(item)
#返回项目类型，标签绑定多个项目则返回第一个
#返回值“arc”, “bitmap”, “image”, “line”, “oval”, “polygon”, “rectangle”, “text”, 或“window”.

xview(how, *args)
#水平调整画布视图
how#调整方式moveto或scroll
*args#请参阅xview_moveto和xview_scroll方法的说明

xview_moveto(fraction)
#调整画布，根据偏移左移
fraction#偏移量，从0.0到1.0

xview_scroll(number, what)
#按给定的量水平滚动画布
number#数量
what#单位units或pages

yview(how, *args)
#垂直调整画布视图
how#调整方式moveto或scroll
*args#请参阅xview_moveto和xview_scroll方法的说明

yview_moveto(fraction)
#调整画布，使给定的偏移位于画布的上边缘
fraction#偏移量，从0.0到1.0

yview_scroll(number, what)
#按给定的量垂直滚动画布
number#数量
what#单位units或pages







```
http://effbot.org/tkinterbook/tkinter-index.htm#class-reference

















