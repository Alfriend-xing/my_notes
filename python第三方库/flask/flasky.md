# flasky
---
## 安装虚拟环境
>克隆项目
>使用virtualenv创建虚拟环境
激活环境
使用pip安装依赖

## 程序基本结构
初始化
```python
from flask import Flask
app = Flask(__name__)
# name为程序主模块或包的名字
```
路由和视图函数
```python
@app.route('/')
def index():
    return '<h1>hello world</h1>'

app.add_url_rule()
```
启动服务器
```python
if __name__=='__main__':
    app.run(debug=True)
```
>将上述代码拼接，即可得到一个完整的程序

上下文全局变量
变量名|上下文|说明
---|---|---
current_app|程序上下文|前激活程序的程序实例
g|程序上下文|处理请求时用作临时存储的对象，每次请求都会重设这个变量
request|请求上下文|请求对象，封装了客户端发出的http请求中的内容
session|请求上下文|用户会话，用于存储请求之间需要记住的值的字典

请求钩子
```python
before_first_request    #注册一个函数，在处理第一个请求之前运行
before_request  #每次请求之前运行
after_request   #如果没有未处理的异常抛出，在每次请求后运行
teardown_request    #即使有异常抛出，也在每次请求后运行
```
>请求钩子函数和视图变量之间共享上下文全局变量g

响应
```python
# 返回字符串和状态码
return 'hello',200  
```

```python
# 返回相应对象
from flask import make_response
response = make_response('hello')
response.set_cookie('name','zhangsan')
return response
```
```python
# 返回重定向
return redirect(url_for('index'))
```
```python
# 返回错误,直接将控制权交给web服务器
abort(404)
```
## flask扩展
Flask_script支持命令行选项
```python
from flask_script import Manager,shell


app = Flask(__name__)
manager = Manager(app)
# 为shell环境导入上下文，包括注册程序，数据库实例和模型
def make_shell_context():
    return dict(app=app,db=db,User=User,Role=Role)

manager.add_command('shell',Shell(make_contextmake_shell_context))
...
if __name__=='__main__':
    manager.run()
```
```shell
pip install flask_script

python hello.py runserver
```
# Flask-cli
>新的flask-cli代替了flask-script
```python
from flask import Flask
from flask_cli import FlaskCLI
app = Flask('myapp')
FlaskCLI(app)

# 自定义命令
@app.cli.command()
def mycmd():
    click.echo("Test")

# flask shell 上下文环境
@app.shell_context_processor
def myctx():
    return return dict(db=db, User=User)

# 添加参数
@app.cli.command()
@click.argument('name')
def create_user(name):
    ...
```
>这样就可以在终端使用 flask mycmd 了。
export FLASK_APP=hello
set FLASK_APP=hello

应用工厂
```python
import os
import click
from flask.cli import FlaskGroup

def create_wiki_app(info):
    from yourwiki import create_app
    return create_app(config=os.environ.get('WIKI_CONFIG', 'wikiconfig.py'))

@click.group(cls=FlaskGroup, create_app=create_wiki_app)
def cli():
    """This is a management script for the wiki application."""

if __name__ == '__main__':
    cli()
```


## 模版
jinja2模版引擎
```
templates/index.html
<h1>hello,{{key}}</h1>
```
渲染模版
```python
return render_template('index.html',key=value)
```
变量
>{{key}}支持常用数据类型 列表 字典 和对象

过滤器
>{{name|capitalize}}
safe|渲染时不转义
capitalize|首字母大写，其余小写
...

控制结构
```html
{%if bool%}
    yes
{%else%}
    no
{%endif%}
```
```html
{%for a in ls%}
    <li>{{a}}</li>
{%endfor%}
```
```html
宏的定义
{%macro func(msg)%}
    <li>{{msg}}</li>
{%endmacro%}
使用
{{func(key)}}
导入
{%import 'index.html' as index%}
{{index.func(key)}}
```
```html
导入代码片段
{%include 'index.html'%}
```
```html
模版
<html>
<head>
    {%block head%}
    <title>{%block title%}{%end block%}</title>
    {%endblock%}
</head>
<body>
    {%block body%}
    {%endblock%}
</body>
</html>

继承
{%extends 'index.html'%}
{%block title%}Index{%endblock%}
{%block head%}
    {{super()}}
    <style></style>
{%endblock%}
{%block body%}
hello
{%endblock%}
```
>super()获取模版原来的内容

