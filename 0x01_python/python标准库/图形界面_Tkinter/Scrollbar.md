Tkinter
---
>1. 该模块是(Tk interface)的缩写
>1. 运行`python-m Tkinter`打开tkinter示例窗口
>1. 在 Python 3中更名为tkinter


## Scrollbar 
>控制屏幕滚动，listboxes, canvases, text
>连接滚动条和组件需要两步
>1. 将组件的yscrollcommand 回调设置为滚动条的set方法
>1. 将滚动条的command回调设置为组件的yview 方法

```python
#例子
from Tkinter import *

master = Tk()

scrollbar = Scrollbar(master)
scrollbar.pack(side=RIGHT, fill=Y)

listbox = Listbox(master, yscrollcommand=scrollbar.set)
for i in range(1000):
    listbox.insert(END, str(i))
listbox.pack(side=LEFT, fill=BOTH)

scrollbar.config(command=listbox.yview)

mainloop()
```
>参考信息
```python
Scrollbar(master=None, **options) #class
#一个滚动条

activate(element)
#激活滚动条，“arrow1”, “slider”, or “arrow2”.未指定返回当前激活参数

config(**options)
activebackground=
activerelief=#默认RAISED
background=/bg=
borderwidth=/bd=#默认0
command=#用于更新关联组件的回调函数，通常是可滚动组件的xview or yview方法
cursor=
elementborderwidth=#默认-1
highlightbackground=
highlightcolor=
highlightthickness=#默认0
jump=#默认0
orient=#方向HORIZONTAL or VERTICAL(默认)
relief=#SUNKEN(默认)FLAT, RAISED, GROOVE, and RIDGE
repeatdelay=#默认300
repeatinterval=#默认100
takefocus=
troughcolor=#槽颜色
width=#默认16

delta(deltax, deltay) 
#返回浮点数，代表当前滚动条移动至给定屏幕坐标所需的偏移量

fraction(x, y)
#返回与给定鼠标坐标对应的滑块位置

get()
#获取当前滚动条位置

identify(x, y)
#标识给定位置的滚动条元素
#返回 “arrow1” (top/left arrow), “trough1”, “slider”, “trough2”, “arrow2”(bottom/right)或空字符串

set(lo, hi)
#移动滚动条到新位置
lo#向上偏移量
hi#向下偏移量
```


http://effbot.org/tkinterbook/tkinter-index.htm#class-reference

