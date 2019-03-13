Tkinter
---
>1. 该模块是(Tk interface)的缩写
>1. 运行`python-m Tkinter`打开tkinter示例窗口
>1. 在 Python 3中更名为tkinter


## Label 
>用于显示文本和图片，可多行显示
可随时修改内容

```python
#例子
from Tkinter import *
master = Tk()
w = Label(master, text="Hello, world!")
w.pack()
mainloop()

#请谨慎使用颜色和字体，建议使用默认值(猜猜可能会导致兼容问题)
w = Label(master, text="Rouge", fg="red")
w = Label(master, text="Helvetica", font=("Helvetica", 16))

#绑定Tkinter 变量，改变变量值，标签会自动更新
v = StringVar()
Label(master, textvariable=v).pack()
v.set("New Text!")

#显示图片和位图
photo = PhotoImage(file="icon.gif")
w = Label(parent, image=photo)
w.photo = photo
w.pack()


```
>参考信息
```python
Label(master=None, **options) #class
config(**options)   
activebackground=
activeforeground=
anchor=#N, NE, E, SE, S, SW, W, NW, or CENTER.
background=/bg=
bitmap=
borderwidth=/bd=
compound=#如何同时显示文字和图片，默认None；CENTER(文字在图片上)
#BOTTOM, LEFT, RIGHT, 或TOP，则文字在图片旁
cursor=#光标
disabledforeground=
font=
foreground=/fg=
height=
highlightbackground=#失焦时高亮边框色，默认与标准背景色相同
highlightcolor=#聚焦时高亮色
highlightthickness=
image=
justify=#如何对齐多行文本，LEFT, RIGHT, or CENTER(默认)
padx=#文本周围水平填充，默认1
pady=#文本周围垂直填充，默认1
relief=#边框装饰，FLAT，SUNKEN, RAISED, GROOVE, 和RIDGE
state=#NORMAL，ACTIVE 和DISABLED
takefocus=#True为聚焦，默认false
text=
textvariable=#变量StringVar
underline=
width=
wraplength= #换行字数

```






http://effbot.org/tkinterbook/tkinter-index.htm#class-reference

















