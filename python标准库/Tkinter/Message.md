Tkinter
---
>1. 该模块是(Tk interface)的缩写
>1. 运行`python-m Tkinter`打开tkinter示例窗口
>1. 在 Python 3中更名为tkinter


## Message 
>是Label的变体,可以保持给定宽高比来显示多行信息

```python
#例子
from Tkinter import *

master = Tk()

w = Message(master, text="this is a relatively long message", width=50)
w.pack()

mainloop()

```
>参考信息
```python
Message(master=None, **options) #class
#多行信息
config(**options)
anchor=#N, NE, E, SE, S, SW, W, NW, or CENTER
aspect=#宽高比，默认150，宽比高150%(??),如果明确设置宽度，忽略此项
background=
bg=
borderwidth=
bd=
cursor=
font=
foreground=
fg=
highlightbackground=
highlightcolor=
highlightthickness=
justify=
padx=
pady=
relief=
takefocus=
text=
textvariable=
width=


```


http://effbot.org/tkinterbook/tkinter-index.htm#class-reference

