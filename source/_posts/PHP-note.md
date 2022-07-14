---
title: PHP 基础知识
date: 2017-09-26 13:20:46
tags:
		- PHP
categories:
		- 编程学习

---

`PHP`基础知识学习。

<!-- more -->


##### [PHP手册](https://secure.php.net/manual/zh/)

##### 0926 变量和数据类型

__数量类型、复合类型(数组、对象)、特殊类型__

var_dump($a);   打印变量的详细信息

浏览器十进制显示

0x 十六进制


__浮点型__

float

	header("Content-type:text/html;charset=utf-8");
	告诉浏览器以什么编码方式解析什么类型的文档，防止中文乱码


__字符串__

string

__单引号中字符串的变量不解析，双引号可以__

	<?php
	
	$f = 'doddodo';
	
	echo '$f','<br/>';
	echo "$f";
	?>


结果

	$f
	doddodo


转义符号

	\n 换行
	\r 回车
	\t 水平制表符


	__单引号只解析\` \\`,双引号解析全部的转义字符__

	<?php
	
	$f = '!\r@\n#\t%a\\b\`\$de';
	$g = "!\r@\n#\t%a\\b\`\$de";
	echo  $f,'<br/>';
	echo $g,'<br/>';
	?>

结果

	!\r@\n#\t%a\b\`\$de
	! @ #	%a\b\`$de

[php中如何换行](https://zhidao.baidu.com/question/518312912081988045.html)


php引擎在解析变量的时候会尽量的向后取合法字符，以为取得越多，这个变量的含义越明确。


__{}__

	{$变量名称}  不用加空格

0927  字符串可以增删替换

	{} 字符串 $string = 'abcedf'
		$string{0}  根据下标找字符


demo

	<?php
	
	$string = 'abdef';
	echo $string;
	echo '<br/>';
	//删除，就把这个位置替换为空
	$string{0} = '';
	echo $string;
	echo '<br/>';
	
	$string{5} = 'g';
	echo $string;
	echo '<br/>';
	
	?>


__heredoc__

相当于 双引号


大段落

	以 <<<EOF 开始 EOF; 结束  没有缩进





__复合类型__

> 数组  array()
> 
> 对象 new Class()

>资源 

>经过unset的值为null

> unset() 销毁对象


__数据类型转换__

>自动转换

>强制转换

__其他类型转换为数值型__

	true -> 1 false -> 0 null -> 0 
	如果字符串以非法数值开始，直接转换为0
	如果字符串以合法数值开始，一直取到第一个非法数值结束



对象不能转换为字符串


__其他类型转换为布尔型__

	if(条件){
	
	}else{
	
	}


> 0 -> false

> 0.0 -> false

> 空字符串 '' '0' --> false

> null -> false

> 空数组 array() -> false



3-3

0929

__数据类型临时转换__

	通过小括号

	(int)$
	
	(unset)$  临时转换为空

通过系统函数转换

	intval()
	floatval()
	realval()
	boolval()


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-9-29/5160777.jpg)

__数据类型永久转换__


	settype()
	gettype() 获取变量类型

