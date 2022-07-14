---
title: Javascript 基础知识
date: 2017-08-30 13:20:46
tags:
		- Javascript
categories:
		- 编程学习

---

前端基础知识 -- `Javascript`

<!-- more -->

20170816

基于对象和事件驱动的客户端脚本语言。

注释   //  单行注释    /**/ 多行注释


区分大小写，分号结束

标识符：不能以数字开头

变量：松散型

变量声明前面必须有： var

一次声明多个变量

* 数据类型 typeof
 
	Undefined Null Boolean Number String 



变量本身没有类型，类型取决于值。。

	console.log() 在控制台中打印
	console.log( typeof name) typeof检测数据类型

* Undefined

声明但没有给变量赋值。。


* null

一个空对象指针，undefined值是派生自null值的，所有undefined==null 的结果是true


* Number

整数，浮点数

NaN 非数值 `18 - "abc"`

任何涉及NaN的操作都会返回NaN。 NaN与任何值都不相等。包括NaN本身。


isNaN 检测n是否是非数值。。对接收的数值，先尝试转换为数值，在检测是否为非数值。


* 数值转换

	id = "16"
    id = Number(id)
    console.log(typeof id)

* parseInt() 转换 

必须以数字开头才可以转换   16abc --- 16

* parseFloat()     

第一个小数字有效

	id = "16.12.13abc"
    console.log(parseInt(id))
    console.log(parseFloat(id))

	16
	16.12

* string

由零个或者多个16为Unicode字符组成的字符序列

必须放在 " ' 之内


toString String

	str.toString() 将str转换为字符串
 	var abc = 11111；
    var aaa = abc.toString();
    console.log(typeof aaa)

* Boolean 

真假

除0之外的所有数字，转换为布尔类型都为true

除了 ""空的所有字符，转换为布尔值都为 true

null和undefined 转换布尔类型为false



1-5
20170817

* 表达式

操作符。。

隐形类型转换

++num1    先返回增加后的值

x3不是48的原因是，调用 x2--，先用其原始值30

		var x1 = 20,
	        x2 = 30,
	        x3 = --x1+x2--;
	        
	    console.log(x1);    //19
	     console.log(x2);   // 29
	      console.log(x3);  // 49
	    

	a+=5  a=a+5

	===  同时比较数据类型是否相等
	x = 10
	y = "10"

* 三元操作符

条件?执行代码1: 执行代码2

如果条件成立，执行代码1，否则执行代码2

 	var soce = 90;
    var result=(soce>=60)?"及格":"不及格";
    console.log(result)

	1.html:14 及格

* 逻辑操作符

	console.log( 80 && 55);
    console.log("hello" && 65 && "abc")

	55       abc

 &&

如果第一个操作数隐式类型转换后为true，则返回第二个操作数。。

如果第一个操作数隐式类型转换后为flase，则返回第一个操作数。。

只要有一个操作数是null，则返回null

只要有一个操作数是NaN，则返回NaN

只要有一个操作数是undefined，则返回undefined



||  

	var m;
    console.log(65 || "" || "abc");
    console.log(0 || 88 || "" || "abc")
    console.log("" || m); // undefined
    console.log("abc"*30 || 55-"def")

	65
	88
	undefined
	NaN

如果第一个操作数隐式类型转换后为true，则返回第一个操作数。。

如果第一个操作数隐式类型转换后为flase，则返回第二个操作数。。

如果两个操作数都是null，则返回null

如果两个操作数都是NaN，则返回NaN

如果两个操作数都是undefined，则返回undefined

* 条件语句

			if elseif else
		
			alert() 弹出警号对话框
		
			<script>
		    //声明
		    
		    var age = 15;
		    if(age<18){
		        alert("好好学习");
		    }
		    
		    </script>

* if 语句的嵌套

		 <script>
		    //声明
		    
		    var password = prompt("请设置您的密码");
		    //判断密码的长度，如果不是6位，否则
		    if(password.length!=6){
		        alert("请输入6位的数字密码");
		    }else{
		        //判断密码类型，数字和非数字
		        if(isNaN(password) == true){
		            alert("密码必须是数字")
		        }else{
		            alert("密码输入正确！")
		        }
		    }

* switch



0818

