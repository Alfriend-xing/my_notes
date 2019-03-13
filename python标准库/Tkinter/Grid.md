Tkinter
---
>1. 该模块是(Tk interface)的缩写
>1. 运行`python-m Tkinter`打开tkinter示例窗口
>1. 在 Python 3中更名为tkinter


## Grid 
>1. 网格几何管理器将小部件放在二维表中。 主窗口小部件被拆分为多个行和列，并且结果表中的每个“单元”可以容纳组件
>1. 网格管理器是Tkinter中最灵活的几何管理器。 如果您不想学习如何以及何时使用所有三个管理器，您至少应该学会这一个
>1. 注意，不要在一个主窗口中同时使用grid和pack
>1. 使用网格管理器很容易。 只需创建小部件，并使用网格方法告诉管理器放置它们的行和列。 您不必事先指定网格的大小; 管理器自动从其中的小部件确定

```python
#例子
#不指定列号则默认0，空的行和列会被忽略
from Tkinter import *

master= Tk()
Label(master, text="First").grid(row=0)
Label(master, text="Second").grid(row=1)

e1 = Entry(master)
e2 = Entry(master)

e1.grid(row=0, column=1)
e2.grid(row=1, column=1)
master.mainloop()

#组件默认被放置在格子的中间，设置sticky改变(N, S, E, W)
Label(master, text="First").grid(row=0, sticky=W)

#组件可以跨越多个格子，columnspan，rowspan
label1.grid(sticky=E)
label2.grid(sticky=E)

entry1.grid(row=0, column=1)
entry2.grid(row=1, column=1)

checkbutton.grid(columnspan=2, sticky=W)

image.grid(row=0, column=2, columnspan=2, rowspan=2,
            sticky=W+E+N+S, padx=5, pady=5)

button1.grid(row=2, column=2)
button2.grid(row=2, column=3)
#在上述例子中，没有为标签指定位置，  在这种情况下，列默认为0，并且行到网格中第一个未使用的行。

```
>参考信息
```python
Grid #class
#网格几何管理器

grid(**options)
#把组件放置到选项描述的网格中去
column=#列号，默认0
columnspan=#多列数量，默认1
in=#将组件放至给定组件内，调用in_
ipadx=#内部水平填充，默认0
ipady=#内部垂直填充，默认0
padx=#水平填充，默认0
pady=#垂直填充，默认0
row=
rowspan=
sticky=#S, N, E, W, or NW, NE, SW, SE

grid_bbox(column=None, row=None, col2=None, row2=None)

grid_columnconfigure(index, **options)
#为一个网格设置参数
minsize=#设置列最小大小，如果列为空，则不会显示
pad=#组件大小基础上添加的大小
weight=#大小变化时的权重，默认0，意味大小不会变化

grid_forget() 
grid_remove()
#移除组件，但组件不会被销毁，他还可以通过grid 或其他方式再次布局

grid_info()
#返回当前网格的调用信息

grid_location(x, y)
#返回离给定坐标最近的网格

grid_propagate(flag)
#启用后(True )，子窗口大小的改变同时影响上级窗口，默认启用

grid_rowconfigure(index, **options)

grid_size()
#返回与当前组件关联的网格大小

grid_slaves(row=None, column=None)
#返回下级组件列表








```


http://effbot.org/tkinterbook/tkinter-index.htm#class-reference

