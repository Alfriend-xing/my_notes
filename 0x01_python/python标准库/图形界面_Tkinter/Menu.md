Tkinter
---
>1. 该模块是(Tk interface)的缩写
>1. 运行`python-m Tkinter`打开tkinter示例窗口
>1. 在 Python 3中更名为tkinter


## Menu 
>1. 用于实现顶级菜单，下拉菜单和弹出菜单
>1. 建议使用此组件实现菜单，不要使用其他组件仿造菜单
>1. 要创建顶级菜单，先创建Menu实例，再向他添加其他菜单项

```python
#例子

#顶级菜单
from Tkinter import *
root = Tk()
def hello():
    print "hello!"
# create a toplevel menu
menubar = Menu(root)
menubar.add_command(label="Hello!", command=hello)
menubar.add_command(label="Quit!", command=root.quit)
# display the menu
root.config(menu=menubar)
root.mainloop()

#下拉菜单
from Tkinter import *
root = Tk()

def hello():
    print "hello!"

menubar = Menu(root)

# create a pulldown menu, and add it to the menu bar
filemenu = Menu(menubar, tearoff=0)
filemenu.add_command(label="Open", command=hello)
filemenu.add_command(label="Save", command=hello)
filemenu.add_separator()
filemenu.add_command(label="Exit", command=root.quit)
menubar.add_cascade(label="File", menu=filemenu)

# create more pulldown menus
editmenu = Menu(menubar, tearoff=0)
editmenu.add_command(label="Cut", command=hello)
editmenu.add_command(label="Copy", command=hello)
editmenu.add_command(label="Paste", command=hello)
menubar.add_cascade(label="Edit", menu=editmenu)

helpmenu = Menu(menubar, tearoff=0)
helpmenu.add_command(label="About", command=hello)
menubar.add_cascade(label="Help", menu=helpmenu)

# display the menu
root.config(menu=menubar)

root.mainloop()

#弹出菜单
from Tkinter import *
root = Tk()

def hello():
    print "hello!"

# create a popup menu
menu = Menu(root, tearoff=0)
menu.add_command(label="Undo", command=hello)
menu.add_command(label="Redo", command=hello)

# create a canvas
frame = Frame(root, width=512, height=512)
frame.pack()

def popup(event):
    menu.post(event.x_root, event.y_root)

# attach popup to canvas
frame.bind("<Button-3>", popup)
root.mainloop()

#更新菜单，使用postcommand绑定回调函数
from Tkinter import *
counter = 0

def update():
    global counter
    counter = counter + 1
    menu.entryconfig(0, label=str(counter))

root = Tk()

menubar = Menu(root)

menu = Menu(menubar, tearoff=0, postcommand=update)
menu.add_command(label=str(counter))
menu.add_command(label="Exit", command=root.quit)

menubar.add_cascade(label="Test", menu=menu)

root.config(menu=menubar)
root.mainloop()
```
>参考信息

```python
Menu(master=None, **options) #class

__init__(master=None, **options)
#**options参考config方法

activate(index)
#激活方法

add(type, **options)
#将给定条目追加至菜单
type#添加种类，“command”, “cascade” (submenu), “checkbutton”, “radiobutton”, or “separator”.
**options#菜单选项
activebackground=
activeforeground=
accelerator=
background=
bitmap=
columnbreak=
command=
font=
foreground=
hidemargin=
image=
indicatoron=
label=
menu=
offvalue=
onvalue=
selectcolor=
selectimage=
state=
underline=
value=
variable=

add_cascade(**options)
add_checkbutton(**options)
add_command(**options)
add_radiobutton(**options)
add_separator(**options) 

config(**options)
**options
activebackground=
activeborderwidth=
activeforeground=
background=/bg=
borderwidth=/bd=
cursor=#默认'arrow'
disabledforeground=#默认'SystemDisabledText'
font=
foreground=/fg=
postcommand=
relief=
selectcolor=
takefocus=#默认0
tearoff=#默认1
tearoffcommand=
title=
type=

delete(index1, index2=None)
#删除一个或多个条目

entrycget(index, option) 
#获取某条目的配置选项

entryconfig(index, **options) 
#重新配置某些选项

index(index)
#转变索引为数字

insert(index, itemType, **options)

insert_cascade(index, **options) 
#插入子菜单

insert_checkbutton(index, **options)
insert_command(index, **options)
insert_radiobutton(index, **options)
insert_separator(index, **options)

invoke(index)
#调用方法

post(x, y)
#在指定的地方显示菜单，相对根菜单的像素坐标

type(index)
#获得菜单类型

unpost()
#移除一个post菜单

yposition(index)
#返回某一条目的垂直偏移量，用于弹出定位菜单，使该选项位于鼠标正下方









```








http://effbot.org/tkinterbook/tkinter-index.htm#class-reference

