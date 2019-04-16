# python代码质量

---
## 结构
### 仓库结构
>如果你的仓库包含了大量的垃圾文件或者混乱嵌套的目录结构，即使有漂亮的自述文档，用户也可能尚未看到就前往查看其他项目了。
```shell
建议的仓库结构：
README.rst
LICENSE
setup.py
requirements.txt
sample/__init__.py
sample/core.py
sample/helpers.py
docs/conf.py
docs/index.rst
tests/test_basic.py
tests/test_advanced.py
```
- 具体代码为 `./sample/` 或 `./sample.py`
如果你的模块内只有一个文件，你可以直接在仓库的根目录下：`./sample.py`
- 法律相关 `./LICENSE` 这个文件中应该包含完整的许可证文本和版权声明。
- 包安装和分发管理`./setup.py`
- 开发中的依赖 `./requirements.txt` 应该放置在仓库的根目录下
- 项目的参考文档 `./docs/`
- 软件包集成和单元测试 `./test_sample.py` 或 `./tests`
- 通用的管理任务 `./Makefile`
- 其他通用的管理脚本 例如 `manage.py` 或 `fabfile.py` 也应放在仓库的根目录。
### 代码结构
>项目结构应尽量避免：
>- 多个杂乱的循环依赖关系：例如不要用两个文件互相从对方那里导入类,产生循环依赖的关系
>- 隐藏的耦合关系：类与类之间的影响，即改变一个类的代码,会使其他类无法正常工作
>- 大量使用全局变量或上下文：如果类中使用了可以被修改的全局变量,而不是明确的使用参数传递,容易产生难以定位的错误
>- 面条式代码：代码有多页嵌套 if 子句、 for 循环，有大量的复制粘贴程序代码，并且没有适当的分块都称为面条式代码
>- 混沌代码：这类代码包含上百段相似的逻辑碎片，通常是缺乏合适结构的类或对象
### 模块
>Python模块是最主要的抽象层之一，我认为一个.py文件就是一个模块。抽象层允许将代码分为不同部分，每个部分包含相关的数据与功能。
>- 可通过 `import` 和 `from ... import` 语句完成
一旦您使用 `import` 语句，就可以使用这个模块。
>- 模块名称要短、使用小写，并避免使用特殊符号；不能使用`.`，也不推荐使用`?` 空格 `-` 和`_`
>- 不要使用带下划线的名称空间，应该使用子模块

>具体来说，`import modu` 语句将 寻找合适的文件，即调用目录下的 `modu.py` 文件（如果该文件存在）。如果没有 找到这份文件，Python解释器递归地在 "`PYTHONPATH`" 环境变量中查找该文件，如果仍没 有找到，将抛出 `ImportError` 异常

>一旦找到 `modu.py`，Python解释器将在隔离的作用域内执行这个模块。**所有顶层语句都会被执行**，包括其他的引用。方法与类的定义将会存储到模块的字典中。然后，这个模块的变量、方法和类通过命名空间暴露给调用方，这是Python中特别有用和强大的核心概念。

>在很多其他语言中，include file 指令被预处理器用来获取文件里的所有代码并‘复制’ 到调用方的代码中。Python则不一样：include代码被独立放在模块命名空间里，这意味着您 一般不需要担心include的代码可能造成不好的影响，例如重载同名方法。

```python
# 推荐的做法
import modu
modu.fun()

# 稍好的做法
from modu import fun
fun()

# 不推荐的做法
from modu import *
fun()
```
>总结：可读性意味着避免 无用且重复的文本和混乱的结构，因而需要花费一些努力以实现一定程度的简洁。但不能 过份简洁而导致简短晦涩。除了简单的单文件项目外，其他项目需要能够明确指出类和方法 的出处，例如使用 modu.func 语句，这将显著提升代码的可读性和易理解性。

### 包
>任意包含 `__init__.py` 文件的目录都被认为是一个Python包。导入一个包里不同 模块的方式和普通的导入模块方式相似，特别的地方是 `__init__.py` 文件将集合所有包范围内的定义。

>pack/ 目录下的 modu.py 文件通过 import pack.modu 语句导入。 该语句会在 pack 目录下寻找 `__init__.py` 文件，并执行其中所有顶层 语句。以上操作之后，modu.py 内定义的所有变量、方法和类在pack.modu命名空 间中均可看到。

