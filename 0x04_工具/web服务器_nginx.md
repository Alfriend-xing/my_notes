# nginx
HTTP服务器，支持反向代理，负载均衡，HTTP缓存，充分使用异步事件模型，削减上下文调度的开销，提高服务器并发能力。采用了模块化设计，提供了丰富模块的第三方模块。

## 安装
```shell
下载依赖包
yum install -y pcre-devel #用于支持重定向
yum -y install gcc make gcc-c++ wget
yum -y install openssl openssl-devel    #用于支持https

下载源码
wget http://nginx.org/download/nginx-1.15.10.tar.gz
tar zxf nginx-1.13.3.tar.gz

编译
cd nginx-1.11.5
./configure

安装
make
make install

测试安装结果
cd /usr/local/nginx/sbin/
./nginx -t
查看是否有安装成功的输出

配置环境变量(所有用户生效，冒号分隔)
vi /etc/profile
添加 PATH=$PATH:/usr/local/nginx/sbin/
source /etc/profile

卸载
yum remove nginx    #使用yum安装
rm -rf /usr/local/nginx #使用编译安装
```
```shell
开机自启

添加服务文件
vi /lib/systemd/system/nginx.service

添加内容
[Unit]
Description=nginx
After=network.target remote-fs.target nss-lookup.target

[Service]
Type=forking
PIDFile=/var/run/nginx.pid
ExecStartPre=/usr/local/nginx/sbin/nginx -t -c /usr/local/nginx/conf/nginx.conf
ExecStart=/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
ExecReload=/bin/kill -s HUP $MAINPID
ExecStop=/bin/kill -s QUIT $MAINPID
PrivateTmp=true

[Install]
WantedBy=multi-user.target

# [Unit]:服务的说明
# Description:描述服务
# After:描述服务类别
# [Service]服务运行参数的设置
# Type=forking是后台运行的形式
# ExecStart为服务的具体运行命令
# ExecReload为重启命令
# ExecStop为停止命令
# PrivateTmp=True表示给服务分配独立的临时空间
# [Service]的启动、重启、停止命令全部要求使用绝对路径

# 启动、停止、重启nginx服务
systemctl start nginx.service
systemctl stop nginx.service
systemctl restart nginx.service
# 查看服务当前状态
systemctl status nginx.service
# 设置开机自启动
systemctl enable nginx.service
# 停止开机自启动
systemctl disable nginx.service
```
```shell
手动操作

#使用前先添加防火墙端口，配置过系统变量则无需带路径操作

# 启动
/usr/local/nginx/sbin/nginx

# 重启
/usr/local/nginx/sbin/nginx -s reload

# 关闭进程
/usr/local/nginx/sbin/nginx -s stop

# 平滑关闭nginx
/usr/local/nginx/sbin/nginx -s quit

# 查看nginx的安装状态，
/usr/local/nginx/sbin/nginx -V 
```

## 配置
在Centos 默认配置文件在 /usr/local/nginx-1.5.1/conf/nginx.conf 我们要在这里配置一些文件。nginx.conf是主配置文件，由若干个部分组成，每个大括号{}表示一个部分。每一行指令都由分号结束;，标志着一行的结束。

### 常用正则
变量|说明|变量|说明|
--|--|--|--|
.|匹配除换行符以外的任意字符|$|匹配字符串的结束|
?|重复0次或1次|{n}|重复n次|
+|重复1次或更多次|{n,}|重复n次或更多次|
*|重复0次或更多次|[c]|匹配单个字符c|
\d|匹配数字|[a-z]|匹配a-z小写字母的任意一个|
^|匹配字符串的开始|||
### 全局变量
变量|说明|变量|说明|
--|--|--|--|
$args|这个变量等于请求行中的参数，同$query_string|$remote_port|客户端的端口。
$content_length|请求头中的Content-length字段。|$remote_user|已经经过Auth Basic Module验证的用户名。
$content_type|请求头中的Content-Type字段。|$request_filename|当前请求的文件路径，由root或alias指令与URI请求生成。
$document_root|当前请求在root指令中指定的值。|$scheme|HTTP方法（如http，https）。
$host|请求主机头字段，否则为服务器名称。|$server_protocol|请求使用的协议，通常是HTTP/1.0或HTTP/1.1。
$http_user_agent|客户端agent信息|$server_addr|服务器地址，在完成一次系统调用后可以确定这个值。
$http_cookie|客户端cookie信息|$server_name|服务器名称。
$limit_rate|这个变量可以限制连接速率。|$server_port|请求到达服务器的端口号。
$request_method|客户端请求的动作，通常为GET或POST。|$request_uri|包含请求参数的原始URI，不包含主机名，如：/foo/bar.php?arg=baz。
$remote_addr|客户端的IP地址。|$uri|不带请求参数的当前URI，$uri不包含主机名，如/foo/bar.html。
$document_uri|与$uri相同。|||
### 符号参考
符号|说明|符号|说明|符号|说明|
--|--|--|--|--|--|
k,K|千字节|m,M|兆字节|ms|毫秒|
s|秒|m|分钟|h|小时|
d|日|w|周|M|一个月, 30天|
### 配置文件

