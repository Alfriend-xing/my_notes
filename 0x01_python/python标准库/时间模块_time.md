# time&datetime&calendar

## time
提供与时间有关的简单功能
>部分函数在部分平台不可用

### 模块函数

```python
time.asctime([t])
#转换元组或struct_time对象为一个字符串，无参数则返回当前时间

time.clock()
#返回程序运行时间，将在3.8版本中移除，
#可用 perf_counter() process_time()代替

time.ctime([secs])
# 将 秒secs 转化为表示本地时间的字符串

time.gmtime([secs])
# 将秒转化为utc时间，返回struct_time对象

time.localtime([secs])
# 将秒转化为本地时间，返回struct_time对象

time.mktime(t)
# 将struct_time对象转化为时间戳

time.perf_counter()
# 最高精度记录程序运行时间 忽略sleep时间，返回float

time.perf_counter_ns()
# 返回纳秒 返回int

time.process_time()
time.process_time_ns()
# 返回进程执行时间

time.sleep(secs)
# 暂停程序执行

time.strftime(format[, t])
# 转换tuple 或 struct_time对象为指定格式的字符串
# %a星期简称 %A星期全称 %b月份简称 %B月份全称
# %c本地时间日期格式
# %d数字日期 01-31 %H小时数 00-23 %I小时数 01-12
# %j天数 001-366 %m月份数 01-12 %M分钟数 00-59
# %p 上下午 AM-PM %S秒数 00-61 %U周数(从周日开始) 00-53
# %w星期数 0(周日)-6 %W周数(从周一开始) 00-53
# %x本地日期格式 %X本地时间格式 
# %y不加世纪的年数 00-99 %Y加世纪的年数
# %z时区偏移 [-23:59, +23:59] %Z时区名
# %%显示%字符
# strftime("%a, %d %b %Y %H:%M:%S +0000", gmtime())

time.strptime(string[, format])
# 将字符串转换为struct_time对象
# time.strptime("30 Nov 00", "%d %b %y")

time.time()
time.time_ns()
# 返回时间戳浮点数 单位 秒

time.thread_time()
time.thread_time_ns()
# 返回线程运行时间 float和int


```
### 模块类
```python
class time.struct_time
	tm_year
    # 年份
    tm_mon
    # 月份[1, 12]
    tm_mday
    # 日期[1, 31]
    tm_hour
    # 小时[0, 23]
    tm_min
    # 分钟[0, 59]
    tm_sec
    # 秒[0, 61] 61为闰秒 不常见
    tm_wday
    # 星期数[0, 6], Monday is 0
    tm_yday
    # 日期数[1, 366]
    tm_isdst
    # 夏令时0, 1 or -1
    tm_zone
    # 时区名
    tm_gmtoff
    # 时区偏移秒数 东八区输出28800
```
### 模块常量
[Clock ID Constants](https://docs.python.org/3/library/time.html#clock-id-constants)
[Timezone Constants](https://docs.python.org/3/library/time.html#timezone-constants)

## datetime
面向对象的日期和时间接口，具有与time模块类似的功能
用于操作、计算 日期和时间

### 模块类
```python
class datetime.date
# 表示日期

class datetime.time
# 表示时间

class datetime.datetime
# 表示日期和时间

class datetime.timedelta
# 表示时间差

class datetime.tzinfo
# 表示时区信息

class datetime.timezone
# 表示时区偏移

```

继承关系
- object
    - timedelta
    - zinfo
        - timezone
    - time
    - date
        - datetime

### timedelta
```python
class datetime.timedelta(days=0, seconds=0, microseconds=0,
 milliseconds=0, minutes=0, hours=0, weeks=0)
# 所有参数默认为0 可设为int和float 正数和负数
# 只有days, seconds 和 microseconds为内部单位 其他选项会转化为上述单位
# 内部单位范围 天 +-999999999 秒0-3600*24(1天) 微秒0-1000000(1秒)

# 类属性
timedelta.min   # 最小时间差负值timedelta(-999999999)
timedelta.max   # 最大时间差正值timedelta(days=99....99)
timedelta.resolution    # 时间差精度timedelta(microseconds=1)

# 类方法
# 加 减 乘(常数) 除(常数) 取模 
# +t1返回时价差  -t1返回时间差负数 abs(t) str(t) repr(t)

# 实例方法
timedelta.total_seconds()

```
### date
```python
class datetime.date(year, month, day)

# 类方法
date.today()    #返回本地时间
date.fromtimestamp(timestamp)   #返回时间戳对应的本地时间
date.fromordinal(ordinal)
date.fromisoformat(date_string) #将字符串转化为date对象 仅支持YYYY-MM-DD

# 类属性
date.min    #date(MINYEAR, 1, 1)
date.max    #date(MAXYEAR, 12, 31)
date.resolution #精度timedelta(days=1)
date.year
date.month
date.day

#类操作
date2 = date1 + timedelta
date2 = date1 - timedelta
timedelta = date1 - date2
date1 < date2

# 实例方法
date.replace(year=self.year, month=self.month, day=self.day)
# 修改date对象的某些值
date.timetuple()
# 返回一个time.struct_time实例 如同time.localtime()
date.toordinal()
date.weekday()
#返回星期数 周一为0
date.isoweekday()
#返回星期数 周日为0
date.isocalendar()
# 返回三元组(ISO year, ISO week number, ISO weekday)
date.isoformat()
# 以YYYY-MM-DD格式返回时间字符串
date.__str__()
# 用于str(d) 等于date.isoformat()
date.ctime()
# 以'Wed Dec 4 00:00:00 2002'格式返回时间字符串
date.strftime(format)
# 按指定的格式返回时间字符串
date.__format__(format)
# 用于支持str.format() 效果同date.strftime
```
### datetime
包含date对象和time对象
```python
class datetime.datetime(year, month, day, hour=0,
 minute=0, second=0, microsecond=0, tzinfo=None, *, fold=0)

# MINYEAR <= year <= MAXYEAR,
# 1 <= month <= 12,
# 1 <= day <= number of days in the given month and year,
# 0 <= hour < 24,
# 0 <= minute < 60,
# 0 <= second < 60,
# 0 <= microsecond < 1000000,
# fold in [0, 1]

# 类方法
datetime.today()
datetime.now(tz=None)
datetime.utcnow()
datetime.fromtimestamp(timestamp, tz=None)
#将时间戳转化为本地时间
datetime.utcfromtimestamp(timestamp)
#将时间戳转化为标准时间
datetime.fromordinal(ordinal)
datetime.combine(date, time, tzinfo=self.tzinfo)
# 合并date对象和time对象
datetime.fromisoformat(date_string)
# 将字符串转换为datetime对象 仅支持格式YYYY-MM-DD[*HH[:MM[:SS]]]
datetime.strptime(date_string, format)
# 指定格式返回时间字符串

# 类属性
datetime.min
datetime.max
datetime.resolution

#实例属性(只读)
datetime.year
datetime.month
datetime.day
datetime.hour
datetime.minute
datetime.second
datetime.microsecond
datetime.tzinfo
datetime.fold

# 实例方法
datetime.date() #返回日期date对象
datetime.time() #返回时间time对象
datetime.timetz()
datetime.replace(year=self.year, month=self.month, day=self.day,
 hour=self.hour, minute=self.minute, second=self.second, 
 microsecond=self.microsecond, tzinfo=self.tzinfo, * fold=0)
datetime.astimezone(tz=None)
datetime.utcoffset()
datetime.dst()
datetime.timestamp()
datetime.weekday()
datetime.isoweekday()
datetime.isocalendar()
datetime.isoformat(sep='T', timespec='auto')
datetime.__str__()
datetime.ctime()
datetime.strftime(format)
datetime.__format__(format)

# 类操作
# datetime2 = datetime1 + timedelta
# datetime2 = datetime1 - timedelta
# timedelta = datetime1 - datetime2
# datetime1 < datetime2
```
### time 
```python
class datetime.time(hour=0, minute=0, second=0, microsecond=0, 
tzinfo=None, *, fold=0)

# 0 <= hour < 24,
# 0 <= minute < 60,
# 0 <= second < 60,
# 0 <= microsecond < 1000000,
# fold in [0, 1].

# 类属性
time.min
time.max
time.resolution

#实例属性(只读)
time.hour
time.minute
time.second
time.microsecond
time.tzinfo
time.fold

#实例方法
time.replace(hour=self.hour, minute=self.minute, second=self.second,
 microsecond=self.microsecond, tzinfo=self.tzinfo, * fold=0)
time.isoformat(timespec='auto')
time.__str__()
time.strftime(format)
time.__format__(format)
time.utcoffset()
time.dst()
time.tzname()
```

## calendar
创建日历对象遍历日期

### 模块类
```python
class calendar.Calendar(firstweekday=0)
#指定一周的开始 0表示周一开始一周 6表示一周从周日开始

# 实例方法
iterweekdays()  #迭代返回星期数
itermonthdates(year, month)
# 迭代返回指定月份的日期，datetime.date对象
itermonthdays(year, month)
# 返回指定月的日期数字，属于本周但不属于本月的日期表示为0
itermonthdays2(year, month)
# 迭代返回二元组(日期数字，星期数字)
itermonthdays3(year, month)
# 迭代返回三元组(年份,月份,日期)
itermonthdays4(year, month)
# 加星期数
monthdatescalendar(year, month)
# 返回指定月份日历[datetime.date]
monthdays2calendar(year, month)
# 返回指定月日历 [(日期,星期)][(日期,星期)][(日期,星期)][(日期,星期)]
monthdayscalendar(year, month)
# [0,0...3][4,5,6...10][11..17][18..24][25..31]
yeardatescalendar(year, width=3)
yeardays2calendar(year, width=3)
yeardayscalendar(year, width=3)
# 返回一年的日历
```
```python
class calendar.TextCalendar(firstweekday=0)
#生成纯文本日历

#实例方法
formatmonth(theyear, themonth, w=0, l=0)
#返回月份格式字符串
prmonth(theyear, themonth, w=0, l=0)
#打印月份字符串
formatyear(theyear, w=2, l=1, c=6, m=3)
pryear(theyear, w=2, l=1, c=6, m=3)

```
```python
calendar.HTMLCalendar(firstweekday=0)
#生成HTML 日历
formatmonth(theyear, themonth, withyear=True)
formatyear(theyear, width=3)
formatyearpage(theyear, width=3, css='calendar.css', encoding=None)

#用于修改css样式的可重写的属性
cssclasses
# 用于修改星期样式
# cssclasses = ["mon", "tue", "wed", "thu", "fri", "sat", "sun"]
# cssclasses = ["mon text-bold", "tue", "wed", "thu", "fri", "sat", "sun red"]
cssclass_noday
cssclasses_weekday_head
cssclass_month_head
cssclass_month
cssclass_year
cssclass_year_head
```
```python
class calendar.LocaleTextCalendar(firstweekday=0, locale=None)
class calendar.LocaleHTMLCalendar(firstweekday=0, locale=None)
#月份名称和星期名称的本地语言显示
```
### 模块函数
```python
calendar.setfirstweekday(weekday)
calendar.firstweekday()
calendar.isleap(year)#如果year是闰年 返回True
calendar.leapdays(y1, y2)#返回y1到y2年之间的闰年
calendar.weekday(year, month, day)#返回指定年月日当天的星期数
calendar.weekheader(n)#返回星期名称
calendar.monthrange(year, month)#返回指定月份第一天的星期数和日期
calendar.monthcalendar(year, month)#返回指定月份的日历[[第1周][第2周]..]
calendar.prmonth(theyear, themonth, w=0, l=0)#打印月份日历
calendar.month(theyear, themonth, w=0, l=0)#返回月历字符串
calendar.prcal(year, w=0, l=0, c=6, m=3)#打印年历
calendar.calendar(year, w=2, l=1, c=6, m=3)#返回年历字符串
calendar.timegm(tuple)#将元组转化为时间戳
```
### 模块属性
```python
calendar.day_name
calendar.day_abbr
calendar.month_name
calendar.month_abbr
```
