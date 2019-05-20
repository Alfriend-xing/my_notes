## StringBuffer 和 StringBuilder 类
- 和 String 类不同的是，StringBuffer 和 StringBuilder 类的对象能够被多次的修改，并且不产生新的未使用对象
- StringBuilder 和 StringBuffer 之间的最大不同在于 StringBuilder 的方法不是线程安全的 ( 不能同步访问 )
- 由于 StringBuilder 相较于 StringBuffer 有速度优势，所以多数情况下建议使用 StringBuilder 类
- 然而在应用程序要求线程安全的情况下，则必须使用 StringBuffer 类
```java
StringBuffer sBuffer = new StringBuffer("简单编程官网：");
sBuffer.append("www");
sBuffer.append(".twle");
sBuffer.append(".com");
System.out.println(sBuffer); 
```
StringBuffer 方法

方法|描述
--|--
public StringBuffer append(String s)|将指定的字符串追加到此字符序列
public StringBuffer reverse()|将此字符序列用其反转形式取代
public delete(int start, int end)|移除此序列的子字符串中的字符
public insert(int offset, int i)|将int参数的字符串表示形式插入此序列中
replace(int start, int end, String str)|使用给定String中的字符替换此序列的子字符串中的字符
其余方法则和 String 类类似