[变量函数](http://php.net/manual/zh/ref.var.php)

通过变量函数检测变量类型。。

	is_int()
	is_float()


##### [常量](http://php.net/manual/zh/reserved.constants.php#reserved.constants.core)

一个简单值的标识符

* 系统常量(php_version,)

* 自定义常量 

	define const

	const 定义常量

	constant 获取常量的值

	defined() 检测常量是否存在

	print_r() 打印数组的信息

	get_defined_constants() 获取所有已定义的常量


测试

	<?php
	
	echo PHP_VERSION;
	
	echo '<br/>';
	
	echo PHP_OS;
	
	echo '<br/>';
	
	define('TEST','come on');
	
	echo TEST;
	
	//常量已经定义不能改变
	
	define('TEST','come php');
	
	echo TEST;
	echo '<br/>';
	const  SEC = 'PHP';
	
	echo SEC,'<br>';
	
	echo constant('PHP_VERSION');
	
	echo '<hr/>';
	
	print_r(get_defined_constants());
	
	?>

结果

	5.4.45<br/>WINNT<br/>come on<br />
	<b>Notice</b>:  Constant TEST already defined in <b>D:\phpStudy\WWW\php\1.php</b> on line <b>17</b><br />
	come on<br/>PHP<br>5.4.45<hr/>Array
	(
	    [E_ERROR] => 1
	    [E_RECOVERABLE_ERROR] => 4096
	    [E_WARNING] => 2
	    [E_PARSE] => 4
	    [E_NOTICE] => 8
	    [E_STRICT] => 2048
	    [E_DEPRECATED] => 8192
	    [E_CORE_ERROR] => 16
	    [E_CORE_WARNING] => 32
	    [E_COMPILE_ERROR] => 64
	    [E_COMPILE_WARNING] => 128
	    [E_USER_ERROR] => 256
	    [E_USER_WARNING] => 512
	    [E_USER_NOTICE] => 1024
	    [E_USER_DEPRECATED] => 16384
	    [E_ALL] => 32767

区分大小写

1-5


魔术常量

	__LINE__ __FILE__ __DIR__ __CLASS__ 

		<?php
	
	echo __LINE__;
	echo '<br/>';
	
	echo __FILE__;
	echo '<br/>';
	
	echo __DIR__;
	
	
	?>

结果

	3
	D:\phpStudy\WWW\php\1.php
	D:\phpStudy\WWW\php


__PHP预定义变量__

所有预定义变量都是全局变量

	$GLOBAL/$_SERVERS/$_ENV/$_COOKIE/$_SESSION/$FILE/$_GET/$_POST 主要接收表单


$_POST[名称]

	<html>
	<head>
	</head>
	<form action='test.php' method='post'>
	
	    <input type="text" name="username" id="" placeholder="请输入合法字符">
	    <input type="submit" value="submit">
	</form>
	
	</html>


POST接收数据

	<?php
	header('content-type:text/html;charset=utf-8');
	
	echo '用户名:',$_POST['username'],'<br/>';


name="username".$_POST接收表单以post形式发送过来的数据，$_POST['name']


__`$_GET`__

主要接受以?形式传递的数据。


__`$_REQUEST`__

	$_GET、$_POST都可以



##### [运算符](http://php.net/manual/zh/language.operators.php)

0930

1011

只有值的东西都可以叫做表达式。

浮点型支持递增递减，布尔型不支持，null，字符串支持递增，不支持递减

	++、-- 前缀和后缀
	$str = 'b1';
	echo ++$str;
	//b2


	ord($str) 得到指定字符串的ASCII值
	chr($ascii) 根据ASCII得到对应的的字符串

测试

	$str = 'b';
	echo ord($str);
	echo '<br/>';
	
	echo chr(98);

	结果
	98
	b

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-10-11/65630831.jpg)


**字符连接 .**


	<?php

	$str1 = 'hello ';
	$str2 = 'world';
	echo $str1,$str2;
	echo '<br/>';
	$newStr1 = $str1.$str2;
	echo $newStr1;
	echo '<br/>';

	$newStr2 = $str1.'~'.$str2.'!';
	echo $newStr2;

	?>

结果

	hello world
	hello world
	hello ~world!


**赋值运算符**

	mt_rand() 随机数

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-10-11/70960394.jpg)

[**比较运算符**](http://php.net/manual/zh/language.operators.comparison.php)

	== 值
	=== 值 类型

1012

**逻辑运算符**

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-10-12/25026138.jpg)

**错误抑制符**

抑制错误输出，通过@符号加到会产生错误的表达式之前


	<?php
	
	$a = 2;
	@settype($a,'test');
	@var_dump($test);


**三元运算符**


	exp1?exp2:exp3
	如果exp1正确，输出exp2，否则输出exp3


#### [流程控制](https://secure.php.net/manual/zh/language.control-structures.php)


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-10-12/54116729.jpg)