```shell
#uwsgi配置文件
[uwsgi]
socket=127.0.0.1:3031
wsgi-file=server.py
master=true
processes=4
threads=2
stats=127.0.0.1:9191
```
```shell
#nginx配置文件
server {
    listen 8080;
    server_name www.wenxi.xin 　　　　#这个是什么都无所谓，只是一个名称标记，虫师说他的要写成ip，这个应该不用，因为这个就相当于server ID,写入log
    charset UTF-8;
    client_max_body_size 75M;
    location / {

        include uwsgi_params;　　　　　　#加载uwsgi
        uwsgi_pass 127.0.0.1:8081;     #是uwsgi 启动的socket端口， 即 myweb_uwsgi.ini 中的socket，应该也可以生成个socket文件，然后访问这个文件！
        uwsgi_read_timeout 5;
        # First attempt to serve request as file, then
        # as directory, then fall back to displaying a 404.
        #try_files $uri $uri/ =404;　　　　　　　　#这个要屏蔽，要不会报502错误，此uri是什么，还没找到

    }
    location /static {　　　　　　　　　　#指定静态文件的别名，这个和Apache差不多

        expires 30d;
        autoindex on; 
        add_header Cache-Control private;
        alias /app/mysit/static/;

        }
```

### 通用配置
```shell
user  www www;
worker_processes  2;

error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

pid        logs/nginx.pid;


events {
    use epoll;
    worker_connections  2048;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on; #开启高效文件传输模式
    # tcp_nopush     on;

    keepalive_timeout  65;

  # gzip压缩功能设置
    gzip on;
    gzip_min_length 1k;
    gzip_buffers    4 16k;
    gzip_http_version 1.0;
    gzip_comp_level 6;
    gzip_types text/html text/plain text/css text/javascript application/json application/javascript application/x-javascript application/xml;
    gzip_vary on;
  
  # http_proxy 设置
    client_max_body_size   10m;
    client_body_buffer_size   128k;
    proxy_connect_timeout   75;
    proxy_send_timeout   75;
    proxy_read_timeout   75;
    proxy_buffer_size   4k;
    proxy_buffers   4 32k;
    proxy_busy_buffers_size   64k;
    proxy_temp_file_write_size  64k;
    proxy_temp_path   /usr/local/nginx/proxy_temp 1 2;

  # 设定负载均衡后台服务器列表 
    upstream  backend  { 
              #ip_hash; 
              server   192.168.10.100:8080 max_fails=2 fail_timeout=30s ;  
              server   192.168.10.101:8080 max_fails=2 fail_timeout=30s ;  
    }

  # 很重要的虚拟主机配置
    server {
        listen       80;
        server_name  itoatest.example.com;
        root   /apps/oaapp;

        charset utf-8;
        access_log  logs/host.access.log  main;

        #对 / 所有做负载均衡+反向代理
        location / {
            root   /apps/oaapp;
            index  index.jsp index.html index.htm;

            proxy_pass        http://backend;  
            proxy_redirect off;
            # 后端的Web服务器可以通过X-Forwarded-For获取用户真实IP
            proxy_set_header  Host  $host;
            proxy_set_header  X-Real-IP  $remote_addr;  
            proxy_set_header  X-Forwarded-For  $proxy_add_x_forwarded_for;
            proxy_next_upstream error timeout invalid_header http_500 http_502 http_503 http_504;
            
        }

        #静态文件，nginx自己处理，不去backend请求tomcat
        location  ~* /download/ {  
            root /apps/oa/fs;  
            
        }
        location ~ .*\.(gif|jpg|jpeg|bmp|png|ico|txt|js|css)$   
        {   
            root /apps/oaapp;   
            expires      7d; 
        }
       	location /nginx_status {
            stub_status on;
            access_log off;
            allow 192.168.10.0/24;
            deny all;
        }

        location ~ ^/(WEB-INF)/ {   
            deny all;   
        }
        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }

  ## 其它虚拟主机，server 指令开始
}
```
## 常用功能

