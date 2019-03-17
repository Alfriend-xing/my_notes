# vps-ss
    使用vps搭建ss服务器
---


- 安装pip
```shell
yum -y install epel-release #安装epel扩展源
yum -y install python-pip

或者

install -y python-setuptools
easy_install pip
```
- 安装ss
```shell
pip install shadowsocks
```
- 配置文件
```shell
vi /etc/shadowsocks.json
# 写入以下内容
```
```json
{
    "server":"your_server_ip",
    "server_port":8989,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"yourpassword",
    "timeout":600,
    "method":"aes-256-cfb",
    "fast_open": false,
    "workers": 1
}
```
- 降低延迟(可选)
```shell
开启TFO,Linux的内核要求在Linux 3.7以上
echo 3 > /proc/sys/net/ipv4/tcp_fastopen
在/etc/sysctl.conf中添加下面的 net.ipv4.tcp_fastopen = 3 
然后需要在shadowsocks的config.json配置文件中置fast_open为"true"
```
- 降低丢包提速(可选)
>高峰期时国际出口网关会限制低优先级数据包，使用bbr算法会一包多发，降低丢包率提高网速
```shell
配置BBR加速

yum更新系统版本
yum update

查看系统版本
cat /etc/redhat-release 
> CentOS Linux release 7.5.1804 (Core)

安装elrepo并升级内核
rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-2.el7.elrepo.noarch.rpm
yum --enablerepo=elrepo-kernel install kernel-ml -y

更新grub文件并重启系统
uname -r
egrep ^menuentry /etc/grub2.cfg | cut -f 2 -d \'
grub2-set-default 0
reboot

重启完成后查看内核是否已更换为4.18版本
uname -r

开启bbr
vim /etc/sysctl.conf 
# 在文件末尾添加如下内容
net.core.default_qdisc = fq
net.ipv4.tcp_congestion_control = bbr

加载系统参数
sysctl -p

确定bbr已经成功开启
sysctl net.ipv4.tcp_available_congestion_control
lsmod | grep bbr
#输出tcp_bbr表示成功开启
```
- 开放端口
>在防火墙打开服务端口，否则无法连接
```shell
#centos7,下面的80换为配置文件中设置的端口

添加
firewall-cmd --zone=public --add-port=80/tcp --permanent    
# --permanent永久生效，没有此参数重启后失效

重新载入
firewall-cmd --reload

查看
firewall-cmd --zone= public --query-port=80/tcp

删除
firewall-cmd --zone= public --remove-port=80/tcp --permanent
```

- 启动服务
```shell
前台运行
ssserver -c /etc/shadowsocks.json

后台运行
ssserver -c /etc/shadowsocks.json -d start
#-d stop -d restart

日志位置
/var/log/shadowsocks.log
```
- 异常处理
>实际使用中，有可能出现无法连接的情况，以下为处理经验
> - 检查ssh是否可以连接
> - 是否可以ping通 
> - 以上两点正常的话，修改ss端口即可解决
> - 以上两点无法正常使用，可能是IP被ban，需要更换vps的IP地址

---
>配置好后可将系统做成镜像方便随时部署，也可将上述步骤写为脚本一步执行

