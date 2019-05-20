### 格式化日期
java.util.SimpleDateFormat 类是一个以语言环境敏感的方式来格式化和分析日期的类
```java
Date dNow = new Date( );
SimpleDateFormat ft = new SimpleDateFormat ("E yyyy.MM.dd 'at' hh:mm:ss a zzz");
System.out.println(ft.format(dNow));
// 输出
// Tue 2018.01.23 at 07:48:53 AM CST
```
格式化符

字母|描述|范例
--|--|--
G|纪元标记|AD
y|四位年份|2001
M|月份|July or 07
d|一个月的日期|10
h|A.M./P.M. (1~12)格式小时|12
H|一天中的小时 (0~23)|22
m|分钟数|30
s|秒数|55
S|毫秒数|234
E|星期几|Tuesday
D|一年中的日子|360
F|一个月中第几周的周几|2 (second Wed. in July)
w|一年中第几周|40
W|一个月中第几周|1
a|A.M./P.M. 标记|PM
k|一天中的小时(1~24)|24
K|A.M./P.M. (0~11)格式小时|10
z|时区|Eastern Standard Time
'|文字定界符|Delimiter
"|单引号|`

### 解析字符串为时间
SimpleDateFormat 类的 parse() 方法可以按照指定的 SimpleDateFormat 对象的格式化存储来解析字符串
```java
SimpleDateFormat ft = new SimpleDateFormat ("yyyy-MM-dd"); 
String input = args.length == 0 ? "1818-11-11" : args[0]; 
System.out.print(input + " Parses as "); 
Date t; 
try { 
    t = ft.parse(input); 
    System.out.println(t); 
} catch (ParseException e) { 
    System.out.println("Unparseable using " + ft); 
}
```