### 反向代理
```shell
server {  
  listen       80;                                                        
  server_name  localhost;                                              
  client_max_body_size 1024M;  # 允许客户端请求的最大单文件字节数

  location / {
    proxy_pass                         http://localhost:8080;
    proxy_set_header Host              $host:$server_port;
    proxy_set_header X-Forwarded-For   $remote_addr; # HTTP的请求端真实的IP
    proxy_set_header X-Forwarded-Proto $scheme;      # 为了正确地识别实际用户发出的协议是 http 还是 https
  }
}

server {
    #侦听的80端口
    listen       80;
    server_name  git.example.cn;
    location / {
        proxy_pass   http://localhost:3000;
        #以下是一些反向代理的配置可删除
        proxy_redirect             off;
        #后端的Web服务器可以通过X-Forwarded-For获取用户真实IP
        proxy_set_header           Host $host;
        client_max_body_size       10m; #允许客户端请求的最大单文件字节数
        client_body_buffer_size    128k; #缓冲区代理缓冲用户端请求的最大字节数
        proxy_connect_timeout      300; #nginx跟后端服务器连接超时时间(代理连接超时)
        proxy_send_timeout         300; #后端服务器数据回传时间(代理发送超时)
        proxy_read_timeout         300; #连接成功后，后端服务器响应时间(代理接收超时)
        proxy_buffer_size          4k; #设置代理服务器（nginx）保存用户头信息的缓冲区大小
        proxy_buffers              4 32k; #proxy_buffers缓冲区，网页平均在32k以下的话，这样设置
        proxy_busy_buffers_size    64k; #高负荷下缓冲大小（proxy_buffers*2）
    }
}
```
### 负载均衡
```shell
upstream gitlab {
    ip_hash;
    # upstream的负载均衡，weight是权重，可以根据机器配置定义权重。weigth参数表示权值，权值越高被分配到的几率越大。
    server 192.168.122.11:8081 ;
    server 127.0.0.1:82 weight=3;
    server 127.0.0.1:83 weight=3 down;
    server 127.0.0.1:84 weight=3; max_fails=3  fail_timeout=20s;
    server 127.0.0.1:85 weight=4;;
    keepalive 32;
}
server {
    #侦听的80端口
    listen       80;
    server_name  git.example.cn;
    location / {
        proxy_pass   http://gitlab;    #在这里设置一个代理，和upstream的名字一样
        #以下是一些反向代理的配置可删除
        proxy_redirect             off;
        #后端的Web服务器可以通过X-Forwarded-For获取用户真实IP
        proxy_set_header           Host $host;
        proxy_set_header           X-Real-IP $remote_addr;
        proxy_set_header           X-Forwarded-For $proxy_add_x_forwarded_for;
        client_max_body_size       10m;  #允许客户端请求的最大单文件字节数
        client_body_buffer_size    128k; #缓冲区代理缓冲用户端请求的最大字节数
        proxy_connect_timeout      300;  #nginx跟后端服务器连接超时时间(代理连接超时)
        proxy_send_timeout         300;  #后端服务器数据回传时间(代理发送超时)
        proxy_read_timeout         300;  #连接成功后，后端服务器响应时间(代理接收超时)
        proxy_buffer_size          4k; #设置代理服务器（nginx）保存用户头信息的缓冲区大小
        proxy_buffers              4 32k;# 缓冲区，网页平均在32k以下的话，这样设置
        proxy_busy_buffers_size    64k; #高负荷下缓冲大小（proxy_buffers*2）
        proxy_temp_file_write_size 64k; #设定缓存文件夹大小，大于这个值，将从upstream服务器传
    }
}
```
负载均衡三种实现方式
- 轮询： 默认情况下使用轮询算法，不需要配置指令来激活它，它是基于在队列中谁是下一个的原理确保访问均匀地分布到每个上游服务器；
```shell
upstream test {
    server localhost:8080;
    server localhost:8081;
}
server {
    listen       81;
    server_name  localhost;
    client_max_body_size 1024M;
 
    location / {
        proxy_pass http://test;
        proxy_set_header Host $host:$server_port;
    }
}
```
```shell
# 指定权重的轮询
upstream test {
    server localhost:8080 weight=9;
    server localhost:8081 weight=1;
}
```
- IP哈希： 通过ip_hash指令来激活，Nginx通过IPv4地址的前3个字节或者整个IPv6地址作为哈希键来实现，同一个IP地址总是能被映射到同一个上游服务器；
```shell
upstream test {
    ip_hash;
    server localhost:8080;
    server localhost:8081;
}
```
- 最少连接数： 通过least_conn指令来激活，该算法通过选择一个活跃数最少的上游服务器进行连接。如果上游服务器处理能力不同，可以通过给server配置weight权重来说明，该算法将考虑到不同服务器的加权最少连接数。
### 屏蔽IP

