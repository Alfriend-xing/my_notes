Tkinter
---
>1. 该模块是(Tk interface)的缩写
>1. 运行`python-m Tkinter`打开tkinter示例窗口
>1. 在 Python 3中更名为tkinter


## Listbox 
>用于显示备选项列表，只能显示文本，可选一个或多个

```python
#例子
from Tkinter import *
master = Tk()
listbox = Listbox(master)
listbox.pack()
listbox.insert(END, "a list entry")
for item in ["one", "two", "three", "four"]:
    listbox.insert(END, item)
mainloop()

#移除项目
listbox.delete(0, END)
listbox.insert(END, newitem)

#选择模式
SINGLE #单选
BROWSE #鼠标移动
MULTIPLE #鼠标点击多选
EXTENDED #选择范围内所有
lb = Listbox(selectmode=EXTENDED)

#列表框不支持command 选项，官方的替代方案是绑定双击事件的回调函数，单击确定执行
lb.bind("<Double-Button-1>", self.ok)

```
>参考信息
```python
Listbox(master=None, **options) #class

activate(index)
#激活给定的索引（它将标有下划线）

bbox(self, index)
#获取指定项的边界框
#返回4元组 (xoffset, yoffset, width, height)

config(**options)
activestyle=#默认下划线
background=/bg=
borderwidth=/bd=#默认2
cursor=#无默认值
disabledforeground=
exportselection=#默认1
font=
foreground=/fg=
height=#默认10
highlightbackground=
highlightcolor=
highlightthickness=
listvariable=#无默认值
relief=#默认SUNKEN
selectbackground=
selectborderwidth=
selectforeground=
selectmode=#默认BROWSE
setgrid=#默认0
state=#默认NORMAL
takefocus=#无默认
width=#默认20
xscrollcommand=#需绑定Scrollbar对象的set方法
yscrollcommand=#需绑定Scrollbar对象的set方法

curselection()
#获取所选项的序号列表，可能是字符串或数字，需要处理

delete(first, last=None)
#删除一个或多个项，delete(0, END)清空

get(first, last=None) 
#获取列表框中的一个或多个项，get(ACTIVE) 获取当前所选项(下划线)

index(index) 
#返回与给定索引对应的数字索引，通常指定ACTIVE，ANCHOR，@x,y

insert(index, *elements)
#在指定位置插入一个或多个项，通常指定END ，ACTIVE 

itemcget(index, option)
#获取列表框中单个项的配置选项

itemconfig(index, **options)
#改变列表框中单个项的配置选项

nearest(y)
#返回离给定坐标最近的索引，部件相对像素坐标

scan_dragto(x, y)
#滚动内容

scan_mark(x, y)
#设置扫描锚点(??)

see(index)
#确定给定的索引是可见的，可以指定索引或END

select_anchor(index)
selection_clear(first, last=None)
#从选择区域中清除一项或多项

selection_includes(index)
#查看某项是否选中

selection_set(first, last=None)
#向选择区域内添加一项或多项

size()
#返回列表框包含项目数量

xview(column, *extra)
#无参数，返回当前显示位置在整体的比例
#一个参数，将指定索引移至列表框边缘
#带moveto和xview_moveto相同

xview_moveto(fraction)
#按比例移动列表框，拖滚动条时也绑定此方法

xview_scroll(number, what)
#根据指定数量滚动，数量和单位(units，pages)

yview(index，*extra)
#控制垂直滚动，用see方法确认给定项目可见

yview_moveto(fraction)

yview_scroll(number, what)



```









http://effbot.org/tkinterbook/tkinter-index.htm#class-reference

