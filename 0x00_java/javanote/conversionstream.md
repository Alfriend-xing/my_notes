## 转换流
字节流与字符流之间的转换，称作转换流

### InputStreamReader
是字符流Reader的子类，是字节流通向字符流的桥梁。可以在构造器重指定编码的方式，如果不指定的话将采用底层操作系统的默认编码方式，例如 GBK 等。要启用从字节到字符的有效转换，可以提前从底层流读取更多的字节，使其超过满足当前读取操作所需的字节。一次只读一个字符。
```java
// InputStreamReader构造函数

InputStreamReader(Inputstream  in) 
//创建一个使用默认字符集的 InputStreamReader。

InputStreamReader(Inputstream  in，Charset cs) 
//创建使用给定字符集的 InputStreamReader。

InputStreamReader(InputStream in, CharsetDecoder dec) 
//创建使用给定字符集解码器的 InputStreamReader。

InputStreamReader(InputStream in, String charsetName)  
//创建使用指定字符集的 InputStreamReader。

```
```java
// 一般方法

void  close() 
// 关闭该流并释放与之关联的所有资源。

String  getEncoding() 
//返回此流使用的字符编码的名称。

int  read()  
//读取单个字符。

int  read(char[] cbuf, int offset, int length) 
//将字符读入数组中的某一部分。

boolean  ready() 
//判断此流是否已经准备好用于读取。

```

### OutputStreamWriter
是字符流Writer的子类，是字符流通向字节流的桥梁。每次调用 write()方法都会导致在给定字符（或字符集）上调用编码转换器。在写入底层输出流之前，得到的这些字节将在缓冲区中累积。一次只写一个字符。

```java
// OutputStreamWriter构造函数

OutputStreamWriter(OutputStream out) 
//创建使用默认字符编码的 OutputStreamWriter

OutputStreamWriter(OutputStream out, String charsetName) 
//创建使用指定字符集的 OutputStreamWriter。

OutputStreamWriter(OutputStream out, Charset cs) 
//创建使用给定字符集的 OutputStreamWriter。

OutputStreamWriter(OutputStream out, CharsetEncoder enc) 
//创建使用给定字符集编码器的 OutputStreamWriter。
```

```java
// 一般方法

void  write(int c)   
//写入的字符长度

void  write(char cbuf[])  
//写入的字符数组

void  write(String str)  
//写入的字符串

void  write(String str, int off, int len)  
//应该写入的字符串，开始写入的索引位置，写入的长度

void  close() 
//关闭该流并释放与之关联的所有资源。
```

InputStreamReader、OutputStreamWriter实现从字节流到字符流之间的转换，使得流的处理效率得到提升，但是如果我们想要达到最大的效率，我们应该考虑使用缓冲字符流包装转换流的思路来解决问题。比如：
```java
BufferedReader in = new BufferedReader(new InputStreamReader(System.in));
```


### 实例
```java
public class A5 {

    public static void main(String[] args) {
        String filePath = "F:/123.txt" ;
        String filePath2 = "F:/abc.txt" ;
        File file = new File( filePath ) ;
        File file2 = new File( filePath2 ) ;
        copyFile( file , file2 );

    }

    private static void copyFile( File oldFile , File newFile ){
        InputStream inputStream = null ;
        InputStreamReader inputStreamReader = null ;

        OutputStream outputStream = null ;
        OutputStreamWriter outputStreamWriter = null ;

        try {
            inputStream = new FileInputStream( oldFile ) ; 
            //创建输入流
            inputStreamReader = new InputStreamReader( inputStream ) ; 
            //创建转换输入流

            outputStream = new FileOutputStream( newFile ) ; 
            //创建输出流
            outputStreamWriter = new OutputStreamWriter( outputStream ) ; 
            //创建转换输出流

            int result = 0 ;

            while( (result = inputStreamReader.read()) != -1){  
                //一次只读一个字符
                outputStreamWriter.write( result ); 
                //一次只写一个字符
            }

            outputStreamWriter.flush();  
            //强制把缓冲写入文件

        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }catch (IOException e) {
            e.printStackTrace();
        }finally{

            if ( outputStreamWriter != null) {
                try {
                    outputStreamWriter.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }

            if ( inputStreamReader != null ) {
                try {
                    inputStreamReader.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }

    }
}
```
## 字节缓冲区流(高效流)
- 输入流对象包含了缓冲区，可以一次性读取多个字节到缓冲区中，这个在读层面不用一个字节一个字节地进行内存拷贝操作，就提高了读的效率；
- 读操作通常伴随着写操作，输出流对象包含了缓冲区，同样地不用每次都一个字节一个字节的写，可以读缓冲区，一次性操作多个字节，这样提高了写的效率。
```java
import java.io.BufferedInputStream;
import java.io.BufferedOutputStream;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
 
public class Main {
 
	public static void main(String[] args) throws IOException {
		long start = System.currentTimeMillis();
		method1("D:\\666\\555.txt", "D:\\666\\444.txt");
		method2("D:\\666\\555.txt", "D:\\666\\444.txt");
		method3("D:\\666\\555.txt", "D:\\666\\444.txt");
		method4("D:\\666\\555.txt", "D:\\666\\444.txt");
		long end = System.currentTimeMillis();
		System.out.println("共耗时：" + (end - start) + "毫秒");
	}
 
	// 高效字节流一次读写一个字节数组：
	public static void method4(String srcString, String destString) throws IOException {
		BufferedInputStream bis = new BufferedInputStream(new FileInputStream(srcString));
		BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream(destString));
 
		byte[] bys = new byte[1024];
		int len = 0;
		while ((len = bis.read(bys)) != -1) {
			bos.write(bys, 0, len);
		}
 
		bos.close();
		bis.close();
	}
 
	// 高效字节流一次读写一个字节：
	public static void method3(String srcString, String destString) throws IOException {
		BufferedInputStream bis = new BufferedInputStream(new FileInputStream(srcString));
		BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream(destString));
 
		int by = 0;
		while ((by = bis.read()) != -1) {
			bos.write(by);
 
		}
 
		bos.close();
		bis.close();
	}
 
	// 基本字节流一次读写一个字节数组
	public static void method2(String srcString, String destString) throws IOException {
		FileInputStream fis = new FileInputStream(srcString);
		FileOutputStream fos = new FileOutputStream(destString);
 
		byte[] bys = new byte[1024];
		int len = 0;
		while ((len = fis.read(bys)) != -1) {
			fos.write(bys, 0, len);
		}
 
		fos.close();
		fis.close();
	}
 
	// 基本字节流一次读写一个字节
	public static void method1(String srcString, String destString) throws IOException {
		FileInputStream fis = new FileInputStream(srcString);
		FileOutputStream fos = new FileOutputStream(destString);
 
		int by = 0;
		while ((by = fis.read()) != -1) {
			fos.write(by);
		}
 
		fos.close();
		fis.close();
	}
 
}
```
[参考链接](https://blog.csdn.net/zhaoyanjun6/article/details/54923506)
[参考链接](https://blog.csdn.net/ITrunnerboy/article/details/70245640)
[参考链接](https://blog.csdn.net/ITrunnerboy/article/details/70245640)