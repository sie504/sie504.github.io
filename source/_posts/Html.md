---
title: HTML 基础知识
date: 2017-08-03 13:20:46
tags:
		- HTML
categories:
		- 编程学习

---

前端基础知识`HTML`

<!-- more -->


####2017/07/31

#### 基本
* 结构
* 标签
* 元素
* 属性
* 注释

头部、主体(网页内容)

在标签后面加入一个反斜杠闭合元素`<hr/>`    一个横杠 <hr/>  



    bgcolor="red"     背景颜色
	<p></p> 段落
	<hr/>        hr是一个水平线
	网页编码     <meta http-equiv="Content-Type" context="text/html" charse="utf-8">



#### 文字和段落

	<h1>一标题</h1>
	<p>段落文字</p>
	<p> 的属性，左对齐，右对齐，居中对齐。。
	align对其属性 left左对齐内容，right右对齐，center居中对齐，justify对每行进行伸展。
	<br/>    换行标签
	&nbsb; 空格
	<pre></pre>     保留空格，预格式化标签，在文本中输入的是什么，显示的就是什么。


	


*水平线

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-7-31/43367430.jpg)

	<hr width="80%" align="left" />

*修饰标签

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-7-31/50762288.jpg)

	<p><i>Hello</i></p>
	<p><b>Hello</b></p>
	<p><sub>Hello</sub>1111</p>
	<p><sup>Hello</sup>111111</p>


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-7-31/61261896.jpg)


* 特殊符号

对特殊符号，用代码表示。。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-7-31/46025425.jpg)

#### 列表标签

* 无需列表    ul标签

无序列表，type属性显示头部

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-7-31/81280364.jpg)

	
		<ul>
			<li>列表项</li>
			<li>列表项</li>
		</ul>


		<ul type="square">
		<li>Hello</li>
		<li>Hello</li>
		<li>Hello</li>
		</ul>

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-7-31/97701523.jpg)


* 有序列表   ol标签

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-7-31/8336201.jpg)

	<ol type="a">
	<li>Hello</li>
	<li>Hello</li>
	<li>Hello</li>
	</ol>
	<ol type="I">
	<li>Hello</li>
	<li>Hello</li>
	<li>Hello</li>
	</ol>

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-7-31/30240383.jpg)

* 定义列表

项目描述，标题及描述。。

	<dt>和<dd>同级标签
	<dl><dt><dd>组合使用

可以将图片和文字一起展示出来。。。


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-7-31/93061899.jpg)


*列表的使用场景

调试工具。。

根据具体场景。。

结构html+css表现+javascript行为

#### 图像和超链接

* 图像

图片的地址。。如果图片和代码在同一级目录，直接引用就行

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-7-31/33600530.jpg)

	<img src=""  />


alt属性可以代替图像显示在浏览器中的内容。。显示图片的命名。。
height/width 显示高和宽

* 超链接

a标签。。链接标签，站内链接和站外链接。。。

		<a  href="" > 内容</a>

* 空链接

		<a href="#" > 保留链接的效果


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-7-31/96696145.jpg)


	target 设置打来方式，_self(当前窗口)、_blank（新的窗口）
	title  链接提示文字


* 锚链接

在一个页面内的显示，如果有太多的内容，设置锚链接快速查找。。 定位的功能。。

定义锚的位置和锚名，设置寻找锚的链接。。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-7-31/86814728.jpg)


	寻找锚的名字 <a href="#name" >
	定义       <a href=""  name="" >



* 不同页面的锚链接

		<a href="html名字#">


* 邮箱链接

点击一个链接的时候，直接会在邮箱里面进行发送了。。

		<a href="mailto:邮件地址"> ....</a>

* 文件下载

		<a href="下载文件的地址">....</a>


规范

	编码
	<和>括起来
	成对出现
	单标签，没有结束标签


####20170801

#### 表格

理解`<tr>行，<td>列`

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-1/85347766.jpg)

	<table>
	<tr>  
     <!-- 一行 -->
	<td></td>
	</tr>

	</table>

创建一个2行3列

	<table border="1">
	    <tr>
	    <td>2015年</td>
	    <td>2016年</td>
	    <td>2017年</td>
	    </tr>
	    <td>2000</td>
	    <td>6000</td>
	    <td>8000</td>
	    <tr>
	    </tr>
	</table>


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-1/75736208.jpg)


* 表格的修改

添加列的时候，每一行都要添加。必须每行的列数的一样的。。

删除和添加一样的道理。。。

* 带表头

	<th></th>

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-1/8770602.jpg)

