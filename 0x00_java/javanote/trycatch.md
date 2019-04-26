## 异常处理
### 捕获
```java
try {
    // 程序代码
}catch(ExceptionName e1) {
    // Catch 块
}

try{

    int a[] = new int[2];
    System.out.println("Access element three :" + a[3]);
} catch(ArrayIndexOutOfBoundsException e) {
    System.out.println("Exception thrown  :" + e);
}

try{
   // 程序代码
}catch(异常类型1 异常的变量名1){
  // 程序代码
}catch(异常类型2 异常的变量名2){
  // 程序代码
}catch(异常类型2 异常的变量名2){
  // 程序代码
}

try
{
    file = new FileInputStream(fileName);
    x = (byte) file.read();
}catch(IOException i) {

    i.printStackTrace();
    return -1;
}catch(FileNotFoundException f) {

    f.printStackTrace();
    return -1;
}
```
### 抛出
```java
public void deposit(double amount) throws RemoteException
{

    // 方法实现
    throw new RemoteException();
}

// 一个方法可以声明抛出多个异常，多个异常之间用逗号隔开
public void withdraw(double amount) throws RemoteException,
                            InsufficientFundsException
{
    // 方法语句块
}

```
### finally 语句
无论是否发生异常，finally 代码块中的代码总会被执行
```java
try{
  // 程序代码
}catch(异常类型1 异常的变量名1){
  // 程序代码
}catch(异常类型2 异常的变量名2){
  // 程序代码
}finally{
  // 程序代码
}
```
- catch 不能独立于 try 存在
- 在 try/catch 后面添加 finally 块并非强制性要求的
- try 代码后不能既没 catch 块也没 finally 块
- try, catch, finally 块之间不能添加任何代码
