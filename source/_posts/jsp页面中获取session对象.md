---
title: jsp页面中获取session对象
comment: true
date: 2017-06-08 15:10:10
tags: [jsp,session,el表达式]
---
# 1.session的介绍

客户首次访问服务器时，会话session对象被创建并分配一个唯一的Id，同时id号发送到客户端，并存入cookie，使得客户端session对象和服务器端一致。

存储会话信息session供浏览器后续请求使用，可以获取并修改变量的值。和cookie一起使用识别同一个客户。

当用户关闭浏览器时，客户针对当前服务器的session即被关闭或超时失效，当客户再次打开浏览器访问的时候，会重新分配会话session ID。若禁止cookie，，同一个客户就会对应多个session对象，服务器无法识别访问这些页面是同一个客户。

# 2.session和cookie的区别

1. session 在服务器端，cookie 在客户端（浏览器）
2. session 默认被存在在服务器的一个文件里（不是内存）
3. session 的运行依赖 session id，而 session id 是存在 cookie 中的。
4. cookie不是很安全，别人可以分析存放在本地的COOKIE并进行COOKIE欺骗
   考虑到安全应当使用session
5. session会在一定时间内保存在服务器上。当访问增多，会比较占用你服务器的性能
   考虑到减轻服务器性能方面，应当使用COOKIE
6. 单个cookie在客户端的限制是3K，就是说一个站点在客户端存放的COOKIE不能3K。(将登陆信息等重要信息存放为SESSION；其他信息如果需要保留，可以放在COOKIE中)


# 3.前端Jsp页面中获取session对象

## 3.1 方法一（EL表达式）

``` javascript
// 前端获取存储在session对象中的用户id:userId
var studentId = "${sessionScope.userId}";
```



## 3.2 方法二（Jsp语法）

``` javascript
// 前端获取存储在session对象中的用户id:userId
var studentId = "<%=session.getAttribute("userId")%>";
```

## 注意小坑

以上代码必须写在Jsp文件中，在引入外部Js文件时上述代码失效。