* 带标题的表格

	 <caption>测试</caption>

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-1/85873496.jpg)

* 带结构的表格

表头、主体、脚注，对表格结构化划分。。。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-1/89508744.jpg)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-1/32701328.jpg)


* 表格的属性

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-1/79425294.jpg)

外边框

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-1/19852394.jpg)

内边框

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-1/52638797.jpg)

表格的属性，内外边框

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-1/59402502.jpg)

* tr、td的属性

tr标签的属性

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-1/7078431.jpg)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-1/71735459.jpg)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-1/86725672.jpg)

表头、主体、脚注，有的时候可以直接在主体设置属性。。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-1/16381835.jpg)

* 跨列属性  colspan

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-1/97368639.jpg)

* 跨行属性

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-1/3074924.jpg)

一列占两行

注意： 涉及到跨行或跨列的情况，记得每一行一列数目是相同的。。

无论如何列每行的列数是相同的。。跨行和跨列，都是改变`td`的数量。。

* 表格的嵌套

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-1/9969647.jpg)

表格的属性，以及跨行和跨列，嵌套，其中的一些属性。。

* 表格的设计

先大到小


先总体到布局。。。

总结：尽量少的使用表格嵌套，尽量使用表格的跨行跨列。。

多思考表格的属性，无非就是宽高居中等位置的变化。。。

	<!DOCTYPE html>
	<html>
	<head>
	    <title>布表格局</title>
	</head>
	
	<body>
	    <table  width="100%" bgcolor="#f2f2f2">
	    <tr heightt="80px" bgcolor="#14191e"> <td>1111</td> </tr>  <!--页头 -->
	    <tr height="10px"> <td></td> </tr>  <!--上空行-->
	    <tr> <td>
	    <table align="center" width="1024px">
	        <tr>
	            <td width="240px" bgcolor="#fffff" valifn="top">
	            <table width="100%">
	            <tr><td align="center" height="60px">关于我们</td></tr>
	            <tr><td align="center" height="60px">团队介绍</td></tr>
	            <tr><td align="center" height="60px">人才招聘</td></tr>
	            <tr><td align="center" height="60px">讲师招募</td></tr>
	            <tr><td align="center" height="60px">常见问题</td></tr>
	            </table>
	            </td>  <!--左内容-->
	            <td width="20px"></td>  <!--左空隙-->
	            <td width="764px"  bgcolor="#fffff"> <!--右内容-->
	            <pre>
	            解决设置 ：
	在开机之前确保 USB光驱已经连接正常；
	更改光驱引导需要在 BIOS 里面修改，开机后按 F10 可以进入 BIOS ，下面是设置从光盘引导的方法
	第一步在 BIOS 里面切换到 Advanced 菜单，选择 Device Options
	1 、选择 USB legacy support 使用左右键把它置为： Enable.
	2 、按F10存盘；
	3 、切换到 Security 菜单，选择 Device Security ；
	4 、选择 CD-ROM boot and/or Floppy boot, 用左右键把它设置为： Enable.
	5 、按F10存盘；
	6 、切换到 File 菜单，选择 Save Changes and Exit ，然后按F10存盘，这时电脑会退出 BIOS 后自动重起；
	启动之后进入 BIOS ，下面更改启动顺序；
	1 、进入 setup
	2 、切换到 Advanced 菜单，选择 Boot Options ;
	3 、把 MutiBoot Option 用左右键把它设为 Enable ；
	4 、如果 USB 光驱已经连接好，那么这个设备会在 Boot Order 中显示出来；
	5 、调整启动顺序，将 USB 光驱调整为首位，然后按F10存盘设置；
	6 、切换到 File 菜单，选择 Save Changes and Exit ，然后按F10存盘，电脑会退出 BIOS 后自动重起；
	设置完成；
	 解决设置 ：
	在开机之前确保 USB光驱已经连接正常；
	更改光驱引导需要在 BIOS 里面修改，开机后按 F10 可以进入 BIOS ，下面是设置从光盘引导的方法
	第一步在 BIOS 里面切换到 Advanced 菜单，选择 Device Options
	1 、选择 USB legacy support 使用左右键把它置为： Enable.
	2 、按F10存盘；
	3 、切换到 Security 菜单，选择 Device Security ；
	4 、选择 CD-ROM boot and/or Floppy boot, 用左右键把它设置为： Enable.
	5 、按F10存盘；
	6 、切换到 File 菜单，选择 Save Changes and Exit ，然后按F10存盘，这时电脑会退出 BIOS 后自动重起；
	启动之后进入 BIOS ，下面更改启动顺序；
	1 、进入 setup
	2 、切换到 Advanced 菜单，选择 Boot Options ;
	3 、把 MutiBoot Option 用左右键把它设为 Enable ；
	4 、如果 USB 光驱已经连接好，那么这个设备会在 Boot Order 中显示出来；
	5 、调整启动顺序，将 USB 光驱调整为首位，然后按F10存盘设置；
	6 、切换到 File 菜单，选择 Save Changes and Exit ，然后按F10存盘，电脑会退出 BIOS 后自动重起；
	设置完成；
	            </pre>
	            </td>
	        </tr>
	    </table>
	    
	    </td></tr> <!--内容-->
	    <tr height="10px"> <td></td> </tr>  <!--下空行-->
	    <tr height="150px" bgcolor="#14191e"> <td>111111</td> </tr>  <!--页脚 -->
	    </table>
	</body>
	</html>


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-1/24667560.jpg)


