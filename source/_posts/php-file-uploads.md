---
title: PHP 文件处理
date: 2017-10-26 13:20:46
tags:
		- PHP
categories:
		- 编程学习

---

学习一下`PHP`中文件上传的基本方法。

<!-- more -->

##### 文件上传原理

将客户端的文件上传到服务器端，在将服务器端的临时文件移动到指定目录中。

##### 文件上传设置

__客户端配置__

1. 表单页面
2. 表单的发送方式
3. 添加 `enctype = "multipart/form-data"`


前端页面

	<!DOCTYPE html>
	<html>
	<head>
	<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
	<title>File uploads</title>
	</head>
	<body>
	    <form action="dofile.php" method="post" enctype="multipart/form-data">
	    请选择上传的文件:
	    <input type="file" name="myfile" /><br/>
	    <input type="submit" value="上传文件" />
	    </form>
	</body>
	</html>

后端接收代码

	<?php

	//$_FILE: 文件上传变量
	print_r($_FILES);


打印出的参数，一个二维数组

	Array
	(
	    [myfile] => Array
	        (
	            [name] => 3.drupal_attacks.conf.app
	            [type] => application/octet-stream
	            [tmp_name] => C:\Windows\Temp\php204A.tmp
	            [error] => 0
	            [size] => 37430
	        )
	
	)


##### $FILES:HTTP文件上传变量

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-10-25/27422064.jpg)

将临时文件移动到指定目录下

`move_uploaded_file`

`copy()`

	<?php
	
	//$_FILE: 文件上传变量
	print_r($_FILES);
	$filename = $_FILES['myfile']['name'];
	$type = $_FILES['myfile']['type'];
	$tmp_name = $_FILES['myfile']['tmp_name'];
	$size = $_FILES['myfile']['size'];
	$error = $_FILES['myfile']['error'];
	
	//将服务器上的临时文件移动到指定目录下
	// move_uploaded_file($tmp_name,$destination):将服务器上的临时文件移动到中指定目录下
	move_uploaded_file($tmp_name,"uploads/".$filename);

##### 文件上传配置以及解析

`php.ini`的配置。

	file_uploads = On 支持HTTP上传
	upload_max_filesize = 2M 允许上传文件的大小
	
![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-10-25/89389181.jpg)

##### 错误信息说明

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-10-25/50712806.jpg)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-10-25/90170854.jpg)

	<?php
	// 通过$_FILES文件上传变量接收上传文件信息
	header('content-type:text/html;charset=utf-8');
	$fileinfo = $_FILES['myfile'];
	$filename = $fileinfo['name'];
	$type = $fileinfo['type'];
	$tmp_name = $fileinfo['tmp_name'];
	$size = $fileinfo['size'];
	$error = $fileinfo['error'];
	
	//判断错误号
	if($error==UPLOAD_ERR_OK){
	    if(move_uploaded_file($tmp_name,"uploads/".$filename)){
	        echo '文件'.$filename.'上传成功';
	    }else{
	        echo '文件'.$filename.'上传失败';
	    }
	    
	}else{
	    //匹配错误信息
	    switch($error){
	    case 1:
	        echo '上传文件超过PHP配置大小';
	        break;
	    case 2:
	        echo '超过了表单限制的大小';
	        break;
	    case 3:
	        echo '文件部分上传';
	        break;
	    case 4:
	        echo '没有选择上传文件';
	        break;
	    case 6:
	        echo '没有找到临时目录';
	        break;
	    case 7:
	    case 8:
	        echo '系统错误';
	        break;
	    }  
	}

__客户端限制__

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-10-25/34019909.jpg)

type accept允许上传的类型

##### 服务器端上传限制

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-10-25/73731988.jpg)

进行上传文件名的重命名以及上传路径的判定，如果不存在则新建，等上传错误的判断。

	<?php
	header('content-type:text/html;charset=utf-8');
	$fileinfo = @$_FILES['myfile'];
	$maxSize=2097152;
	$allowExt=array('jpg','png','gif');
	//判断错误号
	if($fileinfo['error']==0){
	    //判断上传文件的大小
	    if($fileinfo['size']>$maxSize){
	        exit('上传文件过大');
	    }
	    //获取文件扩展名
	    //$ext = strtolower(end(explode('.',$fileinfo['name'])));
	    $ext = pathinfo($fileinfo['name'],PATHINFO_EXTENSION);
	    if(!in_array($ext,$allowExt)){
	        exit('非法文件类型');
	    }
	    //判断文件是否通过HTTP POST的方式上传来的
	    if(!is_uploaded_file($fileinfo['tmp_name'])){
	        exit('文件不是通过HTTP POST方式上传来的');
	    }
	    //检测是否为真实的文件类型
	    
	    $path = 'uploads';
	    //判断上传路径是否存在,不存在的话就创建目录
	    if(!file_exists($path)){
	        mkdir($path,0777,true);
	        chmod($path,0777);
	    }
	    
	    //确保文件名唯一，防止重命名产生覆盖
	    $uniName=md5(uniqid(microtime(true),true)).'.'.$ext;
	    //echo $uniName;exit;
	    $destination=$path.'/'.$uniName;
	    if(move_uploaded_file($fileinfo['tmp_name'],$destination)){
	        echo '文件上传成功';
	    }else{
	        echo '文件上传失败';
	    }
	    
	}else{
	    //匹配错误信息
	    switch($fileinfo['error']){
	    case 1:
	        echo '上传文件超过PHP配置大小';
	        break;
	    case 2:
	        echo '超过了表单限制的大小';
	        break;
	    case 3:
	        echo '文件部分上传';
	        break;
	    case 4:
	        echo '没有选择上传文件';
	        break;
	    case 6:
	        echo '没有找到临时目录';
	        break;
	    case 7:
	    case 8:
	        echo '系统错误';
	        break;
	}
	}


##### 参考

[PHP实现文件上传与下载](http://www.imooc.com/learn/219)

[php 怎样实现单个文件上传](http://blog.csdn.net/u012275531/article/details/16802127)

