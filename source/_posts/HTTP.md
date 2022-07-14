---
title: HTTP 学习
date: 2017-11-07 18:13:23
tags:
		- HTTP
categories:
		- 计算机基础
---

`HTTP`学习，持续补充中。

<!-- more -->



#### HTTP 概述

记录《HTTP权威指南》学习笔记。

##### Web客户端和服务器

Web内容都存储在Web服务器上的。Web服务器使用的是HTTP协议，因此经常被称为HTTP服务器。这些HTTP服务器存储了因特网中的数据，如果客户端发出请求，它们会提供数据。客户端向服务器发送HTTP请求，服务器会在HTTP响应中回送所请求的数据。

##### 方法

HTTP支持几种不同的请求命令，这些命令被称为HTTP方法。每条HTTP请求报文包含一个方法。这个方法告诉服务器执行什么动作。

	GET   从服务器向客户端发送命名资源
	PUT   将来自客户端的数据存储到一个命名的服务器资源中去
	DELETE 从服务器删除命名资源
	POST   将客户端数据发送到一个服务器网关应用程序
	HEAD  仅发送命名资源响应中的HTTP首部


##### [状态码](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes)

每条HTTP响应报文返回时都会携带一个状态码。状态码是一个三位数字的代码，告知客户端是否请求成功或者是否需要其他动作。

##### 报文

HTTP报文是由一行一行的简单字符串组成的。HTTP报文都是纯文本，不是二进制代码，所以人们可以很方便的对其进行读写。

##### 连接

HTTP协议位于TCP之上，HTTP使用TCP来传输其报文数据。

比如对一个`www.sec-note.com`发送请求。

1. 浏览器从URL中解析出服务器的主机名
2. DNS 获取域名的IP地址 151.101.25.147
3. 获取 IP端口号
4. TCP/IP 建立连接 IP:PORT
5. 发送一条HTTP GET请求
6. 从服务器读取HTTP响应
7. 关闭连接。浏览器显示文档。

#### URL和资源

##### [URL](https://www.oschina.net/translate/what-every-web-developer-must-know-about-url-encoding)

	http://www.sec-note.com/index.html
	方案     主机 				路径

所有出现在不应出现位置的 保留符（例如路径片段——以文件名为例——可能包含"?"）必须被URL 编码。

##### 编码机制

编码机制，用来在URL中表示各种不安全的字符。编码机制是通过转义表示法表示不安全的字符，这种转义表示法包含一个百分号(%)，后面跟着两个表示字符ASCII码的十六进制数。

#### [HTTP 报文](https://blog.csdn.net/u010256388/article/details/68491509)

报文包括：

1. 起始行
2. 首部
3. 主体


请求报文格式

	<method 方法> <request-URL 请求URL> <version>版本
	<headers>   首部
	
	<entity-body> 请求体


响应报文格式

	<version版本> <status 状态码> <reason-phrase 原因短语>
	<headers>       首部

	<entity-body> 主体
	

__HEAD__

HEAD方法和GET方法的行为类似，但服务器在响应中只返回首部。不会返回实体的主体部分。

1. 在不获取资源的情况下了解资源的情况
2. 通过查看响应中的状态吗，查看某个对象是否存在
3. 通过查看首部，看资源是否被修改

__TARCE__

客户端发起一个请求时，这个请求可能会经过防火墙/代理/网关/或其他一些应用程序。每个中间节点都有可能修改原始的HTTP请求。TRACE方法允许客户端最终将请求发送给服务器时，看它变成什么样子。

__OPTIONS__

OPTIONS方法请求Web服务器告知其支持的功能。可以询问服务器支持那些方法。

__通用首部__

客户端和服务器端都可以使用的通用首部。比如Date首部就是一个通用首部，每一端都可以用它来说明构建报文的时间和日期。

__请求首部__

	Client-IP     提供了运行客户端的机器IP
	From			提供了客户端用户的E-mail地址
	Host		  给出了接受请求的服务器的主机名和端口号
	Referer		 提供了包含当前URI的文档的URL

__Accept首部__

	Accept     告诉服务器能够发送那些媒体类型
	Accept-Charset	告诉服务器发送那些字符集
	Accept-Encoding  告诉服务器发送那些编码方式
	Accept-Language  告诉服务器发送那些语言

	

#### 参考

[HTTP请求行、请求头、请求体详解](https://blog.csdn.net/u010256388/article/details/68491509)