不同语言的基本的流程处理是类似的，if、elseif、else、

	max() 最大值


1013

	if elseif else
	switch() case

测试

	<?php
	//年月日 星期
	header('content-type:text/html;charset=utf-8');
	date_default_timezone_set('PRC');
	$day=3;
	if($day==1){
	    echo date("Y年m月d日").' 星期一<br/>';
	}elseif($day==2){
	    echo date("Y年m月d日").' 星期二<br/>';
	}elseif($day==3){
	    echo date("Y年m月d日").' 星期三<br/>';
	}elseif($day==4){
	    echo date("Y年m月d日").' 星期四<br/>';
	}elseif($day==5){
	    echo date("Y年m月d日").' 星期五<br/>';
	}elseif($day==6){
	    echo date("Y年m月d日").' 星期六<br/>';
	}elseif($day==0){
	    echo date("Y年m月d日").' 星期日<br/>';
	}else{
	    echo '非法日期<br/>';
	}
	
	
	// date($format,$time) 格式化日期时间
	// date_default_timezone_set($timezone) 设置时区
	// date_default_timezone_get($timezone) 得到默认时区
	//time() 得到当前的时间戳
	/*
	Y:4位的年份
	m:2位月份
	d:2位的日
	H:2位的小时
	i:2位的分钟
	s:2位秒数
	w:返回一周内的前几天 0-6(0代表星期日)
	*/
	echo @date("Y年m月d日 H:i:s");
	echo '<br/>';
	
	echo @date_default_timezone_get();
	echo '<br/>';
	
	echo @date("Y/m/d",time());
	
	echo '<hr/>';
	
	//得到当前一周的第几天
	$day = date('w');
	
	if($day==0){
	    $str = '星期日';
	}elseif($day==1){
	    $str = '星期一';
	}elseif($day==2){
	    $str = '星期二';
	}elseif($day ==3){
	    $str = '星期三';
	}elseif($day==4){
	    $str = '星期四';
	}elseif($day==5){
	    $str = '星期五';
	}elseif($day==6){
	    $str = '星期六';
	}
	
	echo date('Y年m月d日 ').$str;
	
	echo '<hr/>';
	
	
	$day = date('w');
	switch($day){
	    case 0:
	    $res = '日';
	    break;
	    case 1:
	    $res = '一';
	    break;
	    case 2:
	    $res = '二';
	    break;
	    case 3:
	    $res = '三';
	    case 4:
	    $res = '四';
	    break;
	    case 5:
	    $res = '五';
	    break;
	    case 6:
	    $res = '六';
	    break;
	    
	}
	
	$dateStr = date("Y年m月d日 星期{$res}");
	echo $dateStr;


结果

	2017年10月13日 星期三
	2017年10月13日 13:36:55
	PRC
	2017/10/13
	2017年10月13日 星期五
	2017年10月13日 星期五


1-10


1017

	eixt() 程序终止执行
	die()


[error_reporting](http://php.net/manual/zh/function.error-reporting.php)


计算器。。在Web页面展示出来，使用到`select`其中的if条件判断很是需要。

if的嵌套，条件语句，以及{}

	<!DOCTYPE html>
	<html>
	<head>
	<title>Test</title>
	<meta charset="UTF-8">
	</head>
	<body>
	    <h1>计算器</h1>
	    <form action="#" method="post">
	    num1:<input type="text" name="num1" id="">
	    <select  class="" name="op">
	        <option value="+">+</option>
	        <option value="-">-</option>
	        <option value="*">*</option>
	        <option value="/">/</option>
	        <option value="%">%</option>
	    </select>
	    num2: <input type="text" name="num2" id="">
	    <hr/>
	    <input type="submit" name="act" value="计算">
	    </form>
	    <?php
	    error_reporting(E_ALL&~E_NOTICE);
	    //接收数据
	    $act = $_POST['act'];
	    if($act){
	    $num1 = $_POST['num1'];
	    $num2 = $_POST['num2'];
	    $op = $_POST['op'];
	    //根据不同的操作符完成不同的选择
	    if(is_numeric($num1)&&is_numeric($num2)){
	        if($op=='+'){
	        $res = $num1+$num2;
	    }elseif($op=='-'){
	        $res = $num1-$num2 ;
	    }elseif($op=='*'){
	        $res = $num1*$num2;
	    }elseif($op=='/'){
	        //判断$num2是否为0
	        if($num2!=0){
	            $res = $num1/$num2;
	        }
	        //exit() 或die()程序终止执行
	        exit('0不能当作除数');
	    }elseif($op=='%'){
	        $res = $num1%$num2;
	    }else{
	    echo '非法操作<br/>';
	    }
	    echo "运算结果为: <br/>{$num1}{$op}{$num2}={$res}";
	    }else{
	        echo '非法操作数<br/>';
	    }
	    }
	    
	?>
	</body>
	</html>


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-10-17/86751290.jpg)



