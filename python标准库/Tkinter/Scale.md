Tkinter
---
>1. 该模块是(Tk interface)的缩写
>1. 运行`python-m Tkinter`打开tkinter示例窗口
>1. 在 Python 3中更名为tkinter


## Scale 
>实现移动滑块选择数值,且限制范围

```python
#例子
from Tkinter import *

master = Tk()

w = Scale(master, from_=0, to=100)
w.pack()

w = Scale(master, from_=0, to=200, orient=HORIZONTAL)
w.pack()

mainloop()

#获取当前值
w = Scale(master, from_=0, to=100)
w.pack()

print w.get()

#默认精度为1，设置resolution 决定精度，-1禁用精度
w = Scale(from_=0, to=100, resolution=0.1)
```
>参考信息
```python
Scale(master=None, **options) #class
#一个滑块
config(**options)
activebackground=
background=/bg=
bigincrement=#大增量(??),默认0
borderwidth=/bd=
command=
cursor=
digits=#默认0
font=
foreground=/fg=
from=#默认0
highlightbackground=
highlightcolor=
highlightthickness=
label=
length=
orient=#方向，水平(默认)，垂直HORIZONTAL
relief=
repeatdelay=#默认300
repeatinterval=#默认100
resolution=#默认1
showvalue=#默认1
sliderlength=#默认30
sliderrelief=#默认RAISED
state=
takefocus=
tickinterval=
to=#默认100
troughcolor=#槽颜色
variable=
width=#默认15

coords(value=None) 
#获取给定刻度值的屏幕坐标

get() 
#获取当前刻度值，返回整数或浮点数

identify(x, y)
#检查屏幕坐标处是否是滑块有效区

set(value)
#设置滑块值


```


http://effbot.org/tkinterbook/tkinter-index.htm#class-reference