>一个常见的问题是往 `__init__.py` 中加了过多代码，随着项目的复杂度增长， 目录结构越来越深，子包和更深嵌套的子包可能会出现。在这种情况下，导入多层嵌套 的子包中的某个部件需要执行所有通过路径里碰到的 `__init__.py`文件。如果 包内的模块和子包没有代码共享的需求，使用空白的 `__init__.py` 文件是正常甚至好的做法。

>最后，导入深层嵌套的包可用这个方便的语法：`import very.deep.module as mod`。该语法允许使用 `mod` 替代冗长的 `very.deep.module`。(使用了`import modu`的方法)

>个人理解： `__init__.py`文件只是将文件夹转化成有效的包，如果`init`文件空白，则导入包内模块时需要指定模块名称，如`from a.b import c`，如果想将包内所有模块全部从包名导入，则需要在init文件中提前导入，之后便可`from a import c`

### 面向对象编程
>Python有时被描述为面向对象编程的语言，这多少是个需要澄清的误导。在Python中 一切都是对象，并且能按对象的方式处理。函数、类、字符串乃至类型都是Python对象：与其他对象一样，他们有类型，能作为 函数参数传递，并且还可能有自己的方法和属性。

>与Java不同的是，python也可以面向过程编程(比如，使用较少甚至不使用类定义，类继承)

>面向过程中的两个概念
>- 函数的隐式上下文：由函数内部访问 到的所有全局变量与持久层对象组成
>- 副作用： 即函数可能使其隐式上下文发生改变(保存或删除)

>把有隐式上下文和副作用的函数与仅包含逻辑的函数(纯函数)谨慎地区分开来，会带来 以下好处：
>- 纯函数的结果是确定的：给定一个输入，输出总是固定相同。
>- 当需要重构或优化时，纯函数更易于更改或替换。
>- 纯函数更容易做单元测试：很少需要复杂的上下文配置和之后的数据清除工作。
>- 纯函数更容易操作、修饰和分发。

>总之，对于某些架构而言，纯函数比类和对象在构建模块时更有效率，因为他们没有任何 上下文和副作用。但显然在很多情况下，面向对象编程是有用甚至必要的

### 装饰器
>装饰器是一个函数或类，它可以 包装(或装饰)一个函数或方法。被 '装饰' 的函数或方法会替换原来的函数或方法。这个机制对于分离概念和避免外部不相关逻辑“污染”主要逻辑很有用处。

### 上下文管理器
>上下文管理器是一个Python对象，为操作提供了额外的上下文信息。 这种额外的信息， 在使用 with 语句初始化上下文，以及完成 with 块中的所有代码时，采用可调用的形式。实现这个功能有两种简单的方法：使用类或使用生成器。
```python
class CustomOpen(object):
    def __init__(self, filename):
        self.file = open(filename)

    def __enter__(self):
        return self.file

    def __exit__(self, ctx_type, ctx_value, ctx_traceback):
        self.file.close()


# 另一种方法
from contextlib import contextmanager

@contextmanager
def custom_open(filename):
    f = open(filename)
    try:
        yield f
    finally:
        f.close()
# 这与上面的类示例道理相通，尽管它更简洁。
```
>如果封装的逻辑量很大，则类的方法可能会更好。 而对于处理简单操作的情况，函数方法可能会更好。


### 动态类型
>Python是动态类型语言，这意味着变量的类型可以变换。

>Python 的动态类型常被认为是它的缺点，的确这个特性会导致复杂度提升和难以调试的代码。

>避免发生类似问题的参考方法：
>- 避免对不同类型的对象使用同一个变量名
---

## 风格
### 明确代码意义
```python
# 不推荐
def fun(*args):
    pass

# 推荐
def fun(a, b):
    pass
```
### 一行一个声明语句
>虽然在 Python 中我们推崇使用形如列表生成式这种简洁明了的复合语句，但是除此以外，我们应该尽量避免将两句独立分割的代码写在同一行。
```python
# 不推荐
if x == 1: print 'one'

# 推荐
if x == 1:
    print 'one'
```
### 函数的参数
>函数的参数可以使用四种不同的方式传递给函数。
```python
# 必选参数(是没有默认值的必填的参数,用于参数较少的函数的构成)
fun(a,b)

# 关键字参数(是非强制的，且有默认值,当一个函数有超过两个或三个位置参数
# 时，函数签名会变得难以记忆，使用带有默认参数的关键字参数有时候会给你带
# 来便利)
fun(a, b, c=None, d=None)

# 任意参数列表(在这个函数体中,args 是一个元组,它包含所有剩余的位置参数)
fun(a, *args)

# 任意关键字参数字典(在函数体中,kwargs是一个字典,它包含所有传递给函数
# 但没有被其他关键字参数捕捉的命名参数。)
fun(a,**kwargs)
```
>后两种技术在非特殊情况下，都要尽量避免使用，因为其缺乏简单和明确的结构来足够表达函数意图。