2017-8-2

#### 表单

	<form> 标签

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-2/47850067.jpg)


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-2/48276368.jpg)

	<input/>
<input/> 标签  

<input type="类型属性" name="名称" ..../>

type属性

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-2/32091762.jpg)


案例1  type的password类型，密码不会显示出来

	<form>
    姓名: <input type="text" name="username" />
    密码: <input type="password" name="pwd" />
    <input type="submit" />
    </form>

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-2/95587718.jpg)


案例2    

* 单行文本域

		<form>
			<input type="text" name="..." />
		</form>



![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-2/89414243.jpg)

* 密码框

		<input type="password" name=".....">


* 文件域

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-2/66156252.jpg)

* 单选框

type="radio"

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-2/28570456.jpg)


* 复选框

type="checkbox"


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-2/28279169.jpg)


<input type="checkbox"  name="dx1" value="read"/>  

当你选中某一条的时候，value值会被传到服务器上。。。 name的值也会被服务器识别。。


可以设置默认选择的。。checked

* 按钮


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-2/29943970.jpg)

提交，普通按钮，重置。。。

* 图像域（图像提交按钮）

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-2/38960436.jpg)

* 隐藏域

某些信息不希望用户看到，但是需要传递给服务器。。。

		<input type="hidden" name=".." value="...">



	其中<input>的type属性，根据不同的场景使用不同的type。。。

form表单和table的一起使用。。这个是有标签的地方，使用table标签来标记其中的两个字段，一行两列。。可以通过lable也可以实现。。。

	<form action="192.168.86.194" method="post">
	<label>文章标题
	<input  type="text" name="title" /></label>
	<br/>
	<label>文章内容
	    <input type="text" name="content" />
	</label>
	<br/>
	    <input type="submit" value="提交">
	</form>

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-2/552772.jpg)

	<!DOCTYPE html>
	<html>
	<head>
	    <title>表单</title>
	</head>
	
	<body>
	    <form>
	    <h1 align="center">注册信息</h1>
	    <hr color="#336699"/>
	    <form>
	    <table width="600px" bgcolor="#f2f2f2" align="center">
	    <tr>
	        <td>姓名: </td>
	        <td><input type="text" name="username" size="25" maxlength="6" placeholder="请输入姓名"/></td>
	    </tr>
	    <tr>
	        <td>邮箱: </td>
	        <td><input type="text" name="email" value="@163.com"/></td>
	    </tr>
	    <tr>
	        <td>密码: </td>
	        <td><input type="password" name="paw" size="25" maxlength="6" placeholder="请输入密码"/></td>
	    </tr>
	    <tr>
	        <td>确认密码</td>
	        <td><input type="password" name="paw_confirm" size="25" maxlength="6" placeholder="请再次输入密码"/></td>
	    </tr>
	    <tr>
	        <td>上传图片</td>
	        <td><input  type="file"name="file"/></td>
	    </tr>
	    <tr>
	        <td>性别</td>
	        <td>男<input type="radio" name="sex" value="man"/>
	            女<input type="radio" name="sex" value="woman"/>
	        </td>
	    </tr>
	    <tr>
	        <td>爱好</td>
	        <td>读书<input type="checkbox"  name="dx1" value="read"/>
	            运动<input type="checkbox"  name="dx2" value="sport"/>
	            写作<input type="checkbox"  name="dx3" value="write"/>
	        </td>
	    </tr>
	    <tr>
	     <td>按钮</td>
	        <td><input type="button" value="来点我" name="button"/>
	            <input type="submit" value="submit" name="submit"/>
	            <input type="reset" value="reset" name="reset"/>
	            <input type="image" name="image_button" src="D:\4 Linux常用命令\学习资源\强力Django 和杀手级xadmin\前端html源码\images\check.jpg"/>
	        </td>
	       
	    </tr>
	    
	    </table>
	    
	    </form>
	</body>
	</html>


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-2/3871291.jpg)





