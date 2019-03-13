Tkinter
---
>1. 该模块是(Tk interface)的缩写
>1. 运行`python-m Tkinter`打开tkinter示例窗口
>1. 在 Python 3中更名为tkinter


## Toplevel 
>与Frame很相似，只不过显示在单独的顶层窗口中
通常有标题，边框和其他装饰
用于对话框和其他弹出窗口

```python
#例子
from Tkinter import *

a = Tk()
def aa():
    top = Toplevel()
    top.title("About this application...")
    about_message = 'about msg'
    msg = Message(top, text=about_message)
    msg.pack()

    button = Button(top, text="Dismiss", command=top.destroy)
    button.pack()
button1 = Button(a, text="pop", command=aa)
button1.pack()
a.mainloop()
```
>参考信息

```python
Toplevel(master=None, **options) #class
#放置于顶层窗口的组件容器

config(**options)
background=/bg=
borderwidth=/bd=
class=#默认Toplevel
colormap=
container=#默认0
cursor=
height=
highlightbackground=
highlightcolor=
highlightthickness=
menu=
padx=
pady=
relief=#FLAT, SUNKEN, RAISED, GROOVE, or RIDGE
screen=
takefocus=
use=
visual=
width=









```






http://effbot.org/tkinterbook/tkinter-index.htm#class-reference