### 一些约定
>Python 允许很多技巧，其中一些具有潜在的危险。例如：任何客户端代码能够重写一个对象的属性和方法（Python 中没有 `private` 关键字）。

>私有属性的主要约定和实现细节是在所有的 内部 变量前加一个下划线。如果客户端代码打破了这条规则并访问了带有下划线的变量，那么因内部代码的改变而出现的任何不当的行为或问题，都是客户端代码的责任。

>任何不开放给客户端代码使用的方法或属性，应该有一个下划线前缀

### 返回值
>为了保持函数的可读性，建议在函数体中避免使用返回多个有意义的值。

>在函数中返回结果主要有两种情况：
>- 函数正常运行并返回它的结果，
>- 以及错误的情况。
>
> 如果你在面对第二种情况时不想抛出异常，返回一个值（比如说 None 或 False ）来表明函数无法正确运行。

>当一个函数在其正常运行过程中有多个主要出口点时，它会变得难以调试其返回结果，所以保持单个出口点可能会更好。这也将有助于提取某些代码路径，而且多个出口点很有可能意味着这里需要重构
### 习语
>编程习语，说得简单些，就是写代码的 方式。
符合习语的 Python 代码通常被称为 Pythonic。

>解包（Unpacking）
如果你知道一个列表或者元组的长度，你可以将其解包并为它的元素取名。比如，`enumerate()` 会对 `list` 中的每个项提供包含两个元素的元组：
`for index, item in enumerate(some_list):`
你也能通过这种方式交换变量：
a, b = b, a

>创建一个被忽略的变量
`filename = 'foobar.txt'`
`basename, __, ext = filename.rpartition('.')`

>创建一个含 N 个对象的列表
使用 Python 列表中的 `*` 操作符：
`four_nones = [None] * 4`

>根据列表来创建字符串
创建字符串的一个常见习语是在空的字符串上使用 `str.join()` ：
`letters = ['s', 'p', 'a', 'm']`
`word = ''.join(letters)`

>在集合体（collection）中查找一个项
查找集合是利用了 Python 中的『集合是可哈希』的特性，所以查询性能好
```python
s = set(['s', 'p', 'a', 'm'])
l = ['s', 'p', 'a', 'm']

def lookup_set(s):
    return 's' in s

def lookup_list(l):
    return 's' in l
```
>因为这些性能上的差异，在下列场景中，使用集合或者字典而不是列表，通常会是个好主意：
>- 集合体中包含大量的项；
>- 你将在集合体中重复地查找项；
>- 你没有重复的项。
>
>对于小的集合体、或者你不会频繁查找的集合体，建立哈希带来的额外时间和内存的开销经常会大过改进搜索速度所节省的时间。

### Python之禅
```python
>>> import this
The Zen of Python, by Tim Peters

Beautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
Complex is better than complicated.
Flat is better than nested.
Sparse is better than dense.
Readability counts.
Special cases aren't special enough to break the rules.
Although practicality beats purity.
Errors should never pass silently.
Unless explicitly silenced.
In the face of ambiguity, refuse the temptation to guess.
There should be one-- and preferably only one --obvious way to do it.
Although that way may not be obvious at first unless you're Dutch.
Now is better than never.
Although never is often better than *right* now.
If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.
Namespaces are one honking great idea -- let's do more of those!
'''
Python之禅 by Tim Peters

优美胜于丑陋（Python 以编写优美的代码为目标）
明了胜于晦涩（优美的代码应当是明了的，命名规范，风格相似）
简洁胜于复杂（优美的代码应当是简洁的，不要有复杂的内部实现）
复杂胜于凌乱（如果复杂不可避免，那代码间也不能有难懂的关系，要保持接口简洁）
扁平胜于嵌套（优美的代码应当是扁平的，不能有太多的嵌套）
间隔胜于紧凑（优美的代码有适当的间隔，不要奢望一行代码解决问题）
可读性很重要（优美的代码是具备高可读性的）
即便假借特例的实用性之名，也不可违背这些规则（这些规则至高无上）
不要包容所有错误，除非您确定需要这样做（精准地捕获异常，不写 `except:pass` 风格的代码）
当存在多种可能，不要尝试去猜测
而是尽量找一种，最好是唯一一种明显的解决方案（如果不确定，就用穷举法）
虽然这并不容易，因为您不是 Python 之父（这里的 Dutch 是指 Guido ）
做也许好过不做，但不假思索就动手还不如不做（动手之前要细思量）
如果您无法向人描述您的方案，那肯定不是一个好方案；反之亦然（方案测评标准）
命名空间是一种绝妙的理念，我们应当多加利用（倡导与号召）'''
```
### PEP 8
>PEP 8 是 Python 实际意义上的代码风格指南，我们可以在 pep8.org 上获得高质量的、可读性更高的 PEP 8 版本。

