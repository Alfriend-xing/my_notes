Tkinter
---
>1. 该模块是(Tk interface)的缩写
>1. 运行`python-m Tkinter`打开tkinter示例窗口
>1. 在 Python 3中更名为tkinter


## Radiobutton 
>用于实现一个多选一项,可以包含文字或图像，为每个按钮绑定函数和方法
每组Radiobutton应该绑定一个变量，每个按钮代表变量的一个值

```python
#例子
from Tkinter import *

master = Tk()

v = IntVar()

Radiobutton(master, text="One", variable=v, value=1).pack(anchor=W)
Radiobutton(master, text="Two", variable=v, value=2).pack(anchor=W)

mainloop()

#如果需要，可以为每个按钮绑定command 

#在循环中创建多个按钮
MODES = [
        ("Monochrome", "1"),
        ("Grayscale", "L"),
        ("True color", "RGB"),
        ("Color separation", "CMYK"),
    ]

v = StringVar()
v.set("L") # initialize

for text, mode in MODES:
    b = Radiobutton(master, text=text,
                    variable=v, value=mode)
    b.pack(anchor=W)

#将单选按钮变为按钮框，将indicatoron设为0
```
>参考信息
```python
Radiobutton(master=None, **options) #class
#单选按钮，通常成组使用，所有子项共享一个公共变量
__init__(master=None, **options)

config(**options)
activebackground=
activeforeground=
anchor=#N, NE, E, SE, S, SW, W, NW, CENTER(默认)
background=/bg=
bitmap=
borderwidth=/bd=
command=
compound=#用于同时显示文本和图片CENTER，BOTTOM, LEFT, RIGHT, TOP
cursor=
disabledforeground=
font=
foreground=/fg=
height=
highlightbackground=#失焦高亮颜色
highlightcolor=#聚焦高亮色
highlightthickness=#高亮厚度
image=
indicatoron=#如果为True，显示标准单选按钮，false显示按钮框，用SUNKEN 
justify=#多行对齐方式 LEFT, RIGHT, or CENTER
offrelief=#默认raised
overrelief=#鼠标在上方时显示的样式，默认relief值
padx=#水平填充
pady=#垂直填充
relief=#边框装饰，SUNKEN, RAISED, GROOVE, RIDGE, or FLAT(默认)
selectcolor=
selectimage=#无默认
state=#NORMAL, ACTIVE or DISABLED
takefocus=#tab移动，默认开启
text=
textvariable=#与此按钮关联的Tkinter 变量，通常是StringVar
underline=#默认-1.无下划线
value=#与此radiobutton相关的值。 同一组中的所有按钮应具有不同的值
variable=#与此按钮关联的变量
width=#按钮宽度
wraplength=#换行字数

deselect()
#取消选择

flash()
#闪烁几次

invoke()
#调用关联command 

select()
#选择按钮




```


http://effbot.org/tkinterbook/tkinter-index.htm#class-reference

