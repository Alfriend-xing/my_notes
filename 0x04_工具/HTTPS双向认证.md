# Nginx https 双向认证

## 认证过程

[双向认证 SSL 协议的具体过程](http://blog.chinaunix.net/uid-26335251-id-3508651.html)

1. 浏览器发送一个连接请求给安全服务器。
1. 服务器将自己的证书，以及同证书相关的信息发送给客户浏览器。
1. 客户浏览器检查服务器送过来的证书是否是由自己信赖的 CA 中心所签发的。如果是，就继续执行协议；如果不是，客户浏览器就给客户一个警告消息：警告客户这个证书不是可以信赖的，询问客户是否需要继续。
1. 接着客户浏览器比较证书里的消息，例如域名和公钥，与服务器刚刚发送的相关消息是否一致，如果是一致的，客户浏览器认可这个服务器的合法身份。
1. 服务器要求客户发送客户自己的证书。收到后，服务器验证客户的证书，如果没有通过验证，拒绝连接；如果通过验证，服务器获得用户的公钥。
1. 客户浏览器告诉服务器自己所能够支持的通讯对称密码方案。
1. 服务器从客户发送过来的密码方案中，选择一种加密程度最高的密码方案，用客户的公钥加过密后通知浏览器。
1. 浏览器针对这个密码方案，选择一个通话密钥，接着用服务器的公钥加过密后发送给服务器。
1. 服务器接收到浏览器送过来的消息，用自己的私钥解密，获得通话密钥。
1. 服务器、浏览器接下来的通讯都是用对称密码方案，对称密钥是加过密的。
上 面所述的是双向认证 SSL 协议的具体通讯过程，这种情况要求服务器和用户双方都有证书。单向认证 SSL 协议不需要客户拥有 CA 证书，具体的过程相对于上面的步骤，只需将服务器端验证客户证书的过程去掉，以及在协商对称密码方案，对称通话密钥时，服务器发送给客户的是没有加过密的 （这并不影响 SSL 过程的安全性）密码方案。这样，双方具体的通讯内容，就是加过密的数据，如果有第三方攻击，获得的只是加密的数据，第三方要获得有用的信息，就需要对加密 的数据进行解密，这时候的安全就依赖于密码方案的安全。而幸运的是，目前所用的密码方案，只要通讯密钥长度足够的长，就足够的安全。这也是我们强调要求使 用 128 位加密通讯的原因。

## 文件格式和编码

[参考1](https://www.jianshu.com/p/36c94e46ee29)
[参考2](https://www.cnblogs.com/lzjsky/archive/2010/11/14/1877143.html)

>CA：相当于一个认证机构，只要经过这个机构签名的证书我们就可以当做是可信任的。我们的浏览器中，已经被写入了默认的CA根证书。

>证书：将我们的公钥和相关信息写入一个文件，CA用它们的私钥对我们的公钥和相关信息进行签名后，将签名信息也写入这个文件后生成的一个文件。

- 证书格式(是一种标准)：
  - x509 : 这种证书只有公钥，不包含私钥。
  - pcks#7 : 这种主要是用于签名或者加密。
  - pcks#12 : 这种含有私钥，同时也含有公钥，但是有口令保护。
- 编码方式：
  - pem 后缀的证书都是base64编码
  - der 后缀的证书都是ASCII编码
- 证书文件：
  - .csr Certificate Signing Request的英文缩写，即证书签名请求文件，是证书申请者在申请数字证书时由CSP(加密服务提供者)在生成私钥的同时也生成证书请求文件，证书申请者只要把CSR文件提交给证书颁发机构后，证书颁发机构使用其根证书私钥签名就生成了证书公钥文件，也就是颁发给用户的证书。
  - .crt .cer 后缀的文件都是证书文件（编码方式不一定，有可能是.pem,也有可能是.der）
    - .CRT = 扩展名CRT用于证书。证书可以是DER编码，也可以是PEM编码。扩展名CER和CRT几乎是同义词。这种情况在各种unix/linux系统中很常见。
    - CER = CRT证书的微软型式。可以用微软的工具把CRT文件转换为CER文件（CRT和CER必须是相同编码的，DER或者PEM）。扩展名为CER的文件可以被IE识别。Windows中的证书扩展名有好几种，比如.cer和.crt。通常而言，.cer文件是二进制数据，而.crt文件包含的是ASCII数据。
- 私钥文件：
  - .key 后缀的文件是私钥文件
- 包含证书和私钥的文件：
  - .keystore .jks .truststore 后缀的文件，是java搞的，java可以用这个格式。这个文件中包含证书和私钥，但是获取私钥需要密码才可以, 这是一个证书库，里面可以保存多个证书和密钥，通过别名可以获取到。
  - .pfx，.p12 浏览器可以使用，包含证书和私钥，获取私钥需要密码才可以

## 使用openssl生成证书

[Nginx https 双向认证](https://www.cnblogs.com/yelao/p/9486882.html)

### 根证书

```sh
# 1 创建根证私钥
openssl genrsa -out root-key.key 1024

# 2 创建根证书请求文件，需要输入一致信息
# challenge password的地方直接回车就好
openssl req -new -out root-req.csr -key root-key.key

# 3 自签根证书
openssl x509 -req -in root-req.csr -out root-cert.cer -signkey root-key.key -CAcreateserial -days 3650

# 4 生成p12格式根证书,需要填密码，如123456
openssl pkcs12 -export -clcerts -in root-cert.cer -inkey root-key.key -out root.p12
```

### 服务端证书

```sh
# 5 生成服务端key
openssl genrsa -out server-key.key 1024

# 6 生成服务端请求文件
# 国家省市公司和 2 保持一致, Common Name 要用你服务器的域名
# 如果测试时无域名，可随意指定，访问时修添加C:\Windows\System32\drivers\etc\hosts 文件中域名和ip的映射
openssl req -new -out server-req.csr -key server-key.key

# 7 生成服务端证书（root证书，rootkey，服务端key，服务端请求文件这4个生成服务端证书）
openssl x509 -req -in server-req.csr -out server-cert.cer -signkey server-key.key -CA root-cert.cer -CAkey root-key.key -CAcreateserial -days 3650
```

### 客户端证书

```sh
# 8 生成客户端key
openssl genrsa -out client-key.key 1024

# 9 生成客户端请求文件
openssl req -new -out client-req.csr -key client-key.key

# 10 生成客户端证书（root证书，rootkey，客户端key，客户端请求文件这4个生成客户端证书）
openssl x509 -req -in client-req.csr -out client-cert.cer -signkey client-key.key -CA root-cert.cer -CAkey root-key.key -CAcreateserial -days 3650

# 11 生成客户端p12格式根证书,需要填密码，如123456
openssl pkcs12 -export -clcerts -in client-cert.cer -inkey client-key.key -out client.p12
```

## nginx配置(服务器端)

```python
worker_processes  1;
events {
    worker_connections  1024;
} 
http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;

    server {
        listen       443 ssl;
        server_name  ttt.com;
        ssl                  on;  
        ssl_certificate      /data/sslKey/server-cert.cer;  #server证书公钥
        ssl_certificate_key  /data/sslKey/server-key.key;  #server私钥
        ssl_client_certificate /data/sslKey/root-cert.cer;  #根级证书公钥，用于验证各个二级client
        ssl_verify_client on;  #开启客户端证书验证  

        location / {
            root   html;
            index  index.html index.htm;
        }
    }
}
```

```python
server {
        listen       80;
        server_name  www.iwen.com;
        #return 301 https://$server_name$request_uri;
 
        if ($request_method !~ ^(GET|POST)$) {
            return 403;
        }
        .......
        .......
}
```

## 最终效果

>如果是测试环境，先在 `C:\Windows\System32\drivers\etc\hosts` 里面做好 域名和ip的映射

在浏览器访问域名时无法打开页面

将client.p12 导入浏览器后,可以访问,但是证书会有红色x号.因为是我们自己签的.浏览器不信任

这时候我们将我们的root.p12也导入,证书存储不用默认的个人,选择 **受信任的根证书颁发机构** , 之后可正常访问。

## java代码访问(客户端)

根据我们在第1点里面的了解,双向认证是需要互相认证证书的. 所以客户端需要认证服务器证书,也要把客户端证书发送给服务器. 用浏览器做客户端的时候,认证服务器证书自动进行,提交客户端证书也是自动进行(需要导入证书到浏览器)

当我们用java代码来做的时候, 也是需要这些步骤.

1)首先是认证服务器证书, jdk有默认的信任证书列表$JRE/lib/security/cacerts

也会默认信任 $JRE/lib/security/jssecacerts 里的证书.

如果你把证书放到别的地方,则需要在代码中指定

(理论上如果是买的根机构签发的证书,是不需要导入到java自己的库里,但是java的和操作系统的信任库可能不一样,我们买的在浏览器就OK,在java中就必须手动导入服务端证书到信任列表中)

用keytool导入的时候注意下 keystore的路径. cacerts的默认密码是changeit

```sh
D:\>cd jdk1.7.0_80\jre7\lib\security
D:\jdk1.7.0_80\jre7\lib\security>keytool -import -alias ttt -keystore cacerts -file e:/HttpsDemo/server-cert.cer
输入密钥库口令:
所有者: CN=*.ttt.com, OU=dc, O=dc, L=bj, ST=bj, C=cn
发布者: CN=root, OU=dc, O=dc, L=bj, ST=bj, C=cn
序列号: a034f5e5d4b1c825
有效期开始日期: Thu Oct 20 00:01:52 CST 2016, 截止日期: Sun Oct 18 00:01:52 CST 2026
证书指纹:
         MD5: 65:CB:C9:0D:C4:E7:66:F9:09:3D:B4:17:E6:6B:E5:AB
         SHA1: 41:AD:9E:EB:61:88:AE:1B:A3:76:CE:F8:2C:BB:5D:74:C8:0D:2D:0D
         SHA256: 0D:17:D4:EF:2E:9D:89:EA:3A:1F:32:44:D5:12:DF:E0:EE:58:61:04:1A:28:BC:91:D4:7C:3F:AF:FE:99:79:16
         签名算法名称: SHA1withRSA
         版本: 1
是否信任此证书? [否]:  y
证书已添加到密钥库中
D:\jdk1.7.0_80\jre7\lib\security>
```

2) java代码(包含加载客户端证书)

```java
import java.io.File;
import java.io.FileInputStream;
import java.io.InputStream;
import java.security.KeyStore;
import javax.net.ssl.SSLContext;
import org.apache.http.HttpEntity;
import org.apache.http.client.methods.CloseableHttpResponse;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.conn.ssl.SSLConnectionSocketFactory;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.ssl.SSLContexts;
import org.apache.http.util.EntityUtils;
 
public class HttpsDemo {
    private final static String PFX_PATH = "e:/HttpsDemo/client.p12";    //客户端证书路径
    private final static String PFX_PWD = "123456";    //客户端证书密码
    
　　 public static String sslRequestGet(String url) throws Exception {
        KeyStore keyStore = KeyStore.getInstance("PKCS12");
        InputStream instream = new FileInputStream(new File(PFX_PATH));
        try {
            keyStore.load(instream, PFX_PWD.toCharArray());
        } finally {
            instream.close();
        }
        SSLContext sslcontext = SSLContexts.custom().loadKeyMaterial(keyStore, PFX_PWD.toCharArray()).build();
        SSLConnectionSocketFactory sslsf = new SSLConnectionSocketFactory(sslcontext
                , new String[] { "TLSv1" }    // supportedProtocols ,这里可以按需要设置
                , null    // supportedCipherSuites
                , SSLConnectionSocketFactory.getDefaultHostnameVerifier());    
  
        CloseableHttpClient httpclient = HttpClients.custom().setSSLSocketFactory(sslsf).build();
        try {
            HttpGet httpget = new HttpGet(url); 
//            httpost.addHeader("Connection", "keep-alive");// 设置一些heander等
            CloseableHttpResponse response = httpclient.execute(httpget);
            try {
                HttpEntity entity = response.getEntity();
                String jsonStr = EntityUtils.toString(response.getEntity(), "UTF-8");//返回结果
                EntityUtils.consume(entity);
                return jsonStr;
            } finally {
                response.close();
            }
        } finally {
            httpclient.close();
        }
    }
 
    public static void main(String[] args) throws Exception {
        System.out.println(System.getProperty("java.home"));
        System.out.println(sslRequestGet("https://www.ttt.com/"));
    }
}
```

## 使用购买的证书(信任机构签发的)

如果是公司使用的话,证书一般是从信任机构那里买的.所以就不需要上面测试的root证书.

信任机构提供 服务端证书和私钥,客户端证书 就可以了.

