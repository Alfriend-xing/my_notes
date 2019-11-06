# 前后端分离中防范csrf

## csrf

即跨站请求伪造，恶意网站在网页中构造危险请求，对已登录的其他网站进行表单提交操作。而[表单提交操作是可以跨源发送的](https://developer.mozilla.org/zh-CN/docs/Web/Security/Same-origin_policy)，并且提交表单时浏览器会自动添加目标网站的cookies，所以目标网站无法判断这个表单是用户提交的还是恶意网站提交的。要阻止跨源提交，需要使用csrf标记，即csrf token。通常csrf会在服务端生成，伴随着表单页面返回到客户端中，用户提交表单时，csrf token会作为表单中的一个隐藏字段发回服务端，服务端验证通过后才会执行操作，验证不通过会返回403错误。这里的csrf token不会被第三方网站的js代码获得，所以就保证了表单都是由用户提交的。

但是在前后端分离的项目中，页面由js生成，不由后端生成，所以csrf token无法给到客户端，这里的解决方法是服务端将生成的token放在cookies中，同源下的js发送请求时，先从cookies中读取该token，将其加入请求头中或表单字段中(注意这里不是放回到cookies中)，最后发送请求，实现csrf防护。

下面介绍下spring security中具体实现方法

```java
@EnableWebSecurity  
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {  
    @Override  
    protected void configure(HttpSecurity http) throws Exception {   
        http  
            .csrf()  
            .csrfTokenRepository(CookieCsrfTokenRepository.withHttpOnlyFalse());  
    }  
}  
```
- 这里首先使用 `.csrf() ` 启动csrf
- `CookieCsrfTokenRepository` 表示将token存至cookies中
- `withHttpOnlyFalse` 将这条cookies标记为js可读，如果不加这段，js将无法读取保存csrf token的cookie

注意

- csrf token 在服务端保存在session中，如果session的过期时间比较短，可能在用户提交表单时登录状态已经失效了，csrf token也会跟着失效，可以通过提示用户及时刷新页面来更新session的过期时间。
- 不仅是登录后的session，登录页面的匿名session也会过期，如果在登录页面停留过长时间(超过session过期时间)，之后即使输入正确的帐号密码登录也会返回403错误。推荐的方法是在点击登录时再获取token，这样可以保证token的有效性

```java
@RestController  
public class CsrfController {  
    @RequestMapping("/csrf")  
    public CsrfToken csrf(CsrfToken token) {   
    return token;  
    }   
} 
```
- 点击登录时先用js请求 `/csrf` 地址，获取到token后再提交登录表单。
- 为了安全不能把cors功能应用到这个路由上。 
- logout请求也需要带csrf token
- 文件上传时token要加在action地址中

```html
<form action="./upload?${_csrf.parameterName}=${_csrf.token}" method="post" enctype="multipart/form- data">  
```



参考地址

- https://www.iteye.com/blog/fengyilin-2413232