---
title: JsWeb框架手册
comment: true
date: 2018-03-10 17:07:24
tags: [Java, JavaWeb, JsWeb]
---

# 第一章 创建工程

## 1.1 创建工程

工程得创建分为两种方式：第一种，从空工程开始创建。第二种，通过intellij idea得项目模板来创建。使用第二种方式创建得时候需要使用网络，由于idea得服务器在国外，所以速度相对比较慢。本节介绍的是第一种方法。

 

第一种方法所需的条件：1.类库文件，2.JsWeb框架的js服务端类库。这些文件在公司的资料共享服务器中。

 

文件-新建-工程，不要选任何选项，点击下一步。创建一个普通的java工程。

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps685C.tmp.jpg) 

输入工程名字

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps685D.tmp.jpg) 

新建完成后工程目录如下：

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps686D.tmp.jpg) 

创建一个lib目录

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps686E.tmp.jpg) 

将类库文件复制到工程中

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps686F.tmp.jpg) 

复制完成后，右键add as library

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps6880.tmp.jpg) 

Library的名字和文件夹的名字保持一致。

 

添加完成后，打开工程属性

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps6881.tmp.jpg) 

 

打开工程属性 添加 facets，选择spring，然后选择web，点击 create artifact

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps6892.tmp.jpg) 

将工程右侧的libraries都添加到左侧output目录里面，如下图

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps6893.tmp.jpg) 

Problems的数量为0.

目前，IDE应该已经自动创建了Web目录和Web.xml文件。

将 配置文件和js类库文件复制到工程中

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps68A3.tmp.jpg) 

复制完成后如下图：

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps68A4.tmp.jpg) 

将JspPageController类复制到Src目录下的如下位置，若包不存在请创建

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps68A5.tmp.jpg) 

## 1.2 工程配置

在JSAPI.xml文件中 进行项目的配置

 

连接字符串的配置:

修改driverclassname,jdbcurl,username,password 默认线程池大小：最小10，最大100

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps68B6.tmp.jpg) 

常见配置方案：

1. MySql

2. Oracle

3. SqlServer





Jsweb扫描器：

配置js服务接口根目录

配置js服务端类库跟目录

配置js服务端自定义类库跟目录

配置licensekey

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps68C6.tmp.jpg) 

 

映射和拦截器：