1018

9x9

	echo '<table border="1" cellpadding="0" cellspacing="0" width="80%">';
	for($i=1;$i<=9;$i++){
	    echo '<tr>';
	    for($j=1;$j<=$i;$j++){
	        echo "<td>{$j}&times;{$i}=".($i*$j)."</td>";
	    }
	    echo '</tr>';
	    }
	    echo '</table>';
	

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-10-18/86074354.jpg)


goto语句，但是不能跳入循环，可以跳出循环

	<?php
	
	echo 'starting...<br/>';
	
	echo '<hr/>';
	
	goto TEST;
	
	echo 'this is a test<br/>';
	
	TEST: //TEST标识符
	echo 'hello php<br/>';
	?>

结果

	starting...

	----------

	hello php


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-10-18/2617173.jpg)



##### PHP函数库

##### 自定义函数

定义--调用--

函数的参数，返回值，return。

变量：

局部变量(动态，静态static)，全局变量(函数外部)。

	&$a 取地址

参数的默认值

可变参数列表 

	func_num_args()    # 获取传递参数的数量
	func_get_args()		#获取传递参数的内容

参数的类型

	array 数组、对象、callable

可变函数

	is_callable()
	function_exists()

函数的嵌套

递归调用

	<?php
	function sum($n,$m){
	    if($m <= $n){
	        return $n;
	    }
	    // $n + ($n + 1) + ... + ($m - 1) + $m 
	    return sum($n,$m - 1) + $m;
	}
	echo sum(1,100);
	
	// sum(1,100) => sum(1,99) .... => sum(1,1) 
	// if ，回溯	

另一个

	<?php
	// 1,1,2,3,5,8
	function fbn($n){
	    if($n <= 2){
	        return 1;
	    }
	    return fbn($n-1) + fbn($n-2);
	}
	echo fbn(6);
	
	// fbn(6) => fbn(5) => fbn(4) ... => fbn(2)

匿名函数

1020

##### 系统函数

内置函数

##### [字符串函数](https://secure.php.net/manual/zh/ref.strings.php)

utf8 汉字3个字节

	strlen函数    用于获取字符串长度
	strtolower	将字符串转换为小写
	strtoupper	将字符串转换为大写
	ucfirst 将句子首字母转换为大写
	ucwords 将每个单词的首字母转换为大写
	str_replace 字符串替换 
	str_replace('a','b','java') 将java中的a替换为b
	str_ireplace 字符串替换，不区分大小写
	htmlspecialchars 预定义的字符串转换为HTML实体
	ltrim 删除字符串开始位置的空格或其他字符
	rtrim 结束为止
	trim 实现删除字符串开始和结束位置空格或其他字符
	字符串位置相关的函数
	strpos 将返回一个字符在另一个字符第一次出现的位置
	stripos 将返回一个字符在另一个字符第一次出现的位置 忽略大小写
	strrpos 将返回一个字符在另一个字符最后一次出现的位置
	strripos 将返回一个字符在另一个字符最后一次出现的位置 忽略大小写
	字符串截取函数
	substr 截取字符串 substr(string,start,length)
	strstr 搜索一个字符串在另一个字符串中第一次出现的位置，区分大小写，返回字符串中其余部分
	stristr
	strrstr 搜索一个字符串在另一个字符串中最后一次出现的位置，区分大小写，返回字符串中其余部分
	strrev 反转字符串 abc --》 cba
	md5 字符串加密
	str_shuffle 随机打乱字符串
	explode 使用一个字符串分隔另一个字符串---python split
	implode 将一个一位数组的值转换为字符串。数组的连接
	sprintf 格式化字符串


