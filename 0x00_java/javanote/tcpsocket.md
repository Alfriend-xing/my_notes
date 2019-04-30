## TCP Socket
java.net.Socket 类代表一个套接字，并且 java.net.ServerSocket类为服务器程序提供了一种来监听客户端，并与他们建立连接的机制

客户端与服务器端 Socket 通讯的过程一般如下
- 服务器实例化一个 ServerSocket 对象，表示通过服务器上的端口通信

- 服务器调用 ServerSocket 类的 accept() 方法，该方法将一直等待，直到客户端连接到服务器上给定的端口

- 服务器正在等待时，一个客户端实例化一个 Socket 对象，指定服务器名称和端口号来请求连接

- Socket 类的构造函数试图将客户端连接到指定的服务器和端口号。如果通信被建立，则在客户端创建一个 Socket 对象能够与服务器进行通信

- 在服务器端，accept() 方法返回服务器上一个新的 socket 引用，该 socket 连接到客户端的 socket

- 连接建立后，通过使用 I/O 流在进行通信，每一个socket都有一个输出流和一个输入流，客户端的输出流连接到服务器端的输入流，而客户端的输入流连接到服务器端的输出流

>TCP 是一个双向的通信协议，因此数据可以通过两个数据流在同一时间发送

下面这些类提供的一套完整的有用的方法来实现 Socket

### ServerSocket 类
服务器应用程序通过使用 java.net.ServerSocket 类以获取一个端口,并且侦听客户端请求
```java
// 构造方法,用于创建非绑定服务器套接字

public ServerSocket(int port) throws IOException
// 创建绑定到特定端口的服务器套接字

public ServerSocket(int port, int backlog) throws IOException
// 利用指定的 backlog 创建服务器套接字并将其绑定到指定的本地端口号

public ServerSocket(int port, int backlog, InetAddress address) throws IOException
// 使用指定的端口、侦听 backlog 和要绑定到的本地 IP 地址创建服务器

public ServerSocket() throws IOException
// 创建非绑定服务器套接字
```
```java
// 常用方法

public int getLocalPort()
// 返回此套接字在其上侦听的端口

public Socket accept() throws IOException
// 侦听并接受到此套接字的连接

public void setSoTimeout(int timeout)
// 通过指定超时值启用/禁用 SO_TIMEOUT，以毫秒为单位

public void bind(SocketAddress host, int backlog)
// 将 ServerSocket 绑定到特定地址 ( IP 地址和端口号 )
```
### Socket 类
java.net.Socket 类代表客户端和服务器都用来互相沟通的套接字
客户端要获取一个 Socket 对象通过实例化 ，而服务器获得一个 Socket 对象则通过 accept() 方法的返回值
```java
// 构造方法

public Socket(String host, int port) throws UnknownHostException, IOException
// 创建一个流套接字并将其连接到指定主机上的指定端口号

public Socket(InetAddress host, int port) throws IOException
// 创建一个流套接字并将其连接到指定 IP 地址的指定端口号

public Socket(String host, int port, InetAddress localAddress, int localPort) throws IOException
// 创建一个套接字并将其连接到指定远程主机上的指定远程端口

public Socket(InetAddress host, int port, InetAddress localAddress, int localPort) throws IOException
// 创建一个套接字并将其连接到指定远程地址上的指定远程端口

public Socket()
// 通过系统默认类型的 SocketImpl 创建未连接套接字
```
```java
// 类方法

public void connect(SocketAddress host, int timeout) throws IOException
// 将此套接字连接到服务器，并指定一个超时值

public InetAddress getInetAddress()
// 返回套接字连接的地址

public int getPort()
// 返回此套接字连接到的远程端口

public int getLocalPort()
// 返回此套接字绑定到的本地端口

public SocketAddress getRemoteSocketAddress()
// 返回此套接字连接的端点的地址，如果未连接则返回 null

public InputStream getInputStream() throws IOException
// 返回此套接字的输入流

public OutputStream getOutputStream() throws IOException
// 返回此套接字的输出流

public void close() throws IOException
// 关闭此套接字
```
### InetAddress 类
InetAddress 类表示互联网协议 (IP) 地址
```java
// 常用方法

static InetAddress getByAddress(byte[] addr)
// 在给定原始 IP 地址的情况下，返回 InetAddress 对象

static InetAddress getByAddress(String host, byte[] addr)
// 根据提供的主机名和 IP 地址创建 InetAddress

static InetAddress getByName(String host)
// 在给定主机名的情况下确定主机的 IP 地址

String getHostAddress()
// 返回 IP 地址字符串（以文本表现形式）

String getHostName()
// 获取此 IP 地址的主机名

static InetAddress getLocalHost()
// 返回本地主机

String toString()
// 将此 IP 地址转换为 String
```

### 实例
```java
// 客户端

import java.net.*;
import java.io.*;

public class GreetingClient
{
   public static void main(String [] args)
   {
      String serverName = args[0];
      int port = Integer.parseInt(args[1]);
      try
      {
         System.out.println("连接到主机：" + serverName + " ，端口号：" + port);
         Socket client = new Socket(serverName, port);
         System.out.println("远程主机地址：" + client.getRemoteSocketAddress());
         OutputStream outToServer = client.getOutputStream();
         DataOutputStream out = new DataOutputStream(outToServer);

         out.writeUTF("Hello from " + client.getLocalSocketAddress());
         InputStream inFromServer = client.getInputStream();
         DataInputStream in = new DataInputStream(inFromServer);
         System.out.println("服务器响应： " + in.readUTF());
         client.close();
      }catch(IOException e)
      {
         e.printStackTrace();
      }
   }
}
```

```java
// 服务端

import java.net.*;
import java.io.*;

public class GreetingServer extends Thread
{
   private ServerSocket serverSocket;

   public GreetingServer(int port) throws IOException
   {
      serverSocket = new ServerSocket(port);
      serverSocket.setSoTimeout(10000);
   }

   public void run()
   {
      while(true)
      {
         try
         {
            System.out.println("等待远程连接，端口号为：" + serverSocket.getLocalPort() + "...");
            Socket server = serverSocket.accept();
            System.out.println("远程主机地址：" + server.getRemoteSocketAddress());
            DataInputStream in = new DataInputStream(server.getInputStream());
            System.out.println(in.readUTF());
            DataOutputStream out = new DataOutputStream(server.getOutputStream());
            out.writeUTF("谢谢连接我：" + server.getLocalSocketAddress() + "\nGoodbye!");
            server.close();
         }catch(SocketTimeoutException s)
         {
            System.out.println("Socket timed out!");
            break;
         }catch(IOException e)
         {
            e.printStackTrace();
            break;
         }
      }
   }
   public static void main(String [] args)
   {
      int port = Integer.parseInt(args[0]);
      try
      {
         Thread t = new GreetingServer(port);
         t.run();
      }catch(IOException e)
      {
         e.printStackTrace();
      }
   }
}
```

### DataOutputStream和DataInputStream
按照一定格式将输入输出，再按照一定格式将数据输入














