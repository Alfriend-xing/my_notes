# VMware ESXi虚拟机克隆及迁移

VMware ESXi 5作为虚拟机的宿主系统，克隆需要用到vCenter Server，
而vCenter Server是收费的

使用vSphere Client手动克隆

1. 进入vSphere client，关闭需要克隆的虚拟机win2003
1. 选中ESXi服务器主机，在右侧点击“配置”选项卡，选择存储器，右侧的存储器名称上点右键，选择“浏览数据存储”
1. 进入win2003文件夹，把win2003.vmx和win2003.vmdk这两个文件复制到文件夹kelong下，如果有快照文件一并复制(win2003-000003.vmdk)
1. 在win2003.vmx文件上点右键，选择“添加到清单”，弹出提示，询问这个虚拟机是移动的还是复制的，选择“I coyied it”，确定。
1. 修改配置IP地址，mac地址和其他需要更改的配置
>核心操作是复制虚拟机文件和虚拟磁盘文件，并添加到虚拟机清单中

[VMware ESXi虚拟机克隆及迁移](https://blog.51cto.com/75clouds/854233)