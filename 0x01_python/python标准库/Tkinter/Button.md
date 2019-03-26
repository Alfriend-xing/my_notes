Tkinter
---
>1. 该模块是(Tk interface)的缩写
>1. 运行`python-m Tkinter`打开tkinter示例窗口
>1. 在 Python 3中更名为tkinter

## Button 
```python
from Tkinter import *

master = Tk()

def callback():
    print "click!"

b = Button(master, text="OK", command=callback)
b.pack()

mainloop()
```
>没有回调的按钮是没意义的
使按钮失效`b = Button(master, text="Help", state=DISABLED)`
如果没有指定大小，按钮会自动适应所显示文字或图片的大小
还可以使用`height`和`width`选项显式设置大小。如果在按钮中显示文本，这些选项将以文本单位定义按钮的大小。如果显示位图或图像，它们将以像素（或其他屏幕单位）定义大小。

```python
# 为文本按钮指定像素大小
f = Frame(master, height=32, width=32)
f.pack_propagate(0) # don't shrink
f.pack()

b = Button(f, text="Sure!")
b.pack(fill=BOTH, expand=1)
```
>参考信息
```python
Button(master=None, **options) #class
master #父组件
**options #组件选项

config(**options)
#用于修改组件选项，如果没有加参数，则返回默认选项字典
    activebackground=   #按钮激活时的背景颜色
    activeforeground=   #前景色
    anchor=     #控制文本（或图像）在按钮中的位置；选项N, NE, E, SE, 
    #S, SW, W, NW, or CENTER. 默认CENTER. (anchor/Anchor)
    background=     #背景色(background/Background)
    bg=     #同上
    bitmap=     #显示位图，如果指定image ，则此选项无效
    borderwidth=/bd=    #按钮边框的宽度(borderWidth/BorderWidth)
    command=    #按下按钮时调用的函数或方法。回调可以是函数、绑定方法
    #或任何其他可调用(callable )的Python对象(command/Command)
    compound=   #控制如何在按钮中组合文本和图像。默认图像或位图会覆盖
    #文本。CENTER参数使文字在图片/位图之上;BOTTOM, LEFT, RIGHT, or
    # TOP使图像在文字旁，默认NONE. (compound/Compound)
    cursor=     #鼠标移动到按钮上时要显示的光标(cursor/Cursor)
    default=    #显示为默认按钮，默认无效DISABLED (default/Default)
    disabledforeground= #禁用按钮时要使用的颜色
    font=   #按钮中使用的字体
    foreground=/fg= #用于文本和位图内容的颜色。
    height=     #按钮的高度。如果按钮显示文本，则大小以文本单元给出。
    #如果按钮显示图像，则以像素（或屏幕单位）给出大小。如果省略了大小，
    #则根据按钮内容计算大小。
    highlightbackground=#当按钮没有焦点时用于突出显示边框的颜色。
    highlightcolor=#当按钮具有焦点时，用于突出显示边界的颜色
    highlightthickness=#高亮边框的宽度
    image=#组件显示的图片，会覆盖位图和文本
    justify=#多行文本的对齐方式LEFT, RIGHT,或CENTER
    overrelief=#鼠标移动到上面的relief方式,未设置则使用relief值
    padx=#文本或图像与边框之间的额外水平填充
    pady=#文本或图像与边框之间的额外垂直填充
    relief=#边框装饰，SUNKEN(按下)，RAISED(升起),GROOVE(沟槽)，RIDGE
    #(山脊)，FLAT(平)
    repeatdelay=#操作延迟
    repeatinterval=#操作间隔
    state=#按钮状态NORMAL, ACTIVE或DISABLED
    takefocus=#可以使用Tab键移动到此按钮，默认开启
    text=#按钮中显示的文本,可以换行，会被覆盖
    textvariable=#关联Tkinter 变量，通常是StringVar，通过赋值变量改变按钮文本显示
    underline=#文本标签中要加下划线的字符,默认-1,不加下划线
    width=#按钮的宽度。如果按钮显示文本，则大小以文本单元给出。如果按钮
    #显示图像，则以像素（或屏幕单位）给出大小。如果大小被省略或为零，则
    #根据按钮内容进行计算
    wraplength=#确定何时应将按钮的文本换行为多行，默认0，不换行；80表示8个字换一行
flash()#闪烁按钮。此方法多次重新绘制按钮，在活动外观和正常外观之间切换。
invoke()#调用与按钮关联的命令

```

```

http://effbot.org/tkinterbook/tkinter-index.htm#class-reference

















