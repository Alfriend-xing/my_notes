Tkinter
---
>1. 该模块是(Tk interface)的缩写
>1. 运行`python-m Tkinter`打开tkinter示例窗口
>1. 在 Python 3中更名为tkinter


## Pack 
>与网格管理器相比，包管理器有点受限，但在一些但很常见的情况下使用起来要容易得多：
>1. 将小部件放在框架（或任何其他容器小部件）中，并让它填充整个框架
>1. 将多个小部件放在一起
>1. 并排放置一些小部件

```python
#例子
#填充整个父窗口，常用于将组件放置在容器组件中并填充整个父窗口
from Tkinter import *

root = Tk()

listbox = Listbox(root)
listbox.pack()

for i in range(20):
    listbox.insert(END, str(i))

mainloop()

#组件有自己的默认大小，要在调整父窗口大小时，内部组件依然填满父窗口，需设置fill和expand
from Tkinter import *

root = Tk()

listbox = Listbox(root)
listbox.pack(fill=BOTH, expand=1)

for i in range(20):
    listbox.insert(END, str(i))

mainloop()
#fill表示扩展组件以填充父窗口，X表示应该水平扩展，Y表示垂直扩展，BOTH表示都扩展
#expand设为1表示分配额外空间给组件，如果父窗口的空间过大，则多余的空间会分给所有expand为1的组件

#将多个小部件放在一起，可以不带任何参数将多个组件放置在一列中
from Tkinter import *

root = Tk()

w = Label(root, text="Red", bg="red", fg="white")
w.pack()
w = Label(root, text="Green", bg="green", fg="black")
w.pack()
w = Label(root, text="Blue", bg="blue", fg="white")
w.pack()

mainloop()
#可以用fill=X使组件与父窗口一样宽
from Tkinter import *

root = Tk()

w = Label(root, text="Red", bg="red", fg="white")
w.pack(fill=X)
w = Label(root, text="Green", bg="green", fg="black")
w.pack(fill=X)
w = Label(root, text="Blue", bg="blue", fg="white")
w.pack(fill=X)

mainloop()

#并排放置一些小部件
from Tkinter import *

root = Tk()

w = Label(root, text="Red", bg="red", fg="white")
w.pack(side=LEFT)
w = Label(root, text="Green", bg="green", fg="black")
w.pack(side=LEFT)
w = Label(root, text="Blue", bg="blue", fg="white")
w.pack(side=LEFT)

mainloop()


```
>参考信息
```python
Pack #class

pack(**options)
anchor=#默认CENTER
expand=#指定是否应展开窗口小部件以填充几何主体中的任何额外空间
fill=#指定窗口小部件是否应占用主服务器为其提供的所有空间。 如果为NONE（默认值），请保持小部件的原始大小。 如果X（水平填充），Y（垂直填充）或BOTH，沿该方向填充给定空间
#要使窗口小部件填充整个主窗口小部件，请将fill设置为BOTH并expand为非零值
ipadx=
ipady=
padx=
pady=
side=#布局方式，垂直TOP(默认)，水平LEFT，还可以用BOTTOM和RIGHT

pack_forget() 

pack_info() 
#返回选项

pack_propagate(flag)
#管理器方法，由布局管理器使用，重设大小以容纳所有组件

pack_slaves()
#返回子项列表
```




http://effbot.org/tkinterbook/tkinter-index.htm#class-reference