###### [数学函数](https://secure.php.net/manual/zh/ref.math.php)

	floor 舍一 3.01 --> 2
	ceil 进1 3.01 --》 4
	pow() 幂运算
	sqrt 平方根
	max	最大值
	min	最小值
	rand	随机数 rand(40,90)
	mt_rand	一个跟好的随机数
	round 实现四舍五入
	number_format 将以千位分隔符方式格式化数字
	fmod 除数的浮点数 7.8 /3 ==> 1.8

###### [日期函数](https://secure.php.net/manual/zh/ref.datetime.php)

	date()   两个参数
	date_default_timezone_set 默认设置时区 亚洲
	date_default_timezone_get 获取默认时区
	time() 时间戳 返回当前Unix时间戳
	strtotime()    获取想要时间的时间戳
	first day of ?? 某月第一天
	microtime 返回当前Unix时间戳和微秒数，计算一个程序执行的时间
	uniqid 生成唯一ID
	getdate 可以获取日期、时间信息
	

######	[数组初始](https://secure.php.net/manual/zh/book.array.php)

定义数组

1. 通过array()形式创建
2. 通过[]形式创建
3. 通过range()和compact()创建
4. 通过define()定义常量数组

索引数组

	array(
	3 => 'a',
	5 => 'b'
	);

关联数组(字典)
	
	array(
	'username'=>'king',
	'age'=>12

	)

如果已有下标为负数，则新添加下标从0开始

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-10-23/23025898.jpg)

二维数组。二维索引+关联的形式(数据库中查询出的记录就是这种方式)

	array(
		'users'=>('king','queen'),
		'age'=>array(12,34,56)
	);



数组，n维。

	const
	define

__使用数组__

取出其中的数据。增删改查数组的数据。。

	键名--键值    键地址	
	unset() 删除一个数组里的元素

__其他类型转换数组__

临时转换、永久转换

	(array)$var

	+ 合并数组

###### [数组的应用](https://secure.php.net/manual/zh/ref.array.php)

1024

遍历数组。

for

	<?php
	echo '<pre>';
	$arr = range('a','z');
	//print_r($arr);
	//echo $arr[0];
	
	for($i=0,$count=count($arr)-1;$i<=$count;$i++){
	    echo $arr[$i],'<br/>';
	}
	echo '</pre>';
	
	?>

foreach 遍历一维二维。不同版本有改变

	<?php
	
	// foreach两种形式
	
	$arr = [
	    5=>'a',
	    12=>'b',
	    -123=>'c'
	];
	foreach($arr as $v){
	    echo $v,'<br/>';
	}

结果

	a
	b
	c

键值

	foreach($arr as $k=>$v){
	    echo $k,'--',$v,'<br/>';

结果

	5--a
	12--b
	-123--c


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-10-24/84532427.jpg)

	key($array) 得到当前指针所在位置的键名
	
	current($array) 得到当前指针所在位置的键值
	
	next()
	
	prev() 前一个
	
	reset() 开始
	
	end() 结束

1025

	each
	list



##### cookie

cookie是服务器发送到用户浏览器并保存在浏览器上的数据，它会在浏览器下一次发起请求时被携带并发送到服务器上。

HTTP cookie是HTTP表头的组成部分

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-10-25/32942182.jpg)

作用。

1. 会话状态管理
2. 个性化设置（用户自定义设置）
3. 浏览器行为跟踪

	setcookie