* 循环

	for、
	for (var i=1;i<=100;i++){
        document.write(i + '<br/>');      <br/> 换行


* 循环嵌套

外层必须为真，才可以执行内层。。直到内层的条件为假才去执行外层。。。

	<script>
	    //声明
	    for (var i=1;i<=3;++i){
	       document.write(i+'<br/>');
	       document.write('<hr>');
	       for(var j=1;j<=5;j++){
	        document.write(j+'<br/>');
	    
	       }
	    }
	
	    </script>

* while  do while

		 var j = 1
		    do{
		        if(j%2==0){
		            console.log(j);
		        }
		        j++;
		    }while(j<=10);



while

			var sum=0, n=1;
			    while(n<=100){
			        sum+=n;
			        n++;
			    }
			    console.log(sum);

* break continue

		var num=0;
		    for(var i=1;i<10;i++){
		    // 如果i是5的倍数，退出
		    if(i%5==0){        //如果不是5的倍数，就不执行break，所以console.log(i)继续执行
		        break;
		    }
		    console.log(i); //1,2,3,4
		    }

* 函数    经常反复执行的封装在一起

	function functionName()

		<script>
	    //声明一个函数
	    function myFun(){
	        alert("I am a function");
	    }
	    myFun();
	    </script>

* 函数返回值 return

		<script>
		    //声明一个函数
		    function add(n1,n2){
		        var sum = n1 + n2;
		        return sum;
		    }
		    
		    console.log(add(3,5));
		    alert(add(3,5));
		    
		    </script>

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-18/38760905.jpg)

*  函数参数 arguments 是一个数值包含参数（js的特性）



![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-18/97632395.jpg)

	<script>
	    //声明一个函数
	    function add(){
	        console.log(arguments.length);    //3
	        console.log(arguments[0]) //索引从零开始的正整数  1
	    }
	    add(1,2,3)
	   
	    </script>

* 函数练习，不管这个数组多长，都可以。。

		<script>
		    //求任意一组数的平均值
		    function getAvg(){
		        var sum = 0,len=arguments.length,i;
		        for (i=0;i<len;i++){
		            //console.log(arguments[i]);
		            sum += arguments[i];
		        }
		        return sum/len;
		    }
		    var avg = getAvg(5,6,7,8,9); //调用的赋值一个名称
		    console.log(avg);
		   
		    </script>

这个类似python 的 *
	
	def my_sum(*arg):
		return sum(arg)/len(arg)

	>>> def my_avg(*arg):
	    return sum(arg)/len(arg)
	
		my_avg(1,2,3)
		2

* 内置对象  0821

* Array: 数组存储一组数据。。length

数组的创建，读取，长度，遍历。。


	创建 Array   : new Array()      
	或者 字面量。。。
	cols = ["red","yellow","green"]

两种方法

	var nums = new Array(1,3,6,9);
    console.log(nums);
    var cols = ["red","yellow","green"];

数组的读写

	cols[0] 从0开始

数组可以声明一个对象，或者直接赋值[]，读取一样可以进行从下标开始读取。数组长度，length。。arr.length，长度是从最后一个索引开始的。。

	var cols = ["red","yellow","green"];
    cols[99] = "orange";
    console.log(cols);
    console.log(cols.length);
结果

	(100) ["red", "yellow", "green", undefined × 96, "orange"]
	100

遍历

	for(var i=0;i<cols.length;i++){
	        console.log(cols[i]);
	    }

* 数组方法 栈方法 push() unshift() pop() shift()

* push() 把值添加到尾部

	ayyayObject.push(newele1,newele2,...neweX)
	功能： 把它的参数顺序添加到arrayObject的尾部
	返回值：把指定的值添加到数组后的新长度。。

案例1

	var nums = new Array(1,3,6,9);
    var len = nums.push(2,3,4);
    console.log(len);     // 7

*  unshift() 把值添加到首部

	 	var nums = new Array(1,3,6,9);
	    var len = nums.unshift(2,3,4);
	    console.log(len);
	    console.log(nums);
结果

		7
		(7) [2, 3, 4, 1, 3, 6, 9]

* pop 删除

		语法： arrayObject.pop()
		功能： 删除arrayObject中的最后的一个元素
		返回值：被删除的那个元素
		
* shift 删除最前面的一个元素

* join 数组转换为字符串。。

		语法：arrayObject.join(separator)
		功能： 用于把数组中的所有元素放入一个字符串
		返回值：字符串

案例1

    var nums = [2,3,4];
    var str = nums.join();
    console.log(str);

* reverse()

	语法： stringObject.reverse()
	功能： 用于颠倒数组中的元素顺序
	返回值： 数组

案例：

	var  strs = ["a","b","c","d"];
    var newstr = strs.reverse()
    console.log(newstr);
    var newstr1 = strs.reverse().join()
    console.log(newstr1);

	(4) ["d", "c", "b", "a"]
	a,b,c,d

* 排序 sort()   //按字符串比较的

	数组中的每一项都是数值，sort()方法比较的也是字符串
	sort()方法可以接收一个比较函数作为参数

案例

	   var arr = [2,5,1,77,66,99];
	   arr.sort(function(a,b) {return b-a}); //降序
	   console.log(arr);   //

* concat()     连接两个或多个数组



		   var arr = [2,5,1,77,66,99];
		   var arr2 = ["a","b","c"];
		   var arr3 = arr.concat(arr2);
		   console.log(arr3);


* slice 截取

	从已有的数组中返回选定的元素
	0 1 2 3 4 下标  end 结束的位置-1