### 约定
>这里有一些你应该遵循的约定，以让你的代码更加易读。

>三目运算
```python
h = "变量1" if a>b else "变量2" #变量
h = a-b if a>b else a+b #公式
```

>检查变量是否等于常量

坏
```python
if attr == True:
    print 'True!'
if attr == None:
    print 'attr is None!'
```

好
```python
# 检查值
if attr:
    print 'attr is truthy!'
# 或者做相反的检查
if not attr:
    print 'attr is falsey!'
# 或者，None 等于 false，你可以直接相较它进行匹配
if attr is None:
    print 'attr is None!'
```
>访问字典元素

坏
```python
d = {'hello': 'world'}
if d.has_key('hello'):
    print d['hello']    # prints 'world'
else:
    print 'default_value'
```
好
```python
d = {'hello': 'world'}

print d.get('hello', 'default_value') # prints 'world'
print d.get('thingy', 'default_value') # prints 'default_value'
```
或者:
```python
if 'hello' in d:
    print d['hello']
```
>操作列表的简便方法

坏:
```python
# Filter elements greater than 4
a = [3, 4, 5]
b = []
for i in a:
    if i > 4:
        b.append(i)
```
好:
```python
a = [3, 4, 5]
b = [i for i in a if i > 4]
# Or:
b = filter(lambda x: x > 4, a)
```
坏:
```python
# Add three to all list members.
a = [3, 4, 5]
for i in range(len(a)):
    a[i] += 3
```
好:
```python
a = [3, 4, 5]
a = [i + 3 for i in a]
# Or:
a = map(lambda i: i + 3, a)
```
>使用 enumerate() 来跟踪正在被处理的元素索引。
```python
a = [3, 4, 5]
for i, item in enumerate(a):
    print i, item
# prints
# 0 3
# 1 4
# 2 5
#比起手动计数，使用enumerate()函数有更好的可读性，而且更加适合在迭代器中使用。
```
>读文件

坏:
```python
f = open('file.txt')
a = f.read()
print a
f.close()
```
好:
```python
with open('file.txt') as f:
    for line in f:
        print line
# 即使在 with 控制块中出现了异常，它也能确保你关闭了文件，
# 因此，使用 with 语法是更加优雅的。
```
>行的延续
当一个代码逻辑行的长度超过可接受的限度时，你需要将之分为多个物理行。如果行的结尾是一个反斜杠，Python 解释器会把这些连续行拼接在一起。这在某些情况下很有帮助， 但我们总是应该避免使用，因为它的脆弱性：如果在行的结尾，在反斜杠后加了空格，这会破坏代码，而且可能有意想不到的结果。
一个更好的解决方案是在元素周围使用括号。左边以一个未闭合的括号开头，Python 解释器会把行的结尾和下一行连接起来直到遇到闭合的括号。同样的行为适用中括号和大括号。