Jsweb接口的基础路径是 /jsons 所有/jsons/*.form的请求都会由jsapicontroller来处理。

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps68D7.tmp.jpg) 

## 1.3 工程结构

工程建立完成后目录结构如下：

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps68D8.tmp.jpg) 

src文件夹用于存放java代码文件

jsapis用于存放jsweb的服务端接口文件

jscustomlibs 用于存放与当前工程相关的函数库文件

jslibs 用于存放通用函数库文件

pages用于存放jsp文件

web目录下用于存放网站的静态资源

applicationContext.xml 为工程中的spring配置文件

dispatcher-servlet.xml 为系统spring配置文件

jsapi.xml jsweb配置文件

web.xml web配置文件

# 第二章 JsWeb框架的简单介绍

## 2.1 框架的工作原理

目前项目的开发框架为：Spring+SpringMVC+JsWeb框架。JsWeb框架本身依赖于Spring 和 Spring MVC框架。框架本身兼容其他第三方框架，JsWeb框架通过Controller与Spring MVC项目相结合，通过实现Controller接口从Spring MVC获取Request、Repsonse、ModelAndView、SevletSession等对象。JsApiController通过调用编译后的JS对请求进行处理。工作原理如图2-1。框架本身是基于SpringMVC框架扩展，与其他框架不产生冲突。

 

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps68E9.tmp.png)

图2-1 JsWeb框架工作原理图

为了能快速的开发服务端接口，开发了JSWEB框架——一个可以使用javascript脚本来开发服务端数据接口的框架。

### 框架优势

JSWEB框架相比传统的Java服务端框架具有以下优势：

- 无model层，当数据库发生变更时无需重新写model类，而对于某些返回数据（由表链接产生，不存在于model层）无需创建大量的model类。
- 基于实体关系的sql生成类库，可动态生成sql。使用类似于mongodb的语法 操纵表连接查询，无需创建大量视图，无惧数据库字段增加删除更改。
- 可直接操作WEB底层（Session,Request,Response,Header,Status等）
- 可灵活扩展第三方插件（支付、快递、短信、邮件等）

## 2.2 框架功能介绍

框架针对Web开发中所遇到的各类问题，封装了相应的函数。下面简单介绍下各个函数库中各个函数库中函数的功能。

### 2.2.1 codec函数库

codec函数库用于做字符串编码，目前函数库里的函数主要用于生成字符串的摘要。主要应用于密码保存。主要函数及功能见表2-1。

| 函数名    | 功能                                           | 参数                                           |
| --------- | ---------------------------------------------- | ---------------------------------------------- |
| md5hex    | 求解字符串的MD5值，输出为16进制表示的字符串    | 1.str 输入的字符串return  16进制表示的byte数组 |
| sha1hex   | 求解字符串的sha1值，输出为16进制表示的字符串   | 1.str 输入的字符串return  16进制表示的byte数组 |
| sha256hex | 求解字符串的sha256值，输出为16进制表示的字符串 | 1.str 输入的字符串return  16进制表示的byte数组 |
| sha384hex | 求解字符串的sha384值，输出为16进制表示的字符串 | 1.str 输入的字符串return  16进制表示的byte数组 |
| sha512hex | 求解字符串的sha512值，输出为16进制表示的字符串 | 1.str 输入的字符串return  16进制表示的byte数组 |

表2-1 codec函数库的主要函数

### 2.2.2 email函数库

email函数库用于邮件的发送和读取。函数库可应用于项目中的用户注册、用户激活账户、邮件找回密码、消息通知、广告推广等模块。主要函数及功能见表2-2。

| 函数名       | 功能               | 参数                                                         |
| ------------ | ------------------ | ------------------------------------------------------------ |
| sendhtmlmail | 发送html富文本邮件 | 1.sender发件人姓名2.senderEmail 发件人邮箱地址3.receiver 收件人姓名4. receiverEmail 收件人邮箱地址5.stmpserver 发件人邮箱的STMP服务器地址6. smtpport STMP服务器的端口7.username发件人邮箱的用户名8.password 发件人的邮箱的密码9.subject邮件主题10.content 邮件内容（html）11.attachementFiles 附件列表（文件地址列表） |
| sendtextmail | 发送纯文本邮件     | 1.sender发件人姓名2.senderEmail 发件人邮箱地址3.receiver 收件人姓名4. receiverEmail 收件人邮箱地址5.stmpserver 发件人邮箱的STMP服务器地址6. smtpport STMP服务器的端口7.username发件人邮箱的用户名8.password 发件人的邮箱的密码9.subject邮件主题10.content 邮件内容（文本）11.attachementFiles 附件列表（文件地址列表） |

表2-2 email函数库的主要函数

### 2.2.3 fileutils函数库

fileutils函数库用于文件操作。函数库可以用于管理服务器端的文件，比如清理用户上传的临时文件。也可以读取服务器端的文本文件。主要函数及功能见表2-3。

| 函数名        | 功能                               | 参数                                                         |
| ------------- | ---------------------------------- | ------------------------------------------------------------ |
| createfile    | 创建文件                           | 1. filepath 需要创建的文件的路径，绝对路径return true/false 成功/失败 |
| listfiles     | 列出目录下的所有文件               | 1.dir 目录的绝对路径return 字符串列表，用于保存目录下所有文件的数组 |
| listfileitems | 列出目录下的所有文件，包括文件属性 | 1.dir目录的绝对路径return 目录下的所有文件的列表，列表中保存了每个文件的详细信息。 |
| searchFiles   | 搜索目录下的文件，包括子目录       | 1.dir目录的绝对路径2.keyword 搜素关键字return 字符串列表，用于保存目录下所有文件的数组 |
| mkdir         | 创建目录                           | 1.dir目录的绝对路径return true/false 成功/失败               |
| deletefile    | 删除文件或文件夹                   | 1. fileordir文件或目录的绝对路径return true/false 成功/失败  |
| readfile      | 读取文本文件的内容                 | 1. filepath需要读取的文件的绝对路径2.encoding 文件的编码，默认为utf-8return 文件中的文本内容 |
| writefile     | 将文本写入文本文件                 | 1. filepath需要读取的文件的绝对路径2.content 要写入的文件内容3.encoding 文件的编码，默认为utf-8return true/false 成功/失败 |

表2-3 fileutils函数库的主要函数

### 2.2.4 image函数库

image函数库用于图像处理相关的应用，基本功能包括生成数字验证码。图片调整大小，获取图片的大小等。主要函数及功能见表2-4。

| 函数名                  | 功能                               | 参数                                                         |
| ----------------------- | ---------------------------------- | ------------------------------------------------------------ |
| generatenumbercheckcode | 生成4位数验证码                    | 1.context jsweb上下文2.sessionkey 存储验证码的session字段return 生成的4位数字验证码 |
| resizeimage2file        | 将图片进行缩放操作，输出到本地文件 | 1.imagefilepath 图片文件的路径2.targetfilepath目标文件的路径3.w处理后图片的宽度4.h处理后图片的高度return true/false 成功/失败 |
| resizeimage2response    | 将图片进行缩放操作，输出到response | 1.context jsweb上下文2.imagefilepath 图片文件的路径3.w处理后图片的宽度4.h处理后图片的高度return true/false 成功/失败 |
| imagesize               | 获取图片得大小                     | 1. imagepath 图片文件的路径return 图片的大小{w,h}            |

表2-4 image函数库的主要函数

### 2.2.5 net函数库

net函数库中包含了与网络相关的函数，函数库的功能包括两大类：1.负责给用户端提供上传、下载服务。2.给其他服务端发送get/post请求。主要函数及功能见表2-5。

| 函数名             | 功能                   | 参数                                                         |
| ------------------ | ---------------------- | ------------------------------------------------------------ |
| uploadfileserver   | 文件上传服务           | 1.context jsweb上下文2.path 上传文件保存位置3.isrelative 是否是相对路径 |
| downloadfileserver | 文件下载服务           | 1.context jsweb上下文2.targetfilepath要下载的文件的路径3.isrelative 路径是否是相对路径4.浏览器下载时的文件名5.输出的content-type |
| postform           | post form数据到某url   | 1.url 远程服务端接口url2.formData 数据，格式为object3.headers http头，格式为keyvalue4.encoding 发送和接收编码，默认为utf-8return 返回{status,content,responseduration} |
| poststring         | post string数据到某url | 1.url 远程服务端接口url2.str 要发送的字符串3.headers http头，格式为keyvalue4.encoding 发送和接收编码，默认为utf-8return 返回{status,content,responseduration} |
| postfile           | post 文件到远程服务器  |                                                              |
| getform            | get 数据从某url        | 1.url 远程服务端接口url2.formData 数据，格式为object3.headers http头，格式为keyvalue4.encoding 发送和接收编码，默认为utf-8return 返回{status,content,responseduration} |
| downloadfile       | 下载远程服务器上的文件 | 1.url 远程文件的url2.method 下载方法get/post3.formdata 发送的form数据4.headers http头，格式为keyvalue5.encoding 发送和接收编码，默认为utf-86.targetfilepath 保存在本地的文件路径，绝对路径return 返回{status,content,responseduration} |

表2-5 net函数库的主要函数

 

### 2.2.6 runtime函数库

runtime函数库中，包含了与运行时相关的函数。主要函数及功能见表2-6。

| 函数名          | 功能                                                         | 参数                                                 |
| --------------- | ------------------------------------------------------------ | ---------------------------------------------------- |
| rumtimeexec     | 执行本地可执行文件                                           | 1.path 可执行文件的路径2.startuppath 执行的开始目录  |
| runinbackground | 创建一个新线程执行一个任务。线程从线程池中创建，如果线程池使用枯竭，则需要等待空闲线程。 | 1.context jsweb上下文2.apiname 要执行的任务的api名称 |
| sleep           | 睡眠，停止当前线程一定时间。                                 | 1.miliseconds 睡眠的毫秒数                           |

表2-6 runtime函数库的主要函数

### 2.2.7 servlet函数库

servlet函数库中包含了与request、response、session相关的处理函数。主要函数及功能见表2-7。

| 函数名              | 功能                       | 参数                                                         |
| ------------------- | -------------------------- | ------------------------------------------------------------ |
| httpstatus          | 设置response的状态值       | 1.context jsweb上下文2.status 状态码，数值类型               |
| contenttype         | 设置response的相应内容     | 1.context jsweb上下文2.contenttype0 content-type             |
| setsession          | 设置session的值            | 1.context jsweb上下文2.key session的键3.value session的值    |
| getsession          | 获取session的值            | 1.context jsweb上下文2.key session的键return session的值     |
| setheader           | 设置header的值             | 1.context jsweb上下文2.key session的键3.value header的值     |
| redirect            | 访问重定向                 | 1.context jsweb上下文2.url 重定向的url地址                   |
| getheader           | 获取header的值             | 1.context jsweb上下文2.key session的键                       |
| setcontenttype      | 设置response的相应内容     | 1.context jsweb上下文2.contenttype0 content-type             |
| setview             | 设置modelandview中的view   | 1.context jsweb上下文2.viewname 视图路径，和spring设置方法一致 |
| rootphysicpath      | 获取网站的绝对路径         | 1.context jsweb上下文2.path 网站根目录的相对路径return 字符串，绝对路径 |
| contextpath         | 获取contextpath            | 1.context jsweb上下文return 字符串，访问的contextpath        |
| writeresponsestream | 写入response输出流         | 1.context jsweb上下文2.inputstream 输入流return true/false 成功/失败 |
| getresponse         | 获取response对象           | 1.context jsweb上下文return response                         |
| getresponsestream   | 获取response输出流         | 1.context jsweb上下文return outputstream                     |
| setresponsefilename | 设置response中输出的文件名 | 1.context jsweb上下文2.filename 输出显示的文件名             |
| getmethod           | 获取当前访问的方式         | 1.context jsweb上下文return 字符串,get/set                   |

表2-7 servlet函数库的主要函数

### 2.2.8 utils函数库

utils工具函数库，主要功能为开发中常见的工具，比如uuid、md5、base64、json序列化反序列化、向内存中获取/保存对象，路径合并等。主要函数及功能见表2-8。

 

| 函数名             | 功能                                          | 参数                                                  |
| ------------------ | --------------------------------------------- | ----------------------------------------------------- |
| uuid               | 生成一个UUID                                  | 无                                                    |
| md5                | 计算输入字符串的md5，输出为base64编码的字符串 | 1.input 输入字符串return 返回摘要字符串               |
| base64             | base64编码函数                                | 1.input 输入字符串return 编码后的字符串               |
| jsonStringify      | 对象的json序列化                              | 1.obj 要进行序列化的对象return 序列化之后的json字符串 |
| jsonParse          | 对象反序列化                                  | 1.jsonstr json字符串return 反序列化后的对象           |
| getmemcache        | 获取运行jsweb服务器里面的缓存对象             | 1.key 对象的名称return 缓存的对象                     |
| putmemcache        | 将对象放入运行jsweb服务器里面的缓存           | 1.key 对象的名称2.value 要缓存的对象                  |
| ismemcachecontains | 判断缓存中是否包含对象的名称                  | 1.key 对象的名称return true/false 成功/失败           |
| console            | 从控制台中输出对象                            | 1.str 要输出的对象                                    |
| threadid           | 获取当前线程的线程id                          | return 线程id，数字                                   |
| createlist         | 创建java的list                                | return list                                           |
| createsafemap      | 创建java中的map                               | return map                                            |
| pathcombine        | 合并路径                                      | 无限参数，路径return 合并后的路径                     |

表2-8 servlet函数库的主要函数

### 2.2.9 compress函数库

compress函数库用于在服务端创建压缩文件。目前功能是可以把文件夹中的文件压缩为一个压缩文件，支持的格式为zip。主要函数及功能见表2-9。

| 函数名             | 功能                                      | 参数                                                         |
| ------------------ | ----------------------------------------- | ------------------------------------------------------------ |
| zipfolder          | 将文件夹压缩为一个zip文件，保存到文件     | 1.rootfolder 要压缩的根目录2.extlist 后缀列表3.outfile 输出文件的路径 |
| zipfolder2response | 将文件夹压缩为一个zip文件，输出到response | 1.context jsweb上下文2.rootfolder 要压缩的根目录3.extlist 后缀列表 |

表2-9 compress函数库的主要函数

应用场景：

1.需要批量下载文件的需求，如：沐诺农场批量下载二维码

2.需要下载的文件过大或数量过多，如：下载整年的统计表

### 2.2.10 sqlbase函数库

sqlbase函数库中包含了sql查询的基本函数。该函数库是jsweb项目中最重要的函数库，在一个项目中该函数库会被高频的使用。主要的函数及功能见表2-10。

| 函数名           | 功能                                                    | 参数                                                         |
| ---------------- | ------------------------------------------------------- | ------------------------------------------------------------ |
| query            | 查询sql并返回查询结果，查询结果为数组                   | 1.fn 查询语句（func注释或字符串格式）2.params sql查询上下文3.mappingstr 映射字符串return 返回查询结果 |
| querypagedata    | 分页查询sql并返回查询结果，查询结果为数组               | 1.fn 查询语句（func注释或字符串格式）2.params sql查询上下文3.mappingstr 映射字符串4.pagenum 当前的页码，从1开始5.pagesize 每页记录的数量return 返回分页查询结果 |
| multiquery       | 查询sql并返回查询结果，查询结果为数组，需要传入jdbc连接 | 1.con jdbc连接2.fn 查询语句（func注释或字符串格式）3.params sql查询上下文4.mappingstr 映射字符串return 返回查询结果 |
| exec             | 执行sql语句。                                           | 1.fn 查询语句（func注释或字符串格式）2.params sql查询上下文return true/false 成功/失败 |
| multiexec        | 执行sql语句，需要传入jdbc连接                           | 1.con jdbc连接2.fn 查询语句（func注释或字符串格式）3.params sql查询上下文return true/false 成功/失败 |
| execbatch        | 批量执行sql语句。                                       | 1.fn 查询语句（func注释或字符串格式）2.datalist 数据列表return true/false 成功/失败 |
| multiexecbatch   | 批量执行sql语句，需要传入jdbc连接                       | 1.con jdbc连接2.fn 查询语句（func注释或字符串格式）3.datalist 数据列表return true/false 成功/失败 |
| createconnection | 创建jdbc连接                                            | 无                                                           |
| closeconnection  | 关闭jdbc连接                                            | 1.con jdbc连接                                               |
| commit           | 提交连接中的数据                                        | 1.con jdbc连接return true/false 成功/失败                    |
| rollback         | 回滚连接中的数据                                        | 1.con jdbc连接return true/false 成功/失败                    |

表2-10 sqlbase函数库的主要函数

### 2.2.11 redissql函数库

redissql 为利用redis内存数据库，对接口的数据进行缓存，从而提高实时性不高的接口的访问性能。提升后的性能接近静态资源访问速度。主要的函数及功能见表2-11。

| 函数名             | 功能                              | 参数                                                         |
| ------------------ | --------------------------------- | ------------------------------------------------------------ |
| redisquery         | 基于redis内存数据的缓存查询。     | 1.fn 查询语句（func注释或字符串格式）2.params sql查询上下文3.mappingstr 映射字符串4. expire 设置超时时间，毫秒return 返回查询结果 |
| redisquerypagedata | 基于redis内存数据的缓存分页查询。 | 1.fn 查询语句（func注释或字符串格式）2.params sql查询上下文3.mappingstr 映射字符串4.pagenum 当前的页码，从1开始5. expire 设置超时时间，毫秒return 返回分页查询结果 |

表2-11 redissql函数库的主要函数

应用场景：

1. 高频且对数据实时性不高的接口，如商城主页的商品列表接口、商品品类接口、商店店铺页面的商品品类和 商品接口。

### 2.2.12 dber/dberjs函数库

dber/dberjs函数库是一个全新的基于实体关系进行数据查询和插入的函数库。与传统的sql不用，基于实体关系的查询根据数据表中预定义的连接关系自动生成查询的sql语句。实体关系查询提供了几种查询源语：query filter search orderby。除了查询功能外还提供基于实体的删除、更新和插入功能。主要的函数及功能见表2-12，2-13。

| 函数名                    | 功能                                                         | 参数                                                         |
| ------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| dbcontext().query         | 根据用户输入的实体结构字符串进行查询                         | 1.structurestr 实体结构字符串                                |
| dbcontext().[filter]()    | 根据过滤条件进行过滤                                         | 1.filterstr 过滤字符串                                       |
| dbcontext().search        | 综合搜索，当用户没有提交搜索变量的值的时候不进行该条件搜索，否则进行搜索。通常用于多条件搜索。 | 1.varname 搜索变量名2.condition 过滤字符串                   |
| dbcontext().orderby       | 排序                                                         | 1.orderbystr 排序字符串                                      |
| dbcontext().buildsqlquery | 构建sql字符串和映射字符串                                    | return 返回{sql,mappingstr}                                  |
| initmysqldb               | 初始化mysql数据库的实体关系                                  | 数据库schema名称                                             |
| db.relation               | 预定义实体与实体的关系                                       | 1.modelpropertystr 实体属性字符串2.modelconnectorstr 实体连接字符串3.relationtype 实体关系，1:1 1:n |

表2-12 dber函数库的主要函数

| 函数名                    | 功能                             | 参数                                                         |
| ------------------------- | -------------------------------- | ------------------------------------------------------------ |
| deleteentity              | 删除数据库中实体                 | 1.entityname 实体名称2.entitydata 实体的对象3.wherestr where查询子句，用于定位删除规则，默认为nullreturn {result,errormessage} |
| updateentity              | 更新数据库中的实体               | 1.entityname 实体名称2.entitydata 实体的对象3.wherestr where查询子句，用于定位删除规则，默认为nullreturn {result,errormessage} |
| insertentity              | 将实体对象插入数据库             | 1.entityname 实体名称2.entitydata 实体的对象3.isuserpk 是否使用entitydata中的主键，默认为null4. where查询子句，用于定位删除规则，默认为nullreturn {result,errormessage} |
| multideleteentity         | 删除数据库中实体，带传入连接     | 1.con jdbc连接2.entityname 实体名称3.entitydata 实体的对象4.wherestr where查询子句，用于定位删除规则，默认为nullreturn {result,errormessage} |
| multiupdateentity         | 更新数据库中的实体，带传入连接   | 1.con jdbc连接2.entityname 实体名称3.entitydata 实体的对象4.wherestr where查询子句，用于定位删除规则，默认为nullreturn {result,errormessage} |
| multiinsertentity         | 将实体对象插入数据库，带传入连接 | 1.con jdbc连接2.entityname 实体名称3.entitydata 实体的对象4.isuserpk 是否使用entitydata中的主键，默认为null5. where查询子句，用于定位删除规则，默认为nullreturn {result,errormessage} |
| updateentitybatch         | 批量更新实体                     | 1.con jdbc连接2.entityname 实体名称3.entitylistdata 实体对象列表4.wherestr where查询子句，用于定位删除规则，默认为nullreturn {result,errormessage} |
| insertentitybatch         | 批量插入实体                     | 1.con jdbc连接2.entityname 实体名称3.entitylistdata 实体对象列表4.isuserpk 是否使用entitydata中的主键，默认为null5. where查询子句，用于定位删除规则，默认为nullreturn {result,errormessage} |
| insertorupdateentity      | 插入或更新实体                   | 1.entityname 实体名称2.entitydata 实体的对象return {result,errormessage} |
| multiinsertorupdateentity | 插入或更新实体，带传入连接       | 1.con jdbc连接2.entityname 实体名称3.entitydata 实体的对象return {result,errormessage} |
| insertorupdateentitybatch | 批量插入或更新实体               | 1.con jdbc连接2.entityname 实体名称3.entitylistdata 实体对象列表return {result,errormessage} |

表2-13 dberjs函数库的主要函数

 

### 2.2.13 text函数库

text函数库是一个文本计算的科学函数库。主要功能为计算文本之间的相似性，提供多种相似性衡量标准，计算文本的最长公共子串。主要的函数及功能见表2-14。

| 函数名                              | 功能                    | 参数                    |
| ----------------------------------- | ----------------------- | ----------------------- |
| textjaccardsimilarity               | 计算文本的jaccard相似性 | 1.str12.str2return 数值 |
| textlongestcommonsubsequnce         | 计算最长公共子串（LCS） | 1.str12.str2return 数值 |
| textlongestcommonsubsequncedistance | 计算最长公共子串相似性  | 1.str12.str2return 数值 |
| texthammingdistance                 | 计算汉明距离            | 1.str12.str2return 数值 |
| textfuzzyscore                      | 计算模糊值              | 1.str12.str2return 数值 |

表2-14 text函数库的主要函数

函数库的可应用场景举例：

1.在输入提示框中显示提示，如果使用子串搜索方法，用户输入错误将导致提示消失。最相似方法 可以容许一定的错误。

2.差异比较，比如比较两段文本 完全相同的部分。

### 2.2.14 velocity函数库

velocity函数库的主要功能是提供模板渲染功能，目前渲染的模板引擎是velocity。主要的函数及功能见表2-15。

| 函数名           | 功能                           | 参数                                      |
| ---------------- | ------------------------------ | ----------------------------------------- |
| velocitytemplate | 将用户输入的模板字符串进行渲染 | 1.tempatestr2.paramsreturn 渲染后的字符串 |

表2-15 velocity函数库的主要函数

应用场景举例：

1. 发送验证码短信

2. 发送激活账号邮件

3. 找回密码邮件/短信

4. 业务通知类短信/邮件/微信客服消息

### 2.2.15 excel函数库

excel函数库主要功能是提供excel操作的相关函数。目前使用的excel文件操作类库是poi。主要的函数及功能见表2-16。

| 函数名                       | 功能                          | 参数                                                         |
| ---------------------------- | ----------------------------- | ------------------------------------------------------------ |
| exportexceltemplate2file     | 导出excel模板文件到指定的路径 | 1.datacontext 数据上下文2.templatefilepath excel模板的路径3.outfilepath 输出文件路径 |
| exportexceltemplate2response | 导出excel模板文件到response   | 1.context jsweb上下文2.datacontext 数据上下文3.templatefilepath excel模板的路径4.outfilename 输出文件名 |
| readexcelrows                | 读取excel文件的部分内容       | 1.excelfilepath excel文件路径2.sheetindex sheet的index3.column 列号，从0开始4.start 起始行号5.end 终止行号return 数组 |
| readexcelcolumns             | 读取excel文件的部分内容       | 1.excelfilepath excel文件路径2.sheetindex sheet的index3.rownumber 行号，从0开始4.start 起始列号5.end 终止列号return 数组 |

表2-16 excel函数库的主要函数

应用场景举例：

1.导出店铺订单

2.导出用户列表

3.批量添加数据

4.导出统计表，带筛选功能，富文本等

# 第三章 JsWeb框架的用法



## 3.1 基本sql查询

数据库查询在传统的web项目中是一个很常见的需求。传统的方法是通过orm框架（如mybatis）将resultset中的数据映射为对象。在JSWeb框架中，方法也是类似的。但是并不需要定义映射对象，也不需要复杂的映射配置。

 

数据库：shunfengtest 本地版，在资料中有，在运行本教程中的例子之前需要先部署好数据库。

设置jsapi.xml文件中的 jdbcUrl为：

jdbc:mysql://localhost/shunfengtest?allowMultiQueries=true

shunfengtest数据库简介：shunfengtest数据库是顺丰通服务的数据库，顺丰通系统中包含一个商城系统和一个外卖系统，商城功能中除了正常的商城功能外，还包含一个三级代理功能，即普通用户通过身份认证后，可以代理商家或一二级代理商的商品，当商品通过代理人的分享售卖后，代理人以及代理人的上级可得到代理分红。系统中表sp开头的的是商城的数据表，rs开头的是外卖的数据表，cm是公用数据表，as是代理数据表，ed是快递的数据表，sys是系统数据表。

 

### 场景1

系统管理员需要管理在商城中注册的商家，所需要获取全部的商家列表。需要显示的数据包括sp_shop表中的 shopname、shopimg、shopshortdescription、shopownername、商店所在的省市区字符串和详细地址。

 

sql语句：

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps6909.tmp.jpg) 

jsweb代码：详见jsapis/jsons/case_2_3_1.js

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps690A.tmp.jpg) 

返回的是json数组。

如果sql语句是多行的 ，用“”或‘’则比较麻烦。可以用函数注释方法进行多行字符串输入。（仅限sqlbase函数库）。

当查询无参数且无需改变默认映射的时候，后面两个参数可以省略。

[{shopid,shopname,shop…..}, {shopid,shopname,shop…..},….]

 

### 场景2

在管理员使用的管理系统中，管理员需要查看各个商家上架的商品。同时，普通用户也需要查看某个商家店铺内的所有商品。因此，需要一个获取某商家全部商品列表的接口。接口需要传入shopid，接口返回该shop内所有商品的列表。

 

sql语句：

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps6949.tmp.jpg) 

jsweb代码：详见jsapis/jsons/case2_3_2.js

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps695A.tmp.jpg) 

测试数据：

shopid=[006e0aca-0aae-11e7-ab6a-00ac2794c53f]()

本接口没有设置映射，默认映射按照数据库表定义进行字段的映射。

sql语句中存在一个变量${shopid},所以在query的第二个参数中构造了一个包含shopid的对象。如果context中包含该字段可以直接传入context作为query函数的第二个参数。

 

### 场景3

在管理员的管理界面中，需要以一个商家商品树，列出全部商家和商家下的全部商品，并显示在easyui的tree中。对于sp_shop表仅需要字段 shopid、shopname，对于商品表sp_good仅需要goodid，goodname即可。返回的json格式应满足easyui中的tree的输入格式。

 

tree的输入格式

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps695B.tmp.jpg) 

sql语句：

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps695C.tmp.jpg) 

[jsweb代码：详见jsapis]()/jsons/case2_3_3.js

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps696D.tmp.jpg) 

输出的部分结果：

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps696E.tmp.jpg) 

### 场景4

管理员或者某商店用户查看所有或者商店对应的商品时，后台easyui框架中通常是使用分页,而不是直接把所有的数据展示出来。接口需要传入两个参数，page和rows。page代表当前是第几页，rows代表每页显示的行数。返回的json格式为rows：当前页的数据，total：一共多少行。

 

结果的返回格式：

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps697E.tmp.jpg) 

sql语句：

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps697F.tmp.jpg) 

jsweb代码：详见jsapis/jsons/case2_3_4.js

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps6990.tmp.jpg) 

 

### 场景5

基于实体关系的查询，使用实体关系查询不需要自己写sql语句，基本的查询语句这样写，首先需要调用dbcontext()方法，然后调用其query方法,参数为数据库名或结构化字符串，然后调用buildsqlquery()方法，最后在query方法中传入buildsqlquery()方法返回值对应的两个参数即可。对于场景1，显示全部的属性，使用实体关系查询。

 

jsweb代码：详见jsapis/jsons/case2_3_5.js

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps6991.tmp.jpg) 

.buildsqlquery()方法返回了其创建的映射关系q.mappingstr，若想使用自己的映射关系将q.mappingstr改为自己想要的映射关系即可。

 

### 场景6

基于场景2，管理员查看某个商家的商品，然后管理员还想要通过商品名进行检索，比如商品产地为沈阳（商品产地为商品名第一项），这种时候书写sql语句就需要使用like关键词。使用实体关系查询就需要使用.filter()方法和.search()方法，具体使用方法见[2.2.12](#_2.2.12_dber/dberjs函数库)。

 

[jsweb代码：详见jsapis]()/jsons/case2_3_6.js

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps6992.tmp.jpg) 

测试数据：

shopid=006e0aca-0aae-11e7-ab6a-00ac2794c53f

keyword=沈阳

 

### 场景7

项目上线后，商城的主页会经常被访问，为了提高性能可以使用内存数据库查询，具体介绍见[2.2.11](#_2.2.11_redissql函数库)。

 

jsweb代码：详见jsapis/jsons/case2_3_7.js

 

 

 

 

 

## 3.2 基本sql操作

数据库操作（插入、更新、删除）在传统的web项目中同样也是一个很常见的需求。传统的方法是通过orm框架（如mybatis）将resultset中的数据映射为对象。在JSWeb框架中，方法也是类似的。但是并不需要定义映射对象，也不需要复杂的映射配置。

 

数据库介绍和配置同2.3。

 

### 场景1

商家需要管理他们商店的商品，首先是上线新的商品，这里就需要向数据库表sp_good表中插入一条数据，需要的字段有goodid、goodname、[gooddetaildescription]()、goodminprice、goodmaxprice、shopid等。

 

sql语句：

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps69A3.tmp.jpg) 

jsweb代码：详见jsapis/jsons/case_2_4_1.js

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps69A4.tmp.jpg) 

测试数据：

goodname：‘大连|樱桃’、[gooddetaildescription]()：‘大连大樱桃’、gooddetaildescription：15、

goodmaxprice：20、goodmaxprice：lirf、goodcategory：水果、

goodcategory：006e0aca-0aae-11e7-ab6a-00ac2794c53f、goodorder：1、goodisputway：1、

goodisputway：2017-5-31、goodisremind：1

返回的是true或false，成功为true，失败为false。

 

### 场景2

在一段时间后，商家可能需要修改商品信息，比如修改商品详情，这样情况下需要修改产品表中的数据。比如根据goodid修改gooddetaildescription字段。

 

sql语句：

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps69B4.tmp.jpg) 

jsweb代码：详见jsapis/jsons/case_2_4_2.js

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps69B5.tmp.jpg) 

测试数据：

goodid: f539819c-45cf-11e7-9b1c-38d547ab4abf

gooddetaildescription:’大连大樱桃,好吃又便宜’

 

### 场景3

商家管理商品，当商家不在卖某个商品时，需要将其删除，根据这个商品的id将其在数据库表中删除。

 

sql语句：

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps69C6.tmp.jpg) 

jsweb代码：详见jsapis/jsons/case_2_4_3.js

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps69C7.tmp.jpg) 

测试数据：

goodid: f539819c-45cf-11e7-9b1c-38d547ab4abf

 

### 场景4

使用实体关系向表中插入数据。方法为insertentity()方法,参数为：第一个为表名、第二个为参数。

 

jsweb代码：详见jsapis/jsons/case_2_4_4.js

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps69C8.tmp.jpg) 

测试数据：

goodname：‘大连|樱桃’、gooddetaildescription：‘大连大樱桃’、gooddetaildescription：15、

goodmaxprice：20、goodmaxprice：lirf、goodcategory：水果、

goodcategory：006e0aca-0aae-11e7-ab6a-00ac2794c53f、goodorder：1、goodisputway：1、

goodisputway：2017-5-31、goodisremind：1

返回的是json格式，执行结果（true|false）和执行的sql语句。

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps69D8.tmp.jpg) 

 

### 场景5

[使用实体关系更新数据库表中的数据]()，方法为updateentity()，参数为：第一个为表名、第二个为参数，第三个为更新的条件。

 

[jsweb代码：详见]()jsapis/jsons/case_2_4_5.js

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps69D9.tmp.jpg) 

[测试数据]()：

goodid: f539819c-45cf-11e7-9b1c-38d547ab4abf

gooddetaildescription:’大连大樱桃,好吃又便宜’

返回的是json格式，执行结果（true|false）和执行的sql语句。

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps69EA.tmp.jpg) 

 

### 场景6

使用实体关系删除数据库表中的数据，方法为updateentity()，参数为：第一个为表名、第二个为实体(删除数据的依据，主要是id)，第三个为更新的条件(可不填)。

 

jsweb代码：详见jsapis/jsons/case_2_4_6.js

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps69EB.tmp.jpg) 

测试数据：

goodname:"大连|樱桃";

gooddetaildescription:"大连大樱桃";

goodminprice:"15";

goodmaxprice:"20";

goodcreator:"lirf";

goodcategory:"水果";

shopid:"ff45459f-45d3-11e7-9b1c-38d547ab4abf";

goodid:"ff45459f-45d3-11e7-9b1c-38d547ab4abf";

goodorder:"1";

goodisputway:"1";

goodproducedate:"2017-5-31";

goodisremind:"1";

返回的是json格式，执行结果（true|false）和执行的sql语句。

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps69FC.tmp.jpg) 

## 3.3 sql事务操作

在对数据库进行插入、更新、删除等操作时，经常遇到同时执行多条sql语句的情况，这种情况下就需要使用事务操作。在mybatis中支持事务操作，只需要在方法前加上注解就可以，在JSWEB框架中，事务操作有其特定的方法，下面来看几个例子。

 

### 场景1

用户提交订单之后进行支付，支付完成之后需要将订单状态改为已支付，同时需要在支付记录中插入一条新的数据。这种情况下需要使用事务来完成，在框架中使用multiexec()方法来完成，具体的参数及使用方法见[2.2.10](#_2.2.10_sqlbase函数库)。

 

sql语句：

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps69FD.tmp.jpg) 

jsweb代码：详见jsapis/jsons/case_2_5_1.js

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps69FE.tmp.jpg) 

测试数据：

orderpayedprice：100

ordernumber：CP1704281613263536

 

### 场景2

场景同上，使用实体关系进行事务操作，需要用到的方法为multideleteentity()、multiupdateentity()、multiinsertentity()，详情见[2.2.12](#_2.2.12_dber/dberjs函数库)。

 

jsweb代码：详见jsapis/jsons/case_2_5_2.js

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps6A0E.tmp.jpg) 

 

## 3.4 实体关系查询

在前面介绍了实体关系查询的基本用法，但是实体关系查询最实用的是当两个或多个表关联查询时可以用比较少的代码来实现。下边来看几个场景。

### 场景1

在管理员的管理界面中，需要列出全部商家和商家下的全部商品，并显示在easyui中。通过实体关系查询可以查询出所有店铺的所有商品，还可以使用filter()或search()方法进行条件筛选。

 

使用实体关系关联查询首先需要shunfengdb.js文件中写配置语句

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps6A0F.tmp.jpg) 

jsweb代码：详见jsapis/jsons/case2_6_1.js

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps6A20.tmp.jpg) 

### 场景2

在前台微信端页面，当用户点进一个商品的详情页之后需要查询该商品的详细信息，包括品类、图片、代理配置以及拥有的规格等信息，这些信息分别存在四个表中，如果写连接查询的sql语句会比较复杂，如果用实体关系查询就非常简单。

 

首先shunfengdb.js文件中的配置语句

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps6A21.tmp.jpg) 

jsweb代码：详见jsapis/jsons/case2_6_2.js

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps6A32.tmp.jpg) 

部分结果如下如所示

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps6A33.tmp.jpg) 

### 场景3

在前台微信端页面，当用户提交订单信息时可能需要根据规格去查询商品信息以及该商品对应的店铺信息，这种情况下也可以使用实体关系查询。

 

首先shunfengdb.js文件中的配置语句

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps6A43.tmp.jpg) 

jsweb代码：详见jsapis/jsons/case2_6_3.js

![img](http://ortur5wom.bkt.clouddn.com/github/jsweb/wps6A44.tmp.jpg) 