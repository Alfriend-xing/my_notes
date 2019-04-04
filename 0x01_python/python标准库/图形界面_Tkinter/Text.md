Tkinter
---
>1. 该模块是(Tk interface)的缩写
>1. 运行`python-m Tkinter`打开tkinter示例窗口
>1. 在 Python 3中更名为tkinter


## Text 
>用于格式化显示文本，图片

>概念

>索引类型
```python
line/column (“line.column”)
"%d.%d" % (line, column)

line end (“line.end”)
行号后紧跟 “.end”

INSERT
光标处

CURRENT
离鼠标最近的字符处

END
文档结尾处

user-defined marks

user-defined tags (“tag.first”, “tag.last”)

selection (SEL_FIRST, SEL_LAST)

window coordinate (“@x,y”)

embedded object name (window, images)

expressions
```
>Marks
```python
通常是嵌入文本间的不可见对象，用于控制文本显示，随文本一起移动
```
>Tags
```python
标签用于将显示样式和/或事件回调与文本范围相关联
```




>参考信息
```python
Text(master=None, **options) #class
#格式化展示或编辑文本

bbox(index)
#计算给定字符的边界框

compare(index1, op, index2)
#比较两个索引，op参数备选项 “<”, “<=”, “==”, “>=”, “>”, or “!=”

config(**options)
autoseparators=#默认1
background=/bg=
borderwidth=/bd=
cursor=
exportselection=
font=
foreground=/fg=
height=#默认24
highlightbackground=
highlightcolor=
highlightthickness=
insertbackground=
insertborderwidth= #默认0
insertofftime=#默认300
insertofftime=#默认600
insertwidth=#默认2
maxundo=#默认0
padx=#默认1
pady=#默认1
relief=#默认SUNKEN
selectbackground=
selectborderwidth=
selectforeground=
setgrid=#默认0
spacing1=#默认0
spacing2=#默认0
spacing3=#默认0
state=#NORMAL
tabs=
takefocus=
undo=#默认0
width=#默认80
wrap=#默认CHAR
xscrollcommand=
yscrollcommand=

debug(boolean=None)
#启用或禁用调试

delete(start, end=None)

dlineinfo(index)
#计算包含给定字符的行的边界框

dump(index1, index2=None, command=None, **kw) 

edit_modified(arg=None)

edit_redo()

edit_reset()

edit_separator()

edit_undo()

get(start, end=None)

image_cget(index, option)
#获取图片参数

image_configure(index, **options)
#配置图片属性
align=
image=
name=
padx=
pady=


image_create(index, cnf={}, **kw) 
#在给定位置插入图像。 图像必须是Tkinter PhotoImage或BitmapImage实例

image_names()
#返回所有图片名称

index(index)
#返回给定索引行列值

insert(index, text, *tags)
#指定位置插入文本，索引通常是 INSERT or END，可以提供一个或多个tags

mark_gravity(self, name, direction=None)
#设定mark的重力，LEFT or RIGHT，默认RIGHT

mark_names()
#查找标记的名称

mark_next(index)

mark_previous(index)

mark_set(name, index)

mark_unset(name)

scan_dragto(x, y)

scan_mark(x, y)

search(pattern, index, stopindex=None, forwards=None,
backwards=None, exact=None, regexp=None, nocase=None, count=None) 
#查找文本或正则
pattern
index
stopindex
forwards
backwards
exact
regexp
nocase
count

see(index)
#确保给定索引可见

tag_add(tagName, index1, *args)

tag_bind(tagName, sequence, func, add=None)

tag_cget(tagName, option)

tag_config(tagName, cnf={}, **kw)

tag_configure(tagName, cnf={}, **kw)

tag_delete(*tagNames)

tag_lower(tagName, belowThis=None)

tag_names(index=None)

tag_nextrange(tagName, index1, index2=None)

tag_prevrange(tagName, index1, index2=None)

tag_raise(tagName, aboveThis=None) 

tag_ranges(tagName)

tag_remove(tagName, index1, index2=None)

tag_unbind(tagName, sequence, funcid=None) 

window_cget(index, option) 

window_config(index, **options)
align=
create=
padx=
pady=
stretch=
window=

window_configure(index, cnf=None, **kw)

window_create(index, **options) 

window_names()

xview(*what)

xview_moveto(fraction)

xview_scroll(number, what)

yview(*what)

yview_moveto(fraction)

yview_pickplace(*what)

yview_scroll(number, what) 




```



http://effbot.org/tkinterbook/tkinter-index.htm#class-reference