##### [数组函数](https://secure.php.net/manual/zh/ref.array.php)


##### 可视化前端页面设置

	http://www.layoutit.com/
	
	http://www.layoutit.cn/


##### 会话控制

什么是session

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-10-25/83685680.jpg)

为什么使用session

	session_start()

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-10-25/27155153.jpg)

session 工作原理

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-10-25/86297380.jpg)

告诉服务器我是谁，然后服务器读取对应的文件。还会存储到cookie中。

sessionID 会话标识符

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-10-25/5441968.jpg)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-10-25/74848165.jpg)

##### [session函数](https://secure.php.net/manual/zh/ref.session.php)

	session_start(); 启动新会话或者重启现有会话
	session_id 获取/设置当前会话ID
	session_destroy 销毁一个会话的全部数据

案例

	<?php
	header('content-type"text/html;charset=utf-8');
	session_start();
	$_SESSION['username'] = 'mooc PHP';

会在服务器中生成一个session文件

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-10-25/76455274.jpg)

	sess_a83e785b9b37p996t74j88vdm4	

1-7

1030

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-10-30/88529263.jpg)

##### [文件函数](https://secure.php.net/manual/zh/ref.filesystem.php)

	<?php
	header('content-type:text/html;charset=utf-8');
	date_default_timezone_set('PRC');
	$filename = "./1.txt";
	
	//filetype($filename): 获取文件的类型
	echo '文件类型为:',filetype($filename),'<br/>';
	
	//filesize 文件大小,返回字节
	echo '文件大小为:',filesize($filename),'<br/>';
	//读取文件创建时间
	echo '文件创建时间为:',filectime($filename),'<br/>';
	echo @date('Y年m月d日 H:i:s',filectime($filename)),'<br/>';
	//filemtime($filename) 文件的修改事件
	echo '文件的修改时间:',date("Y/m/d H:i:s",filemtime($filename)),'<br/>';
	echo '文件的节点:',fileinode($filename),'</br>';
	//检测文件是否可读，可写，可执行is_readable(),is_writeable(),is_executable()
	var_dump(
	    is_readable($filename),
	    is_writable($filename),
	    is_executable($filename)
	);
	echo '<br>';
	//is_file 检测是否为文件
	var_dump(is_file($filename));

结果

	文件类型为:file
	文件大小为:23
	文件创建时间为:1509350917
	2017年10月30日 16:08:37
	文件的修改时间:2017/10/30 16:08:54
	文件的节点:0
	bool(true) bool(true) bool(false) 
	bool(true)


1031

	文件后缀
	文件创建时间
	文件是否存在
	文件重命名
	复制
	剪切

1101

	file_get_contents  读取
	file_put_contents 写入
	类似python中的
	with as

文件常用操作函数的封装
	

allow_url_fopen

1-9

2-7


文件的下载函数

	href 

##### [压缩与归档扩展](https://secure.php.net/manual/zh/refs.compression.php)
	
	异常处理，首先判断文件是否存在之类的问题
	使用if判断

##### [URL 函数](http://php.net/manual/zh/ref.url.php)


##### [数据库扩展](https://secure.php.net/manual/zh/refs.database.php)


##### [Mysql](https://secure.php.net/manual/zh/set.mysqlinfo.php)

PHP操作数据库的三种方式

	Mysql： 非永久连接，性能比较低
	MySQLi:永久连接，减轻服务器压力，支持支MySQL
	PDO: 能实现MySQLi的常用功能，支持大部分数据库

`php.ini`中有设置，连接的设置。

__MySQL连接数据库__

	1：连接数据库   mysql -uroot -p12344
	mysql_connect($server,$username,$passwd)
	2:选择数据库     use db;
	mysql_select_db($database_name)
	3:设置字符集 set names utf8
	mysql_set_charset($charset)
	4: mysql_query($query)
	5: 查询，需要将结果返回出来。 ,mysql_fetch_


