---
title: 科普 WAF
date: 2017-04-22 15:54:24
tags:
		- Bypasswaf
		 
categories:
		- WAF
---

一篇关于`WAF`的科普文。

<!-- more -->

之前在`SecWiki`团队的一个`WAF`[科普分享](https://mp.weixin.qq.com/s?__biz=MjM5NDM1OTM0Mg==&mid=2651050493&idx=1&sn=1d81ff6aff52fa93f329522021bf93e0&scene=0#wechat_redirect)。

##### 背景

让我们回顾下历史，Web1.0时代，那个时候并没有WAF，随着Web2.0技术的日益发展，其中的攻击也层出不穷。比如应用层的SQL注入、XSS、文件包含等Web漏洞。由于传统的防火墙是放在网关处，不能很好的防御此漏洞，所以WAF就应运而生。WAF是部署在Web服务器集群的前端，对Web服务器进行防护。

##### 那什么是WAF呢

WAF是基于对HTTP/HTTPS流量的双向解码分析，应对HTTP/HTTPS应用中的各类安全威胁，如SQL注入、XSS、目录遍历、命令执行等。

##### WAF分类

一些国内的厂商。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-2-5/42056696.jpg)

##### WAF规则

依据一些规则来防护基于漏洞类型的攻击，如下图所示。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-2-5/34332651.jpg)

##### WAF的配置和分析

这里我们用Modsecirity和Nginx+Lua来进行分析。Modsecurity是一个集入侵检测和防御引擎功能的开源Web应用安全程序(或Web应用程序防火墙),它以Apache Web服务器的模块方式运行。 目标是增强Web应用程序的安全性, 防止Web应用程序受到已知或未知的攻击，而Nginx想来大家也不会陌生，它是一个高性能的HTTP和反向代理服务器。最后一个Lua，它是一种轻量小巧的脚本语言，用标准C语言编写并以源代码形式开放， 其设计目的是为了嵌入应用程序中，从而为应用程序提供灵活的扩展和定制功能。

##### 步骤和注意事项

这里我们用`Ubuntu 14.04.3 安装ModSecurity`

安装软件之前更新源

	apt-get install update

安装`apache2`

	apt-get install apache2

安装`Modsecurity`

	apt-get install libxml2 libxml2-dev libxml2-utils libaprutil1 libaprutil1-dev libapache2-modsecurity

使用以下命令查看`Modsecurity`版本

	dpkg -s libapache2-modsecurity | grep Version

重新加载配置文件

	service apache2 reload

配置`Modsecurity`，启用拦截模式

	~# cd /etc/modsecurity/

	:/etc/modsecurity# mv modsecurity.conf-recommended modsecurity.conf

	:/etc/modsecurity# vim modsecurity.conf 


将 `SecRuleEngine off 改为SecRuleEngine On`，目的是设置为拦截模式。

使用`Modsecurity`核心规则集

规则集放置的目录为 `/usr/share/modsecurity-crs/activated_rules`

选择启用base规则集

在其目录下可以看到规则集文件

	~# cd /usr/share/modsecurity-crs/activated_rules/

	:/usr/share/modsecurity-crs/activated_rules# for f in $(ls ../base_rules/); do ln -s ../base_rules/$f; done

多规则文件按文件名加载  `modsecurity-crs_数字_*`。如图：

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-2-5/83774358.jpg)

修改`apache2模块配置，启用规则集(2.7版本)`

	vim /etc/apache2/mods-available/security2.conf  

 添加相应的配置文件，如图：

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-2-5/54904960.jpg)

启用Modsecurity模块

	a2enmod headersa2enmod security2

	service apache2 restart

需要注意问题

IP访问问题

配置完之后，通过访问IP发现，显示403。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-2-5/51460627.jpg)

原因是为了考虑完全性，有一条规则是禁止以IP方式来访问网站的。


我们查看日志可知(拦截日志在 `/var/log/apache2/modsec_audit.log`)

	vim /var/log/apache2/modsec_audit.log 

其中的规则文件`/usr/share/modsecurity-crs/activated_rules/modsecurity_crs_21_protocol_anomalies.conf中的960017`拦截了该访问，如图：

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-2-5/90458108.jpg)

解决方案

在测试的时候，只需要在该文件中对应的规则SecRule前面加入"#"符号，来注释掉这条规则，就可以用IP访问网站了。

##### Modsecurity 处理阶段

