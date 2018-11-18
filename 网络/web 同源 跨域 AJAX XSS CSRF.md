# todo
等个人博客那个网站搭建好后，
新增加两个测试
1. XSS 攻击测试
2. CSRF 攻击测试
我现在不太明白：
如果正常登陆后，又访问了另外一个域名的服务，在这个域名服务下，产生了CSRF，此时cookie是被利用了吗？


# web 同源 跨域、AJAX、XSS CSRF

## 1. 浏览器 同源策略---Same-origin policy
### 1.1. Definition of an origin


### 1.2. Cross-origin network access

参考文献：
[Same-origin policy](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy)

[不要再问我跨域的问题了](https://segmentfault.com/a/1190000015597029)

### 1.3. 为什么需要跨域
### 1.4. 跨域的解决方案


跨域资源共享 CORS 详解
https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS


[HTTP趣谈]支持跨域及相关cookie设置

## 2. AJAX
再也不学AJAX了


## 3. 单点登录(Single sign-on、SSO)





## 4. XSS & CSRF

### 4.1. 名字的起源
> Cross-site scripting (XSS) is a type of computer security vulnerability typically found in web applications. XSS enables attackers to inject client-side scripts into web pages viewed by other users.  
---wikipedia  


Cross-site scripting (XSS) 正常来说简写应该是 CSS ，但是 CSS 这个简称已经被( cascading style sheet )占用，所以简称就变成了 XSS 。

### 4.2. XSS v.s. CSRF
> xss：用户过分信任网站，放任来自浏览器地址栏代表的那个网站代码在自己本地任意执行。如果没有浏览器的安全机制限制，xss代码可以在用户浏览器为所欲为  

> csrf：网站过分信任用户，放任来自所谓通过访问控制机制的代表合法用户的请求执行网站的某个特定功能。  
---[如何用简洁生动的语言说明 XSS 和 CSRF 的区别？](https://www.zhihu.com/question/34445731)  
黄玮 的回答


[用大白话谈谈XSS与CSRF](https://segmentfault.com/a/1190000007059639)
[如何防范XSS和CSRF？](https://segmentfault.com/a/1190000007766732)
