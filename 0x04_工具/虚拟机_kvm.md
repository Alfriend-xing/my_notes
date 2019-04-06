# kvm
>在vmware虚拟宿主机中安装需设置cpu虚拟化的两个选项

## 安装包
```shell
qemu-kvm        #主要的KVM程序包
python-virtinst #创建虚拟机所需要的命令行工具和程序库
virt-top        #虚拟机统计命令s
libvirt         #C语言工具包，提供libvirt服务
libvirt-client  #为虚拟客户机提供的C语言工具包
virt-install    #基于libvirt服务的虚拟机创建命令
bridge-utils    #创建和管理桥接设备的工具
```
```shell
桌面环境需要的包
virt-manager    #GUI虚拟机管理工具
virt-viewer     #GUI连接程序，连接到已配置好的虚拟机
```
```shell
重启查看模块加载
reboot
...
lsmod | grep kvm
```
```shell
安装后开启服务
systemctl start libvirtd
systemctl enable libvirtd
```

## 安装虚拟机
```shell
创建磁盘文件用于保存虚拟机
qemu-img create -f qcow2 c1.qcow2 6G

kvm虚拟机默认使用raw格式的镜像格式，性能最好，速度最快，
它的缺点就是不支持一些新的功能，如支持镜像,派生镜像,zlib磁盘压缩,AES加密等
```

```shell
创建虚拟机
virt-install \
--virt-type=kvm \   #可以不指定[kvm,qemu]
--name=centos1 \   #虚拟机名称
--vcpus=2 \         #cpu个数
--memory=4096 \     #内存
--location=/tmp/CentOS-7-x86_64-Minimal-1511.iso \  #安装镜像
--disk path=/home/vms/centos78.qcow2,size=40,format=qcow2 \ #保存镜像
--network bridge=br0 \  #使用网桥模式，可不指定
--graphics none \   #终端模式安装
--extra-args='console=ttyS0' \  #使用串口登陆系统
--force #防止交互式提示，类似于yum -y

安装选项与实体机类似，包括时区、键盘、分区、root密码、
...等待安装...
```
>串口登陆，无网络连接时，使用物理方法登陆设备，具体操作为使用电缆连接设备串口和另外一台电脑，在另外的电脑上使用仿真软件登陆设备

## 管理虚拟机
```shell
virsh console centos1   #连接虚拟机
virsh start centos1     # 虚拟机开启（启动）：
virsh reboot centos1    # 虚拟机重新启动
virsh shutdown centos1  # 虚拟机关机
virsh destroy centos1   # 强制关机（强制断电）
virsh suspend centos1   # 暂停（挂起）KVM 虚拟机
virsh resume centos1    # 恢复被挂起的 KVM 虚拟机
virsh undefine centos1  # 该方法只删除配置文件，磁盘文件未删除
virsh autostart centos1 # 随物理机启动而启动（开机启动）
virsh autostart --disable centos1 # 取消标记为自动开始（取消开机启动）

virsh list --all           # 查看所有运行和没有运行的虚拟机
virsh list                 # 查看在运行的虚拟机
virsh dumpxml vm-name      # 查看kvm虚拟机配置文件
virsh define file-name.xml # 根据配置文件定义虚拟机
```
```shell
配置管理

更改CPU
virsh setvcpus centos1 --maximum 4 --config

更改内存
virsh setmaxmem centos1 1048576 --config    #单位kb 1048576kb=1gb

从配置文件更改
找到'memory'和'vcpu'标签修改即可
```