ModSecurity 2.x允许把规则置于下述五个阶段之一：

请求头(REQUEST_HEADERS)

请求体(REQUEST_BODY)

响应头(RESPONSE_HEADERS)

响应体(RESPONSE_BODY)

记录(LOGGING)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-2-5/25800084.jpg)


##### WAF防御

以phpcms v9.6.0为案例

在网站注册页面填入一下信息

BurpSuite抓包，写入payload，根据回显可以看到显示403拦截。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-2-5/87338269.jpg)

防御日志

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-2-5/60310952.jpg)

防御的具体规则。通过下图可以看到，对应的正则匹配到的攻击载荷。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-2-5/66123485.jpg)

##### 规则考虑不全被绕过

有时候WAF的规则考虑的场景不全面，在具体的程序业务面前，可能由于一些正则匹配不全被绕过问题。

绕过WAF的实际上去构造WAF不能匹配的payload，但是它能在应用程序能够执行成功的攻击载荷。

比如以下的一个问题，业务程序本身已经过滤了“=”号，但是WAF规则上可能只是匹配了`<script>`。所以当在这个业务网站上输入<s==ri==pt>，WAF是不能匹配到的，但是经过WAF在，在其后面处理应用层数据的硬件或者软件，已经将"="过滤，为`<script>`执行。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-2-5/83535370.jpg)

以上只是一个特定业务场合下绕过。去寻找位于WAF设备之后处理应用层数据包的硬件/软件的特性，利用这些特性构造WAF不能命中，但是在应用程序能够执行成功的载荷，绕过WAF。这些特性需要各大安全爱好者共同研究来发现。

##### 规则编写

	格式：SecRule VARIABLES OPERATOR [ACTIONS]
	
	VARIABLES :主要是一些匹配的变量，ARGS、ARGS_GET、ARGS_POST、MATCHED_VAR、 REQUEST_COOKIES等。
	
	OPERATOR:主要一些操作，比如用正则，或者以那些变量开始等。


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-2-5/35448839.jpg)

ACTIONS：描述当操作进行成功的匹配一个变量时具体怎么做。指定如果VARIABLE有符合OPERATOR的情况时，要执行何种动作，比如最简单的是给与拦截或者警告。

一个小例子。根据s2-045引起漏洞处分析。可以简单编写这样的规则。

	SecRule REQUEST_HEADERS:Content-Type "%\{.+\}?|\$\{.+\}?"  "id:123456,block"


##### Ubuntu 16.04 LTS 配置Nginx+Lua WAF

由于Nginx的高性能，现在很多互联网公司都会用nginx+lua来自研WAF，接下主要是部署Nginx+lua WAF。

下载需要的安装包

在目录/usr/local/src下载nginx、pcre、luajit、ngx-devel_kit、lua-nginx-module

之后的命令：

	/usr/local/src# wget http://nginx.org/download/nginx-1.9.4.tar.gz

	/usr/local/src#wget ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-8.39.tar.gz
	
	/usr/local/src# wget 
	
	http://luajit.org/download/LuaJIT-2.0.4.tar.gz
	
	/usr/local/src# wget  https://github.com/simpl/ngx_devel_kit/archive/v0.2.19.tar.gz
	
	 /usr/local/src# wget https://github.com/openresty/lua-nginx-module/archive/v0.9.16.tar.gz


创建Nginx运行的普通用户


	root@ubuntu16:/usr/local/src# useradd -s /sbin/nologin -M www

解压NDK和lua-nginx-module

	root@ubuntu16:/usr/local/src# tar zxvf v0.2.19.tar.gz
	
	root@ubuntu16:/usr/local/src# tar zxvf v0.9.16.tar.gz


安装LuaJIT Luajit是Lua即时编译器


	root@ubuntu16:/usr/local/src# tar zxvf LuaJIT-2.0.4.tar.gz
	
	root@ubuntu16:/usr/local/src# cd LuaJIT-2.0.4
	
	root@ubuntu16:/usr/local/src/LuaJIT-2.0.4# make && make install


安装pcre

	root@ubuntu16:/usr/local/src# tar zxvf pcre-8.39.tar.gz
	
	root@ubuntu16:/usr/local/src# cd pcre-8.39
	
	root@ubuntu16:/usr/local/src/pcre-8.39# ./configure
	
	root@ubuntu16:/usr/local/src/pcre-8.39# make && make install


