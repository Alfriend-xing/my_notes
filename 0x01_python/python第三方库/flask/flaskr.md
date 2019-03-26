# flaskr

---
## 创建文件夹
```python
/flaskr
    /static
    /templates
```
> flaskr 文件夹不是一个 Python 的包，只是我们放置文件的地方。 在接下来的步骤中我们会直接把数据库模式和主模块放在这个文件夹中。
## 读取配置
```python
app = Flask(__name__)
app.config.from_object(__name__)
```
>from_object() 将会寻找给定的对象(如果它是一个字符串，则会导入它)， 搜寻里面定义的全部大写的变量。
```python
app.config.from_envvar('FLASKR_SETTINGS', silent=True)
```
>这种方法我们可以设置一个名为 FLASKR_SETTINGS 环境变量来设定一个配置文件载入后是否覆盖默认值。 静默开关告诉 Flask 不去关心这个环境变量键值是否存在。






