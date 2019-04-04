Tkinter
---
>1. 该模块是(Tk interface)的缩写
>1. 运行`python-m Tkinter`打开tkinter示例窗口
>1. 在 Python 3中更名为tkinter


## PanedWindow 
>PanedWindow部件是一个几何管理器部件，它可以包含一个或多个子窗口小部件（“窗格”）。 通过使用鼠标移动分隔线（“sashes”），用户可以调整子窗口小部件的大小

```python
#创建两窗格布局
from Tkinter import *

m = PanedWindow(orient=VERTICAL)
m.pack(fill=BOTH, expand=1)

top = Label(m, text="top pane")
m.add(top)

bottom = Label(m, text="bottom pane")
m.add(bottom)

mainloop()

#创建三窗格布局
from Tkinter import *

m1 = PanedWindow(sashwidth=9)
m1.pack(fill=BOTH, expand=1)

left = Label(m1, text="left pane",bg='yellow')
m1.add(left)

m2 = PanedWindow(m1, orient=VERTICAL,sashwidth=9)
m1.add(m2)

top = Label(m2, text="top pane",bg='red')
m2.add(top)

bottom = Label(m2, text="bottom pane",bg='blue')
m2.add(bottom)

mainloop()

```
>参考信息
```python
PanedWindow(master=None, **options) #class
#一个paned window管理组件，通过移动边界改变所管理的组件的大小

add(child, **options)
#添加被管理的组件

config(**options)
background=bg=
borderwidth=/bd=
cursor=
handlepad=#默认8
handlesize=#默认8
height=
orient=#默认HORIZONTAL
relief=#默认FLAT
sashcursor=
sashpad=#默认2
sashrelief=
sashwidth=
showhandle=
width=

forget(child)
remove(child)
#移除一个子项

identify(x, y)
#标识给定位置的窗口小部件元素

panecget(child, option)
#获取一个子项的选项参数

paneconfig(child, **options)
paneconfigure(child, **options)
#设置子项参数
after=#在此小部件后插入
before=#在此小部件前插入
height=
minsize=#最小尺寸（水平窗格的宽度，垂直窗格的高度）
padx=#水平填充
pady=#垂直填充
sticky=#子项的扩展方式S, N, E, W, NW, NE, SW, SE
width=#组件宽度

panes() 
#返回子项列表

proxy_coord()
#获取最新的代理位置

proxy_forget()
#移除代理(??)

proxy_place(x, y)
#在给定位置放置代理

sash_coord(index)
#获取一个sash(窗扇)当前位置，返回2元组(x,y)

sash_dragto(index, x, y)
#移动窗扇到新位置

sash_mark(index, x, y)
#注册当前鼠标位置

sash_place(index, x, y)
#移动窗格到指定位置



```


http://effbot.org/tkinterbook/tkinter-index.htm#class-reference

