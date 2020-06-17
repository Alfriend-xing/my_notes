## 自签名https证书

[参考](https://www.jianshu.com/p/7a72851676f1)


### 生成 CA 证书

```sh
# 生成 CA 私钥
openssl genrsa -out ca.key 1024
# 生成证书签名请求文件
openssl req -new -key ca.key -out ca.csr
# 自签名证书
openssl x509 -req -in ca.csr -signkey ca.key -out ca.crt
```

### 生成务器端密钥

```sh
# 生成服务器端私钥
openssl genrsa -out server.key 1024
# 生成服务器端公钥
# -pubout:根据私钥提取出公钥
openssl rsa -in server.key -pubout -out server.pem
```

### 生成服务器端证书

```sh
# 服务器端需要向 CA 机构申请签名证书，在申请签名证书之前依然是创建自己的 CSR 文件
openssl req -new -key server.key -out server.csr
# 向自己的 CA 机构申请证书，签名过程需要 CA 的证书和私钥参与，最终颁发一个带有 CA 签名的证书
openssl x509 -req -CA ca.crt -CAkey ca.key -CAcreateserial -in server.csr -out server.crt
```

### 生成cer文件

```sh
# 使用openssl 进行证书编码转换
命令：openssl x509 -in server.crt -out server.cer -outform der
```

[openssl用法详解](https://www.cnblogs.com/yangxiaolan/p/6256838.html)


## Openssl生成自签名证书

```sh
# 通过openssl生成私钥
openssl genrsa -out server.key 1024

# 根据私钥生成证书申请文件csr，common name要输入域名
openssl req -new -key server.key -out server.csr

# 使用私钥对证书申请进行签名从而生成证书
openssl x509 -req -in server.csr -out server.crt -signkey server.key -days 3650
```
这样就生成了有效期为：10年的证书文件，对于自己内网服务使用足够。

## Nginx安装证书

[参考](https://blog.csdn.net/qq_14989227/article/details/78142663)

打开Nginx安装目录下conf目录中的nginx.conf文件 
找到：

```
# HTTPS server 
# 
#server { 
#    listen       443; 
#    server_name  localhost; 
#    ssl            on; 
#    ssl_certificate      cert.pem; 
#    ssl_certificate_key  cert.key; 
#    ssl_session_timeout  5m; 
#    ssl_protocols  SSLv2 SSLv3 TLSv1; 
#    ssl_ciphers  ALL:!ADH:!EXPORT56:RC4+RSA:+HIGH:+MEDIUM:+LOW:+SSLv2:+EXP; 
#    ssl_prefer_server_ciphers   on; 
#    location / { 
#        root   html; 
#        index  index.html index.htm; 
#    } 
#} 
```

将其修改为 :

```
server { 
       listen       443; 
       server_name  localhost; 
       ssl                  on; 
       ssl_certificate      1_root_bundle.crt;      （证书公钥）
       ssl_certificate_key      2_ domainname.com.key;      （证书私钥）
       ssl_session_timeout  5m; 
       ssl_protocols TLSv1 TLSv1.1 TLSv1.2; 
       ssl_ciphers AESGCM:ALL:!DH:!EXPORT:!RC4:+HIGH:!MEDIUM:!LOW:!aNULL:!eNULL;

       ssl_prefer_server_ciphers   on; 
       location / { 
           root   html; 
           index  index.html index.htm; 
       } 
```

