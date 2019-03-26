Tkinter
---
>1. 该模块是(Tk interface)的缩写
>1. 运行`python-m Tkinter`打开tkinter示例窗口
>1. 在 Python 3中更名为tkinter


## Basic Widget Methods
>所有组件都提供的方法

>参考信息
```python
Widget #class
#小部件实现类。 此类用作所有窗口小部件类的mixin。

after(delay_ms, callback=None, *args)
#指定时间后调用回调函数，只执行一次，持续回调需要自己内部调用
class App:
    def __init__(self, master):
        self.master = master
        self.poll() # start polling

    def poll(self):
        ... do something ...
        self.master.after(100, self.poll)

after_cancel(id)#取消回调

after_idle(callback, *args)

bbox(column=None, row=None, col2=None, row2=None) 

bell(displayof=0) 

bind(sequence=None, func=None, add=None)
#绑定事件和回调函数

bind_all(sequence=None, func=None, add=None)
#将事件绑定添加到应用程序级别

bind_class(className, sequence=None, func=None, add=None)
#绑定事件到一类组件

bindtags(tagList=None)
#














```







http://effbot.org/tkinterbook/widget.htm
http://effbot.org/tkinterbook/tkinter-index.htm#class-reference

