Tkinter
---
>1. 该模块是(Tk interface)的缩写
>1. 运行`python-m Tkinter`打开tkinter示例窗口
>1. 在 Python 3中更名为tkinter


## Variable 
>用于追踪变量的变化
BooleanVar, DoubleVar, IntVar, StringVar

```python
#例子
var = StringVar()
var = StringVar()
var.set("hello")

#内容变化时回调
def callback(*args):
    print "variable changed!"
var = StringVar()
var.trace("w", callback)
var.set("hello")
```
>参考信息
```python
get/set
#设置和获取值

trace(mode, callback) 
#返回字符串
trace_variable(mode, callback)
#返回观察者名
#mode，r表示被读取时回调，w表示被赋值时回调，u变量被删除时回调

trace_vdelete(mode, observer name)
#删除观察者

trace_vinfo()
#返回列表
```
http://effbot.org/tkinterbook/tkinter-index.htm#class-reference

