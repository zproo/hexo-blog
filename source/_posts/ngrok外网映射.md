---
title: ngrok外网映射
comment: true
date: 2017-06-20 11:32:50
tags: [ngrok, 外网映射, 微信开发]
---
# 1.为什么需要外网映射

我遇到的主要场景是在微信公众号开发中，因为用户跟微信公众号产生的交互行为，微信会把用户的相关信息推送到我们自己的服务器，而这个推送的前提是微信能够访问到我们的服务，如果服务在本地，那微信当然无法推送给我们，这使得开发功能的时候调试相当麻烦。外网映射就是把本地站点映射出去，解决微信推送给我们的用户信息等消息进行实时本地调试。

# 2.外网映射工具ngrok

>  官方介绍：ngrok is a reverse proxy that creates a secure tunnel from a public endpoint to a locally running web service. ngrok captures and analyzes all traffic over the tunnel for later inspection and replay.
>
>  翻译： ngrok是一个能够将本地的网络服务安全的映射到公共网络的反向代理工具。ngrok捕获和分析所有通过隧道的流量后检查和重放。

# 3. 使用方式

## 3.1 ngrok几个问题

- 目前国内访问该网站提供的服务(服务器在国外)相当不稳定，经常连接不上

- 映射的外网域名是随机的。(想要固定域名需要付费服务)

  ![ScreenClip](http://ortur5wom.bkt.clouddn.com/ngrok0.png)

## 3.2 ngrok国内服务器+固定域名

国内有不少第三方的ngrok服务提供，如natapp、花生壳、Sunny-Ngrok等。这里我们选用Sunny-Ngrok，官网地址：[http://www.ngrok.cc/](http://www.ngrok.cc/)。

**使用步骤：**

1. 进入官网完成注册,登陆后点击 `开通隧道`(免费)

2. 按照下图填写信息。

   ![2214876990875395](http://ortur5wom.bkt.clouddn.com/ngrok1.png)

   点击 `确定添加`，获得`隧道id`。

   ![ScreenClip1](http://ortur5wom.bkt.clouddn.com/ngrok2.png)


3. 官网下载客户端，使用`隧道id`启动已经创建好的隧道。

   ![ScreenClip1](http://ortur5wom.bkt.clouddn.com/ngrok3.png)

   详细步骤地址：[http://www.sunnyos.com/article-show-71.html](http://www.sunnyos.com/article-show-71.html)

