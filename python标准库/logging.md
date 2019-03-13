# logging
    日志记录模块
---
## 基本用法

```python
import logging
# debug(), info(), warning(), error() and critical()
logging.warning('Watch out!')
logging.info('I told you so')

#记录到文件
logging.basicConfig(filename='example.log',level=logging.DEBUG)

#输出数据
logging.warning('%s before you %s', 'Look', 'leap!')

#设置输出格式
logging.basicConfig(format='%(levelname)s:%(message)s', level=logging.DEBUG)
logging.debug('This message should appear on the console')
#DEBUG:This message should appear on the console
logging.basicConfig(format='%(asctime)s %(message)s')
logging.warning('is when this event was logged.')
#2010-12-12 11:41:42,612 is when this event was logged.

#设置时间格式
logging.basicConfig(format='%(asctime)s %(message)s', datefmt='%m/%d/%Y %I:%M:%S %p')
logging.warning('is when this event was logged.')
#12/12/2010 11:46:36 AM is when this event was logged.
```

basicConfig(**kwargs)

格式 | 描述
-----|-----
filename | 保存日志的文件名
filemode |如果指定了filename，打开文件的方式，默认'a'
format | 日志格式
datefmt |事件格式 time.strftime().
style | 如果指定了format ，指定格式字符串的风格，'%' '{' '$'
level | 日志级别
stream | 使用指定的流初始化StreamHandler，与filename不相容
handlers | 如果指定，参数为创建好的handler所组成的迭代器，这些迭代器会被添加到root logger中，此参数与filename或stream不兼容

## Logger
    logger不应直接实例化，应通过模块函数logging.getLogger(name)
```python
logger = logging.getLogger(__name__)
#建议以模块名命名logger
#debug(), info(), warning(), error() critical()与rootlogger相同
#默认格式severity:logger name:message

Logger.setLevel()
#设置日志记录级别

Logger.addHandler()
Logger.removeHandler()
#添加handler

Logger.addFilter()
Logger.removeFilter()
#添加过滤器



```

## Handler

    用于定义处理日志消息的方式，应实例化其子类

```python
setLevel(level)

setFormatter(fmt)
#fmt=logging.Formatter

addFilter(filter)
removeFilter(filter)

logging.StreamHandler(stream=None)
#将日志记录输出发送到标准输出

logging.FileHandler(filename, mode='a', encoding=None, delay=False)
#将日志输出到文件

logging.handlers.WatchedFileHandler(filename, mode='a', encoding=None, delay=False)
#它监视要记录的文件。 如果文件发生更改，则会关闭该文件并使用文件名重新打开。只适用于Unix / Linux

logging.handlers.RotatingFileHandler(filename, mode='a', maxBytes=0, backupCount=0, encoding=None, delay=False)
#日志文件轮转

logging.handlers.TimedRotatingFileHandler(filename, when='h', interval=1, backupCount=0, encoding=None, delay=False, utc=False, atTime=None)
#按时间周期记录日志
#when可选参数s,m,h,d(秒分时天),w0-w6,midnight
#interval表示翻转时覆盖的文件序号，1表示覆盖最早的文件
#backupCount指定备份数量
#utc参数为true，则使用UTC时间; 否则使用当地时间
#atTime需指定datetime.time对象

```
>Handler还支持Queue,socket,http,Memory,smtp,NTEvent,SysLog等方式

## Formatter

```python
logging.Formatter(fmt=None, datefmt=None, style='%')
```

实例

[Logging Cookbook](https://docs.python.org/3/howto/logging-cookbook.html)