一个思想。。如果 start为负数，那就从这个负数加上这个数组的长度计算。。这个和python的有类似。。

	   var arr = [2,5,1,77,66,99];
	   var newarr = arr.slice(-5,3); // 1,3
	   console.log(newarr);

实现 b数组对a的复制。。

第一种

	var a = [2,5,1,77];
	   b = new Array();
	   for(var i=0;i<a.length;i++){
	        b.push(a[i]);
	   }
	   console.log(b);

第二种

		var a = [2,5,1,77];
	   var b = [].concat(a);   //数组。。
	   console.log(b);

第三种

	 var a = [2,5,1,77];
	   var b = a.slice(0);
	   console.log(b);

* splice 删除，插入、替换

	删除
	arrayObject.splice(index,count)
	功能
	删除从index处开始的零个或多个元素
	返回值
	含有被删除的元素的数组
	count是要删除的项目数量，如果为0，就不删除项目，不设置，则删除从index开始的所有值

删除

	var a = [2,5,1,77];
	   var b = a.splice(1,2);
	   console.log(b);     // 5,1

插入 

	arrayObject.splice(index,0,item1,item2...itemX)
	功能： 在指定位置插入值
	参数
	index 起始位置
	0 要删除的项数
	index1...indexX 要插入的项
	
案例

	 var a = [2,5,1,77];
	   var b = a.splice(2,0,"i",99);
	   console.log(a);
 	(6) [2, 5, "i", 99, 1, 77]

替换

	替换 
	arrayObject.splice(index,count,item1...itemX)
	功能： 
	在指定位置插入值，且同时删除任意数量的项
	参数
	index 起始位置
	count 要删除的项数
	item1..itemX 要插入的项
	返回值：从原始数组中删除的项

先删除，再插入

	   var a = [2,5,1,77];
	   var b = a.splice(1,2,"x","y","x");
	   console.log(b);
	   console.log(a);

	(2) [5, 1]
	[2, "x", "y", "x", 77]

indexOf()

	语法：
	arrayObject.indexOf(searchvalue,startINdex)
	功能：
	从数组的开头开始向后查找
	参数
	searchvalue 必须，要查找的项
	startIndex 可选，起始位置的索引
	返回值
	number，查找的项在数组中的位置，没有找到返回-1

案例：

		 var a = [2,5,1,77];
	   var pos = a.indexOf(1);
	   console.log(pos); //2

lastIndexof()   从末尾查找。。。。

用一个函数实现Indexof

		function ArrayIndex(arr,value){
	   for(var i=0;i<a.length;i++){
	    if(arr[i] ===value){
	        return i;
	    }
	   }
	   return -1;
	   }
	   var pos = ArrayIndex(a,5);  //函数调用的时候，把它的结果赋值一个参数
	   console.log(pos);    
   

#### 4-3 String 0822


字符串，取某个特定的子串，查找某个子串的位置，查看某个位置的子串，截取，从开始到结束，截取几个，字符串转换为数组，替换，大小写。。

字符串

* charAt() 与 charCodeAt()

charAt()

	语法：
	stringObject.charAt(index)
	功能：
	返回stringObject中的index位置的字符

案例

	 	var str = "hello world";
   		console.log(str.charAt(0)); // h

charCodeAt()

	语法：
	stringObject.charCodeAt(index)
	功能：
	返回stringObject中的index位置字符的字符编码

案例，返回的是编码

	 var str = "hello world";
	 console.log(str.charCodeAt(0));	// 104

* indexOf()

		语法：
		stringObject.indexOf("o") 
		功能：
		从一个字符串中搜索给定的子字符串，返回字符串的位置
		返回值：数值
		说明：如果没有找到该字符串，则返回 -1.

案例：

		var email = "python@sina.com";
	   console.log(email.indexOf("@")); // 6

* lastIndexOf() 

		语法：
		stringObject.lastIndexOf("o") 
		功能：
		从一个字符串中搜索给定的子字符串，从右边开始。。返回字符串的位置
		返回值：数值
		说明：如果没有找到该字符串，则返回 -1.

* slice 

		语法：
		stringObject.slice(start,end)
		功能：截取子字符串
		说明：
		start 必需，指定子字符串开始位置
		end，可选，表示到哪里结束，end本身不截取
		当参数为负数时，会将传入的负值和字符串的长度相加

案例：

	   var str = "hello world";
	   // 截取 orl
	   console.log(str.slice(7,10)); // orl
		
* substring()

		语法和slice完全一样、、
		当参数为负数时，自动将参数转换为0
		substring()会将较小的数作为开始位置，将较大的数作为结束位置

案例：

		var str = "hello world";
   		// 截取 orl
  		 console.log(str.substring(7,10));

		var str = "hello world";
	   // 截取 orl
	   console.log(str.substring(2,-5)); // (0,2) he