## 网络配置
```shell
默认的网络模式为NAT，装好后只能通过宿主机访问
进入虚拟机后的网络配置和实体机类似，网关地址可在宿主机查看虚拟网卡得知
```
```shell
配置为桥接模式，则可从其他主机访问虚拟机，且虚拟机有自己的内网IP
```
```shell
先修改宿主机配置(修改前注意备份原文件)
vi /etc/sysconfig/network-scripts/ifcfg-ens33

TYPE=Ethernet
BOOTPROTO=none
NAME=ens33
DEVICE=ens33
ONBOOT=yes
BRIDGE=br0

创建网桥文件 
vi ifcfg-br0

TYPE=Bridge
BOOTPROTO=static
NAME=br0
DEVICE=br0
ONBOOT=yes
GATWAY=192.168.222.2    #宿主机所在网段的网关
IPADDR=192.168.222.101  #配置静态IP
NETMASK=255.255.255.0
DNS1=8.8.8.8

修改虚拟机配置文件(如果安装时没有指定网桥)
vi  /etc/libvirt/qemu/centos1.xml

<interface type='bridge'>
    <mac address='52:54:00:15:28:f5'/>
    <source bridge='br0'/>
    <model type='virtio'/>
    <address type='pci' domain='0x0000' bus='0x00' slot='0x03' function='0x0'/>
</interface>
如果不想输入文件路径，可使用
virsh edit centos1
```
```shell
启动
ifup br0
ifup ens33
systemctl restart network.service   #如果这里报错是配置文件写错了

brctl show  #如果显示br0与ens33在一行，说明网卡绑定成功
```

## 硬盘扩容
```shell
创建新磁盘文件
qemu-img create -f raw /disk/sdb6/c1d6.img 10G

编辑配置文件
virsh edit centos1
在原先的硬盘配置后追加新配置
<disk type='file' device='disk'>
<driver name='qemu' type='raw'/>
<source file='/disk/sdb1/c1.img'/>
<target dev='vda' bus='virtio'/>
<address type='pci' domain='0x0000' bus='0x00' slot='0x04' function='0x0'/>
</disk>
后面加上
<disk type='file' device='disk'>
<driver name='qemu' type='raw' cache='none'/>
<source file='/disk/sdb6/c1d6.img'/>
<target dev='vdb' bus='virtio'/>
<address type='pci' domain='0x0000' bus='0x00' slot='0x06' function='0x0'/>
</disk>

前面的步骤可换为(不支持qcow2格式的磁盘文件，只支持raw格式的磁盘文件)
qemu-img resize xp_4_test.disk01 +150M

使用destroy重启虚拟机
virsh destroy centos1
virsh start centos1
virsh console centos1

检查硬盘 分区
fdisk -l
fdisk /dev/vdb
分区生效
partprobe
fdisk -l

格式化
mkfs -t ext4 /dev/vdb1
挂载
mount /dev/vdb1 /home/cos/hadoopb
df -h
```

## 制作镜像

```shell
创建磁盘文件
centos2.qcow2

克隆虚拟机
virsh shutdown centos1
virt-clone -o centos1 -n centos2 -f /home/vms/centos2.qcow2
```
```shell
创建镜像
cp /home/vms/centos1.qcow2 /home/vms/centosbase.qcow2
virsh dumpxml centos1 > /home/vms/centosbase.xml

根据镜像创建虚拟机
cp /home/vms/centosbase.qcow2 /home/vms/centos2.qcow2
cp /home/vms/centosbase.xml /home/vms/centos2.xml

编辑新虚拟机配置
vi /home/vms/centos2.xml
修改
<domain type='kvm'>
  <name>centos7.113</name>
  <uuid>1e86167a-33a9-4ce8-929e-58013fbf9122</uuid>
  <devices>
    <disk type='file' device='disk'>
      <source file='/home/vms/centos7.113.img'/>
    </disk>
    <interface type='bridge'>
      <mac address='00:00:00:00:00:04'/>
    </interface>    
    </devices>
</domain>
生成UUID
uuidgen 

创建主机但不启动
virsh define /home/vms/centos2.xml
```
>直接克隆需要在关机或暂停的状态下操作
复制配置文件与磁盘文件的方式适用于异机的静态迁移

## 创建快照
>快照是数据存储的某一时刻的状态记录；镜像则是数据存储的某一个时刻的副本

```shell
创建快照
virsh snapshot-create-as centos1
或
virsh snapshot-create-as centos1 my_snapshot

关机
virsh destroy centos1

恢复快照(最新的快照版本)
virsh snapshot-revert centos1 --current
或
virsh snapshot-revert centos1 my_snapshot

删除快照
snapshot-delete centos1 my_snapshot
```


