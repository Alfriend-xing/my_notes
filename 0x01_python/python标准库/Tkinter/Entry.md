Tkinter
---
>1. 该模块是(Tk interface)的缩写
>1. 运行`python-m Tkinter`打开tkinter示例窗口
>1. 在 Python 3中更名为tkinter


## Entry 
>用于输入或显示单行文本

```python
#简单例子

#插入和删除
e = Entry(master)
e.pack()
e.delete(0, END)
e.insert(0, "a default value")

#获取文本信息
s = e.get()

#绑定StringVar 变量，设置或获取文本
v = StringVar()
e = Entry(master, textvariable=v)
e.pack()
v.set("a default value")
s = v.get()


```
>定位方式(小写也可用)
```python
Numerical indexes
#以数组形式定位字符串

ANCHOR 
#对应选择区域的开始位置，也可以用select_from 设置

END 
#对应字符串最后一个字的下一位；(0, END)表示整个字符串

INSERT 
#对应光标位置，也可用icursor 改变

"@%d" % x
#使用鼠标位置(??)


```

>参考信息
```python
Entry(master=None, **options) #class
#单行文本输入域
config(**options)
#设置多个参数，无参数返回当前参数
background=/bg=
borderwidth=/bd=
cursor=#光标
disabledbackground=
disabledforeground=
exportselection=
#如果为true，则所选文本将自动导出到剪贴板。 默认为true
font=
foreground=/fg=
highlightbackground=#失焦时的高亮
highlightcolor=#聚焦时的高亮
highlightthickness=#高亮厚度
insertbackground=#插入时光标的颜色
insertborderwidth=#插入式光标边框的宽度，设为非0值时使用RAISED 样式
insertofftime=/insertontime=#控制光标闪烁，单位毫秒
insertwidth=#插入时光标的宽度
invalidcommand=/invcmd=
justify=#对齐方式LEFT(默认), CENTER, 或RIGHT
readonlybackground=#只读状态背景颜色，默认或设为空值，显示标准背景颜色
relief=#边框样式SUNKEN(默认)，FLAT, RAISED, GROOVE或 RIDGE
selectbackground=#选择区域背景色
selectborderwidth=#选择区域边框宽度
selectforeground=#选择区域文本颜色，前景色
show=#显示方式，例如密码(show="*")
state=#NORMAL(默认), DISABLED(不可输入或选取),或readonly(不可输入可选取)，
#后两个也不支持 insert 和 delete方法
takefocus=#tab键移动
textvariable=#绑定Tkinter 变量，通常是StringVar变量
validate=#指定何时验证,focus(获得或失去焦点时)，focusin(获得焦点时)
#focusout(失去焦点时)，key(任何修改时)；默认NONE ，不验证
validatecommand=/vcmd=#验证内容是否有效的函数，内容有效返回True，需要validate不为None
width=#输入区域的显示宽的，字符长度为单位，默认20
xscrollcommand=#设置滚动条，值为滚动条的set 方法

delete(first, last=None)
#删除字符，使用(0, END)删除全部，不指定last则指删除一个字

get()
#返回当前内容

icursor(index)
#移动插入光标

index(index) 
#返回给定索引的数字位置

insert(index, string)
#在给定位置插入字符，(INSERT, text)从当前光标位置，(END, text)末尾插入

scan_dragto(x)
#设置锚点快速滚动(??)

scan_mark(x)
#滚动内容

selection_adjust(index)
select_adjust(index)
#调整选择以包括给定的字符

selection_clear()
#清除选择区域

selection_from(index)
#开始一个新选择，设置ANCHOR index

selection_present() 
#检查文本是否被选中

selection_range(start, end)
#设置选择范围，start应小于end

selection_to(index)
#选择ANCHOR 和指定区域之间的文字

xview(index)
#显示内容，必要时会滚动

xview_moveto(fraction)
#调整视图，使给定的偏移位于画布的左边缘，0.0是开头，1.0是结尾

xview_scroll(number, what)
#按给定数滚动内容
number#数量
what#单位，units/pages


```








http://effbot.org/tkinterbook/tkinter-index.htm#class-reference

















