Tkinter
---
>1. 该模块是(Tk interface)的缩写
>1. 运行`python-m Tkinter`打开tkinter示例窗口
>1. 在 Python 3中更名为tkinter


## Checkbutton 
>1. 该组件是Tkinter的标准组件，可以实现开关选项或选择二值项，可以包含图片和文字(一种字体，多行显示，支持下划线和tab键移动),可以关联函数和方法，当按下时调用
>1. 每个Checkbutton组件应该关联一个变量

```python
#一些例子

from Tkinter import *
master = Tk()
var = IntVar()
c = Checkbutton(master, text="Expand", variable=var)
c.pack()
mainloop()

#自定义变量二值
var = StringVar()
c = Checkbutton(
    master, text="Color image", variable=var,
    onvalue="RGB", offvalue="L"
    )

#跟踪变量值
v = IntVar()
c = Checkbutton(master, text="Don't show this again", variable=v)
c.var = v

#封装和绑定回调函数
def __init__(self, master):
    self.var = IntVar()
    c = Checkbutton(
        master, text="Enable Tab",
        variable=self.var,
        command=self.cb)
    c.pack()

def cb(self):
    print "variable is", self.var.get()


```

>参考信息
```python
Checkbutton(master=None, **options) #class
#勾选按钮
config(**options)
#修改多个部件选项，未指定参数返回所有当前选项
activebackground=
activeforeground=
anchor=
#按钮关联图片或文字的位置N, NE, E, SE, S, SW, W, NW,或CENTER(默认)
#如果改变默认值，建议同时设置padx或pady
background=/bg=
bitmap=
borderwidth=/bd=#按钮边框的宽度
command=#按钮被按下执行的函数或方法
compound=#默认None
cursor=#鼠标在上方时的光标
disabledforeground=
font=
foreground=/fg=
height=#指定高度，文本行高或像素
highlightbackground=
highlightcolor=
highlightthickness=#高亮厚度，默认1
image=
indicatoron=#控制指针的显示，默认启用，设为false将使用relief，选择按钮后显示为SUNKEN(凹陷)
justify=#多行文本对齐方式LEFT, RIGHT,或CENTER 
offrelief=#默认raised
offvalue=#与未选中按钮对应的值，默认0
onvalue=#与选中按钮对应的值，默认1
overrelief=#无默认值
padx=#按钮填充。 默认值为1
pady=#按钮填充。 默认值为1
relief=#边框装饰，默认FLAT ，除非使用indicatoron 
selectcolor=
selectimage=
state=
takefocus=#Tab 移动，默认开启
text=
textvariable=#绑定Tkinter ，用于改变文本
underline=#默认-1，即不使用下划线
variable=
#将Tkinter变量关联到按钮。 按下按钮时，变量在offvalue和onvalue之间切换。 
#按钮会自动反映对变量的显式更改
width=#按钮宽度
wraplength=#换行字数

deselect()
#取消选择

flash()
#闪烁几次

invoke() 
#调用关联命令

select()
#设为选择

toggle()
#切换按钮



```

http://effbot.org/tkinterbook/tkinter-index.htm#class-reference

















