Tkinter
---
>1. 该模块是(Tk interface)的缩写
>1. 运行`python-m Tkinter`打开tkinter示例窗口
>1. 在 Python 3中更名为tkinter


## BitmapImage&PhotoImage  

```python
#位图
bitmap = BitmapImage(data=BITMAP)
bitmap = BitmapImage(file="bitmap.xbm")
bitmap = BitmapImage(
    data=BITMAP,
    foreground="white", background="black"
    )
bitmap.config(foreground="blue")
bitmap["foreground"] = "red"

#图片
photo = PhotoImage(file="image.gif")
photo = PhotoImage(file="lenna.pgm")

#其他文件格式
from PIL import Image, ImageTk
image = Image.open("lenna.jpg")
photo = ImageTk.PhotoImage(image)

#避免图片被自动垃圾回收
label = Label(image=photo)
label.image = photo # 保留一个引用
label.pack()
```


http://effbot.org/tkinterbook/tkinter-index.htm#class-reference