Flask-bootstrap
```python
pip install flask-bootstrap

from flask_bootstrap import Bootstrap
bootstrap = Bootstrap(app)
```
```html
{%extends 'bootstrap/base.html'%}
```
自定义错误页面
```python
@app.errorhandler(404)
def page_not_found():
    return render_template('404.html'),404

@app.errorhandler(500)
def internal_server_error():
    return render_template('500.html'),500
```
静态文件
>在static文件夹下的图片，js，css
`url_for('sraric',filename='css/style.css')`

本地化时间
>pip install flask-moment
```python
# 初始化
from flask_moment import Moment
moment = Moment(app)
```
```html
引入moment.js库
{%block scripts%}
{{super()}}
{{moment.include_moment()}}
{{moment.lang('zh-cn')}}
{%endblock%}
```
```python
# 加入datetime变量
from datetime import datetime

@app.route('/')
def index():
    return render_templte('index.html',current_time=datetime.utcnow())
```
```html
显示
<p>localtime is {{moment(current_time).format('LLL')}}</p>
<p>that was {{moment(current_time).fromNow(refresh=True)}}</p>
```
## Web表单
>pip install flask-wtf
```python
from flask_wtf import FlaskForm
from wtforms import StringField, SubmitField
from wtforms.validators import DataRequired


class NameForm(FlaskForm):
    name = StringField('What is your name?', validators=[DataRequired()])
    submit = SubmitField('Submit')
```
>表单字段和验证函数见P35

表单渲染
```html
{%import "bootstrap/wtf.html" as wtf%}
{{wtf.quick_form(form)}}
```
视图函数
```python
app.config['SECRET_KEY']='key'
@app.route('/',methods=['GET','POST'])
def index():
    name=None
    form = NameForm()
    if form.validate_on_submit():
        name = form.name.data
        form.name.data=''
    return render_template('index.html',
                           current_time = datetime.utcnow(),
                           name = name,
                           form = form)
```
重定向和用户会话
```python
from flask import Flask,render_template,session,redirect,url_for

@app.route('/',methods=['GET','POST'])
def index():
    form = NameForm()
    if form.validate_on_submit():
        session['name'] = form.name.data
        return redirect(url_for('index'))
    return render_template('index.html',
                           current_time = datetime.utcnow(),
                           name = session.get('name'),
                           form = form)
```
Flash消息
```python
from flask import flash

flash('msg')
```
```html
{%for msg in get_flashed_messages()%}
{{msg}}
{%endfor%}
```
## 数据库
Flask-SQLAlchemy
>pip install flask-sqlalchemy
```
mysql://username:password@hostname/database
postgresql://uname:pwd@host/data
(unix)sqlite:////abspath
(windows)sqlite:///abspath
```
配置数据库
```python
from flask_sqlalchemy import SQLAlchemy

basedir=os.path.abspath(os.path.dirname(__file__))

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URL']='sqlite:///'+os.path.join(basedir,'data.sqlite')
app.config['SQLALCHEMY_COMMIT_ON_TEARDOWN']=True
db=SQLAlchemy(app)
```
定义模型
```python
class Role(db.Model):
    __tablename__='roles'
    id=db.Column(db.Integer,primary_key=True)
    name = db.Column(db.String(64),unique=True)

    def __repr__(self):
        return '<Role %r>'%self.name


class User(db.Model):
    __tablname__='users'
    id=db.Column(db.Integer,primary_key=True)
    username=db.Column(db.String(64),unique=True,index=True)

    def __repr__(self):
        return '<Role %r>'%self.username
```
>SQLAlchemy字段常用数据类型和选项见P48

关系
```python
class Role(db.Model):
    #...
    users = db.relationship('User',backref='role')


class User(db.Model):
    #...
    role_id=db.Column(db.Integer,db.ForeignKey('roles.id'))
```
>1. role_id为外键，值为roles表中的id值；
>1. Role模型中的users属性代表这个关系的面相对象视角。对于一个Role类的实例，其users属性将返回与角色相关联的用户组成的列表
>1. db.relationship()中的backref参数向User模型中添加了一个role属性，从而定义反向关系。这个属性可以代替role_id 访问Role模型，此时获取的是模型对象，而非外键的值

>SQLAlchemy常用关系选项见P49

