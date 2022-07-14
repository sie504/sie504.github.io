---
title: PHP 会话控制
date: 2017-10-17 13:20:46
tags:
		- PHP
categories:
		- 编程学习

---

PHP的会话控制，`Cookie`和`Session`。

<!-- more -->




##### 会话控制

HTTP无状态，会话状态。

	keep-live
	使用同一个TCP连接来发送和接收多个HTTP请求/应答，而不是为每一个新的请求/应答打开新的连接的方法


##### Cookie

让服务器知道客户是谁。保存在客户端。内存`cookie`有保存在浏览器中，有保存在硬盘中的。

[setcookie](https://secure.php.net/manual/zh/function.setcookie.php)
	

	bool setcookie ( string $name [, string $value = "" [, int $expire = 0 [, string $path = "" [, string $domain = "" [, bool $secure = false [, bool $httponly = false ]]]]]] )

	$name 指定Cookie的名字
	$value Cookie的值
	$expire 设置Cookie的过期时间，默认为0，单位是秒数
	$path 设置cookie的有效路径
	$domain 设置Cookie的作用域
	$secure 安全性
	$httponly 是否只使用HTTP访问Cookie，默认值是false。如果设置为true，那么客户端的JS就无法操作这个Cookie了，使用这个参数减少XSS攻击。


	setrawcookie

	读取cookie var_dump($_COOKIE)
	删除：过期时间 time()-1
	更新和删除的时候需要保证$path和$domain和之前的保持一致才可以



##### `header` 操作 `cookie`

	header("Set-Cookie:name=value[;expires=date][;domain=domain][;path=path][;secure][;httponly]")


	<?php
	//header形式设置cookie
	header('Set-Cookie:b=2');



##### 通过JS操作Cookie

2-10 10:23

	封装Cookie
	Cookie保存数组形式的数据

Cookie缺点

	Cookie中不要存储敏感数据
	Cookie中保存的最大字节数是4k
	Cookie设置之后每次都附着HTTP头发布

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-16/75659053.jpg)


**HTML5 localStorage**

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-16/12076974.jpg)




##### Session

	会话。服务器和浏览器保有的共同的小秘密的这段时间

	Session工作原理
	理发办卡，把信息录入他们的系统中，给我一个卡号，再次来的时候，对卡号来认识。

	一个会话，保存在Cookie中。
	1：建立会话的时候，PHP会先查看时候包含session_id，如果没有服务器会在内存中创建一个变量。这个变量就是session_id
	2：服务器会把这个session_id发送到浏览器保存，一般浏览器会把这个id保存在cookie中
	3：之后每次浏览器访问服务器的时候，都会携带cookie中的session_id
	4:服务器端这个session变量可以存放任意合法的会话数据，这些数据是经过序列化之后存放进去的
	5：每次浏览器访问服务器，凭借自己的session_id到服务器这个变量中认领自己的信息
	6：想销毁会话，可以删除会话中in个的数据，销毁会话文件


[session和cookie区别](https://www.zhihu.com/question/19786827)

	session一般保存在cookie中，当作其中的一个变量。。


禁用cookie之后，session还可以使用。可能会在地址栏中传送了。

具体实现，要用代码写出来。使用`session_id`传递。

	session 也可以当作登录凭证、、



会话管理器。Django直接将`session`保存在了数据库中。

	session 保存在数据库
