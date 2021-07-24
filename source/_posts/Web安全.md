---
title: Web安全
date: 2021-05-14 11:39:38
tags:
categories:
---

# Web安全

# XSS

## 概念

Cross Site Script，跨站脚本攻击

 XSS 的原理是恶意攻击者往 Web 页面里插入恶意可执行网页脚本代码，当用户浏览该页之时，嵌入其中 Web 里面的脚本代码会被执行，从而可以达到攻击者盗取用户信息或其他侵犯用户安全隐私的目的

例子：

留言板，正常网页需要展示用户的留言，如果留言内容是

```text
<script>alert(“hey!you are attacked”)</script>
```

那么网页解析到这条留言时，就会执行这段js代码

危害：

- **窃取网页浏览中的cookie值**
- **劫持流量实现恶意跳转**

## 分类

- **反射型XSS**：是指xss代码在请求的url中，而后提交到服务器，服务器解析后，XSS代码随着响应内容一起传给客户端进行解析执行。（直接反射显示在页面）
- **存储型XSS**：具有攻击性的脚本被保存到了服务器端（数据库，内存，文件系统）并且可以被普通用户完整的从服务的取得并执行，从而获得了在网络上传播的能力。（这种攻击多见于论坛、博客和留言板，攻击者在发帖的过程中，将恶意脚本连同正常信息一起注入帖子的内容中。随着帖子被服务器存储下来，恶意脚本也永久的被存放在服务器的后端存储器中。当其它用户浏览这个被注入了恶意脚本的帖子时，恶意脚本会在他们的浏览器中得到执行）
- **DOM型XSS**：使用DOM可以使程序和脚本能够动态访问和更新文档的内容、结构以及样式；DOM型XSS是一种特殊类型的反射型XSS，是基于DOM文档对象模型的一种漏洞

## XSS防范

- 编码：对用户输入的数据进行HTML Entity 编码。把字符转换成 转义字符。Encode的作用是将$var等一些字符进行转化，使得浏览器在最终输出结果上是一样的
- 过滤：将用户数据输出到html 标签的属性时，必须经过标签属性的转义。注意：不包含href, src, style和事件处理函数属性（比如onmouseover）
- 校正：避免直接对HTML Entity进行解码。使用DOM Parse转换，校正不配对的DOM标签
- HttpOnly：防止直接拿到cookie



# CSRF

## 概念

Cross-site request forgery 跨站请求伪造

也被称为One Click Attack或者session riding，通常缩写为CSRF或者XSRF

挟制用户在当前已登录的Web应用程序上执行非本意的操作的攻击方法

例子：

1. 用户Alice登录和访问某银行网站A，保留`cookie`。
2. Alice被某些信息诱导访问危险网站B。
3. 危险网站B上有一个`<img>`标签：`<img src="http://www.examplebank.com/account=Alice&amount=1000&payfor=Badman">`
4. 这个标签的src不指向一张图片，而是一个http请求，这个请求向银行要求将Alice的1000元转给Badman，由于Alice的浏览器上有`cookie`，这样浏览器发出的这个请求就能得到响应执行。
5. 这样Alice的钱就被偷了。

# 防范

- 验证码
- 请求来源限制：通过HTTP Referer字段，验证请求来源地址是否合法
- token：服务端发送token，前端解析出token放到请求中，服务端验证token是否正确