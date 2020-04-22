# supervisor

---
## 安装
```shell
pip install supervisor
```
## 创建配置文件
```shell
echo_supervisord_conf > /etc/supervisord.conf
```
>如果生成到其他目录下，启动`supervisord` 和 `supervisorctl` 需要加 `-c DIR`

## 运行命令
```shell
supervisord 选项(用于启动主进程)
-c 自定义配置文件的路径
-n 在前端执行supervisord
-d 指定工作路径
-l 指定supervisord日志路径
-v 打印版本

supervisorctl 选项(用于管理子进程)
-c 配置文件路径
#启动进程
start all
start [<name> <name>]   
#查看进程状态
status [<name> <name>]  
#结束进程
stop [<name> <name>]    
stop all
restart pname    # 重启 pname 程序
reread    ＃ 读取有更新（增加）的配置文件，不会启动新添加的程序
update    ＃ 重启配置文件修改过的程序
pid #获取supervisord进程id
pid <name>  #获取指定子进程id
pid all     #获取所有子进程id

supervisorctl不进入交互环境
$ supervisorctl status
$ supervisorctl stop usercenter
$ supervisorctl start usercenter
$ supervisorctl restart usercenter
$ supervisorctl reread
$ supervisorctl update
```

## 配置文件

```shell
自动生成的配置文件包括三个进程
[unix_http_server]  #http服务进程
file = /tmp/supervisor.sock
chmod = 0777
chown= nobody:nogroup
username = user
password = 123

[inet_http_server]
port = 127.0.0.1:9001
username = user
password = 123

[supervisord]       #主进程
logfile = /tmp/supervisord.log  #主进程日志路径
logfile_maxbytes = 50MB         #日志大小，超出会自动分成两个文件
logfile_backups=10          #日志最大数
loglevel = info             #日志级别
pidfile = /tmp/supervisord.pid  #指定pid文件
nodaemon = false        #false表示后台启动
minfds = 1024           #可以打开的文件描述符的最小值
minprocs = 200          #可以打开的进程数的最小值

[supervisorctl]     #控制进程
serverurl = unix:///tmp/supervisor.sock

[include]
files = relative/directory/*.ini    #指定自定义的配置文件
```
```shell
自定义配置文件
[program:xx]    #xx表示程序名
command=/home/venv/bin/python /home/test.py  #程序启动命令
process_name=%(program_name)s   #进程名，当numprocs不为1时，可指定
# group_name, host_node_name, process_num, program_name
numprocs=1           #进程启动数量
directory=/tmp       #进程工作路径
autostart=true       #随supervisord启动而启动
startsecs=10         #启动10秒后没退出，就认为正常启动，默认为1秒
autorestart=true     #自动重启,可选[unexpected,true,false]
startretries=3       #启动失败自动重试次数，默认是3
priority=999         #进程启动优先级，默认999，值小的优先启动
redirect_stderr=true #把stderr重定向到stdout，默认false
stdout_logfile_maxbytes=20MB  #stdout 日志文件大小，默认50MB
stdout_logfile_backups = 20   #stdout 日志文件备份数，默认是10
stdout_logfile=/home/log/test.log   #指定日志文件

[group:foo]     #进程组
programs=bar,baz    #指定包含的进程
priority=999
```

## 开机自启

[参考链接](https://www.jianshu.com/p/03619bf7d7f5)

### ubuntu

```sh
vi /etc/rc.local
# 在exit 0 之前加入以下命令
/usr/local/bin/supervisord
# 修改rc.local权限
chmod +x /etc/rc.local

```
### centos

新建文件supervisord.service

```sh
#supervisord.service

[Unit] 
Description=Supervisor daemon

[Service] 
Type=forking 
ExecStart=/usr/bin/supervisord -c /etc/supervisord.conf 
ExecStop=/usr/bin/supervisorctl shutdown 
ExecReload=/usr/bin/supervisorctl reload 
KillMode=process 
Restart=on-failure 
RestartSec=42s

[Install] 
WantedBy=multi-user.target
```
将文件拷贝到/usr/lib/systemd/system/

`cp supervisord.service /usr/lib/systemd/system/`

启动服务

`systemctl enable supervisord`

验证一下是否为开机启动

`systemctl is-enabled supervisord`


---

[supervisord官网](http://supervisord.org)