mysql

	<?php
	header('content-type:text/html;charset=utf-8');
	//连接数据库
	$link = mysql_connect('localhost','root','root') or die('数据库连接错误');
	
	//var_dump($link);
	
	mysql_select_db('waf') or die('选择的数据库不存在');
	
	//设置字符集
	mysql_set_charset('utf8');
	
	//添加数据
	//$result = mysql_query("insert into news values(3,'java','Learn Java')");
	
	//修改 update users set name="" where id=1;
	
	//查询，返回结果资源应该返回给mysql_fetch_array()
	
	$result = mysql_query("select * from news");
	// $line = mysql_fetch_row($result);
	// $line = mysql_fetch_assoc($result);
	while($line = mysql_fetch_array($result)){
	    $data[] = $line;
	}
	var_dump($data);
	//var_dump($result);
	
	//关闭连接
	mysql_close($link);

[__MySQLi__](https://secure.php.net/manual/zh/book.mysqli.php)

	1:面向过程方式连接数据库
	$connect = mysqli_connect('host','username','password','database');
	2:执行SQL语句
	$result = mysqli_query($connect,$sql);
	3:获取结果集
	mysqli_fetch_all($result);


测试代码

	<?php
	header('content-type:text/html;charset=utf-8');
	$conn = mysqli_connect('localhost','root','root','test');
	mysqli_query($conn,'set names utf8');
	
	$sql = 'select * from news';
	$result = mysqli_query($conn,$sql);
	$data = mysqli_fetch_all($result,MYSQLI_ASSOC);
	var_dump($data);
	mysqli_close($conn);




浏览器乱码的情况

	header('content-type:text/html;charset=utf-8');


#####  数据库

20171108

	数据库
	判断数据库是否存在
	create database if not exists waf;
	show warnings;
	指定编码方式
	charcter set 'utf8'
	show create database imooc_o2o;  显示编码方式
	修改编码方式
	alter database test default character set 'utf8';
	得到当前打开的数据库
	select database()
	数据库操作的时候，也先判断一下是否存在
	创建表
	engine=存储引擎

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-8/59969222.jpg)

数据类型

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-8/85398739.jpg)

	create table if not exists user(

	)engine=innodb charset=utf8;

功能显示

	mysql> ? show
	Name: 'SHOW'
	Description:
	SHOW has many forms that provide information about databases, tables,
	columns, or status information about the server. This section describes
	those following:
	
	SHOW AUTHORS
	SHOW {BINARY | MASTER} LOGS
	SHOW BINLOG EVENTS [IN 'log_name'] [FROM pos] [LIMIT [offset,] row_count]
	SHOW CHARACTER SET [like_or_where]
	SHOW COLLATION [like_or_where]
	SHOW [FULL] COLUMNS FROM tbl_name [FROM db_name] [like_or_where]

查看数据表的信息

	show create table tal_name
	show create table o2o_category;
	desc table 表结构
	drop table 删除表
	

约束条件

	unsigened  无符号，没有负数
	not null 非空约束。插入的数据必须给值
	primary key 主键
	unique key 唯一性
	auto_increment
	foreign key 外键

表结构相关操作

	alter table tal_name add 字段名称 字段属性 
	drop 字段名称
	
修改、字段属性

	alter table table_name modify 字段名称 字段属性 

添加主键

	alter table table_name add primary_key(字段)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-9/78295063.jpg)

##### mysql存储引擎

	MyISAM 存储引擎
	单表支持的最大的数据量2的64次方记录
	每个表最多可以建立64个索引

	innoDB 存储引擎
	设计遵循ACID，支持事务，具有从服务崩溃中恢复能力，最大保护用户的数据
	支持外键，

给字段，表起别名。`as`

	where
	比较运算符  < > >=
	区间 between 1 and 3
	指定集合 [not]in
	逻辑运算符 and or
	like
	% 任意长度的字符
	分组
	group by 
	聚合函数
	count、sum.max等
	having 字句对分组结果进行二次筛选
	order by  id 排序  ASC DESC
	limit 限制结果集 limit 0,5 从0开始，取5条
	多表查询
	