* 下拉菜单和列表标签


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-2/6002446.jpg)

下来菜单

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-2/4725217.jpg)


	<select>
            <option value="bj">北京</option>
            <option value="sh">上海</option>
            <option value="sz">深圳 </option>
            <option value="gz">广州</option>
            <option value="sz">深圳</option>
        </select>


其中的北京只是在页面的展示，value才是传递到服务器上的。。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-2/54976316.jpg)

select的属性

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-2/13556505.jpg)

option 

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-2/83948697.jpg)


* 分组菜单

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-2/4569557.jpg)

	 <select name="city" size="6">
	            <option>--请选择--</option>
	            <optgroup label="北上">
	            <option value="bj" selected>北京</option>
	            <option value="sh">上海</option>
	            </optgroup>
	            <optgroup label="南下">
	            <option value="sz">深圳 </option>
	            <option value="gz">广州</option>
	          
	            </optgroup>
	        </select>


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-2/38212830.jpg)

* 多文本域

`textarea`

	<textarea>



![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-2/61221884.jpg)


*  表单的原理

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-2/26257917.jpg)


<form action="action.php" >

* 表单总结

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-2/28857963.jpg)
	
		<form>    表单
		<input> 表单输入标签，其中的type属性，很多种，根据提交的内容不同来设置，比如单行文本，多行文本，密码，按钮，文件等。。


单行文本、密码框，复选框，图像域，下拉菜单和列表标签，



#### 0803

div标签为网页划分独立的板块。。