数据库操作
```python
#创建表
pythonshell
from hello import db
db.create_all() #如果存在则不会改动

db.drop_all()   #删除旧表

#插入行
from hello import User,Role
role_admin=Role(name='Admin')
role_user=Role(name='User')
user_leon=User(name='leon',role=role_admin)
db.session.add(role_admin)
db.session.commit()
db.session.add_all([role_user,user_leon])
db.session.commit()

# 回滚
db.session.roleback()

# 修改行
role_admin.name='Administrator'
db.session.add(role_admin)
db.session.commit()

# 删除行
db.session.delete(role_admin)
db.session.commit()

#查询行
Role.query.all()
User.query.filter_by(role=role_user).all()

#加载python对象
role_user=Role.query.filter_by(name='User').first()

# 关系查询
users=role_user.users
#返回该角色对应的用户列表
# 加入参数禁止自动查询
class Role(db.Model):
    #...
    users=db.relationship('User',backref='role',lazy='dynamic')
    #...
# 之后查询该属性会返回一个未执行的查询，可以添加过滤器
role_user.users.order_by(User.username).all()
```
>SQLAlchemy常用查询过滤器见P53
>SQLAlchemy常用查询执行函数见P54

视图函数中操作数据库
```python
user=User.query.filter_by(username=form.name.data).first()
if user is None:
    user=User(username=form.name.data)
    db.session.add(user)
    session['known']=False
else:
    session['known']=True
session['name']=form.name.data
```
Flask-Migrate数据库迁移
>用于修改数据库模型后的更新，无需删除旧表再创建新表。
Flask-Migrate是基于Alembic的轻量包装，并集成到了flask-script
>pip install flask-migrate

```python
from flask_migrate import Migrate,MigrateCommand

migrate=Migrate(app,db)
migrate.add_command('db',MigrateCommand)


#创建迁移仓库
python hello.py db init
#会生成migrations文件夹，需要加入git中
#迁移脚本 改动upgrade()，还原downgrade()
#自动生成迁移脚本
python hello.py db migrate -m 'initial migration'
#自动生成可能会漏掉一些细节，需要人工检查

#更新数据库
python hello.py db upgrade
# 首次使用相当于db.create_all(),之后的迁移能把改动应用到数据库中且不影响其中保存的数据
```
## 电子邮件
>用于向用户发送消息，Flask-Mail封装了python的smtplib标准库，能更好的与flask集成。
pip install flask-mail
SMTP服务器配置见P59

flask-mail使用邮箱
```python
app.config['MAIL_SERVER']='smtp.google.com'
app.config['MAIL_PORT']=587
app.config['MAIL_USE_TLS']=True
app.config['MAIL_USERNAME']=os.environ.get('MAIL_USERNAME')
app.config['MAIL_PASSWORD']=os.environ.get('MAIL_PASSWORD')
#敏感信息建议从环境变量中导入
```
初始化
```python
from flask_mail import Mail
mail=Mail(app)

```
设置环境变量
```shell
(linux)
export MAIL_USERNAME=<gmail username>
export MAIL_PASSWORD=<gmail password>
(windows)
set MAIL_USERNAME=<gmail username>
set MAIL_PASSWORD=<gmail password>

```
在shell中发送邮件
```shell
python hello.py shell
from flask_mail import Message
from hello import mail
msg=Message('test msg',sender='xx@xx.com',recipients=['xx@xx.com'])
msg.body='text body'
msg.html='<b>HTML</b> body'
with app.app_context():
    mail.send(msg)
```
在程序中集成电子邮件功能
```python
from flask_mail import Message
from threading import Thread
#邮件主题前缀和发件人地址
app.config['FLASKY_MAIL_SUBJECT_PREFIX']='[Flasky]'
app.config['FLASKY_MAIL_SENDER']='Flasky Admin <flasky@xx.com>'

def send_email(to,subject,template,**kwargs):
    #to:收件人地址
    #subject:主题
    #template渲染模版
    #关键字参数 渲染用
    msg=Message(app.config['FLASKY_MAIL_SUBJECT_PREFIX']+subject,
    sender=app.config['FLASKY_MAIL_SENDER'],recipients=[to])
    msg.body=render_template(template+'.txt',**kwargs)
    msg.html=render_template(template+'.html',**kwargs)
    mail.send(msg)

# 异步发送
def send_async_email(app,msg):
    with app.app_context():
        mail.send(msg)

def send_email_new(to,subject,template,**kwargs):
    msg=Message(app.config['FLASKY_MAIL_SUBJECT_PREFIX']+subject,
    sender=app.config['FLASKY_MAIL_SENDER'],recipients=[to])
    msg.body=render_template(template+'.txt',**kwargs)
    msg.html=render_template(template+'.html',**kwargs)
    thr=Thread(target=send_async_email,args=[app,msg])
    thr.start()
    return thr
```
>邮件样例见P62

## 大型程序的结构
![prostruct](prostruct.png)