```shell
# nginx.conf中加入如下配置，可以放到http, server, location, limit_except语句块
include blockip.conf;

# blockip.conf加入内容

deny 165.91.122.67;

deny IP;   # 屏蔽单个ip访问
allow IP;  # 允许单个ip访问
deny all;  # 屏蔽所有ip访问
allow all; # 允许所有ip访问
deny 123.0.0.0/8   # 屏蔽整个段即从123.0.0.1到123.255.255.254访问的命令
deny 124.45.0.0/16 # 屏蔽IP段即从123.45.0.1到123.45.255.254访问的命令
deny 123.45.6.0/24 # 屏蔽IP段即从123.45.6.1到123.45.6.254访问的命令

# 如果你想实现这样的应用，除了几个IP外，其他全部拒绝
allow 1.1.1.1; 
allow 1.1.1.2;
deny all; 
```
### 重定向
```shell
# 重定向整个网站
server {
    server_name old-site.com
    return 301 $scheme://new-site.com$request_uri;
}

# 重定向单页
server {
    location = /oldpage.html {
        return 301 http://example.org/newpage.html;
    }
}

# 重定向整个子路径
location /old-site {
    rewrite ^/old-site/(.*) http://example.org/new-site/$1 permanent;
}
```
### 性能优化
- 内容缓存
```shell
# 允许浏览器基本上永久地缓存静态内容。 Nginx将为您设置Expires和Cache-Control头信息。
location /static {
    root /data;
    expires max;
}

# 如果要求浏览器永远不会缓存响应（例如用于跟踪请求），请使用-1。
location = /empty.gif {
    empty_gif;
    expires -1;
}
```
- Gzip压缩
```shell
gzip  on;
gzip_buffers 16 8k;
gzip_comp_level 6;
gzip_http_version 1.1;
gzip_min_length 256;
gzip_proxied any;
gzip_vary on;
gzip_types
    text/xml application/xml application/atom+xml application/rss+xml application/xhtml+xml image/svg+xml
    text/javascript application/javascript application/x-javascript
    text/x-json application/json application/x-web-app-manifest+json
    text/css text/plain text/x-component
    font/opentype application/x-font-ttf application/vnd.ms-fontobject
    image/x-icon;
gzip_disable  "msie6";
```
- 打开文件缓存
```shell
open_file_cache max=1000 inactive=20s;
open_file_cache_valid 30s;
open_file_cache_min_uses 2;
open_file_cache_errors on;
```
- SSL缓存
```shell
ssl_session_cache shared:SSL:10m;
ssl_session_timeout 10m;
```


