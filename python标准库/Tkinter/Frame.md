Tkinter
---
>1. 该模块是(Tk interface)的缩写
>1. 运行`python-m Tkinter`打开tkinter示例窗口
>1. 在 Python 3中更名为tkinter


## Frame
>1. frame(框架)是屏幕上的矩形区域，用于放置其他部件，可以作为master，或者分隔部件使他们保持间距
>1. 用于分组多个部件，形成更复杂的布局，填充空隙，或者封装其他部件时作为基类
>1. 可以为视频或其他外部进程做占位符


```python
#框架部件可用于装饰
from Tkinter import *
master = Tk()
Label(text="one").pack()
separator = Frame(height=2, bd=1, relief=SUNKEN)
separator.pack(fill=X, padx=5, pady=5)
Label(text="two").pack()
mainloop()

#用作占位符，背景颜色设为空，并保留颜色贴图
frame = Frame(width=768, height=576, bg="", colormap="new")
frame.pack()
video.attach_window(frame.window_id())

```
>参考信息
```python
Frame(master=None, **options) #class
#一个组件容器
config(**options)
background=/bg=#背景颜色
borderwidth=/bd=#边框宽度，默认0
class=#默认为Frame
colormap=#设置color map，使用“new”会分配一个新的color map，创建框架后无法修改(??)
container=#默认0
cursor=#鼠标在上方时光标的显示
height=#默认0
highlightbackground=
highlightcolor=
highlightthickness=#默认0
padx=#默认0
pady=#默认0
relief=#FLAT(默认)SUNKEN, RAISED, GROOVE,和RIDGE；需要borderwidth 不为0
takefocus=#tab键聚焦
visual=#无默认值
width=#默认0




```












http://effbot.org/tkinterbook/tkinter-index.htm#class-reference

















