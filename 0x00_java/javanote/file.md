## 读写文件

### 读文件
```java
// 使用字符串类型的文件名来创建一个输入流对象来读取文件
InputStream f = new FileInputStream("C:/java/hello");
```
方法及描述
```java
public void close() throws IOException{}
// 关闭此文件输入流并释放与此流有关的所有系统资源。抛出IOException异常

protected void finalize()throws IOException {}
// 这个方法清除与该文件的连接。确保在不再引用文件输入流时调用其 close 方法。抛出 IOException 异常

public int read(int r)throws IOException{}
// 这个方法从 InputStream 对象读取指定字节的数据。返回为整数值。
// 返回下一字节数据，如果已经到结尾则返回 -1

public int read(byte[] r) throws IOException{}
// 这个方法从输入流读取r.length长度的字节。返回读取的字节数。如果是文件结尾则返回 -1

public int available() throws IOException{}
// 返回下一次对此输入流调用的方法可以不受阻塞地从此输入流读取的字节数。返回一个整数值
```

### 写文件

```java
// 使用字符串类型的文件名来创建一个输出流对象
OutputStream f = new FileOutputStream("/tmp/hello.txt")
```
```java
public void close() throws IOException{}
// 关闭此文件输入流并释放与此流有关的所有系统资源。抛出IOException异常

protected void finalize() throws IOException {}
// 清除与该文件的连接。确保在不再引用文件输入流时调用其 close 方法。抛出IOException异常。

public void write(int w) throws IOException{}
// 把指定的字节写到输出流中

public void write(byte[] w)
// 把指定数组中 w.length 长度的字节写到 OutputStream 中
```

### 实例

```java
// 创建文件 test.txt，并把给定的数字以二进制形式写进该文件，同时输出到控制台上
try{
    byte bWrite [] = {11,21,3,40,5};
    OutputStream os = new FileOutputStream("test.txt");
    for(int x=0; x < bWrite.length ; x++){
    os.write( bWrite[x] ); // writes the bytes
}
os.close();

InputStream is = new FileInputStream("test.txt");
int size = is.available();

for(int i=0; i< size; i++){
    System.out.print((char)is.read() + "  ");
}
    is.close();
}catch(IOException e){
    System.out.print("Exception");
}  
```
```java
// 解决乱码问题
File f = new File("a.txt");
FileOutputStream fop = new FileOutputStream(f);
// 构建FileOutputStream对象,文件不存在会自动新建

OutputStreamWriter writer = new OutputStreamWriter(fop, "UTF-8");
// 构建OutputStreamWriter对象,参数可以指定编码,默认为操作系统默认编码,windows上是gbk

writer.append("中文输入");
// 写入到缓冲区

writer.append("\r\n");
//换行

writer.append("English");
// 刷新缓存冲,写入到文件,如果下面已经没有写入的内容了,直接close也会写入

writer.close();
//关闭写入流,同时会把缓冲区内容写入文件,所以上面的注释掉

fop.close();
// 关闭输出流,释放系统资源

FileInputStream fip = new FileInputStream(f);
// 构建FileInputStream对象

InputStreamReader reader = new InputStreamReader(fip, "UTF-8");
// 构建InputStreamReader对象,编码与写入相同

StringBuffer sb = new StringBuffer();
while (reader.ready()) {
    sb.append((char) reader.read());
    // 转成char加到StringBuffer对象中
}
System.out.println(sb.toString());
reader.close();
// 关闭读取流

fip.close();
// 关闭输入流,释放系统资源
```