安装libssl-dev

	root@ubuntu16:/usr/local/src# apt-get install libssl-dev

编译nginx

	root@ubuntu16:/usr/local/src# tar zxvf nginx-1.9.4.tar.gz
	
	root@ubuntu16:/usr/local/src# cd nginx-1.9.4
	
	root@ubuntu16:/usr/local/src/nginx-1.9.4# export LUAJIT_LIB=/usr/local/lib
	
	root@ubuntu16:/usr/local/src/nginx-1.9.4#export LUAJIT_INC=/usr/local/include/luajit-2.0
	
	root@ubuntu16:/usr/local/src/nginx-1.9.4# ./configure --prefix=/usr/local/nginx --user=www --group=www --with-http_ssl_module --with-http_stub_status_module --with-file-aio --with-http_dav_module --add-module=/usr/local/src/ngx_devel_kit-0.2.19/ --add-module=/usr/local/src/lua-nginx-module-0.9.16/ --with-pcre=/usr/local/src/pcre-8.39
	
	root@ubuntu16:/usr/local/src/nginx-1.9.4# make -j2 && make install
	
	root@ubuntu16:/usr/local/src/nginx-1.9.4#  ln -s /usr/local/lib/libluajit-5.1.so.2 /lib64/libluajit-5.1.so.2


启动服务器:

	root@ubuntu16:~#  /usr/local/nginx/sbin/nginx -t
	
	root@ubuntu16:~#  /usr/local/nginx/sbin/nginx 


安装完毕且启动后，第一个配置：

在配置文件里，写入下面的信息测试

	root@ubuntu16:~# vim /usr/local/nginx/conf/nginx.conf 

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-2-5/60936154.jpg)

我们现在访问一下，可以看到nginx+lua环境已经配置好。

配置WAF

从github上下载loveshell编写的WAF到nginx安装路径/usr/local/nginx/conf/:

 	root@ubuntu16:/usr/local/nginx/conf# git clone https://github.com/loveshell/ngx_lua_waf.git

并将下载文件命名waf

	root@ubuntu16:/usr/local/nginx/conf# mv ngx_lua_waf/ waf/

在nginx.conf 的http段中添加

	lua_package_path "/usr/local/nginx/conf/waf/?.lua";
	
	lua_shared_dict limit 10m;
	
	init_by_lua_file  /usr/local/nginx/conf/waf/init.lua;
	
	access_by_lua_file /usr/local/nginx/conf/waf/waf.lua;


配置WAF规则文件

配置文件

	root@ubuntu16:/usr/local/nginx/conf/waf/config.lua


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-2-5/47953373.jpg)

这里我们需要创建log存储目录，并且需要nginx用户的可写权限。

	root@ubuntu16:/usr/local/nginx/conf/waf# chown www.www /usr/local/nginx/logs/hack/
	
	root@ubuntu16:/usr/local/nginx/conf/waf# /usr/local/nginx/sbin/nginx -s reload


测试防御

	http://192.168.145.150/?id=1%20../../../etc/passwd

拦截情况如图：

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-2-5/85447477.jpg)

查看日志文件


	root@ubuntu16:/usr/local/nginx/logs/hack# ls
	
	localhost_2017-04-20_sec.log 

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-2-5/23463334.jpg)

创建规则

过滤规则在wafconf下，可根据需求自行调整，每条规则需换行，或者用|分割。

args里面的规则get参数进行过滤的。

url是只在get请求url过滤的规则。

post是只在post请求过滤的规则。

whitelist是白名单，里面的url匹配到不做过滤。

user-agent是对user-agent的过滤规则。


##### 总结

本次分享主要是基于开源的WAF来讲解其简单部署和防御，来分析WAF对Web漏洞上攻击的防御。但是WAF还有很多的不足，大多数基于正则的WAF，为了保护业务的正常运行，而不产生误报，再加上语言极其灵活的语法，有时很容易被绕过。还有WAF缺乏对0day漏洞的防御，缺少基础的业务防御能力，比如上次周技术分享的逻辑漏洞，WAF对此的防御就捉襟见肘了。对于WAF新技术上语义分析、机器学习等的应用，我们将拭目以待。



##### 参考

http://www.freebuf.com/articles/web/43559.html

https://github.com/loveshell/ngx_lua_waf

http://blog.oldboyedu.com/nginx-waf/

http://www.freebuf.com/articles/network/128370.html

http://www.freebuf.com/articles/neopoints/125807.html