config.py配置
```python
import os
basedir = os.path.abspath(os.path.dirname(__file__))


class Config:
    SECRET_KEY = os.environ.get('SECRET_KEY') or 'hard to guess string'
    MAIL_SERVER = os.environ.get('MAIL_SERVER', 'smtp.googlemail.com')
    MAIL_PORT = int(os.environ.get('MAIL_PORT', '587'))
    MAIL_USE_TLS = os.environ.get('MAIL_USE_TLS', 'true').lower() in \
        ['true', 'on', '1']
    MAIL_USERNAME = os.environ.get('MAIL_USERNAME')
    MAIL_PASSWORD = os.environ.get('MAIL_PASSWORD')
    FLASKY_MAIL_SUBJECT_PREFIX = '[Flasky]'
    FLASKY_MAIL_SENDER = 'Flasky Admin <flasky@example.com>'
    FLASKY_ADMIN = os.environ.get('FLASKY_ADMIN')
    SQLALCHEMY_TRACK_MODIFICATIONS = False

    @staticmethod
    def init_app(app):
        pass


class DevelopmentConfig(Config):
    DEBUG = True
    SQLALCHEMY_DATABASE_URI = os.environ.get('DEV_DATABASE_URL') or \
        'sqlite:///' + os.path.join(basedir, 'data-dev.sqlite')


class TestingConfig(Config):
    TESTING = True
    SQLALCHEMY_DATABASE_URI = os.environ.get('TEST_DATABASE_URL') or \
        'sqlite://'


class ProductionConfig(Config):
    SQLALCHEMY_DATABASE_URI = os.environ.get('DATABASE_URL') or \
        'sqlite:///' + os.path.join(basedir, 'data.sqlite')


config = {
    'development': DevelopmentConfig,
    'testing': TestingConfig,
    'production': ProductionConfig,

    'default': DevelopmentConfig
}

```

工厂函数
```python
from flask import Flask
from flask_bootstrap import Bootstrap
from flask_mail import Mail
from flask_moment import Moment
from flask_sqlalchemy import SQLAlchemy
from flask_login import LoginManager
from config import config

bootstrap = Bootstrap()
mail = Mail()
moment = Moment()
db = SQLAlchemy()

login_manager = LoginManager()
login_manager.login_view = 'auth.login'


def create_app(config_name):
    app = Flask(__name__)
    app.config.from_object(config[config_name])
    config[config_name].init_app(app)

    bootstrap.init_app(app)
    mail.init_app(app)
    moment.init_app(app)
    db.init_app(app)
    login_manager.init_app(app)

    from .main import main as main_blueprint
    app.register_blueprint(main_blueprint)

    from .auth import auth as auth_blueprint
    app.register_blueprint(auth_blueprint, url_prefix='/auth')

    return app

```
蓝本
>init文件中创建蓝本，导入其他视图模块`from . import views, errors`
其他视图模块从当前包中导入蓝本`from . import main`
```python
#main/__init__.py
from flask import Blueprint

main = Blueprint('main', __name__)
# 蓝本定义之后再导入视图模块，避免循环导入依赖
from . import views, errors


#main/errors.py
from flask import render_template
from . import main


@main.app_errorhandler(404)
def page_not_found(e):
    return render_template('404.html'), 404

# auth/views.py
@auth.before_app_request
def before_request():
    if current_user.is_authenticated \
            and not current_user.confirmed \
            and request.endpoint \
            and request.blueprint != 'auth' \
            and request.endpoint != 'static':
        return redirect(url_for('auth.unconfirmed'))
```
>1. 蓝本中使用errorhandler只能出发蓝本中的错误，注册全局错误处理需要使用app_errorhandler
>1. 在蓝本中使用url_for(),视图函数需要加命名空间，即蓝本名
如url_for('main.index')
或url_for('.index')
表示当前请求所在的蓝本，跨蓝本的重定向必须带蓝本名

启动脚本
>manage.py
```python
import os
from app import create_app,db
from app.models import User,Role
from flask_script import Manager,Shell
from flask_migrate import Migrate,MigrateCommand

app = create_app(os.getenv('FLASK_CONFIG') or "default")
manager = Manager(app)
migrate = Migrate(app,db)

def make_shell_context():
    return dict(app=app,db=db,User=User,Role=Role)

manager.add_command("shell",Shell(make_context=make_shell_context))
manager.add_command("db",MigrateCommand)

if __name__=="__main__":
    manager.run()
```

需求文件
```shell
pip freeze > requirements.txt
pip install -r requirements.txt
```
单元测试
```python
# 见书P72
```
创建数据库
```shell
python manage.py db upgrade
```































