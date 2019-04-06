# centos网络配置

## 使用DHCP服务获取ip
```
# dhclient
```
## 配置静态IP
```shell
vi /etc/sysconfig/network-scripts/ifcfg-ens33 

修改
BOOTPROTO=static
ONBOOT=yes

添加
IPADDR=192.168.127.128 
NETMASK=255.255.255.0 
GATEWAY=192.168.127.2 
DNS1=8.8.8.8

重启服务
systemctl restart network.service
```