---
title: CSS 基础知识
date: 2017-08-15 13:20:46
tags:
		- CSS
categories:
		- 编程学习

---

前端基础知识--`CSS`。

<!-- more -->


#### CSS基本语法

CSS控制网站外观

JavaScript行为

CSS层碟样式表(Cascading Style Sheets)

样式单独在样式表

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-3/53110631.jpg)

CSS引用

	<style></style>



![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-3/63718086.jpg)

相同值可以写在一起。。

	p,h1,h2,h3{font-size:30px;}

注释

	/*注释语句*/


总结，css使用`<style>`标签，写在<head>里面。其是对HTML一些标签的修饰，比如颜色，字体。不用在HTML中直接写了。。


#### CSS使用方法

* 行内样式（内联样式）

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-3/36385116.jpg)

在对应的标签中直接对其进行修饰。。

	<h1 style="color:red";font-size:20px;>CSS层叠</h1>

* 内部样式表 （嵌入样式）

使用style标签，写在head中。。。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-3/42272296.jpg)


* 外部样式表

将css设置为一个独立的文件。。。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-3/77953655.jpg)


* 导入式

写在style的@import..


* 四种使用方法

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-3/55086642.jpg)


不同的开发形式来使用。。


#### CSS使用优先级

行内样式>内部样式>导入样式>链入link

就近原则。。。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-3/86992806.jpg)

0804

#### 3章 选择器

* 标签选择器

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-4/11007772.jpg)


* 类选择器

	.$123{
		color:blue;
		}
	<class="123">

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-4/55347793.jpg)

同一个类设置不同的样式规则，只需要在类的前面指明特定的标签。。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-4/80804202.jpg)

可以引用多个类

	<class a b>


* ID选择器

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-4/22674418.jpg)

	#two
	id="two"


* 全局选择器

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-4/96813476.jpg)

使用的时候需要谨慎。。

* 群组选择器

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-4/18600096.jpg)

	class和id区分大小写

* 后代选择器

每包含一层，一个缩进。。

结构关系。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-4/46614834.jpg)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-4/94358023.jpg)

*  伪类

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-4/45630259.jpg)


正常的时候是蓝色，点击黄色，点击后灰色。。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-4/11256585.jpg)


	 <style type="text/css">
	    
	        a:link{color:blue;} /*未访问状态*/
	        a:visited{color:gray;}  /*已访问状态*/
	        a:hover{color:green;}   /*鼠标悬浮状态*/
	        a:active{color:orange;}  /*激活状态*/
	   
	    }
	    </style>
	</head>
	<body>
	
	<a href="http://www.imooc.com" target="_blank">
	<p>前端学习</p>
	


伪链接类的顺序

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-4/22964718.jpg)



#### CSS继承

	<div>下的内容可以继承其中的属性。。

IEtest可以选择版本。。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-4/56632782.jpg)

#### CSS使用优先级

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-4/27316741.jpg)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-4/41578555.jpg)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-4/889832.jpg)

权值规则

根据权值的大小来显示对应的样式。。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-4/73722611.jpg)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-4/73722611.jpg)

通过设置优先级来显示CSS的样式。。


#### CSS样式的命名

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-4/73732013.jpg)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-4/77357301.jpg)


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-4/75781344.jpg)


id和class使用

id谨慎使用

适当使用class

0806

#### CSS字体和文本样式

字体和文本样式

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-6/54782149.jpg)

文字样式属性

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-6/32384037.jpg)

* `font-family`字体属性


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-6/8311538.jpg)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-6/30333341.jpg)

* `font-size`文字大小

相对单位 

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-6/31668771.jpg)

绝对单位

* 文字颜色


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-6/34135481.jpg)

[Websafecolors](http://www.bootcss.com/p/websafecolors/)

* 字体加粗

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-6/84418479.jpg)

* 设置斜体

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-6/95663273.jpg)

* 字体变形

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-6/35275843.jpg)

0806

* 文本样式


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-7/42722655.jpg)
	
	<div>可以设置为块


* 元素内容的垂直方式

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-7/69806503.jpg)

