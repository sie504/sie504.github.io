---
title: iis-shortname
date: 2018-01-10 10:43:04
tags:
		- iis-shortname
categories:
		- 安全研究	
---

接触到很多用户反馈的问题，说是被扫描到了`IIS`短文件漏洞，参考一些文章，学习一下其中的原理，如何进行更好的防御。

<!-- more -->

##### 漏洞原因

参考[IIS短文件名暴力猜解漏洞分析](http://www.lijiejie.com/iis-win8-3-shortname-brute/)

为了兼容16位`MS-DOS`程序，`Windows`为文件名较长的文件(和文件夹)生成了对应的`windows 8.3短文件名`

[8.3格式](https://baike.baidu.com/item/8.3%E6%A0%BC%E5%BC%8F)

`8.3`格式通常是指较旧的`Windows`操作系统或DOS的文件命名限制。8指文件名最大长度为8，3指扩展名最大长度为3。

若不符合以上限制则会以"~"作延长名称如"Program Files"会变成"Progra~1"
若同一文件夹有相似的名称，末端的数值则会自动递增

返回`Bad request`错误的话，说明不存在该文件或者目录。
返回`404`，则代表存在该文件或者目录。

短文件名的规律：前6个字符+`~1-9`+扩展名前三位

比如在`E\test`创建一个文件`1234556789.html`

	 E:\test>dir /x
	 驱动器 E 中的卷是 文档
	 卷的序列号是 000D-D35E
	
	 E:\test 的目录
	
	2018/01/10  15:39    <DIR>                       .
	2018/01/10  15:39    <DIR>                       ..
	2018/01/10  15:36                 0 123455~1.HTM 1234556789.html
	               1 个文件              0 字节
	               2 个目录 30,344,765,440 可用字节

其结果为`123455~1.HTM`

该文件名的特征：

- 文件名的前六个字符直接显示，后续的用`~1`指代。其中的数字1还可以递增，如果存在多个文件名类似的文件(名称前6为必须相同，且后缀名的前3位必须相同)
- 后缀名最长只有3位，多余的被截断。

环境：启用`.net`的`IIS`暴力列举短文件名。

- 访问构造的某个存在的短文件名，会返回404
- 访问构造的某个不存在的短文件名，返回400

##### 漏洞利用

需要使用通配符`*`。在`windows`中，`*`可以匹配n个字符，n可以为0。

利用的最后面为什么会加`/.aspx`。

论文中这样说的。

>“validlong.extx” in 8.3 format would be “VALIDL~1.EXT”. I used “/.aspx” to redirect the request to .Net framework to getaclearerresponse. However, I found out it is possible to use other extensions n  different  scenarios.  Further,the asterisk sign (“*”)andquestion mark (“?”) can also be used to check the validityof one letter in the file or the extension name.

>I used “/.aspx” to redirect the request to .Net framework to getaclearerresponse. 
>
>我使用“/.aspx”将请求重定向到.Net框架，以便通用。



##### 漏洞修复



1.  升级.net framework


1. 修改注册表键值：

	HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\FileSystem
	
	修改NtfsDisable8dot3NameCreation为1。


##### 参考

[IIS短文件名暴力猜解漏洞分析](http://www.lijiejie.com/iis-win8-3-shortname-brute/)

[iis-shortname-scanner-poc](https://code.google.com/archive/p/iis-shortname-scanner-poc/)

[Paper-Microsoft IIS Tilde Character Short File/Folder Name Disclosure](https://www.exploit-db.com/docs/english/19527-microsoft-iis-tilde-character-short-filefolder-name-disclosure.pdf)

[Nmap—http-iis-short-name-brute](https://nmap.org/nsedoc/scripts/http-iis-short-name-brute.html)

[完美时空某分站短文件名泄露](https://www.secpulse.com/archives/9086.html)

[IIS短文件/文件夹漏洞(汇总整理)](http://www.freebuf.com/articles/4908.html)
