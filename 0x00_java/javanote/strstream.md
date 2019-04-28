
## 流 ( Stream )
输入流表示从一个源读取数据，输出流表示向一个目标写数据
`Java.io` 包几乎包含了所有操作输入、输出需要的类
```java
// 读取控制台输入
BufferedReader br = new BufferedReader(new 
                      InputStreamReader(System.in));
// BufferedReader 对象创建后，我们就可以使用 read() 方法从控制台读取一个字符，
// 或者用 readLine() 方法读取一个字符串

// 从控制台读取多字符输入
// 使用 read() 方法从控制台不断读取字符直到用户输入 "q"
public static void main(String args[]) throws IOException {
    char c;
    // 使用 System.in 创建 BufferedReader 
    BufferedReader br = new BufferedReader(new 
                    InputStreamReader(System.in));
    System.out.println("输入字符, 按下 'q' 键退出。");
    // 读取字符
    do {
        c = (char) br.read();
        System.out.println(c);
    } while(c != 'q');
}

// 从控制台读取一行字符串
BufferedReader br = new BufferedReader(new
                            InputStreamReader(System.in));
do {
    str = br.readLine();
    System.out.println(str);
} while(!str.equals("end"));

// 控制台输出
System.out.write('word');
// write() 方法不经常使用，因为 print() 和 println() 方法用起来更为方便
```
## Scanner 类
java.util.Scanner 可以用来获取用户的输入
可以使用 next() 与 nextLine() 方法获取输入的字符串
还可以使用 hasNext() 与 hasNextLine() 两个方法判断是否还有输入的数据
```java
Scanner scan = new Scanner(System.in);

// 从键盘接收数据
// next方式接收字符串
System.out.println("next方式接收：");

// 判断是否还有输入
if (scan.hasNext()) {
    String str1 = scan.next();
    System.out.println("输入的数据为：" + str1);
}
scan.close();
```