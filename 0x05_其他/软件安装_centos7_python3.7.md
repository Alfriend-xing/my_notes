安装 Python 3.7.2 至 CentOS

安装依赖环境
```shell
yum install gcc openssl-devel bzip2-devel libffi-devel
```
下载python3.7
```shell
cd /usr/src
wget https://www.python.org/ftp/python/3.7.2/Python-3.7.2.tgz
tar xzf Python-3.7.2.tgz
```
编译和安装
```shell
cd Python-3.7.2
./configure --enable-optimizations
make altinstall
```
> `rm /usr/src/Python-3.7.2.tgz`

查看版本
```shell
python3.7 -V
```

安装路径
```shell
cd /user/local/
```

[参考链接](https://tecadmin.net/install-python-3-7-on-centos/)