给div命名，结构更加清晰，`<div id="版块名称">...</div>`




	<!DOCTYPE html>
	<html lang="en">
	<head>
	    <meta charset="UTF-8">
	    <title>html案例</title>
	</head>
	<body>
	    <!--头部内容-->
	    <div>
	    <!-- logo -->
	    <div><img src="html.jpg"  /></div>
	    <!--导航-->
	    <div>
	    <ul>
	        <li><a href="http://www.imooc.com">HTML</a></li>
	        <li><a href="http://www.imooc.com">JavaScript</a></li>
	        <li><a href="http://www.imooc.com">CSS</a></li>
	        <li><a href="http://www.imooc.com">PHP</a></li>
	        <li><a href="http://www.imooc.com">Python</a></li>
	        <li><a href="http://www.imooc.com">Java</a></li>
	        <li><a href="http://www.imooc.com">C</a></li>
	    </ul>
	    </div>
	    <!-- banner图 -->
	    <div><img src="html.jpg"  /></div>
	    </div>
	    <!--主体内容-->
	    <div>
	    <!-- 文章内容-->
	    <div>
	    <h1>如何成为一名优秀的前端工程师</h1>
	    <h6>2天前&nbsp;&nbsp;308浏览&nbsp;&nbsp;1评论</h6>
	    <p>
	    从 Y Combinator 的保罗·格雷厄姆到连续创业者史蒂夫·史密斯，到「精益创业」的支持者埃里克·里斯，创业者们都有一个明确的共识：创业公司必须不断成长才能蓬勃发展，仅仅生存下来是不够的。
	
	提升用户数量、扩大商业规模、获取更大的市场份额、增加利润和收入，再扩展到其他市场。创业的终极要义——要么成长，要么死亡。
	
	但事实上，走过创业初期，用户和业务规模平稳增长，但很多创业者会突然发现，增长并不像预期的那么快了，创业公司发展进入了瓶颈期。所以，当创业公司发展停滞不前时，该怎么做才能突破困境呢？在本期星课堂中，Appster 创始人 Josiah Humphrey 从帮助多家创业公司突破发展瓶颈的经验出发，分析 3 条经过验证的策略和见解。
	
	纵向增长 
	
	纵向增长，有时被称为「扩展到不同的垂直市场」，也就是通过增加销售额来获得更大的市场份额。
	
	Josiah Humphrey 解释说：「纵向增长意味着在现有业务范围内扩展你的产品/服务。通过深耕当前市场，创业公司有机会增加用户对产品的需求。」
	
	对于希望发展其业务的创业公司来说，纵向增长的吸引力在于它瞄准的是目标市场的动态、客户需求以及存在的问题。以下通过扩展纵向市场的两种主要方式。
	
	1、为现有产品增加更多功能、性能和服务：由于现有的服务未能满足客户的复杂需求，创业公司可以将新功能添加到当前产品中，用以吸引新客户。通过新服务解决更复杂的需求，引进更多的新客户，将有助于增加更多的收入来源。
	    </p>
	    <p>
	    从 Y Combinator 的保罗·格雷厄姆到连续创业者史蒂夫·史密斯，到「精益创业」的支持者埃里克·里斯，创业者们都有一个明确的共识：创业公司必须不断成长才能蓬勃发展，仅仅生存下来是不够的。
	
	提升用户数量、扩大商业规模、获取更大的市场份额、增加利润和收入，再扩展到其他市场。创业的终极要义——要么成长，要么死亡。
	
	但事实上，走过创业初期，用户和业务规模平稳增长，但很多创业者会突然发现，增长并不像预期的那么快了，创业公司发展进入了瓶颈期。所以，当创业公司发展停滞不前时，该怎么做才能突破困境呢？在本期星课堂中，Appster 创始人 Josiah Humphrey 从帮助多家创业公司突破发展瓶颈的经验出发，分析 3 条经过验证的策略和见解。
	
	纵向增长 
	
	纵向增长，有时被称为「扩展到不同的垂直市场」，也就是通过增加销售额来获得更大的市场份额。
	
	Josiah Humphrey 解释说：「纵向增长意味着在现有业务范围内扩展你的产品/服务。通过深耕当前市场，创业公司有机会增加用户对产品的需求。」
	
	对于希望发展其业务的创业公司来说，纵向增长的吸引力在于它瞄准的是目标市场的动态、客户需求以及存在的问题。以下通过扩展纵向市场的两种主要方式。
	<ol>
	<li>为现有产品增加更多功能、性能和服务：</li>
	<li>由于现有的服务未能满足客户的复杂需求，创业公司可以将新功能添加到当前产品中，</li>
	<li>用以吸引新客户。通过新服务解决更复杂的需求，引进更多的新客户，</li>
	<li>将有助于增加更多的收入来源。将有助于增加更多的收入来源。</li>
	</ol>
	    </p>
	    <p>
	    从 Y Combinator 的保罗·格雷厄姆到连续创业者史蒂夫·史密斯，到「精益创业」的支持者埃里克·里斯，创业者们都有一个明确的共识：创业公司必须不断成长才能蓬勃发展，仅仅生存下来是不够的。
	
	提升用户数量、扩大商业规模、获取更大的市场份额、增加利润和收入，再扩展到其他市场。创业的终极要义——要么成长，要么死亡。
	
	但事实上，走过创业初期，用户和业务规模平稳增长，但很多创业者会突然发现，增长并不像预期的那么快了，创业公司发展进入了瓶颈期。所以，当创业公司发展停滞不前时，该怎么做才能突破困境呢？在本期星课堂中，Appster 创始人 Josiah Humphrey 从帮助多家创业公司突破发展瓶颈的经验出发，分析 3 条经过验证的策略和见解。
	
	纵向增长 
	
	纵向增长，有时被称为「扩展到不同的垂直市场」，也就是通过增加销售额来获得更大的市场份额。
	
	Josiah Humphrey 解释说：「纵向增长意味着在现有业务范围内扩展你的产品/服务。通过深耕当前市场，创业公司有机会增加用户对产品的需求。」
	
	对于希望发展其业务的创业公司来说，纵向增长的吸引力在于它瞄准的是目标市场的动态、客户需求以及存在的问题。以下通过扩展纵向市场的两种主要方式。
	
	1、为现有产品增加更多功能、性能和服务：由于现有的服务未能满足客户的复杂需求，创业公司可以将新功能添加到当前产品中，用以吸引新客户。通过新服务解决更复杂的需求，引进更多的新客户，将有助于增加更多的收入来源。
	    </p>
	    <h6>作者:Lmlb&nbsp;&nbsp;时间:20170803</h6>
	    </div>
	    <!--链接区 -->
	    <div>
	    <!-- html-->
	    <dl>
	    <dt>HTML标记语言</dt>
	    <dd><img src="html.jpg"></dd>
	    <dd>HTML标记语言</dd>
	    </dl>
	    <!--css-->
	    <dl>
	    <dt>CSS</dt>
	    <dd><img src="html.jpg"></dd>
	    <dd>CSS</dd>
	    </dl>
	    <!--JavaScript-->
	    <dl>
	    <dt>JavaScript</dt>
	    <dd><img src="html.jpg"></dd>
	    <dd>JavaScript</dd>
	    </dl>
	    </div>
	    </div>
	    <!--页脚内容-->
	    <div><p>前端学习</p><div>
	
	</body>
	</html>
	  

  