* substr()   截取的长度

		语法：
		stringObject.substr(start,len)
		功能：
		截取子字符串
		参数说明：
		start 必需，指定子字符串的开始位置
		len 可选，表示截取的字符总数，省略时截取至字符串的末尾
		start为负数时候，会将传入的负值和字符串的长度相加
		len为负数式，返回空字符串

案例，获取扩展名

		<script>
		    // 获取扩展名
		    var url = "http://baidu.com/index.html";
		    function getFileFormat(url){
		        //获取.在url中出现的位置，从右边开始
		        var pos = url.lastIndexOf(".");
		        // console.log(pos);  22
		        return url.substr(pos);
		    }
		        var formatName = getFileFormat(url);
		        console.log(formatName);
		    </script>        
			// .html

* split()   字符串转换为数组

		语法：
		stringObject.split(separator)
		功能：
		把一个字符串分隔成字符串数组
		返回值： Array
		说明：
		separator: 必须，分隔符

案例

		var str = "welcome-to-beijing";
	    //使用split将str转换为数组
	    var str1 = str.split("-");
	    console.log(str1);
		// ["welcome", "to", "beijing"]

* replace() 

		语法：stringObject.replave(regex/substr,replacement)
		功能：
		在字符串中用一些字符替换另一些字符，或替换一个和正则表达式匹配的子串
		返回值： String
		参数：
		regexp 必需
		replacement 必需，一个字符串值

* toUpperCase toLowerCase

案例，驼峰形式。。 将字符串拆分为数组，遍历数组，改变除第一个单词之外的所有单词的首字母，拼接字符串并返回。。

		<script>
		    function camelback(str){
		        // 通过-这个分隔符将str拆分为数组
		        var arr = str.split("-");
		        var newstr = arr[0];
		        // console.log(arr); //(3) ["border", "left", "color"]
		        for (var i=1,len=arr.length;i<len;i++){
		            var world = arr[i];
		            //console.log(world);
		            //将每一个单词的首字母转换为大写，连接剩余字符
		            newstr += world.charAt(0).toUpperCase()+world.substr(1);
		            //console.log(newstr);
		           
		            }
		             return newstr;
		    }
		    var camelbackFormat = camelback("border-left-color");
		    console.log(camelbackFormat);
		    
		    </script>

结果

			borderLeftColor

Math  

Date	


#### DOM 0823

javascript由ECMAScript(语法)、Browser Objects(DOM BOM) 特性组成。

*DOM查找方法(通过ID，标签名称)取出来可以设置样式。。动态设置

* document.getElementById


根据一个id获取元素

	语法：document.getElementById("id")
	功能：返回对拥有指定ID的第一个对象的引用
	返回值：DOM对象
	说明： id为DOM元素上id属性的值

案例

	 <div class="box" id="box">
	   元素
	   </div>
	   
	     <script>
	     //获取id为box的这个元素
	     var box=document.getElementById("box");
	     console.log(box);
	    </script>     //元素

结果

	元素 // 在屏幕打印出元素

* document.getElementsByTagName("tag")

按照标签来进行查找，比如 li


		语法：document.getElementsByTagName("tag")
		功能： 返回一个对所有tag标签引用的集合
		返回值：数组
		说明： tag为要获取的标签名称

案例

		<ul id="list1">
	   <li>123</li>
	   <li>456</li>
	   <li>789</li>
	   </ul>
	     <script>
	     // 获取页面中的所有的li
	     var lis = document.getElementsByTagName("li");
	     console.log(lis);
		结果： [li, li, li]

可以某个id下的标签

	document.getElementById("list").document.getElementsByTagName("li")

* 设置元素样式

		语法：ele.style.styleName = styleValue
		功能： 设置ele元素的CSS样式
		说明：
		ele为要设置样式的DOM对象
		styleName 为要设置的样式名称
		styleValue为设置的样式值
		属性是复合形式的话，采用驼峰形式比如 fontWeight

案例1
		
		<div class="box" id="box">
		   元素
		   </div>
		   
		     <script>
		     // 获取id为box的这个元素的文字颜色
		     var lis = document.getElementById("box");
		     box.style.color='#f00';


设置样式必须是一个dom对象。。

		<ul id="list">
		        <li>111111</li>
		        <li>222222</li>
		        <li>3333333</li>
		        <li>4444444</li>
		   </ul>
		     <script>
		     // 获取id为box的这个元素的文字颜色
		     var lis = document.getElementById("box");
		     box.style.color='#f00';
		     
		     var lis = document.getElementById("list").getElementsByTagName("li");
		     //遍历每一个li
		     for(var i=0,len=lis.length;i<len;i++){
		        lis[i].style.color ='#00f';
		        if(i==0){
		            lis[i].style.backgroundColor = "#ccc";
		        }
		        else if(i==1){
		            lis[i].style.backgroundColor = "#666";
		        }
		        else if(i==2){
		            lis[i].style.backgroundColor = "#999";
		        }
		        else {
		            lis[i].style.backgroundColor = "#333";
		        }
		     }


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-23/8904827.jpg)