- 上游Keepalive
```shellupstream backend {
    server 127.0.0.1:8080;
    keepalive 32;
}
server {
    ...
    location /api/ {
        proxy_pass http://backend;
        proxy_http_version 1.1;
        proxy_set_header Connection "";
    }
}

```
### 监控
使用ngxtop实时解析nginx访问日志，并且将处理结果输出到终端，功能类似于系统命令top
```shell
# 安装 ngxtop
pip install ngxtop

# 实时状态
ngxtop

# 状态为404的前10个请求的路径：
ngxtop top request_path --filter 'status == 404'

# 发送总字节数最多的前10个请求
ngxtop --order-by 'avg(bytes_sent) * count'

# 排名前十位的IP，例如，谁攻击你最多
ngxtop --group-by remote_addr

# 打印具有4xx或5xx状态的请求，以及status和http referer
ngxtop -i 'status >= 400' print request status http_referer

# 由200个请求路径响应发送的平均正文字节以'foo'开始：
ngxtop avg bytes_sent --filter 'status == 200 and request_path.startswith("foo")'

# 使用“common”日志格式从远程机器分析apache访问日志
ssh remote tail -f /var/log/apache2/access.log | ngxtop -f common
```

### 跨域问题
使API支持跨域请求
```shell
server {
  listen 80;
  server_name api.xxx.com;
    
  add_header 'Access-Control-Allow-Origin' '*';
  add_header 'Access-Control-Allow-Credentials' 'true';
  add_header 'Access-Control-Allow-Methods' 'GET,POST,HEAD';

  location / {
    proxy_pass http://127.0.0.1:3000;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header Host  $http_host;    
  } 
}
```
### 跳转至www
```shell
server {
    listen 80;
    # 配置正常的带www的域名
    server_name www.wangchujiang.com;
    root /home/www/wabg/download;
    location / {
        try_files $uri $uri/ /index.html =404;
    }
}
server {
    # 这个要放到下面，
    # 将不带www的 wangchujiang.com 永久性重定向到  https://www.wangchujiang.com
    server_name wangchujiang.com;
    rewrite ^(.*) https://www.wangchujiang.com$1 permanent;
}
```
### 代理转发
```shell
upstream server-api{
    # api 代理服务地址
    server 127.0.0.1:3110;    
}
upstream server-resource{
    # 静态资源 代理服务地址
    server 127.0.0.1:3120;
}
server {
    listen       3111;
    server_name  localhost;      # 这里指定域名
    root /home/www/server-statics;
    # 匹配 api 路由的反向代理到API服务
    location ^~/api/ {
        rewrite ^/(.*)$ /$1 break;
        proxy_pass http://server-api;
    }
    # 假设这里验证码也在API服务中
    location ^~/captcha {
        rewrite ^/(.*)$ /$1 break;
        proxy_pass http://server-api;
    }
    # 假设你的图片资源全部在另外一个服务上面
    location ^~/img/ {
        rewrite ^/(.*)$ /$1 break;
        proxy_pass http://server-resource;
    }
    # 路由在前端，后端没有真实路由，在路由不存在的 404状态的页面返回 /index.html
    # 这个方式使用场景，你在写React或者Vue项目的时候，没有真实路由
    location / {
        try_files $uri $uri/ /index.html =404;
        #                               ^ 空格很重要
    }
}
```
### ssl配置

