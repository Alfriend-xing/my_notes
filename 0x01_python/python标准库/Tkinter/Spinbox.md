Tkinter
---
>1. 该模块是(Tk interface)的缩写
>1. 运行`python-m Tkinter`打开tkinter示例窗口
>1. 在 Python 3中更名为tkinter


## Spinbox 
>是Tkinter Entry 的变体，可用于从固定数量的值中进行选择

```python
#例子
from Tkinter import *

master = Tk()

w = Spinbox(master, from_=0, to=10)
w.pack()

mainloop()  

#指定选项集合
w = Spinbox(values=(1, 2, 4, 8))
w.pack()
```
>参考信息
```python
Spinbox(master=None, **options) #class
#一个spinbox组件

bbox(index)
#返回指定字符的边界框

config(**options)
activebackground=
background=/bg=
borderwidth=/bd=
buttonbackground=
buttoncursor=
buttondownrelief=#默认RAISED
buttonuprelief=#默认RAISED
command=
cursor=
disabledbackground=
disabledforeground=
exportselection=#默认True
font=
foreground=/fg=
format=
from=#最小值
highlightbackground=
highlightcolor=
highlightthickness=
increment=#默认1.0
insertbackground=#插入光标的颜色
insertborderwidth=#插入光标的边框宽度
insertofftime=/insertontime=#控制光标闪烁
insertwidth=#插入光标的宽度
invalidcommand=/invcmd=
justify=#默认LEFT
readonlybackground=
relief=#默认SUNKEN
repeatdelay=/repeatinterval=#控制按钮自动重复，毫秒单位
selectborderwidth=
selectforeground=
state=#NORMAL, DISABLED, or “readonly”
takefocus=
textvariable=
to=
validate=
validatecommand=/vcmd=
values=#指定范围元组，覆盖from和to，increment
width=
wrap=#如果设为True，向上和向下按钮将环绕
xscrollcommand=

delete(first, last=None)
#删除某些字符

get()
#返回当前内容

icursor(index)
#移动光标到指定位置

identify(x, y)
#确认坐标处的组件项目，返回“none”, “buttondown”, “buttonup”, or “entry”.

index(index)
#返回索引的数字位置

insert(index, text)

invoke(element) 
#调用一个按钮，必须是“buttonup” or “buttondown”.之一

scan_dragto(x)

scan_mark(x)

selection_adjust(index)
#调整选择以包括给定的字符

selection_clear() 

selection_element(element=None)

```



http://effbot.org/tkinterbook/tkinter-index.htm#class-reference

