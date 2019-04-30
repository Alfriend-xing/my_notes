## 发送邮件
要安装 JavaMail API 和Java Activation Framework (JAF)

下载最新版本的 JavaMail 和 JAF ( 版本 1.1.1)

下载并解压缩这些文件，然后把 mail.jar 和 activation.jar 文件添加到 CLASSPATH 中
```java
// 实例

import java.util.*;
import javax.mail.*;
import javax.mail.internet.*;
import javax.activation.*;

public class SendEmail
{
   public static void main(String [] args)
   {   
      // 收件人电子邮箱
      String to = "abcd@gmail.com";

      // 发件人电子邮箱
      String from = "web@gmail.com";

      // 指定发送邮件的主机为 localhost
      String host = "localhost";

      // 获取系统属性
      Properties properties = System.getProperties();

      // 设置邮件服务器
      properties.setProperty("mail.smtp.host", host);

      // 获取默认session对象
      Session session = Session.getDefaultInstance(properties);

      try{
         // 创建默认的 MimeMessage 对象
         MimeMessage message = new MimeMessage(session);

         // Set From: 头部头字段
         message.setFrom(new InternetAddress(from));

         // Set To: 头部头字段
         message.addRecipient(Message.RecipientType.TO,
                                  new InternetAddress(to));

         // Set Subject: 头部头字段
         message.setSubject("This is the Subject Line!");

         // 设置消息体
         message.setText("This is actual message");

         // 发送消息
         Transport.send(message);
         System.out.println("Sent message successfully....");
      }catch (MessagingException mex) {
         mex.printStackTrace();
      }
   }
}
```
### 指定多个收件人
```java
void addRecipients(Message.RecipientType type,Address[] addresses) throws MessagingException

// type
// 设置为 TO, CC 或者 BCC，这里 CC 代表抄送、BCC 代表秘密抄送
// 举例： Message.RecipientType.TO

// addresses	这是 email ID 的数组
// 指定电子邮件 ID 时，需要使用 InternetAddress() 方法
```
### 发送 HTML 格式的电子邮件
```java
// 将
message.setText("This is actual message");
// 改为
message.setContent("<h1>This is actual message</h1>",
                            "text/html" );
```
### 发送带有附件的邮件
```java
// 创建消息部分
BodyPart messageBodyPart = new MimeBodyPart();

// 消息
messageBodyPart.setText("This is message body");

// 创建多重消息
Multipart multipart = new MimeMultipart();

// 设置文本消息部分
multipart.addBodyPart(messageBodyPart);

// 附件部分
messageBodyPart = new MimeBodyPart();
String filename = "file.txt";
DataSource source = new FileDataSource(filename);
messageBodyPart.setDataHandler(new DataHandler(source));
messageBodyPart.setFileName(filename);
multipart.addBodyPart(messageBodyPart);

// 发送完整消息
message.setContent(multipart );

//   发送消息
Transport.send(message);
```

### 用户认证部分
邮件服务器可能需要提供用户名和密码来达到用户认证的目的
```java
props.put("mail.smtp.auth", "true");
props.setProperty("mail.user", "myuser");
props.setProperty("mail.password", "mypwd");
```
```java
// 实例

import java.util.Properties;

import javax.mail.Authenticator;
import javax.mail.Message;
import javax.mail.MessagingException;
import javax.mail.PasswordAuthentication;
import javax.mail.Session;
import javax.mail.Transport;
import javax.mail.internet.InternetAddress;
import javax.mail.internet.MimeMessage;

public class SendEmail2
{
   public static void main(String [] args)
   {
      // 收件人电子邮箱
      String to = "xxx@qq.com";

      // 发件人电子邮箱
      String from = "xxx@qq.com";

      // 指定发送邮件的主机为 smtp.qq.com
      String host = "smtp.qq.com";  //QQ 邮件服务器

      // 获取系统属性
      Properties properties = System.getProperties();

      // 设置邮件服务器
      properties.setProperty("mail.smtp.host", host);

      properties.put("mail.smtp.auth", "true");
      // 获取默认session对象
      Session session = Session.getDefaultInstance(properties,new Authenticator(){
        public PasswordAuthentication getPasswordAuthentication()
        {
         return new PasswordAuthentication("xxx@qq.com", "qq邮箱密码"); //发件人邮件用户名、密码
        }
       });

      try{
         // 创建默认的 MimeMessage 对象
         MimeMessage message = new MimeMessage(session);

         // Set From: 头部头字段
         message.setFrom(new InternetAddress(from));

         // Set To: 头部头字段
         message.addRecipient(Message.RecipientType.TO,
                                  new InternetAddress(to));

         // Set Subject: 头部头字段
         message.setSubject("This is the Subject Line!");

         // 设置消息体
         message.setText("This is actual message");

         // 发送消息
         Transport.send(message);
         System.out.println("Sent message successfully....from twle.cn");
      }catch (MessagingException mex) {
         mex.printStackTrace();
      }
   }
}
```


