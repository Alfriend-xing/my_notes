# Regular expression operations
    提供正则表达式匹配操作

## 语法
```
'.' 在默认模式下可匹配除换行符外的任意字符，如果指定re.DOTALL标志则可匹配任意字符

'^' 匹配字符串的开头部分，在re.MULTILINE模式也可以匹配每行的开头(换行符之后)

'$' 匹配字符串结尾(换行符之前)

'*' 匹配0个或多个(ab*会匹配a,ab,abb,abbbb等)

'+' 匹配一个或多个(ab+会匹配ab,abb,abbbb,而不会匹配a)

'?' 匹配0个或1个
上述三个重复限定符都是贪婪的，(<.*> 会匹配整个 <a> b <c> 而不是<a>)，
后接一个?会以非贪婪模式匹配，即匹配尽可能少的字符
'*?' '+?' '??'

{m} 指定前方正则匹配的次数(a{6}会匹配6个a，不匹配5个a)
{m,n} 指定匹配次数从m到n次；{m,}表示匹配不少于m次；{m,n}
{m,n}? 非贪婪版本(对于'aaaaaa'，a{3,5}将匹配5个'a'字符，而a{3,5}?只匹配3个字符)

'\' 转义特殊字符 (\* \? 可以匹配*和?)
[] 表示一组字符 ([amk]将匹配'a','m'或'k';[a-z]将匹配任何小写ASCII字母;
如果-被转义'[a\-z]'或位于开头或结尾'[a-]'，则匹配'-';
[^5]将匹配除5外任何字符;'[|]','[\]'将匹配'|' '\')

'|' A|B匹配A或B

'()'匹配括号内的正则表达式，并指示组的开始和结束

'(?...)'

'(?iLmsux)'

'(?:...)'

'(?P<name>...)'

\w	匹配字母数字及下划线
\W	匹配非字母数字及下划线
\s	匹配任意空白字符，等价于 [\t\n\r\f].
\S	匹配任意非空字符
\d	匹配任意数字，等价于 [0-9].
\D	匹配任意非数字
\A	匹配字符串开始
\Z	匹配字符串结束，如果是存在换行，只匹配到换行前的结束字符串。
\z	匹配字符串结束
\G	匹配最后匹配完成的位置。
\b	匹配一个单词边界，也就是指单词和空格间的位置。例如， 'er\b' 可以匹配"never" 中的 'er'，但不能匹配 "verb" 中的 'er'。
\B	匹配非单词边界。'er\B' 能匹配 "verb" 中的 'er'，但不能匹配 "never" 中的 'er'。
\n, \t, 等.	匹配一个换行符。匹配一个制表符。等
\1...\9	匹配第n个分组的内容。
\10	匹配第n个分组的内容，如果它经匹配。否则指的是八进制字符码的表达式
```

## 模块函数

### re.compile

```python
re.compile(pattern, flags=0)
#构建正则表达式对象
#pattern为正则表达式
#flags为匹配模式，可以为以下值

re.A
re.ASCII
#用ascii匹配\w，\W，\b，\B，\d，\D，\s和\S

re.I
re.IGNORECASE
#匹配时不区分大小写

re.L
re.LOCALE
#以当前的语言环境匹配\w，\W，\b，\B

re.M
re.MULTILINE
#使^和$匹配每行的开头(识别换行符)，默认只匹配字符串的开头

re.S
re.DOTALL
#使.匹配所有字符，默认不匹配换行符

re.X
re.VERBOSE
#允许正则表达式具有可读性，忽略空白，换行符，允许注释#


```

### re.search

```python
re.search(pattern, string, flags=0)
#查找第一个匹配的位置，返回match对象，无匹配返回None
```
### re.match
```python
re.match(pattern, string, flags=0)
#从开头进行匹配
```
### re.fullmatch

```python
re.fullmatch(pattern, string, flags=0)
#如果整个字符串与表达式匹配则返回match对象，否则返回None
```
### re.split
```python
re.split(pattern, string, maxsplit=0, flags=0)
#以表达式分割字符串，返回list
```
### re.findall
```python
re.findall(pattern, string, flags=0)
#列表形式返回所有匹配的项
```
### re.finditer
```python
re.finditer(pattern, string, flags=0)
#以迭代器形式返回所有匹配的项
```
### re.sub
```python
re.sub(pattern, repl, string, count=0, flags=0)
#返回repl替换匹配位置后的字符串
```
### re.purge()
```python
re.purge()
#清除正则表达式缓存
```
## 正则表达式对象
```python
Pattern.search(string[, pos[, endpos]])
#匹配第一个符合项
#pos指定起始位置，endpos指定结束位置

Pattern.match(string[, pos[, endpos]])
#从开头匹配

Pattern.fullmatch(string[, pos[, endpos]])
#如果整个字符串与表达式匹配则返回match对象，否则返回None

Pattern.split(string, maxsplit=0)

Pattern.findall(string[, pos[, endpos]])

Pattern.finditer(string[, pos[, endpos]])

Pattern.sub(repl, string, count=0)

Pattern.flags
#匹配模式

Pattern.groups
#正则组个数

Pattern.groupindex
#如果有，返回带有名称的匹配组

Pattern.pattern
#返回表达式字段

```

## Match对象
    由match,serch方法返回，bool值为True，以此判断是否有匹配结果
```python
Match.expand(template)

Match.group([group1, ...])
#返回指定的匹配组，如果只有一个组，返回字符串
#参数可以是组索引，组名

Match.groups(default=None)
#返回所有匹配组

Match.groupdict(default=None)
#返回所有组索引名称

Match.pos
Match.endpos
Match.re
Match.string
```

## 实例
[Regular Expression Examples](https://docs.python.org/3/library/re.html#regular-expression-examples)