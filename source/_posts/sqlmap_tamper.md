---
title: SQLMAP tamper 学习
date: 2018-03-28 15:54:24
tags:
		- Bypasswaf
		- SQLi
		 
categories:
		- WAF
---

`sqlmap tamper`脚本的学习。

<!--more -->

#### [SQLMAP tamper](https://github.com/sqlmapproject/sqlmap/tree/master/tamper)


`SQLMAP`在注入语句的时候，有时会遇到`WAF`或其他防御软件，导致不能成功注入出语句。在`SQLMAP`的`tamper`目录下面有一些绕过脚本，结合网上的一些资料学习整理以下。

关于绕过`WAF`，要模糊测试，找到它的特性，具体情况具体绕过。



#### tamper 简介

可以参考以下文章，看看整理分类。

[SQLMAP --tamper 绕过WAF脚本分类整理](https://blog.csdn.net/hxsstar/article/details/22782627)	

[SQLMAP-Tamper绕过waf使用](http://www.vuleyes.com/2016/08/21/sqlmap-tamper%E7%BB%95%E8%BF%87waf%E4%BD%BF%E7%94%A8/)

[SQLMAP --tamper ](http://www.hackxc.cc/?post=191)

在`tamper`目录下面就是其中的脚本，在脚本里面有相应的代码和解释。以下只是简单的列举出了前几个。

	apostrophemask.py     Replaces apostrophe character with its UTF-8 full width counterpart
	apostrophenullencode.py   Replaces apostrophe character with its illegal double unicode counterpart
	appendnullbyte.py	Appends encoded NULL byte character at the end of payload
	base64encode.py 	Base64 all characters in a given payload
	between.py			Replaces greater than operator ('>') with 'NOT BETWEEN 0 AND #'
    Replaces equals operator ('=') with 'BETWEEN # AND #'
	bluecoat.py			Replaces space character after SQL statement with a valid random blank character.
    Afterwards replace character = with LIKE operator


针对不同的数据库，会有不同的脚本来进行绕过。

[SQLMap Tamper Scripts (SQL Injection and WAF bypass)](https://forum.bugcrowd.com/t/sqlmap-tamper-scripts-sql-injection-and-waf-bypass/423)

__General Tamper testing:__

	tamper=apostrophemask,apostrophenullencode,base64encode,between,chardoubleencode,charencode,charunicodeencode,equaltolike,greatest,ifnull2ifisnull,multiplespaces,nonrecursivereplacement,percentage,randomcase,securesphere,space2comment,space2plus,space2randomblank,unionalltounion,unmagicquotes

__MSSQL__

	tamper=between,charencode,charunicodeencode,equaltolike,greatest,multiplespaces,nonrecursivereplacement,percentage,randomcase,securesphere,sp_password,space2comment,space2dash,space2mssqlblank,space2mysqldash,space2plus,space2randomblank,unionalltounion,unmagicquotes


__MySQL__

	tamper=between,bluecoat,charencode,charunicodeencode,concat2concatws,equaltolike,greatest,halfversionedmorekeywords,ifnull2ifisnull,modsecurityversioned,modsecurityzeroversioned,multiplespaces,nonrecursivereplacement,percentage,randomcase,securesphere,space2comment,space2hash,space2morehash,space2mysqldash,space2plus,space2randomblank,unionalltounion,unmagicquotes,versionedkeywords,versionedmorekeywords,xforwardedfor


#### [tamper 绕过案例](http://cb.drops.wiki/drops/tools-4760.html)

##### 案例一 [SQLMAP Tamper Scripts for The Win](https://pen-testing.sans.org/blog/2017/10/13/sqlmap-tamper-scripts-for-the-win)

上面链接的案例很好，该程序对输入的参数内容进行了加密，经过加密后到达服务器端进行解密，然后将数据发送给数据库。如何知道这个加密函数呢？通过一个个字符的替换，然后得到对应的加密内容。

	Normal Letter = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789'
	
	Encrypted Letters = 'QqnPvka03wMU6ZybjmK4BRSEWdVishgClpI1AouFNOJ9zrtL2Yef7Tc8GxDHX5'

编写对应的`tamper`

	#!/usr/bin/env python
	
	from lib.core.data import kb
	from lib.core.enums import PRIORITY
	import string
	
	__priority__ = PRIORITY.NORMAL
	
	def dependencies():
	    pass
	
	def tamper(payload, **kwargs):
	    orig = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789"
	    srvr = "QqnPvka03wMU6ZybjmK4BRSEWdVishgClpI1AouFNOJ9zrtL2Yef7Tc8GxDHX5"
	    return payload.translate(string.maketrans(orig,srvr))


##### 案例二 [WooYun-2015-144854](http://cb.drops.wiki/bugs/wooyun-2015-0144854.html) [space2comment.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/space2comment.py)

用`sqlmap`跑不出数据，经测试有过滤，将空格和一些关键词过滤了。

[space2comment.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/space2comment.py) 使用注释来代替空格。

	 >>> tamper('SELECT id FROM users')
    'SELECT/**/id/**/FROM/**/users'


##### 案例三  [equaltolike.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/equaltolike.py)

使用 `like`代替等号

	>>> tamper('SELECT * FROM users WHERE id=1')
    'SELECT * FROM users WHERE id LIKE 1'

[青果教务系统存在SQL注入漏洞（简单绕过WAF保护）](https://wooyun.shuimugan.com/bug/view?bug_no=087296)

[https://wooyun.shuimugan.com/bug/view?bug_no=074790](https://wooyun.shuimugan.com/bug/view?bug_no=074790)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-3-28/59343134.jpg)




[自动化注入Bypass](https://zhuanlan.zhihu.com/p/25678670)



#### tamper 编写

[sqlmap-tamper编写指南](https://bbs.ichunqiu.com/thread-13556-1-1.html)

[sqlmap 的源码学习笔记二之编写tamper脚本](https://blog.csdn.net/qq_29277155/article/details/52266478)

[sqlmap tamper脚本编写](https://blog.csdn.net/whatday/article/details/62059263)



#### tamper 文章集锦

[使用sqlmap中tamper脚本绕过waf](http://cb.drops.wiki/drops/tools-4760.html)

[sqlmap 的源码学习笔记二之编写tamper脚本](https://blog.csdn.net/qq_29277155/article/details/52266478)

[sqlmap 的源码学习笔记一之目录结构](https://blog.csdn.net/qq_29277155/article/details/51646932)

[Sqlmap Tamper大全（1）](http://www.91ri.org/7852.html)

[sqlmap注入之tamper绕过WAF防火墙过滤](http://www.vuln.cn/2086)