糟糕：
```python
my_very_big_string = """For a long time I used to go to bed \
early. Sometimes,when I had put out my candle, my eyes would \
close so quickly that I had not eventime to say "I'm going to \
sleep."""

from some.deep.module.inside.a.module import a_nice_function,\
another_nice_function,yet_another_nice_function
```
优雅：
```python
my_very_big_string = (
    "For a long time I used to go to bed early. Sometimes,"
    "when I had put out my candle, "
    "my eyes would close so quickly that I "
    "had not even time to say \"I'm going to sleep.\""
)

from some.deep.module.inside.a.module import (a_nice_function,
another_nice_function, yet_another_nice_function)
```
>尽管如此，通常情况下，必须去分割一个长逻辑行意味着你同时想做太多的事，这可能影响可读性。
---
## 推荐的阅读 Python 项目
1. Howdoi Howdoi 使用 Python 实现的代码搜索工具。
1. Flask Flask 是基于 Werkzeug and Jinja2 的 Python 微框架。 它的目的是快速入门并开发实现你头脑中的好主意。
1. Diamond Diamond 是使用 python 实现的用于收集监控数据的工具，主要收集 metrics 类型的数据，并将其发布到 Graphite 或其他后台。它能够收集 cpu ， 内存， 网络， i/o ，负载和磁盘 metrics 数据。此外，它还提供 API 用以实现自定义收集器从任意来源中收集指标数据。
1. Werkzeug Werkzeug 最初是 WSGI 应用程序的各种实用工具的简单集合，并已成为最先进的 WSGI 实用程序模块之一。它包括强大的调试器、功能齐全的请求和响应对象、处理实体标记的 HTTP 实用程序、缓存控制头、HTTP 日期、cookie 处理、文件上传、强大的 URL 路由系统和一群社区贡献的插件模块。
1. Requests Requests 是一个用 Python 实现的 Apache2 授权的 HTTP 库供大家使用。
1. Tablib Tablib 是用 Python 实现的无格式的表格数据集库。
---
## 文档

### 项目文档
>在根目录下 `README 文件`应该对用户和项目维护者提供概述信息。它应该是原始文本或非常容易阅读的标记写成，如 `reStructuredText` 或 `Markdown`。 它应该包含几行对项目或库的作用的解释（假设用户不知道项目的任何内容），软件源站点的 URL 和一些基本的信用信息。这个文件应该是代码阅读者的主要入口点。

>`INSTALL 文件`对 Python 来说不是必需的。安装指令通常被简化为一个命令，如 `pip install module` 或 `python setup.py install` 并且添加到 `README 文件`中。

>`LICENSE 文件`应该 始终 存在并且详细说明软件在什么许可证下对公众可用。

>`TODO 文件`或 `README 文件`中的 `TODO` 部分应该列出代码的开发计划。

>`CHANGELOG 文件`或在 `README` 对应的部分应该基于最新版本编写一个代码变更概述。
### 其他文档
>根据项目的不同，文档中最好能包含下列部分或所有的内容：
>- 一份 简短介绍 ，应该包含几个简化的用例，简要概述该产品能够用来做什么。
>- 一份 教程 ，应该展示一些主要的用例以及更多的使用细节。读者能够跟着一步步成功搭建工作原型。
>- 一份 API 文档，通常从代码中产生（参见 docstrings）。它会列出所有的可供公共访问的接口、参数和返回值。
>- 一份 贡献文档 适用于潜在贡献者。这可以包括项目的代码规范和通用设计的策略讲解。
### 相关工具
>[Sphinx](https://pythoncaff.com/docs/python-guide/2018/documentation/20#Sphinx) 
[reStructuredText](https://pythoncaff.com/docs/python-guide/2018/documentation/20#reStructuredText)
### 源码文档建议
>在 Python 中我们使用 文档字符串（docstrings） 用来描述模块、类和函数：

```python
def square_and_rooter(x):
    """Return the square root of self times self."""
    ...
```
>一般来说，我们要遵循 [PEP 8#comments](https://www.python.org/dev/peps/pep-0008/#comments) （" Python 风格指南"）的注释部分。 更多关于 docstrings 的内容可以在 [PEP 0257#specification](https://www.python.org/dev/peps/pep-0257/#specification) （docstrings 约定指南） 上找到。

### 注释代码块
>请不要使用三引号去注释代码
---
## 代码测试
https://pythoncaff.com/docs/python-guide/2018/tests/21

---
## 日志记录
https://pythoncaff.com/docs/python-guide/2018/logging/22

---
## 常见陷阱
https://pythoncaff.com/docs/python-guide/2018/gotchas/23

---
## 选择许可
https://pythoncaff.com/docs/python-guide/2018/license/24










---

[参考：python最佳实践指南](https://pythoncaff.com/docs/python-guide/2018)
[参考：python最佳实践指南](https://pythonguidecn.readthedocs.io/zh/latest/)
