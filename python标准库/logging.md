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













