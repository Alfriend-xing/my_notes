# socket
用于实现底层网络连接

## 模块异常类
```python
exception socket.error
# 相当于OSError，不推荐使用

exception socket.herror
# OSError的子类，表示host主机地址相关的错误

exception socket.gaierror
# OSError的子类，表示host主机地址相关的错误

exception socket.timeout
# 超时
```

## 模块常量
```python
# 表示socket地址和连接协议族，用于socket()第一个参数
socket.AF_UNIX
socket.AF_INET
socket.AF_INET6

# 表示连接类型，用于socket()第二个参数
socket.SOCK_STREAM
socket.SOCK_DGRAM
socket.SOCK_RAW
socket.SOCK_RDM
socket.SOCK_SEQPACKET

# 以原子方式设置标志位
socket.SOCK_CLOEXEC
socket.SOCK_NONBLOCK
```

## 模块函数
```python
# 下面5个方法用于创建socket对象

socket.socket(family=AF_INET, type=SOCK_STREAM, proto=0, fileno=None)
# 使用指定的协议族创建socket 对象
# family [AF_INET, AF_INET6, AF_UNIX, AF_CAN, AF_PACKET, or AF_RDS]
# type [SOCK_STREAM (the default), SOCK_DGRAM, SOCK_RAW, SOCK_*]
# proto通常是0

socket.socketpair([family[, type[, proto]]])
# 返回一对已经连接的socket 对象

socket.create_connection(address[, timeout[, source_address]])
# 返回一个连接到tcp服务的socket 对象，可以直接指定ipv4或ipv6地址

socket.fromfd(fd, family, type, proto=0)
# 从socket文件中读取信息创建socket对象

socket.fromshare(data)
# 从socket.share()获取信息创建socket对象

#------------------------------------------------------------------
#其他函数
socket.close(fd)
# 关闭一个socket文件

socket.getaddrinfo(host, port, family=0, type=0, proto=0, flags=0)
# 解析指定IP端口所使用的连接协议组，连接类型等，
# 返回五元组(family, type, proto, canonname, sockaddr)

socket.getfqdn([name])
# 指定域名 返回合格的域名

socket.gethostbyname(hostname)
# 指定域名 返回其IP地址

socket.gethostbyname_ex(hostname)
# 将域名转换为ipv4地址格式 返回(hostname, aliaslist, ipaddrlist)
# aliaslist表示相同IP的其他主机名，通常为空

socket.gethostname()
# 返回本机的主机名

socket.gethostbyaddr(ip_address)
# 返回(hostname, aliaslist, ipaddrlist)

socket.getnameinfo(sockaddr, flags)
# 返回(host, port)

socket.getprotobyname(protocolname)
# 输入协议名称，返回对应的socket常量，用于socket()的第三个参数
# 通常protocol 指定为0时会自动选择

socket.getservbyname(servicename[, protocolname])
# 将网络服务名转为端口号http->80

socket.getservbyport(port[, protocolname])
# 将端口转为服务名80->http

socket.ntohl(x)
# 将32位正整数转为主机字节序，如果主机和网络的字节序相同则无操作

socket.ntohs(x)
# 16位

socket.htonl(x)
# 将32位正整数从主机字节序转为网络字节序

socket.htons(x)
# 16位

socket.inet_aton(ip_string)
# 将点分格式的IP地址转为32位字节格式

socket.inet_ntoa(packed_ip)
# 将32位字节格式转为点分IP地址

socket.inet_pton(address_family, ip_string)
socket.inet_ntop(address_family, packed_ip)
#用于转换ipv6或ipv4地址

socket.getdefaulttimeout()
# 返回socket对象的默认超时时间，引入后直接调用返回None

socket.setdefaulttimeout(timeout)
# 设置新建socket对象的默认超时时间，默认None

socket.sethostname(name)
# 设置主机名


```
## socket对象
支持上下文管理器，离开管理器时相当于调用close()
```python
# 对象方法
socket.accept()
# 用于接收一个连接，需要提前绑定地址和监听连接请求
# 返回(conn, address)，conn是一个新的socket对象用于收发,add是对方的地址

socket.bind(address)
# 绑定地址，只能调用一次

socket.close()
# 关闭连接，之后该对象无法执行其他操作

socket.connect(address)
# 连接到一个地址

socket.connect_ex(address)
# 同上，但返回错误代码，正确连接返回0

socket.detach()
# 关闭socket连接但保留底层文件，返回文件描述(用于其他用途)

socket.dup()
# 复制socket对象

socket.fileno()
# 返回socket的文件描述(实用于select.select())

socket.get_inheritable()
# 返回True表示对象可被继承

socket.getpeername()
# 返回远程连接地址

socket.getsockname()
# 返回socket对象占用的地址，常用于获取本地端口号

socket.getsockopt(level, optname[, buflen])

socket.getblocking()
# 返回True表示socket对象为blocking模式

socket.gettimeout()
# 返回超时时间

socket.ioctl(control, option)
# 用于win32平台，具体功能还不明白

socket.listen([backlog])
# 允许socket对象接收连接，backlog表示缓存队列
# 即当前连接未被接收时能保存的最大连接数，超过会拒绝
# 默认0，表示当前连接未accept时后续连接请求全部拒绝

socket.makefile(mode='r', buffering=None, *, 
encoding=None, errors=None, newline=None)
# 返回关于socket的文件对象 windows上无法正常使用

socket.recv(bufsize[, flags])
# 从连接中接收数据，接收的值为字节对象bytes，
# bufsize指定了最大接收量，最好为2的n次方，如4096

socket.recvfrom(bufsize[, flags])
# 返回数据和发送方地址(bytes, address)

socket.recvmsg(bufsize[, ancbufsize[, flags]])
# 接收正常数据和辅助数据，返回(data, ancdata, msg_flags, address)

socket.recvmsg_into(buffers[, ancbufsize[, flags]])
# 将收到的数据写入变量buffers
# 返回(nbytes, ancdata, msg_flags, address)nbytes表示接收数据长度

socket.recvfrom_into(buffer[, nbytes[, flags]])
# 写入变量，返回(nbytes, address)

socket.recv_into(buffer[, nbytes[, flags]])
# 接收nbytes长度的数据 写入变量

socket.send(bytes[, flags])
# 发送字节数据，返回发送bytes 长度

socket.sendall(bytes[, flags])
# 发送所有数据，成功返回None

socket.sendto(bytes, address)
socket.sendto(bytes, flags, address)
# 在未建立连接的情况下发送数据，返回发送字节数

socket.sendmsg(buffers[, ancdata[, flags[, address]]])
# 发送正常数据和辅助数据

socket.sendfile(file, offset=0, count=None)
# 发送文件 返回总字节数

socket.set_inheritable(inheritable)
# 设置可继承标志

socket.setblocking(flag)
# 设置blocking 标志， True表示blocking 模式

socket.settimeout(value)
# blocking模式超时时间

socket.setsockopt(level, optname, value: int)
socket.setsockopt(level, optname, value: buffer)
socket.setsockopt(level, optname, None, optlen: int)
# 设置socket选项

socket.shutdown(how)
# 关闭部分连接，SHUT_RD禁止后续接收动作
# SHUT_WR 禁止后续发送 ，SHUT_RDWR禁止后续收发

socket.share(process_id)
# 复制socket对象，准备

# 只读属性
socket.family
socket.type
socket.proto
```

## 例子
```python
# Echo server program
import socket

HOST = ''                 # Symbolic name meaning all available interfaces
PORT = 50007              # Arbitrary non-privileged port
with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
    s.bind((HOST, PORT))
    s.listen(1)
    conn, addr = s.accept()
    with conn:
        print('Connected by', addr)
        while True:
            data = conn.recv(1024)
            if not data: break
            conn.sendall(data)
```
```python
# Echo client program
import socket

HOST = 'daring.cwi.nl'    # The remote host
PORT = 50007              # The same port as used by the server
with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
    s.connect((HOST, PORT))
    s.sendall(b'Hello, world')
    data = s.recv(1024)
print('Received', repr(data))
```





