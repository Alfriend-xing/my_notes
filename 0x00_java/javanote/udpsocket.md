## UDP Socket

UDP协议提供的服务不同于TCP协议的端到端服务，它是面向非连接的，属不可靠协议，UDP套接字在使用前不需要进行连接。实际上，UDP协议只实现了两个功能：
- 在IP协议的基础上添加了端口；
- 对传输过程中可能产生的数据错误进行了检测，并抛弃已经损坏的数据。


Java通过`DatagramPacket`类和`DatagramSocket`类来使用UDP套接字，客户端和服务器端都通过`DatagramSocket`的`send()`方法和`receive()`方法来发送和接收数据，用`DatagramPacket`来包装需要发送或者接收到的数据。发送信息时，Java创建一个包含待发送信息的`DatagramPacket`实例，并将其作为参数传递给`DatagramSocket`实例的`send()`方法；接收信息时，Java程序首先创建一个`DatagramPacket`实例，该实例预先分配了一些空间，并将接收到的信息存放在该空间中，然后把该实例作为参数传递给`DatagramSocket`实例的`receive()`方法。在创建`DatagramPacket`实例时，要注意：如果该实例用来包装待接收的数据，则不指定数据来源的远程主机和端口，只需指定一个缓存数据的byte数组即可（在调用`receive()`方法接收到数据后，源地址和端口等信息会自动包含在`DatagramPacket`实例中），而如果该实例用来包装待发送的数据，则要指定要发送到的目的主机和端口。

### UDP客户端
- 创建一个DatagramSocket实例，可以有选择地对本地地址和端口号进行设置，如果设置了端口号，则客户端会在该端口号上监听从服务器端发送来的数据；

- 使用DatagramSocket实例的send（）和receive（）方法来发送和接收DatagramPacket实例，进行通信；

- 通信完成后，调用DatagramSocket实例的close（）方法来关闭该套接字。


### UDP服务端

- 创建一个DatagramSocket实例，指定本地端口号，并可以有选择地指定本地地址，此时，服务器已经准备好从任何客户端接收数据报文；

- 使用DatagramSocket实例的receive（）方法接收一个DatagramPacket实例，当receive（）方法返回时，数据报文就包含了客户端的地址，这样就知道了回复信息应该发送到什么地方；

- 使用DatagramSocket实例的send（）方法向服务器端返回DatagramPacket实例。

### 实例

UDP程序在receive()方法处阻塞，直到收到一个数据报文或等待超时。由于UDP协议是不可靠协议，如果数据报在传输过程中发生丢失，那么程序将会一直阻塞在receive()方法处，这样客户端将永远都接收不到服务器端发送回来的数据，但是又没有任何提示。为了避免这个问题，我们在客户端使用DatagramSocket类的setSoTimeout()方法来制定receive()方法的最长阻塞时间，并指定重发数据报的次数，如果每次阻塞都超时，并且重发次数达到了设置的上限，则关闭客户端。

```java
// 客户端
 
import java.io.IOException;
import java.io.InterruptedIOException;
import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetAddress;
 
public class UDPClient {
    private static final int TIMEOUT = 5000;  //设置接收数据的超时时间
    private static final int MAXNUM = 5;      //设置重发数据的最多次数
    public static void main(String args[])throws IOException{
        String str_send = "Hello UDPserver";
        byte[] buf = new byte[1024];
        //客户端在9000端口监听接收到的数据
        DatagramSocket ds = new DatagramSocket(9000);
        InetAddress loc = InetAddress.getLocalHost();
        //定义用来发送数据的DatagramPacket实例
        DatagramPacket dp_send= new DatagramPacket(str_send.getBytes(),str_send.length(),loc,3000);
        //定义用来接收数据的DatagramPacket实例
        DatagramPacket dp_receive = new DatagramPacket(buf, 1024);
        //数据发向本地3000端口
        ds.setSoTimeout(TIMEOUT);              //设置接收数据时阻塞的最长时间
        int tries = 0;                         //重发数据的次数
        boolean receivedResponse = false;     //是否接收到数据的标志位
        //直到接收到数据，或者重发次数达到预定值，则退出循环
        while(!receivedResponse && tries<MAXNUM){
            //发送数据
            ds.send(dp_send);
            try{
                //接收从服务端发送回来的数据
                ds.receive(dp_receive);
                //如果接收到的数据不是来自目标地址，则抛出异常
                if(!dp_receive.getAddress().equals(loc)){
                    throw new IOException("Received packet from an umknown source");
                }
                //如果接收到数据。则将receivedResponse标志位改为true，从而退出循环
                receivedResponse = true;
            }catch(InterruptedIOException e){
                //如果接收数据时阻塞超时，重发并减少一次重发的次数
                tries += 1;
                System.out.println("Time out," + (MAXNUM - tries) + " more tries..." );
            }
        }
        if(receivedResponse){
            //如果收到数据，则打印出来
            System.out.println("client received data from server：");
            String str_receive = new String(dp_receive.getData(),0,dp_receive.getLength()) + 
                    " from " + dp_receive.getAddress().getHostAddress() + ":" + dp_receive.getPort();
            System.out.println(str_receive);
            //由于dp_receive在接收了数据之后，其内部消息长度值会变为实际接收的消息的字节数，
            //所以这里要将dp_receive的内部消息长度重新置为1024
            dp_receive.setLength(1024);   
        }else{
            //如果重发MAXNUM次数据后，仍未获得服务器发送回来的数据，则打印如下信息
            System.out.println("No response -- give up.");
        }
        ds.close();
    }  
} 
```
```java
// 服务端
import java.io.IOException;
import java.net.DatagramPacket;
import java.net.DatagramSocket;
 
public class UDPServer { 
    public static void main(String[] args)throws IOException{
        String str_send = "Hello UDPclient";
        byte[] buf = new byte[1024];
        //服务端在3000端口监听接收到的数据
        DatagramSocket ds = new DatagramSocket(3000);
        //接收从客户端发送过来的数据
        DatagramPacket dp_receive = new DatagramPacket(buf, 1024);
        System.out.println("server is on，waiting for client to send data......");
        boolean f = true;
        while(f){
            //服务器端接收来自客户端的数据
            ds.receive(dp_receive);
            System.out.println("server received data from client：");
            String str_receive = new String(dp_receive.getData(),0,dp_receive.getLength()) + 
                    " from " + dp_receive.getAddress().getHostAddress() + ":" + dp_receive.getPort();
            System.out.println(str_receive);
            //数据发动到客户端的3000端口
            DatagramPacket dp_send= new DatagramPacket(str_send.getBytes(),str_send.length(),dp_receive.getAddress(),9000);
            ds.send(dp_send);
            //由于dp_receive在接收了数据之后，其内部消息长度值会变为实际接收的消息的字节数，
            //所以这里要将dp_receive的内部消息长度重新置为1024
            dp_receive.setLength(1024);
        }
        ds.close();
    }
}
```

[参考链接](https://blog.csdn.net/ns_code/article/details/14128987)