# flask-quick

---
## 一个最小的应用
```python

from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello World!'

if __name__ == '__main__':
    app.run()
```
## 外部可见服务器
```python
app.run(host='0.0.0.0')
```
## 调试模式
```python
app.debug = True
或
app.run()
```
>尽管交互式调试器不能在分叉( forking )环境上工作(这使得它几乎不可能在生产服务器上使用)， 它依然允许执行任意代码。这使它成为一个巨大的安全风险，因此它 绝对不能用于生成环境。

## 路由
```python
@app.route('/')
def index():
    return 'Index Page'

@app.route('/hello')
def hello():
    return 'Hello World'
```

## 变量规则
```python
@app.route('/user/<username>')
def show_user_profile(username):
    # show the user profile for that user
    return 'User %s' % username

@app.route('/post/<int:post_id>')
def show_post(post_id):
    # show the post with the given id, the id is an integer
    return 'Post %d' % post_id
```
>存在如下转换器:

 类型 | 描述
------------ | ------------- 
int|接受整数
float|同 int 一样，但是接受浮点数
path|和默认的相似，但也接受斜线

## 唯一 URLs / 重定向行为
```python
#访问一个结尾不带斜线的 URL 会被 Flask 重定向到带斜线的规范URL去
@app.route('/projects/')
def projects():
    return 'The project page'

#访问结尾带斜线的 URL 会产生一个 404 “Not Found” 错误
@app.route('/about')
def about():
    return 'The about page'
```
## 构建 URL
```python
>>> from flask import Flask, url_for
>>> app = Flask(__name__)
>>> @app.route('/')
... def index(): pass
...
>>> @app.route('/login')
... def login(): pass
...
>>> @app.route('/user/<username>')
... def profile(username): pass
...
>>> with app.test_request_context():
...  print url_for('index')
...  print url_for('login')
...  print url_for('login', next='/')
...  print url_for('profile', username='John Doe')
...
/
/login
/login?next=/
/user/John%20Doe
```
>为什么你愿意构建 URLs 而不是在模版中硬编码？这里有三个好的理由：
>1. 反向构建通常比硬编码更具备描述性。更重要的是，它允许你一次性修改 URL， 而不是到处找 URL 修改。
>1. 构建 URL 能够显式地处理特殊字符和 Unicode 转义，因此你不必去处理这些。
>1. 如果你的应用不在 URL 根目录下(比如，在 /myapplication 而不在 /)， url_for() 将会适当地替你处理好。

## HTTP方法
```python
@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        do_the_login()
    else:
        show_the_login_form()
```
>HTTP 方法（通常也称为“谓词”）告诉服务器客户端想要对请求的页面 做 什么。下面这些方法是比较常见的：
>>GET
浏览器通知服务器只 获取 页面上的信息并且发送回来。这可能是最常用的方法。
>
>>HEAD
浏览器告诉服务器获取信息，但是只对 头信息 感兴趣，不需要整个页面的内容。 应用应该处理起来像接收到一个 GET 请求但是不传递实际内容。在 Flask 中你完全不需要处理它， 底层的 Werkzeug 库会为你处理的。
>
>>POST
浏览器通知服务器它要在 URL 上 提交 一些信息，服务器必须保证数据被存储且只存储一次。 这是 HTML 表单通常发送数据到服务器的方法。
>
>>PUT
同 POST 类似，但是服务器可能触发了多次存储过程，多次覆盖掉旧值。现在你就会问这有什么用， 有许多理由需要如此去做。考虑下在传输过程中连接丢失：在这种情况下浏览器 和服务器之间的系统可能安全地第二次接收请求，而不破坏其它东西。对于 POST 是不可能实现的，因为 它只会被触发一次。
>
>>DELETE
移除给定位置的信息。
>
>>OPTIONS
给客户端提供一个快速的途径来指出这个 URL 支持哪些 HTTP 方法。从 Flask 0.6 开始，自动实现了它。
## 静态文件
>动态的 `web` 应用同样需要静态文件。`CSS` 和 `JavaScript` 文件通常来源于此。理想情况下， 你的 web 服务器已经配置好为它们服务，然而在开发过程中 `Flask` 能够做到。 只要在你的包中或模块旁边创建一个名为 `static` 的文件夹，在应用中使用 `/static` 即可访问。
给静态文件生成 `URL` ，使用特殊的 `'static'` 端点名:

```python
url_for('static', filename='style.css')
```
## 渲染模板
```python
from flask import render_template

@app.route('/hello/')
@app.route('/hello/<name>')
def hello(name=None):
    return render_template('hello.html', name=name)

#这里是一个模版的例子：
<!doctype html>
<title>Hello from Flask</title>
{% if name %}
  <h1>Hello {{ name }}!</h1>
{% else %}
  <h1>Hello World!</h1>
{% endif %}

```
>Flask 将会在 templates 文件夹中寻找模版。因此如果你的应用是个模块，这个文件夹在模块的旁边，如果它是一个包，那么这个文件夹在你的包里面:
在模版中你也可以使用 request, session 和 g [1] 对象，也能使用函数 get_flashed_messages()

## 接收请求数据
```python
from flask import request

with app.test_request_context('/hello', method='POST'):
    # now you can do something with the request until the
    # end of the with block, such as basic assertions:
    assert request.path == '/hello'
    assert request.method == 'POST'
```
## 文件上传
```python
from flask import request

@app.route('/upload', methods=['GET', 'POST'])
def upload_file():
    if request.method == 'POST':
        f = request.files['the_file']
        f.save('/var/www/uploads/uploaded_file.txt')
    ...
```
## Cookies
读取 cookies:
```python
from flask import request

@app.route('/')
def index():
    username = request.cookies.get('username')
    # use cookies.get(key) instead of cookies[key] to not get a
    # KeyError if the cookie is missing.
```
存储 cookies:
```python
from flask import make_response

@app.route('/')
def index():
    resp = make_response(render_template(...))
    resp.set_cookie('username', 'the username')
    return resp
```
## 重定向和错误
```python
from flask import abort, redirect, url_for

@app.route('/')
def index():
    return redirect(url_for('login'))

@app.route('/login')
def login():
    abort(401)
    this_is_never_executed()

#定制错误页面
from flask import render_template

@app.errorhandler(404)
def page_not_found(error):
    return render_template('page_not_found.html'), 404
```
## 定制响应
>Flask 把返回值转换成响应对象的逻辑如下：
>1. 如果返回的是一个合法的响应对象，它会被从视图直接返回。
>1. 如果返回的是一个字符串，响应对象会用字符串数据和默认参数创建。
>1. 如果返回的是一个元组而且元组中元素能够提供额外的信息。这样的元组必须是 (response, status, headers) 形式且至少含有一个元素。 status 值将会覆盖状态代码，headers 可以是一个列表或额外的消息头值字典。
>1. 如果上述条件均不满足，Flask 会假设返回值是一个合法的 WSGI 应用程序，并转换为一个请求对象。

如果你想要获取在视图中得到的响应对象，你可以用函数 make_response() 。
想象你有这样一个视图:

```python
@app.errorhandler(404)
def not_found(error):
    return render_template('error.html'), 404
```
你只需要用 make_response() 封装返回表达式，获取结果对象并修改，然后返回它：
```python
@app.errorhandler(404)
def not_found(error):
    resp = make_response(render_template('error.html'), 404)
    resp.headers['X-Something'] = 'A value'
    return resp
```
## 会话
>除了请求对象，还有第二个称为 session 对象允许你在不同请求间存储特定用户的信息。 这是在 cookies 的基础上实现的，并且在 cookies 中使用加密的签名。这意味着用户可以查看 cookie 的内容， 但是不能修改它，除非它知道签名的密钥。

要使用会话，你需要设置一个密钥。这里介绍会话如何工作:
```python

from flask import Flask, session, redirect, url_for, escape, request

app = Flask(__name__)

@app.route('/')
def index():
    if 'username' in session:
        return 'Logged in as %s' % escape(session['username'])
    return 'You are not logged in'

@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        session['username'] = request.form['username']
        return redirect(url_for('index'))
    return '''
        <form action="" method="post">
            <p><input type=text name=username>
            <p><input type=submit value=Login>
        </form>
    '''

@app.route('/logout')
def logout():
    # remove the username from the session if it's there
    session.pop('username', None)
    return redirect(url_for('index'))

# set the secret key.  keep this really secret:
app.secret_key = 'A0Zr98j/3yX R~XHH!jmN]LWX/,?RT'
```
## 消息闪烁
>使用 `flash()` 方法来闪现一个消息，使用 `get_flashed_messages()` 能够获取消息， `get_flashed_messages()` 也能用于模版中。


