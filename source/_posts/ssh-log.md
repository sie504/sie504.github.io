---
title: ssh_log
date: 2017-03-01 15:54:24
tags:
		- ssh-log
		- 
categories:
		- 渗透技巧
---

日志污染结合文件漏洞进行利用。

<!-- more -->

##### 文件包含+日志污染

PHP等脚本语言能够从外部文件读取脚本源码的一部分。PHP中对应的函数有require、require_once、include、include_once。

如果外界能够指定include的对象文件名，就可能会发生意料之外的文件被include而遭到攻击。这被称为文件包含漏洞。

测试：含有本地文件包含的代码。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-1-15/80365472.jpg)

在同级目录下，包含一个lfi.txt。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-1-15/35522367.jpg)

浏览器访问，可以看到执行成功。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-1-15/53599383.jpg)

ssh污染日志可以写入攻击代码。
用ssh连接目标机(192.168.145.191)，同时写入攻击代码，密码任意填入，是错误的密码。
root@kali:/var/log# ssh '<?php system($_GET[c]); ?>'@192.168.145.191
在目标机/var/log/auth.log登录日志可以看到写入的代码。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-1-15/4306620.jpg)

日志中的攻击代码是否可以执行成功，得看/var/log/auth.log文件的权限，如果它对其他用户具有读权限，则可以成功，否则，不可以。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-1-15/76389039.jpg)


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-1-15/61569392.jpg)

下图可以看到可以执行成功。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-1-15/41690106.jpg)


##### 参考

[http://www.hackingarticles.in/web-server-exploitation-ssh-log-poisoning-lfi/](http://www.hackingarticles.in/web-server-exploitation-ssh-log-poisoning-lfi/)

[http://www.cnbraid.com/2017/ssh-poison-lfi-shell.html](http://www.cnbraid.com/2017/ssh-poison-lfi-shell.html)
