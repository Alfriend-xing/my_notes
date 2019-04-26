## 日历
- java.util.Calendar 类可以设置和获取日期数据的特定部分，在日期的这些部分加上或者减去值
- Calendar 类实现了公历日历
- GregorianCalendar 是 Calendar 类的一个具体实现
- Calendar 的 getInstance（）方法返回一个默认用当前的语言环境和时区初始化的 GregorianCalendar 对象
```java
Calendar c = Calendar.getInstance();//默认是当前日期

//创建一个代表2009年6月12日的Calendar对象
// 月份从0开始，所以用6 - 1
Calendar c1 = Calendar.getInstance();
c1.set(2009, 6 - 1, 12);
// 只设定某个字段
c1.set(Calendar.DATE,10);

// 增减某个字段
Calendar c1 = Calendar.getInstance();
c1.add(Calendar.DATE, 10);
c1.add(Calendar.DATE, -10);

// 获取某个字段
// 获得年份
int year = c1.get(Calendar.YEAR);
// 获得月份(注意+1)
int month = c1.get(Calendar.MONTH) + 1;
```
常量|描述
--|--
Calendar.YEAR|年份
Calendar.MONTH|月份
Calendar.DATE|日期
Calendar.DAY_OF_MONTH|日期，和上面的字段意义完全相同
Calendar.HOUR|12小时制的小时
Calendar.HOUR_OF_DAY|24小时制的小时
Calendar.MINUTE|分钟
Calendar.SECOND|秒
Calendar.DAY_OF_WEEK|星期几