* innerHTML 获取和设置标签之间的文本和html内容


		语法： ele.innerHTML
		功能： 返回ele元素开始和结束标签之间的HTML
		语法： eld.innerHTML = "html"
		功能： 设置ele元素开始和结束标签之间的HTML内容为html


案例

	<ul id="list">
	        <li><i>111111</i></li>
	        <li>222222</li>
	        <li>3333333</li>
	        <li>4444444</li>
	   </ul>
	     <script>
	     
	     
	     var lis = document.getElementById("list").getElementsByTagName("li");
	     //遍历每一个li
	     for(var i=0,len=lis.length;i<len;i++){
	        console.log(lis[i].innerHTML);
	     }


结果

			<i>111111</i>
		1.html:25 222222
		1.html:25 3333333
		1.html:25 4444444

* className  给一个元素添加类。。动态覆盖本身的。。

	语法:ele.className
	功能： 返回ele元素的class属性
	语法： ele.className = "cls"
	功能：设置ele元素的class属性为 cls


案例：把current添加进去了

		<style>
		        .current{background:#ccc;color:#f00;}
		    </style>
		var lis = document.getElementById("list").getElementsByTagName("li");
		     //遍历每一个li
		     for(var i=0,len=lis.length;i<len;i++){
		        console.log(lis[i].innerHTML);
		        lis[1].className="current";
		     }


* 获取属性

		语法： ele.getAttribute('attribute')
		功能：获取ele元素的attribute属性
		说明： ele是要操作的dom对象
			attribute是要获取的html属性


案例 标签自带的属性直接就可以获取了。。标签自定义属性用getAttribute

	<p id="text" align="center" data-type="title">文本</p>
	   <script>
	        var p = document.getElementById("text");
	        console.log(p.getAttribute('data-type'));
			console.log(p.align); // center
	   </script>
		// 输出结果为 title

* 设置属性

		语法： ele.getAttribute('attribute','value')
		功能：在ele元素上设置属性
		说明： ele是要操作的dom对象
			attribute为要设置的属性名称
			value为设置的attribute属性的值


案例

		 p.setAttribute('data-color',"red");

* 删除属性

		语法： ele.removeAttribute('attribute')
		功能：删除ele元素的attribute属性
		说明： ele是要操作的dom对象
			attribute是要删除的html属性名称

#### javascript事件

通过事件进行交互。

事件就是文档或浏览器窗口中发生的一些特定的交互瞬间。。

* html事件

直接在HTML元素标签内添加事件，执行脚本

	语法： <tag 事件= "执行脚本"> </tag>
	功能： 在HTML元素绑定事件
	说明：执行脚本可以是一个函数调用

案例1

	 <input type="button" value="弹出" onclick="alert('我是按钮')"> 
	绑定了一个点击按钮触发的事件

案例2，， 指定函数

开始按钮本身是蓝色，鼠标点击变为红色，离开返回蓝色。。调用函数来。。注意this的用法。。。

			<style>
		        .btn{width:140px;heigth:30px;line-height:30px;background:#00f;font-size"14px;text-align:center;border-radius:5px;margin-top:30px;}
		    </style>
		</head>
		
		<body>
		   <input type="button" value="弹出" onclick="alert('我是按钮')">
		   <!-- this 对div事件的引用-->
		   <div id="btn" class="btn" onmouseover="mouseoverFn(this)" onmouseout="mouseoutFn(this)">开始</div>
		   <script>
		        function mouseoverFn(btn){
		            //鼠标划过的时候，按钮的背景编程红色
		            btn.style.background="#f00";
		        }
		        function mouseoutFn(btn){
		            btn.style.background="#00f";
		        }
		   </script>


* this指向

在事件触发的函数中，this是对该DOM对象的引用

* 6-2 DOM0级事件 0824

1:通过DOM获取HTML元素

2：(获取HTML元素) 事件=执行脚本

	语法： ele.事件=执行脚本
	功能： 在DOM对象上绑定事件
	说明：执行脚本可以是一个匿名函数，也可以是一个函数的调用

案例：

可以看到使用getElementById()获取DOM元素。。 

		<style>
		        .lock{width:140px;heigth:30px;line-height:30px;background:#00f;font-size"14px;text-align:center;border-radius:5px;margin-top:30px;}
		        .unlock{width:140px;heigth:30px;line-height:30px;background:#666;font-size"14px;text-align:center;border-radius:5px;margin-top:30px;}
		    </style>
		</head>
		
		<body>
		   <input type="button" value="弹出" onclick="alert('我是按钮')">
		   <!-- this 对div事件的引用-->
		   <div class="lock" id="btn" >绑定</div>
		   <script>
		        // 获取按钮
		        var btn=document.getElementById("btn");
		        // 给按钮绑定事件,this是对该DOM元素的引用
		        btn.onclick=function(){
		            // 判断如果按钮是锁定，则显示解锁，否则显示锁定
		            if(this.className=="lock"){
		                this.className="unclock";
		                this.innerHTML="解锁";
		            }else{
		                this.className="lock";
		                this.innerHTML="锁定";
		            }
		        }
		        
		   </script>

不建议使用HTML事件原因

1. 多元素绑定相同事件时，效率低
2. 不建议在HTML元素中写JavaScript代码




##### 6-3 事件类型 0825

事件类型

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-25/16521438.jpg)

	onload 页面的内容全部加载后执行js代码
	onfoucs 事件用于 input标签 type为text、password  textarea标签
	onblur 
	onchange 事件，一般用于 select checkbox radion 上

获得焦点时候触发，离开触发。。焦点事件。。value获取值

		<style>
		        .box{
		            padding:50px;
		        }
		        .left,.tip{
		            float:left;
		        }
		        .left{
		            margin-right:10px;
		        }
		        .tip{display:none;font-size:14px;}
		         </style>
		        <script>
		            window.onload=function(){
		            // 获取文本框和提示框
		            var phone = document.getElementById("phone"),
		               tip = document.getElementById("tip");
		            // 给文本框绑定激活事件
		            phone.onfocus=function(){
		            //让tip显示出来
		                tip.style.display='block';
		        
		            }
		            //给文本框绑定失去焦点事件
		            phone.onblur = function(){
		                //获取文本框的值,value用于获取表单元素的值
		                //console.log(this);
		                var phoneVal = this.value;
		                //console.log(phoneVal);
		                //检测手机号码是否为11数字且不为非数字
		                // 如果输入正确，则显示对号图标，否则显示错号
		                if(phoneVal.length == 11 && isNaN(phoneVal)==false){
		                    tip.innerHTML='<img src="img/right.png">';
		                }else{
		                    tip.innerHTML='<img src="img/error.png">';
		                }
		            }
		            }
		        </script>
		   
		</head>    
		   
		<body>   
		    <div class="box">
		        <div class="left">
		        <input type="text" id="phone" placeholder="请输入手机号">
		        </div>
		        <div class="tip" id="tip">
		           请输入有效的手机号码
		        </div>
		    </div>
		</body>


onchange 事件

			<script>
		        //页面加载
		        window.onload = init;
		        // 初始化
		        function init(){
		            // 获取下来菜单
		            var menu = document.getElementById("menu");
		            //给菜单绑定change事件
		            menu.onchange=function(){
		               // console.log("abc");
		               //var bgcolor = this.value;
		               var bgcolor = menu.options[menu.selectedIndex].value;
		               //console.log(bgcolor);
		               // 如果bgcolor为空，则下面的不执行
		               if(bgcolor =="") {
		                document.body.style.background="#fff";
		               }else{
		               //设置body的 背景颜色
		               document.body.style.background=bgcolor;
		            }
		            }
		        }
		    </script>
		</head>
		
		
		<body>
		    <div class="box">
		        请选择喜欢的背景颜色
		        <select name="" id="menu">
		        <option value="">请选择</option>
		        <option value="#f00">红色</option>
		        <option value="#0f0">绿色</option>
		        <option value="#00f">蓝色</option>
		        <option value="#ff0">黄色</option>
		        <option value="#ccc">灰色</option>
		        
		        </sclect>
		    
		    </div>
		
		</body>		

事件的绑定

	<body>
	 
	    <div class="box" id="bo">拖动</div>
	    <script>
	    var box = document.getElementById("bo");
	    //绑定按下的事件
	    box.onmousedown=function(){
	        console.log("我被按下了");
	    }
	    //绑定移动事件
	    box.onmousemove =function(){
	        console.log("我被移动了");
	    }
	    //浏览器窗口尺寸大小发生改变时
	    
	    
	    </script>
	 </body>

设置id，然后获取其中的id。对这个id所在的标签进行一个事件的设置。。

##### 总结

javascript 事件的学习。获取html中的标签做出对应的操作。
事件很多，比如点击，提交，聚焦，离开聚焦等，鼠标在按钮上按下时触发，移动事件，松开事件，点击事件，浏览器窗口大小改变式，拖动事件。。

标签中要设置id，然后script获取其中的id，就是对这个标签做相应的操作。。
	

解析从上到下。。 `<script>`放在最后。


####6-4  0828 键盘事件与keyCOde属性

	onkeydown: 在用户按下一个键盘按键时发生
	onkeypress: 在键盘按键按下并释放一个键时发生
	onkeyup 在键盘按键被松开时发生
	keyCode 

小案例，一个文本框评论的时候，剩余多少个字。

		<body>
		    <div>
		        <p class="text">您还可以输入<span><em  id="count">30</em>/30</span></p>
		        <div class="input">
		            <textarea name="" id="text" cols="70" rows="4"></textarea>
		        </div>
		    </div>
		    <script>
		        // 获取文本框
		        var text=document.getElementById("text");
		        var total = 30;
		        var count=document.getElementById("count");
		        // 绑定键盘事件
		        document.onkeyup=function(){
		        // 获取文本框值的长度
		        var len = text.value.length;
		        // 剩余多少个字
		        var allow=total-len;
		        count.innerHTML=allow;
		        }
		        
		    </script>
		 </body>
	
![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-28/63343689.jpg)

##### bom

BOM(brower object model)浏览器对象模型

BOM对象有`window、navigator、screen、history、location、document、event`

window(全局对象)是浏览器的一个实例，在浏览器中，window对象有双重角色，它既是通过JavaScript访问浏览器窗口的一个接口，又是ECMAScript规定的Global对象。Global对象--全局对象。


	所有的全局变量和全局方法都被归在window上

window对象的方法

	语法： window.alert("content")
	功能： 显示带有一段消息和一个确认按钮的警告框
	语法： window.confirm("message")
	功能： 显示一个带有指定消息和OK及取消按钮的对话框
	返回值：如果用户点击确定按钮，则confirm() 返回 true 布尔值
			如果用户点击取消按钮，则confirm() 返回 false

* alert


* confirm确认对话框

		<body>
		    <div id="box">
		        <span>iphone8</span>
		        <input type="button" value="删除" id="btn">
		    </div>
		    <script>
		       //confirm
		       // 获取按钮
		       var btn=document.getElementById("btn");
		       btn.onclick=function(){
		            //弹出对话框
		            var result = window.confirm("您确定要删除吗？");
		            // console.log(result); 确定 true
		            if(result) {
		                document.getElementById("box").style.display="none"; //隐藏起来
		            }
		       }
		    </script>
		    
		 </body>

*  prompt 。 用户输入

	语法： window.prompt("text,defaultText")
	参数说明：
	text： 要在对话框中显示纯文本
	defaultText: 默认的输入文本
	返回值：如果用户点击提示框的取消按钮，则返回null
			如果用户点击确认按钮，则返回输入字段当前显示的文本


* open

	语法：window.open(pageURL,name,parameters)
	功能：打开一个新的浏览器窗口或查找一个已命名的窗口
	参数说明：
		pageURL 子窗口路径
		name 子窗口句柄
		parameters 窗口参数（各参数用逗号分隔）

打开一个html的时候，打开一个子窗口。。

* close 关闭子窗口

* 定时器

	javascript是单线程语言，就是所执行的代码必须按照顺序。。

*  超时调用

	语法： setTimeout(code,millisec)
	功能： 在指定的毫秒数后调用函数或计算表达式
	参数说明：
		code 要调用的函数或要执行的jaavascript
		millisec 在执行代码前需要等待的毫秒数

	说明： setTimeout() 只执行code一次，如果要多次调用，可以让code自身再次调用setTimeout()


案例

	<script>
        //setTimeout("alert('hello')",4000);
        var fnCall=function(){
            alert("world"); // 自定义函数
        }
        setTimeout(function(){
            alert("hello");      //匿名函数
        },2000)
        
        setTimeout(fnCall,5000);
    </script>


* 取消超时调用

	语法： clearTimeout(id_of_settimeout)
	功能： 取消由 setTimeout()方法设置的timeout
	参数说明：
		id_of_settimeout : 由setTimeout()返回的ID值，该值标识要取消的延迟执行代码块

案例

	 var timeout1 = setTimeout(function(){
            alert("hello");      //匿名函数
        },2000)
        
       clearTimeout(timeout1);

* 间歇调用

	语法：setInterval(code,millisec)
	功能： 每个指定的时间执行一次代码
	参数说明：
		code 要调用的韩式或要执行的代码串
		millisec 周期性执行或调用code之间的时间间隔，以毫秒计算

	
* 取消间歇调用

	clearInterval(id)

案例 	

	 <script>
        var inter1 = setInterval(function(){
            console.log("您好");
        },1000)
        //10秒之后停止打印
        setTimeout(function(){
            clearInterval(inter1);
        },10000);
    </script>


总案例

	 <script>
	        var  num = 1, max = 10, timer=null;
	        // 每隔1秒num递增一次，直到num的值等于max清除
	        timer = setInterval(function(){
	            num++;
	            if(num>max){
	                clearInterval(timer);
	            }
	            console.log(num);
	        },1000)
	    </script>

可以使用超时调用来实现上面的功能

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-28/93068620.jpg)

	<script>
	        var  num = 1, max = 10, timer=null;
	        // 每隔1秒num递增一次，直到num的值等于max清除
	        function inCrementNum(){
	            console.log(num);
	            num ++;
	            if(num<=max){
	                setTimeout(inCrementNum,1000);
	            }else{
	                clearTimeout(timer);
	            }
	        }
	        timer = setTimeout(inCrementNum,1000);
	    </script>

* 7-2 0829 BOM -- location

location对象提供了当前窗口中加载的文档有关的信息，还提供一些导航的功能，它既有window对象的属性，也是document对象的属性。

	语法：location.href
	功能：返回当前加载页面的完整URL
	说明：location.href 与 window.location.href 等价
	语法 location.hash 
	功能： 返回URL的hash，#号的内容。。如果不包含则返回空字符串
	语法： location.host
	功能： 返回服务器名称和端口号
	语法： location.hostname
	功能： 返回不带端口号的服务器名称
	语法： location.pathname
	功能： 返回URL中的目录和文件名
	语法： location.port
	功能： 返回URL中指定的端口号，如果没有则返回空字符串
	语法： location.protocol
	功能： 返回页面使用的协议
	语法： location.search
	功能： 返回URL的查询字符串，这个字符串以问号开头

位置操作

	改变浏览器位置的方法
	location.href 属性
	location对象其他属性也可以改变URL
	location.hash
	location.search
	语法： location.replace(url)
	功能：重定向URL
	说明：使用location.replace不会在历史记录中生成新记录
	replace 回退按钮将不可使用。。。
	location.reload()
	语法： location.reload()
	功能： 重新加载当前显示的页面
	说明： location.reload()有可能从缓存中加载
			location.reload(true) 从服务器重新加载
			建议放在代码最后面



replace

		<script>
	        setTimeout(function(){
	            //location.href='select.html';
	            // window.location='select'
	            location.replace('select.html');
	        },1000)
	         
	   </script>


##### history

	语法： history.back()
	功能： 回到历史记录的上一步
	说明：相当于使用了 history.go(-1)
	语法： history.go(-n)
	功能： 回到历史记录的前n步
	语法： history.go(n)
	功能： 回到历史记录的后n步
	语法： location.forward()
	功能： 回到历史记录的下一步
	说明：相当于使用history.go(1)

后退

		<p><input type="button" value="后退" id="btn"></p>
		    <script>
		        var btn = document.getElementById("btn");
		        //点击btn按钮回到历史记录的上一部
		        btn.onclick=function(){
		            history.back();
		        }
		    </script>

history

index9.html

	<body>
	   <a href="index10.html">index10.html</a>
	   <p>这是index9.html</p>
	    
	 </body>

index10.html


	<a href="index11.html">index11.html</a>
	 </head>   
	 
	 <body>
	  <p>这是index10.html</p>

index11.html

		<p><input type="button" value="后退" id="btn"></p>
		  <script>
		        var btn = document.getElementById("btn");
		        //点击btn按钮回到历史记录的上一部
		        btn.onclick=function(){
		            //history.back();
		            history.go(-1);
		        }
		  </script>

在index11.html的时候，go(-1) 返回的是 index10.html, go(-2)返回的是index9.html

* forward

	location.forward()
	回到历史记录的下一步
	相当于使用了history.go(1)


按照顺序进行排序。。go -1 后退 1 前进

* 7-4 0830 navigator



		userAgent 识别浏览器名称、版本、引擎以及操作系统等信息的内容
		var a = navigator.userAgent;
	    alert(a);
	
函数封装..getBrower() 来验证是什么浏览器。字符串函数来判断其中的内容，输入是什么浏览器。。

	<script>
       //检测浏览器类型
       function getBrower(){
       // 获取userAgent属性
       var explore = navigator.userAgent.toLowerCase(),brower;
       if(explore.indexOf("mise")>-1){
        brower = "IE";
       }else if(explore.indexOf("chrome")>-1){
            brower = "chrome";
       }else if  (explore.indexOf("safari")>-1){
            brower = "safari";
       }
       return brower;
       }
       var ex = getBrower();
       alert("您当前使用的是: "+ex+" 浏览器");
	
* screem

	语法：scree.availWidth
	功能： 返回可用的屏幕宽度
	语法： screen.avaiHeight
	功能： 返回可用的屏幕高度

窗口文档的高度和宽度， innerHeight innerWidth

屏幕的高度和宽度 availHeight availWidth

案例

	<script>
       console.log("页面宽:" + screen.availWidth);
       console.log("页面高:" + screen.availHeight);
       
   		</script>

		 console.log("page宽:" + window.innerWidth);
       	console.log("page高:" + window.innerHeight);
       

#### javsscript

	alert 弹出对话框
	prompt 弹出输入框，点击确定，返回输入内容，点击取消，返回null
	string.length 获取string的长度
	isNaN 检测是否是数字
	new Date().getDay() 星期
	document.write("内容") 向浏览器输出内容
	变量+字符串

案例：

	


案例

	var age=prompt("请输入您的年龄");
	console.log(age);

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-17/79123031.jpg)



----------
indexOf() 方法返回某个指定的字符串值在字符串中首次出现的位置，如果没有出现过，返回-1.
