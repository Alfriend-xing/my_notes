Tkinter
---
>1. 该模块是(Tk interface)的缩写
>1. 运行`python-m Tkinter`打开tkinter示例窗口
>1. 在 Python 3中更名为tkinter


## OptionMenu 
>是一个显示已选弹出菜单条目的按钮，获取当前选项需要绑定变量
```python
#例子
from Tkinter import *

master = Tk()

var = StringVar(master)
var.set("one") # initial value

option = OptionMenu(master, var, "one", "two", "three", "four")
option.pack()

#
# test stuff

def ok():
    print "value is", var.get()

button = Button(master, text="OK", command=ok)
button.pack()

mainloop()
```

```python
#从列表创建OptionMenu

from Tkinter import *

# the constructor syntax is:
# OptionMenu(master, variable, *values)

OPTIONS = [
    "egg",
    "bunny",
    "chicken"
]

master = Tk()

variable = StringVar(master)
variable.set(OPTIONS[0]) # default value

w = apply(OptionMenu, (master, variable) + tuple(OPTIONS))
w.pack()

mainloop()

```


http://effbot.org/tkinterbook/tkinter-index.htm#class-reference