```shell
# 创建自签证书

# 首先，创建证书和私钥的目录
mkdir -p /etc/nginx/cert
cd /etc/nginx/cert
# 创建服务器私钥，命令会让你输入一个口令：
openssl genrsa -des3 -out nginx.key 2048
# 创建签名请求的证书（CSR）：
openssl req -new -key nginx.key -out nginx.csr
# 在加载SSL支持的Nginx并使用上述私钥时除去必须的口令：
cp nginx.key nginx.key.org
openssl rsa -in nginx.key.org -out nginx.key
# 最后标记证书使用上述私钥和CSR：
openssl x509 -req -days 365 -in nginx.csr -signkey nginx.key -out nginx.crt
```
```shell
#配置文件
server {
    listen       443 ssl;
    server_name  localhost;

    ssl_certificate /etc/nginx/ssl/nginx.crt;
    ssl_certificate_key /etc/nginx/ssl/nginx.key;
    # 禁止在header中出现服务器版本，防止黑客利用版本漏洞攻击
    server_tokens off;
    # 设置ssl/tls会话缓存的类型和大小。如果设置了这个参数一般是shared，buildin可能会参数内存碎片，默认是none，和off差不多，停用缓存。如shared:SSL:10m表示我所有的nginx工作进程共享ssl会话缓存，官网介绍说1M可以存放约4000个sessions。 
    ssl_session_cache    shared:SSL:1m; 

    # 客户端可以重用会话缓存中ssl参数的过期时间，内网系统默认5分钟太短了，可以设成30m即30分钟甚至4h。
    ssl_session_timeout  5m; 

    # 选择加密套件，不同的浏览器所支持的套件（和顺序）可能会不同。
    # 这里指定的是OpenSSL库能够识别的写法，你可以通过 openssl -v cipher 'RC4:HIGH:!aNULL:!MD5'（后面是你所指定的套件加密算法） 来看所支持算法。
    ssl_ciphers  HIGH:!aNULL:!MD5;

    # 设置协商加密算法时，优先使用我们服务端的加密套件，而不是客户端浏览器的加密套件。
    ssl_prefer_server_ciphers  on;

    location / {
        root   html;
        index  index.html index.htm;
    }
}
```
### 将http重定向到https
```shell
server {
    listen       80;
    server_name  example.com;
    rewrite ^ https://$http_host$request_uri? permanent;    # 强制将http重定向到https
    # 在错误页面和“服务器”响应头字段中启用或禁用发射nginx版本。 
    # 防止黑客利用版本漏洞攻击
    server_tokens off;
}
```
### 多域名
```shell
# 静态网站配置
http {
    server {
        listen          80;
        server_name     www.domain1.com;
        access_log      logs/domain1.access.log main;
        location / {
            index index.html;
            root  /var/www/domain1.com/htdocs;
        }
    }
    server {
        listen          80;
        server_name     www.domain2.com;
        access_log      logs/domain2.access.log main;
        location / {
            index index.html;
            root  /var/www/domain2.com/htdocs;
        }
    }
}
```
```shell
# 标准配置
http {
  server {
    listen          80 default;
    server_name     _ *;
    access_log      logs/default.access.log main;
    location / {
       index index.html;
       root  /var/www/default/htdocs;
    }
  }
}
```
### 爬虫过滤(初级爬虫)
```shell
location / {
    if ($http_user_agent ~* "python|curl|java|wget|httpclient|okhttp") {
        return 503;
    }
    # 正常处理
    # ...
}
```
### 防盗链
```shell
location ~* \.(gif|jpg|png|swf|flv)$ {
   root html
   valid_referers none blocked *.nginxcn.com;
   if ($invalid_referer) {
     rewrite ^/ www.nginx.cn
     #return 404;
   }
}
```
### 虚拟目录配置
```shell
location /img/ {
    alias /var/www/image/;
}
# 访问/img/目录里面的文件时，ningx会自动去/var/www/image/目录找文件
location /img/ {
    root /var/www/image;
}
# 访问/img/目录下的文件时，nginx会去/var/www/image/img/目录下找文件。
```
### 防盗图配置
```shell
location ~ \/public\/(css|js|img)\/.*\.(js|css|gif|jpg|jpeg|png|bmp|swf) {
    valid_referers none blocked *.jslite.io;
    if ($invalid_referer) {
        rewrite ^/  http://wangchujiang.com/piratesp.png;
    }
}
```
### 屏蔽.git等文件
```shell
location ~ (.git|.gitattributes|.gitignore|.svn) {
    deny all;
}
```
### 忽略后缀
```shell
http://wangchujiang.com/api/index.php?a=1&name=wcj
                                  ^ 有后缀

http://wangchujiang.com/api/index?a=1&name=wcj
                                 ^ 没有后缀
# nginx rewrite规则如下：

rewrite ^/(.*)/$ /index.php?/$1 permanent;
if (!-d $request_filename){
        set $rule_1 1$rule_1;
}
if (!-f $request_filename){
        set $rule_1 2$rule_1;
}
if ($rule_1 = "21"){
        rewrite ^/ /index.php last;
}
```