文本基线

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-7/79949499.jpg)

垂直居中应用


* 行高

行距离

	line-height 行与行之间的距离 px   

不同的浏览器有默认的行高

	font-size   字体大小

* 文本样式属性

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-7/88547165.jpg)

	word-sapcing  单词空隙


####     CSS样式应用

	<style type="text/css">
	    p{background-color:#ececec;height:150px;text-align:center;line-height:150px;}
	    .one{font-size:80px;color:#c9394a;font-weight:bold;}
	    .two{font-size:40px;color:gray;text-decoration:underline;letter-spacing:5px;vertical-align:15px;}
	    .three{font-size:80px;color:gray;font-style:oblique}
	    </style>
	</head>
	<body>
	<p><span class="one">开发</span>&nbsp;&nbsp;<span class="two">好好学习</span><span class="three">!</span></p>

	
![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-7/39882242.jpg)

经验技巧：

* 网页制作从整体到局部
* 借助手册和网络。。。使用属性


总结：

字体、大小、

#### CSS背景和列表

*  背景颜色

	background-color:颜色 | transpartent

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-7/22522978.jpg)


背景区包含的内容。。

*  背景图片

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-7/42239190.jpg)


0808

*  背景图片重复

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-8/4036715.jpg)

* 背景图片显示方式

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-8/44533132.jpg)


* 背景图片定位

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-8/47355963.jpg)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-8/62102721.jpg)


* 列表项样式

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-8/25485163.jpg)


CSS对列表的修饰。。

* 列表项标记

使用图片作为列表项标记

	list-style-image: url(image)

* 设置列表项标记位置

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-8/81603247.jpg)

* 列表样式缩写

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-8/31781360.jpg)

* 总结

背景样式、列表样式、


* 盒子模型


盒子模型的概念，

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-8/73061613.jpg)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-8/17908254.jpg)

0809

* 宽度   width    

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-9/55965954.jpg)

* 高度

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-9/76292921.jpg)

如果有统一的设置，先在首部的设置。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-9/56505507.jpg)

只有上面的可以设置高度和宽度。。

高度和宽度总结

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-9/81643286.jpg)

* 边框属性

边框宽度 (border-width)

边框颜色(border-color)

边框样式(border-style)

四个不同的方向

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-9/72004212.jpg)

上边框，左右下边框可以单独设置。。四个方向可以设置不同的属性

* 盒子内容到边框的距离

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-9/87405968.jpg)

* 内边距属性

值不能为负数

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-9/98124166.jpg)


上右下左

* 外边距属性  margin

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-9/35473083.jpg)

值可以设置为负值。。

margin 上 右 下 左

盒子的内边距以及外边距。。。盒子内外。。


	<style>

在前面加入 <!DOCTYPE HTML>采用标准的样式。。

* 元素分类

块级元素

	<p> <div> <h1>~<h6> <ul> <li> <ol> <dl> <dt> <dd>

行内元素，一行显示
	
	<span> <a> <em>

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-9/40733911.jpg)

可以将块级元素进行转变。。

内联元素设置。。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-9/14561796.jpg)

* display

margin 上边框、 border内边框

盒子模型

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-9/25810563.jpg)

display属性      

* 5 浮动

0810

float中的四个基本的参数

- float: left 左浮动
- float: right
- float: none
- float: inherit 继承浮动


标准流、定位、浮动、

* 浮动副作用的解决

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-10/53669700.jpg)

	clear 属性 
	clear


0811

* 定位

标准流

position: 其中的几个参数，需要灵活使用。。


* z-index

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-11/5072768.jpg)

* 网页布局     0814


行列布局。。

* 行布局  width

* 列布局     

一列一列

	float:left, float:right

三列布局，三列自适应

	container 
	float:left float:right float:left width:20%


* 混合布局

行与列

* 圣杯布局

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-14/97849656.jpg)

适合管理后台。。

* 双飞翼布局

* 行布局、列布局、混合布局、圣杯布局、

* 页面布局基础8

20170815

*案例分析

头部，主体，底部。。

三行布局

