# click
---
>Click 相比于 Argparse，就好比 requests 相比于 urllib。

## `@click.command()`
>装饰一个函数，使之成为命令行接口
```python
import click
@click.command()
def func():
    click.echo('hello')
    
if __name__=="__main__":
    func()
```
>`python hello.py`
## `@click.option()`
```python
import click
@click.command()
@click.option('-c','--count', default=1, help='Number of greetings.')
@click.option('-n','--name', prompt='Your name', help='The person to greet.')
def hello(count, name):
    """Simple program that greets NAME for a total of COUNT times."""
    for x in range(count):
        click.echo('Hello %s!' % name)
if __name__ == '__main__':
    hello()
```
>option 最基本的用法就是通过指定命令行选项的名称，从命令行读取参数值，再将其传递给函数
option 常用的设置参数如下：
> - default: 设置命令行参数的默认值
> - help: 参数说明
> - type: 参数类型，可以是 string, int, float 等
> - prompt: 当在命令行中没有输入相应的参数时，会根据 prompt 提示用户输入(如$password:)
## `@click.group()`
>把方法装饰为可以拥有多个子命令的 Group 对象。由 Group.add_command() 方法把 Command 对象关联到 Group 对象。
```python
@click.group()
def cli():
    pass

@cli.command()
def initdb():
    click.echo('Initialized the database')

@cli.command()
def dropdb():
    click.echo('Dropped the database')
```
>添加子命令时装饰器的对象为click组`@cli.command()`,而不是`@click.command()`
添加子命令的参数应使用装饰器 `@click.option()`

## `click.Choice`
>用于限定输入值属于备选项
```python
import click
@click.command()
@click.option('--gender', type=click.Choice(['man', 'woman']))    # 限定值
def choose(gender):
    click.echo('gender: %s' % gender)
if __name__ == '__main__':
    choose()
```
```shell
#执行
$ python click_choice.py --gender boy
Usage: click_choice.py [OPTIONS]
Error: Invalid value for "--gender": invalid choice: boy. (choose from man, woman)
$ python click_choice.py --gender man
gender: man
```

## `click.IntRange`
>用于限定参数范围
```python
@click.command()
@click.option('--count', type=click.IntRange(0, 20, clamp=True))
@click.option('--digit', type=click.IntRange(0, 10))
def repeat(count, digit):
    click.echo(str(digit) * count)

if __name__ == '__main__':
    repeat()
```
>默认模式，当超出范围后抛出异常
>clamp=True时，越界后取极值

## `nargs`
>多参数
```python
import click
@click.command()
@click.option('--center', nargs=2, type=float, help='center of the circle')
@click.option('--radius', type=float, help='radius of the circle')
def circle(center, radius):
    click.echo('center: %s, radius: %s' % (center, radius))
if __name__ == '__main__':
    circle()
```
>nargs表示接收参数的数量，此处接受两个参数
```shell
# 执行
$ python click_multi_values.py --center 3 4 --radius 10
center: (3.0, 4.0), radius: 10.0
```
## `hide_input`
>用于输入密码
```python
import click
@click.command()
@click.option('--password', prompt=True, hide_input=True, confirmation_prompt=True)
def input_password(password):
    click.echo('password: %s' % password)
if __name__ == '__main__':
    input_password()
```
>hide_input 用于隐藏输入，confirmation_promt 用于重复输入
```shell
$ python click_password.py
Password:                         # 不会显示密码
Repeat for confirmation:          # 重复一遍
password: 666666
```
## `is_flag`
>无参数选项
```python
import click
def print_version(ctx, param, value):
    if not value or ctx.resilient_parsing:
        return
    click.echo('Version 1.0')
    ctx.exit()

@click.command()
@click.option('--version', is_flag=True, callback=print_version,
              expose_value=False, is_eager=True)
@click.option('--name', default='Ethan', help='name')
def hello(name):
    click.echo('Hello %s!' % name)
if __name__ == '__main__':
    hello()
```
>is_eager=True 表明该命令行选项优先级高于其他选项；
expose_value=False 表示如果没有输入该命令行选项，会执行既定的命令行流程；
callback 指定了输入该命令行选项时，要跳转执行的函数；

## `@click.argument()`
>直接输入参数，不用指定选项
```python
import click
@click.command()
@click.argument('x')
@click.argument('y')
@click.argument('z')
def show(x, y, z):
    click.echo('x: %s, y: %s, z:%s' % (x, y, z))
if __name__ == '__main__':
    show()
```
```shell
$ python click_argument.py 10 20 30
x: 10, y: 20, z:30
```
接收不定量的参数
```python
import click
@click.command()
@click.argument('src', nargs=-1)
@click.argument('dst', nargs=1)
def move(src, dst):
    click.echo('move %s to %s' % (src, dst))
if __name__ == '__main__':
    move()
```
>nargs=-1 表明参数 src 接收不定量的参数值，参数值会以 tuple 的形式传入函数。如果 nargs 大于等于 1，表示接收 nargs 个参数值
```shell
$ python click_argument.py file1 file2 file3 trash   # src=('file1', 'file2', 'file3')  dst='trash'
move (u'file1', u'file2', u'file3') to trash
```

## `@click.confirmation_option()`
>危险操作弹出确认提示
```python
import click
@click.command()
@click.confirmation_option(prompt='Are you sure you want to drop the db?')
def dropdb():
    click.echo('Dropped all tables!')
    
if __name__ == '__main__':
    dropdb()
```
```shell
$ python ctx.py
Are you sure you want to drop the db? [y/N]: n
Aborted!
$ python ctx.py
Are you sure you want to drop the db? [y/N]: y
Dropped all tables!
```
## `click.secho()`
>彩色输出
>pip install colorama
```python
import click
@click.command()
@click.option('--name', help='The person to greet.')
def hello(name):
    click.secho('Hello %s!' % name, fg='red', underline=True)
    click.secho('Hello %s!' % name, fg='yellow', bg='black')
if __name__ == '__main__':
    hello()
```
>fg 表示前景颜色（即字体颜色），可选值有：BLACK, RED, GREEN, YELLOW, BLUE, MAGENTA, CYAN, WHITE 等；
bg 表示背景颜色，可选值有：BLACK, RED, GREEN, YELLOW, BLUE, MAGENTA, CYAN, WHITE 等；
underline 表示下划线，可选的样式还有：dim=True，bold=True 等；



参考：
[命令行神器 Click](http://funhacks.net/2016/12/20/click/)
[Python Click 学习笔记](https://isudox.com/2016/09/03/learning-python-package-click/)

