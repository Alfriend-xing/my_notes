Tkinter
---
>1. 该模块是(Tk interface)的缩写
>1. 运行`python-m Tkinter`打开tkinter示例窗口
>1. 在 Python 3中更名为tkinter


## LabelFrame 
>是 Tkinter Frame 的变体，默认在子组件周围画边框，还可以设置标题
常用于结合一组相关的组件

```python
from Tkinter import *

master = Tk()

group = LabelFrame(master, text="Group", padx=5, pady=5)
group.pack(padx=10, pady=10)

w = Entry(group)
w.pack()

mainloop()

```
>参考信息
```python
LabelFrame(master=None, **options) #class
config(**options)
background=/bd=
borderwidth=/bd=
class=#默认为Labelframe
colormap=#指定新的colormap，默认继承父框架，创建后不可更改
container=#设为True，表示此框架为组件容器，默认false，创建后不可更改
cursor=#光标显示
foreground=/fg=
font=
height=#高度，单位像素
highlightbackground=
highlightcolor=
highlightthickness=
labelanchor=#标签显示位置，默认左上NW
labelwidget=#标签使用的组件
padx=#内部水平填充，默认0
pady=#内部垂直填充，默认0
relief=#GROOVE，FLAT, RAISED, SUNKEN, 和RIDGE
takefocus=#True，可以使用tab移动
text=#标签内容
visual=
width=

```














http://effbot.org/tkinterbook/tkinter-index.htm#class-reference





