---
title: java 基础知识学习
date: 2017-11-13 13:20:46
tags:
		- java
categories:
		- 编程学习

---

`Java`基础知识学习。

<!-- more -->


#### 前言

20171113

大学学过一些`java`基础，但是什么都不会编写，就这样毕业了。参加工作后，看到身边的程序员大多是做`java`开发的，`java`的需求量还是很大的，尤其是大公司。让自己下决心学习的原因是，`2017`年爆出了很多的`Struts2`命令执行漏洞，作为一个有志于在安全研究上有一定建树的伙伴来说，不能总等到别人分析结果或者`Poc`出来后，才可以支持公司产品或者学习此漏洞。

今天狠下心来，买了一套学习视频，开始学习。希望能坚持下去，多去从代码层理解学习漏洞。20171113

#### Java初识

	Eclipase 开发工具
	面向对象的程序设计语言
	Sun-->Oracle
	JDK 8.0 工具包
	JVM Java虚拟机
	JVM是Java平台无关性实现的关键

	程序执行过程
	源文件-->编辑器-->字节码-->解析器--执行

	JDK Java语言的软件开发包
	Javac 编译器，将源程序转换为字节码
	java 运行编译后的程序

	JRE Java Runtime Environment
	面向使用者

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-13/51639628.jpg)

	Java SE 桌面程序
	Java EE Web程序 企业版
	

**执行流程**

保存的名字为类的名字`Hello.java`

	class Hello{
	    public static void main(String[] args){
	        System.out.println("Hello World");
	    }
	}


然后运行

	D:\java>javac Hello.java  //生成Hello.class 二进制文件

	D:\java>java Hello    //不用带扩展名
	Hello World


**命令行参数**

类似于`python`中的`sys.args`

	class ArgsDemo{
    public static void main(String[] args){
        System.out.println(args[0]);
        System.out.println(args[1]);
    }
}

运行

	D:\java>java ArgsDemo Come java
	Come
	java


**java程序结构**


	class Hello{  //类
	    public static void main(String[] args){ //主方法
	        System.out.println("Hello World");
	    }
	}

	class 定义类 {}
	主方法
	定义函数 String[] 不能变
	System.out.println 输出
	

**eclipse**

[JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html)

[eclipse](https://www.eclipse.org/downloads/packages/eclipse-ide-java-developers/oxygen1a)



	An error has occurred.See the log file 

安装环境的时候，出现了一些问题。

64位的JDK 必须对应 64位的eclipse，最好都用最新的版本。

2017114

创建一个工程(Java Project)--一个包(Package)--一个类(在包的名字上右键点击class)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-14/49183256.jpg)

	字体设置

	选择Window->Preferences,然后选择General->Appearance->Colors and Fonts->Java->Java Editor Text Font

	如何调节Eclipse下console输出字体的大小？？


	打开window - preferences-- general - appearance - colors and fonts --debug - console font 就可以调节了。


##### 总结

	JDK JRE JVM 三者关系
	JRE = JVM + JavaSE标准类库
	JDK = JRE + 开发工具集(例如Javac编译工具)


#### Java常量与变量

	标识符
	关键字
	变量
	数据类型
	数据转换
	常量

##### 标识符
	

标识符： 字母、数字、下划线、和美元符($)。不能以数字开头。严格区分大小写，不能是Java关键字和保留字，最好能反应其作用


##### 关键字

	package
	class
	static
	void

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-14/61909798.jpg)


##### 变量

数据存储，使用变量

变量类型、变量名和变量值

	变量命名规则：
	满足标识符命名规则
	符合驼峰法命名规范 age_Test
	简单

	类的命名规则
	满足Pascal命名规范。 第一个字符必须大写

**数据类型**

	基本数据类型

	数值型(整数类型 byte short int long、浮点类型 float double)
	字符型
	布尔型

	引用数据类型(类、接口interface、数组)


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-14/17166233.jpg)

	八进制
	0开头，包括0-7的数字
	十六进制
	0x 0-9 a-f
	
变量声明

	int n; 声明整型变量 n
	long count; 声明长整型
	= 进行赋值，将运算符右边的赋值给左边的
	定义变量的同时赋值，变量的初始化


[单精度与双精度是什么意思，有什么区别？](https://www.zhihu.com/question/26022206)

3-7

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-15/69246072.jpg)

**字符型字面值**

	单引号引起来的 一个字符 'a'
	char a = 'a';

	ASCII 编码
	Unicode 编码 目标支持世界上所有的字符集
	在值前面加上 \u

	char c = '\u005d';
		System.out.println("c="+c);

	c=]


**布尔类型**

	true
	false


**字符串**

	"" 双引号

	print 不换行
	println 换行
	1.23e5 1.25 10*5 5次方


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-15/85927890.jpg)


**类型转换**

	自动类型转换
	实线：无信息丢失的数据类型转换
	虚线：可能在转换时，出现精度丢失

	
![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-15/9288468.jpg)

强制类型转换

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-15/16343054.jpg)

**常量**

	final 大写字母
	final double PI = 3.1415;

		
![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-15/62347742.jpg)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-15/81224365.jpg)	
	


##### 运算符



1115

表达式

复合赋值运算符，算术运算符、关系运算符

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-16/55723091.jpg)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-16/18387052.jpg)

前后用`+`连接。

	++    自增
	--	  自减

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-16/85619584.jpg)

关系运算符

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-16/1074084.jpg)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-16/77253128.jpg)


在条件结构中主要应用关系运算符


	if(条件){
	<语句块>
	}
	else
	{
	<语句块>
	}


终端输入

	public static void main(String[] args) {
			// TODO Auto-generated method stub
			System.out.println("请输入一个整数:");
			Scanner s = new Scanner(System.in);
			int n = s.nextInt();
			if(n%2==0) {
				System.out.println("n是偶数");
			}else {
				System.out.println("n+是奇数");
			}
		}

从键盘接收数据，输入

	Scanner s = new Scanner(System.in);
			int n = s.nextInt();


逻辑运算符

	|| && !

&& ||  短路运算符

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-17/34847549.jpg)


条件运算符。三元运算符

	布尔表达式?表达式1：表达式2

	max=a>b?a:b;


基本的语法也就是数据类型，运算符，以及表达式和循环等。不同语言的这些基本语法都是类似的，但是，每种语言会有自己的特性，学会这种特性，就能掌握这种语言。

**运算符优先级**

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-17/67515185.jpg)


	System.out.println("请输入一个年份:");
		Scanner s = new Scanner(System.in);
		int year = s.nextInt();
		if(((year%4==0)&(year%100!=0))|(year%400==0)){
			System.out.println(year+"是闰年");
		}else {
			System.out.println(year+"不是闰年");
		}
	}
	

##### Java流程控制之选择结构

	控制流程三大语句： 顺序、选择、循环

**顺序结构**

	代码顺序按照顺序执行

**选择结构**

	if if else
	多重if
	嵌套 if
	switch结构


**循环结果**

	while do-while for
	循环嵌套


多重if结构

	if
	else if 
	else

嵌套if结构

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-20/63486200.jpg)

switch结构

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-20/42440192.jpg)

**循环结构**

	while do-while for 循环嵌套


初始化变量，判断`while`。

2-2

	int n=1;
		int sum=0; //sum存放和
		while(n<=5) {
			sum+=n;
			n++;
		}
		System.out.println("1到5的累加和:"+sum);


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-21/96339486.jpg)

	//循环输出26个英文字母，两行输出
		char ch='a';
		int count = 1;
		while(ch<='z') {
			System.out.print(ch+"");
			if(count%13==0)
				System.out.println();
			count++;
			ch++;
		}

结果

	abcdefghijklm
	nopqrstuvwxyz


__do while__

局部变量使用之前，要初始化。

	package com.Imooc;
	
	import java.util.Scanner;
	
	public class Hello {
	
		public static void main(String[] args) {
			//猜字游戏
			// 给定要猜的数字
			//循环结构
			//循环进行条件：猜测的数据和实际的值不等
			//循环体的内容，输入实际值，及进行判断
			//输出猜到的数值
			int number = 6;
			int guess;
			System.out.println("猜测一个介于1到10之间的数");
			do {
				System.out.println("请输入您猜测的数");
				Scanner sc = new Scanner(System.in);
				guess = sc.nextInt();
				if(guess>number) {
					System.out.println("太大");
				}else if(guess<number){
					System.out.println("太小");
				}
			}while(number!=guess);
			System.out.print("您猜中了！答案为"+guess);
	}
	}
			

使用随机数生成数值

	int number = (int)(Math.random()*10+1); //[0,1]


__for__

	for(a;b;c){
		语句;
	}

局部变量只在大括号里面有效。

	for循环
	第一个变量，必须定义，可以在循环内，或外部

循环嵌套


	package com.Imooc;
	
	public class for_demon {
		public static void main(String[] args) {
			int m = 1; //外层循环的变量
			int n = 1;
			System.out.println("输出四行四列");
			while(m<=4) {
				//内层循环，控制每行输出几个*号
				n = 1;
				while(n<=4) {
					System.out.print("*");
					n++;
				}
				System.out.println();
				m++;
			}
			
		}
	
	}


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-21/16228340.jpg)

阶乘

	package com.Imooc;
	
	public class jiecheng {
	
		public static void main(String[] args) {
			// TODO Auto-generated method stub
			int s=1,sum=0;
			for(int i=1;i<=4;i++) {
				s=1; //每次s的值都要设置为1，下面的s会变化
				for(int j=1;j<=i;j++) {
					s*=j; //s存放阶乘计算的结果
				}
				sum+=s;
			}
			System.out.println("1!+2!+3!+4!="+sum);
	
		}
	
	}
	//1!+2!+3!+4!=33


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-21/44158367.jpg)

continue

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-21/82928405.jpg)

__Debug调试__

调试的作用。逻辑错误。


设置断点，程序执行到这个时候，停止。

调试-Debug

然后，一步一步调试。。

F6是进行单步调试的快捷键

多个断点。

	流程控制语句：顺序、选择、循环。



tyb.sdibt.edu.cn/admin/admin_news_pl_view.asp?id=23

##### 数组

相同类型的数据按顺序组成的一中引用数据。

数组声明

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-22/34715351.jpg)


数组创建

	int[] arr;
	arr = new int[10]
	创建一个长度为10的整型数组

第二种

	声明的同时创建数组
	int[] arr = new int[10];

数组在内存中一个连续的内存空间。

数组的初始化，声明的同时给数组赋值，长度就是给的元素的个数

	int a = {1,2,3,4,5,6,7}
			a[0] a[1]
		a.length //长度

输入求累加和

	package com.Imooc;
	
	import java.util.Scanner;
	
	public class array_demo {
	
		public static void main(String[] args) {
			// TODO Auto-generated method stub
			//声明一个整型数组
			int[] intArray;
			int[] a = new int[5];
			Scanner sc = new Scanner(System.in);
			for(int i=0;i<a.length;i++) {
				System.out.println("请输入第"+(i+1)+"个元素");
				a[i] = sc.nextInt();
				
			}
			System.out.println("数组元素的内容为:");
			for(int i=0;i<a.length;i++) {
				System.out.print(a[i]+" ");
			}
			//数组累加和
			int sum = 0;
			for(int i=0;i<a.length;i++) {
				sum = sum + a[i];
			}
			System.out.println("数组累加和为:"+sum);
		}
	
	}


	1 3 4 5 6 数组累加和为:19

for和数组的使用

	 foreach 循环
	for(int n:a)  a数组

	两个变量替换
	int a=3,b=5
	int temp
	temp = a;
	a=b;
	b=temp;

冒泡排序

第一次比较把最大的放在末尾了。比较的次数越来越小。



	package com.Imooc;
	
	public class max_demo {
	
		public static void main(String[] args) {
			// TODO Auto-generated method stub
			int[] a= {34,54,12,32,56,17};
			System.out.println("排序前的数组元素为");
			for(int n:a) {
				System.out.print(n+ " ");
			}
			System.out.println();
			int temp;
			//外部循环判断次数，内部判断最大值
			for(int i=0;i<a.length-1;i++) {
				// 内层循环控制每次排序
				for(int j=0;j<a.length-i-1;j++) {
					if(a[j]>a[j+1]) {
						temp = a[j];
						a[j] = a[j+1];
						a[j+1] = temp;
					}
				}
			}
			System.out.println("从小大到的元素为:");
			for(int n:a) {
				System.out.print(n+ " ");
			}
			
		}
	
	}

__二维数组__

	int[][] num = {{1,2,3},{4,5,6},{7,8,9}};

	int[][] num = {{1,2,3},{4,5,6},{7,8,9}};
		for(int i=0;i<num.length;i++) {
			for(int j=0;j<num[i].length;j++) {
				System.out.print(num[i][j]+" ");
			}
			System.out.println();

	1 2 3 
	4 5 6 
	7 8 9 

数组存储数据的一种方式，python中有(元组)、[列表]、｛字典｝

##### 方法

	类 对象 方法 
	对象名调用方法
	方法就是解决一类问题的代码

方法的声明

	访问修饰符 返回类型 方法名(参数列表){
		方法体
	}

	void 不返回任何的内容

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-22/61221149.jpg)

有返回值，就是`return`

__无参无返回值方法__


	package Method;
	
	public class Method1 {
	
		// 打印输出星号
		public void printStar() {
			System.out.println("*******");
		}
	
		public static void main(String[] args) {
			//类 对象名字 = new 类();
			//调用 对象名字.方法();
			Method1 myMethod = new Method1();
			myMethod.printStar();
			System.out.println("Welcome to Java World!");
			myMethod.printStar();
	
		}
	
	}

__无参带返回值__

	Scanner 类
	next()方法，返回值是String类型

案例

	package Method;
	
	public class m2 {
		//求长方形面积
		public int area() {
			int length = 10;
			int width = 5;
			int getArea = length*width;
			return getArea; //返回语句
		}
		public static void main(String[] args) {
			m2 rc = new m2(); //实例化一个类， new m2()
			System.out.println("长方形的面积为 "+rc.area());
	
		}
	
	}
	
	长方形的面积为 50

__带参无返回值__


	package Method;
	
	public class m3 {
	
		//求最大值  a,b局部变量
		public void max(float a,float b) {
			float max;
			if(a>b) {
				max=a;
			}else {
				max=b;
			}
			System.out.println("两个数的最大值为"+max);
		}
		public static void main(String[] args) {
			m3 Max = new m3();
			int a=5,b=3;
			Max.max(a, b);
			float m=5.6f,n=3.8f;
			Max.max(m, n);
			Max.max(12.8f, 6.6f);
		}
	
	}

	两个数的最大值为5.0
	两个数的最大值为5.6
	两个数的最大值为12.8


__带参有返回值__

	package Method;
	
	public class m4 {
	
		public int fac(int n) {
			int s = 1;
			for(int i=1;i<=n;i++) {
				s*=i;
			}
			return s;
		}
		//主方法
		public static void main(String[] args) {
			m4 Fac = new m4();
			int n = Fac.fac(3);
			System.out.println("3!="+n);
			int sum=0;
			//求 1!+2!+3!+4!+5!
			for(int i=1;i<=5;i++) {
				int fac = Fac.fac(i);
				sum+=fac;		
			}
			System.out.println("1!+2!+3!+4!+5!= "+sum);
	
		}
	
	}

	3!=6
	1!+2!+3!+4!+5!= 153

__数组作为参数__

	package Method;
	
	public class m5 {
		
		//数组元素
		public void printArray(int[] arr) {
			for(int i=0;i<arr.length;i++) {
				System.out.print(arr[i]+" ");
			}
			System.out.println();
		}
	
		public static void main(String[] args) {
			int[] arr = {10,23,34,44,55};
			m5 am = new m5();
			am.printArray(arr);
		}
	
	}

	10 23 34 44 55 


案例2

	package Method;
	
	import java.util.Scanner;
	
	public class m6 {
		//查找数组元素值的方法
		public boolean search(int n,int[] arr) {
			boolean flag=false; //默认没有找到
			for(int i=0;i<arr.length;i++) {
				if(arr[i]==n) {
					flag=true; //找到了
					break;
				}
			}
			return flag;
		}
		public static void main(String[] args) {
			int[] arr1 = {1,2,3,4,5,6};
			Scanner sc = new Scanner(System.in);
			System.out.println("输入要查找的数据:");
			int n1=sc.nextInt();
			m6 sc1= new m6();
			boolean flag=sc1.search(n1, arr1);
			if(flag) {
				System.out.println("找到了");
			}else {
				System.out.println("没找到了");
			}
			
	
		}
	
	}

	输入要查找的数据:
	7
	没找到了


___方法的重载__

方法名相同，参数不同。

	package Method;
	
	public class m7 {
		
		//求两个int类型数的和
		public int plus(int m,int n) {
			return m+n;
		}
		//求两个double类型和
		public double plus(double m,double n) {
			return m+n;
		}
		//求数组元素的累加和
		public int plus(int[] arr) {
			int sum=0;
			for(int i=0;i<arr.length;i++) {
				sum+=arr[i];
			}
			return sum;
		}
		
		public static void main(String[] args) {
			int m=5,n=10;
			int[] arr= {1,2,3,4,5};
			m7 Plus = new m7();
			System.out.println("int类型的和: "+Plus.plus(m,n));
			System.out.println("double类型的和: "+Plus.plus(6.5,3.4));
			System.out.println("数组类型的和: "+Plus.plus(arr));
		}
	
	}

	//
	int类型的和: 15
	double类型的和: 9.9
	数组类型的和: 15


参数的传递问题

	package Method;
	
	public class Exchange {
	
		//交换方法
		public void swap(int a,int b) {
			int temp;
			System.out.println("交换前: a="+a+",b="+b);
			temp=a;a=b;b=temp;
			System.out.println("交换后: a="+a+",b="+b);
		}
		public static void main(String[] args) {
			int m=4,n=5;
			Exchange ed = new Exchange();
			System.out.println("交换前: m="+m+",n="+n);
			ed.swap(m, n);
			System.out.println("交换后: m="+m+",n="+n);
	
		}
	
	}

结果

	交换前: m=4,n=5
	交换前: a=4,b=5
	交换后: a=5,b=4
	交换后: m=4,n=5

只是把m,n的值传递给了其中的参数，其他的就没有关系了。 位置不变，即内存。

一个封装方法，然后主方法调用。

	package Method;
	
	public class Exchange {
	
		//交换方法
		public void swap(int a,int b) {
			int temp;
			System.out.println("交换前: a="+a+",b="+b);
			temp=a;a=b;b=temp;
			System.out.println("交换后: a="+a+",b="+b);
		}
		//封装，然后在主方法里面再次调用
		public void swapTest() {
			int m=4,n=5;
			System.out.println("交换前: m="+m+",n="+n);
			swap(m, n);
			System.out.println("交换后: m="+m+",n="+n);
		}
		public static void main(String[] args) {
			
			Exchange ed = new Exchange();
			ed.swapTest();
			
	
		}
	
	}


__数组传值__

数组的的值，可以被改变。会影响主方法中的值。

__可变参数__

6-1

	sum(int... n)

可变参数放在后面。和数组等价，可变参数可为数组[]

	package Method;
	
	public class look {
		private static final String LK = null;
		//查找
		public void search(int n,int... a) {
			boolean flag=false;
			for(int a1:a) {
				if(a1==n) {
					flag=true;
					break;
				}
			}
			if(flag) {
				System.out.println("找到了！"+n);
			}else {
				System.out.println("没找到！"+n);
			}
		}
		public static void main(String[] args) {
			look Lk = new look();
			Lk.search(8,3,1,2,3,4,5);
			int[] a= {1,2,3,3,4};
			Lk.search(3, a);
	
		}
	
	}

	结果

	没找到！8
	找到了！3


__可变参数列表作为方法参数的重载问题__

	package Method;
	/**
	 * 关于可变参数列表和重载的问题
	 * @author Administrator
	 * @version 1.0
	 *
	 */
	public class M8 {
		//可变参数列表所在的方法是最后被访问的。
		/*
		 * 
		 */
		public int plus(int a,int b) {
			System.out.println("不带可变参数的方法被调用");
			return a+b;
		}
			public int plus(int... a) {
				int sum=0;
				for(int n:a) {
					sum=sum+n;
				}
				System.out.println("带可变参数的方法被调用");
				return sum;
			}
		
		public static void main(String[] args) {
			M8 ad = new M8();
			System.out.println("和为:"+ad.plus(1,2));
	
		}
	
	}



__方法的调试__

	F5 方法内部调试
	F7 由方法内部返回调试处

__作业__

定义一个类，对数组的数据进行管理

对每个功能定义在方法里面

插入数据 `public int[] insertData(){}`

显示所有数据 `public viod showData(int[] a,int length ){}`

在指定位置插入数据 `public void insertAtArray(int[] a,int n,int k ){}`

查询能被3整除的数据 `public void divThree(int[] a){}`

显示提示信息： `public void notice()`

主方法

##### Java面向对象

稳定性，可重用性


	属性：对象具有的各种静态特征
	对象有什么
	方法：对象具有的各种动态行为
	对象能做什么


类

	抽象的概念
	模版

对象

	类的实例化

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-27/24126864.jpg)

定义一个类`Cat.java`

	package com_secnote;
	/**
	 * 猫类
	 * @author Administrator
	 *
	 */
	
	public class Cat {
	
		//成员属性:昵称、年龄、体重、品种
		String name; //默认值 null
		int month;  //默认值 0
		double weight; //默认值 0.0
		String species; // 默认值 null
		
		//方法：跑动、吃东西
		//跑动方法
		public void run() {
			System.out.println("小猫快跑");
		}
		public void run(String name) {
			System.out.println(name+"快跑");
		}
		//吃东西方法
		public void eat() {
			System.out.println("小猫快吃");
		}
		
	
	}


实例化类`CatTest.java`


	package com_secnote;
	
	public class CatTest {
		public static void main(String[] args) {
			//对象实例化
			Cat one = new Cat();
			//测试
			one.eat();
			one.run();
			one.name = "花花";
			one.month = 2;
			one.weight = 1000;
			one.species = "加菲猫";
			System.out.println("昵称 "+one.name);
			System.out.println("年龄 "+one.month);
			System.out.println("体重 "+one.weight);
			System.out.println("品种 "+one.species);
			one.run(one.name);
			
		}
	
	}

结果

	小猫快吃
	小猫快跑
	昵称 花花
	年龄 2
	体重 1000.0
	品种 加菲猫
	花花快跑




单一职责

一个类只有一个功能

单一职责原则


**new关键字**

	
	声明对象 Cat one(栈) = new Cat()(堆) 

声明对象和实例化对象在内存中的不同地方完成的。 通过 `=` 关联起来。把堆的地址给内存。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-27/33555282.jpg)


__构造方法__

无参构造

1. 构造方法与类同名没有返回值
2. 构造方法的语句格式
3. 只能在对象实例化的时候调用

对象实例化的时候，会调用构造方法。再回到实例化中。

2-2

就近原则



	public Cat(String Newname,int Newmonth,double Newweight,String Newspecies ) {
			name=Newname;
			month=Newmonth;
			weight=Newweight;
			species=Newspecies;
			
		}

对属性赋值，改一下参数名字，不能和属性的名字相同。

__this__


代表当前对象，即实例化的对象。

	Cat one = new Cat("花花",2,100,"美国");


构造方法，使用`this`

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-28/8517320.jpg)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-28/57283500.jpg)

声明对象，实例化对象。两个部分。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-28/7719838.jpg)

通过`this()`调用构造方法，必须放在方法体内第一行。

##### 封装

封装

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-28/68754772.jpg)

案例

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-28/17776626.jpg)

特点：

	只能通过规定的方法访问数据
	隐藏类的实例细节，方便修改和实现


__封装的代码实现__


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-28/61671486.jpg)

定义

	//修改属性的可见性 --private	限定只能在当前类内访问
	private String name; //默认值 null
	//创建get/set方法
	//在get/set方法中添加对属性的限定
	public void setName(String name) {
		this.name=name;
	}
	public String getName() {
		return "我是一只叫 "+this.name+"的宠物猫";
	}

实例化

	//对象实例化
		Cat one = new Cat();
		one.setName("花花");
		System.out.println(one.getName());

右键`sources`--generate Get/Set 方法

封装，主要是对类中属性的进行设置，比如`private`，在类中可以`set/get`，对其中的属性进行一些设置，然后在实例化话时候，调用其中的设置和读取方法。

__使用包进行类管理__

跨包的类调用

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-28/95364829.jpg)

导入包

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-28/44254330.jpg)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-28/89611921.jpg)

	package com_secnote_test; //定义包
	//import com_secnote.*; //加载com_secnote下的所有类
	import com_secnote.Cat; //加载指定包下的指定类 效率高
	
	public class Test {
	
		public static void main(String[] args) {
			Cat cat = new Cat();
			//程序代码中直接加载
			
			com_secnote.CatTest tex=new com_secnote.CatTest();
	
		}
	
	}


__static__

	static 静态成员、类成员
	类对象共享
	类加载时产生，销毁时释放，生命周期长



	public  int price;

	实例化的时候
	one.price=2000;
	two.price=150;

	输出结果是两个不同的值

	public static int price;
	实例化的时候
	one.price=2000;
	two.price=150;

	输出结果就是 150。
	绑定了，静态

static

>static+属性 --》 静态属性、类属性
> static+方法--》类方法 

调用方式

	类名.静态成员


静态属性，静态方法，没有静态类。static方法内的局部变量也不行。

在成员方法中，可以直接访问类中静态成员。

静态方法不能访问非静态成员，只能调用静态成员，不能使用this。



代码块

	{
	//出现在方法里面
	}


直接在类中出现代码块。构造代码块。


静态代码块

	static {
	//出现在方法里面
	}

无论产生多少个类实例，静态代码只执行一次。

在一个方法里面，定义一个变量，其全局的作用。

封装

把类里面的属性和方法，进行修改(装饰)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-12-1/19029213.jpg)

包

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-12-1/44422167.jpg)

__封装案例__

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-12-1/86477746.jpg)

对象，以及对象的特征

学科专业类

	专业名称，编号，学制年限

学生类	

	姓名，学好，性别，年龄

首先定义学科的类，定义其中的属性，对属性的封装，set/get，然后使用构造方法，进行属性的赋值。

构造方法，有无参数构造和有参数构造。

一个类，功能模块写完之后，要测试。

对属性构造方法的时候，统一使用 set方法进行。

**学科类的编写**


	package com_secnote.model;
	/**
	 * 专业类
	 * @author Administrator
	 *
	 */
	public class Subject {
		// 成员属性：学科名称、学科编号、学制年限
		// 私有属性，实现代码的封装
		private String subjectName;
		private String subjectNo;
		private int subjectLife;
	
		// 无参数构造方法
		public Subject() {
	
		}
	
		// 有参数构造方法，实现对属性的全部赋值
		public Subject(String subjectName, String subjectNo, int subjectLife) {
			// 赋值.两种，一种直接，一种用set
			// this.subjectName = subjectName;
			this.setSubjectName(subjectName);
			this.setSubjectNo(subjectNo);
			
			this.setSubjectLife(subjectLife);
	//		this.subjectLife = subjectLife;
	
		}
	
		// 对每个属性设置 get/set
		public void setSubjectName(String subjectName) {
			this.subjectName = subjectName;
		}
	
		public String getSubjectName() {
			return this.subjectName;
		}
	
		public String getSubjectNo() {
			return subjectNo;
		}
	
		public void setSubjectNo(String subjectNo) {
			this.subjectNo = subjectNo;
		}
	
		public int getSubjectLife() {
			return subjectLife;
		}
	
		// 设置学制年限
		public void setSubjectLife(int subjectLife) {
			
			if (subjectLife <= 0)
				return;
			this.subjectLife = subjectLife;
		}
		/**
		 * 专业介绍的方法
		 * @return 专业介绍的相关信息，名称，编号，年限
		 */
		public String info() {
			String str = "专业信息如下: \n专业名称: " + this.getSubjectName() + "\n专业编号: " + this.getSubjectNo() + "\n专业年限: "
					+ this.getSubjectLife();
			return str;
		}
	
	}


学科的测试

	package com_secnote_test;
	
	import com_secnote.model.Subject;
	
	public class SchoolTest {
	
		public static void main(String[] args) {
			// TODO Auto-generated method stub
	//		测试Subject
			Subject sub1 = new Subject("计算机科学与技术","Js0001",-4);
			System.out.println(sub1.info());
	
		}
	
	}

结果

	专业信息如下: 
	专业名称: 计算机科学与技术
	专业编号: Js0001
	专业年限: 0


**学生类**

	package com_secnote.model;
	
	public class Student {
		// 成员属性:学号、姓名、性别、年龄
		private String studentNo;
		private String studentName;
		private String studentSex;
		private int studentAge;
	
		// 无参构造方法
		public Student() {
	
		}
	
		// 有参构造方法
		public Student( String studentName,String studentNo, String studentSex, int studentAge) {
	
			this.setStudentNo(studentNo);
			this.setStudentName(studentName);
			this.setStudentSex(studentSex);
			this.setStudentAge(studentAge);
		}
	
		public String getStudentNo() {
			return studentNo;
		}
	
		public void setStudentNo(String studentNo) {
			this.studentNo = studentNo;
		}
	
		public String getStudentName() {
			return studentName;
		}
	
		public void setStudentName(String studentName) {
			this.studentName = studentName;
		}
	
		public String getStudentSex() {
			return studentSex;
		}
	
		public void setStudentSex(String studentSex) {
				
			this.studentSex = studentSex;
		}
	
		public int getStudentAge() {
			return studentAge;
		}
		/**
		 * 给年龄赋值，限定必须在10--100之间，反之赋值为18
		 * @param studentAge
		 */
		public void setStudentAge(int studentAge) {
			if(studentAge<10 || studentAge >100)
				this.studentAge=18;
			else
				this.studentAge = studentAge;
		}
	
		/**
		 * 学生自我介绍的方法
		 * @return 自我介绍的信息:包括姓名、学号、性别、年龄
		 */
		public String introduction() {
			String str = "学生信息如下:\n姓名:" + this.getStudentName() + "\n学号: " + this.getStudentNo() + "\n性别: "
					+ this.getStudentSex() + "\n年龄: " + this.getStudentAge();
			return str;
		}
	}


可以通过`equals()`方法进行字符串内容的判断，是否等于、

**类的关联**

在`Student`类中，又添加两个介绍函数中添加两个参数

	/**
		 * 学生自我介绍的方法 
		 * @param subject 所学专业
		 * @param subjectlife 年限
		 * @return 自我介绍的信息:包括姓名、学号、性别、年龄、专业、年限
		 */
		public String introduction(String subject,int subjectlife) {
			String str = "学生信息如下:\n姓名:" + this.getStudentName() + "\n学号: " + this.getStudentNo() + "\n性别: "
					+ this.getStudentSex() + "\n年龄: " + this.getStudentAge() +"\n所学专业 "+ subject +"\n年限 "+subjectlife;
			return str;
	}

结果

	学生信息如下:
	姓名:小里
	学号: 2017
	性别: 男
	年龄: 16
	所学专业 计算机
	年限 4


第二种

在`Student`的方法中引用对象作为参数

	public String introduction(Subject mysubject) {
			String str = "学生信息如下:\n姓名:" + this.getStudentName() + "\n学号: " + this.getStudentNo() + "\n性别: "
					+ this.getStudentSex() + "\n年龄: " + this.getStudentAge() + "\n所学专业 " + mysubject.getSubjectName()
					+ "\n年限 " + mysubject.getSubjectLife();
			return str;
		}


调用

	Subject sub1 = new Subject("计算机科学与技术","JS0001",4);
	System.out.println("**********");
		Student stu3 = new Student("小红","2017","女",16);
		System.out.println(stu3.introduction(sub1));


第三种

将专业视为成员属性

其中的类视为`student`中的一个成员属性

	private Subject studentSubject;
	public Student(String studentName, String studentNo, String studentSex, int studentAge, Subject studentSubject) {

		this.setStudentNo(studentNo);
		this.setStudentName(studentName);
		this.setStudentSex(studentSex);
		this.setStudentAge(studentAge);
		this.setStudentSubject(studentSubject);
	}

	public Subject getStudentSubject() {
		if (this.studentSubject == null)
			this.studentSubject = new Subject();
		return studentSubject;
	}

	public void setStudentSubject(Subject studentSubject) {
		this.studentSubject = studentSubject;
	}

	方法引用

	public String introduction() {
		String str = "学生信息如下:\n姓名:" + this.getStudentName() + "\n学号: " + this.getStudentNo() + "\n性别: "
				+ this.getStudentSex() + "\n年龄: " + this.getStudentAge()+ "\n所学专业 " + this.getStudentSubject().getSubjectName() + "\n年限 " + this.getStudentSubject().getSubjectLife();
		return str;
	}

调用

	Subject sub1 = new Subject("计算机科学与技术","JS0001",4);
		System.out.println(sub1.info());
		System.out.println("**********");
		//测试Student
		Student stu1 = new Student("小明","20134","男",160,sub1);
		System.out.println(stu1.introduction());


三个解决方案

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-12-4/49632500.jpg)

在方法中通过对象作为参数，传递的是它的引用，可以通过引用获取该对象所有信息。

**新增需求**

知道在这个专业有多少学生。

数组存放学生。

新添加属性

	private Student[] myStudents;
	private int studentNum;

设置`get`/`set`

新建方法

	public void addStudent(Student stu) {
			/**
			 * 将学生保存在数组中
			 * 将学生个数保存到studentNum
			 */
			//将学生保存在数组中
			int i;
			for(i=0;i<this.getMyStudents().length;i++) {
				if(this.getMyStudents()[i]==null) {
					this.getMyStudents()[i]=stu;
					//将学生个数保存到studentNum
					this.studentNum=i+1;
					return;
				}
					
			}


测试调用方法

	//测试指定专业有多少人学习
		sub1.addStudent(stu1);
		sub1.addStudent(stu2);
		sub1.addStudent(stu3);
		System.out.println(sub1.getSubjectName()+"的专业中有 "+sub1.getStudentNum()+" 学生报名");


##### 继承

类与类之间的关系

继承的实现

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-12-6/62057893.jpg)

创建项目--包--类

	extends  继承

子类可以访问父类非私有的成员

父类

	package com_animal;
	
	public class Animal {
		private String name;
		private int month;
		private String species;
		
	//	无参构造
		public Animal(){
			
			}
	
		public String getName() {
			return name;
		}
	
		public void setName(String name) {
			this.name = name;
		}
	
		public int getMonth() {
			return month;
		}
	
		public void setMonth(int month) {
			this.month = month;
		}
	
		public String getSpecies() {
			return species;
		}
	
		public void setSpecies(String species) {
			this.species = species;
		}
		//吃东西
		public void eat(){
			System.out.println(this.getName()+"在吃东西");
		}
		
	}


子类继承父类，但自己👉初始化一个方法

	package com_animal;
	
	public class Cat extends Animal {
		private double weight;
		
		//无参构造方法
		public Cat(){
			
		}
	
		public double getWeight() {
			return weight;
		}
	
		public void setWeight(double weight) {
			this.weight = weight;
		}
		
		//跑动方法
		public void run(){
			System.out.println(this.getName()+"是一只"+this.getSpecies()+"，它在快乐的跑");
		}
		
	}

测试

	package com_test;
	
	import com_animal.Cat;
	
	
	public class Test {
	
		public static void main(String[] args) {
			// TODO Auto-generated method stub
			Cat one = new Cat();
			one.setName("花花");
			one.setSpecies("中华");
			one.eat();
			one.run();



**方法重写**

对父类中方法的重写。

	方法重载
	在同一个类中
	方法名相同，参数列表不同(参数顺序，个数、类型)
	方法返回值、访问修饰符任意
	与方法的参数名无关

方法的重写

	有继承关系的子类中
	方法名相同、参数列表相同、（参数顺序、个数、类型）、方法返回值相同
	访问修饰符。访问范围需要大于等于父类的访问范围
	与方法的参数名无关


当子类重写父类的方法之后，子类对象调用的是重写后的方法


** 访问修饰符 **

	公有的： public 	允许在任意位置访问  权限最大
	私有的： private 只允许在本类中进行访问。权限最小
	受保护的: protected 当前类可以访问，同包子类/非子类可以访问，跨包调用
	默认 什么都不加。 允许在当前类、同胞子类可以调用、


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-12-7/65304074.jpg)

** super **


	super 父类对象引用

	父类的构造方法不允许被继承和重写，但是会影响子类对象的构造方法




** 继承的初始化顺序 **

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-12-7/39434122.jpg)

子类构造默认调用父类的无参构造方法。父类中无参构造很重要要

调用父类的指定的构造方法，通过`super()`。必须放在子类构造方法有效代码第一行。


super 

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-12-7/22285009.jpg)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-12-7/75091408.jpg)

	super this  不能在静态方法中被调用

	super 父类中方法

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-12-7/40944916.jpg)


继承。类与类之间的关系。

方法重写和方法重载

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-12-7/56990877.jpg)


** Object **

Object是所有类的父类


	equals,toString 在子类中重写

** final **

	final class 该类没有子类 
	java.lang

	final 方法 该方法不能被子类重写，但是可以正常被子类继承使用

	final int temp = 10 方法内的变量，一旦赋值，不能被修改
	final temp 成员属性： 在构造代码中赋值或者定义时候，定义时直接赋值

引用数据类型 final.

	类、String、System、数组

	String = new s();


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-12-8/64425857.jpg)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-12-8/91186667.jpg)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-12-8/16980122.jpg)

可配合 static 使用

** 注解 **

	标记。
	可以声明在包、类、属性、方法、局部变量、方法参数的前面，用来对这些元素进行说明、注释

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-12-8/78291821.jpg)

	@Override
	帮助检测重写的方法是否合理


** Java设计模式**

设计模式是软件开发人员在软件开发过程中面临的一般问题的解决方案。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-12-11/56397895.jpg)

问题场景的解决方案。

**单例模式**

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-12-11/69601246.jpg)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-12-11/43058532.jpg)

饿汉式

	package com.single;
	//饿汉式：创建对象实例的时候直接初始化。空间换事件。创建对象的时候，先实例化放在那里。什么时候用直接引用就行
	public class SingleOne {
		//1、创建类中私有构造
		private SingleOne(){
			
		}
		// 2、创建该类型的私有静态实例
		private static SingleOne instance = new SingleOne();
		
		//3、创建公有静态方法返回静态实例对象
		public static SingleOne getInstance(){
			return instance;
		}
	
		
	}

测试

	package com_test;
	
	import com.single.SingleOne;
	
	public class Test {
	
		public static void main(String[] args) {
			// TODO Auto-generated method stub
			SingleOne one = SingleOne.getInstance();
			SingleOne two = SingleOne.getInstance();
			System.out.println(one);
			System.out.println(two);
		}
	
	}

结果

	com.single.SingleOne@2f63e9a1
	com.single.SingleOne@2f63e9a1


懒汉式

	package com.single;
	// 懒汉式：类内实例化对象创建时并不直接初始化，直到第一次用get方法时，才完成初始化操作
	// 用时间换空间。什么时候用，检测是否调用
	public class SingleTwo {
		//1、创建私有构造方法
		private SingleTwo(){
			
		}
		
		//2、创建静态的该类实例对象
		private static SingleTwo instance=null;
		
		//3、创建开放的静态方法提供实例对象
		public static SingleTwo getInstance(){
			if(instance==null)
				instance= new SingleTwo();
			
			return instance;
			
		}
	}
	

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-12-11/99408372.jpg)


** 多态 **

比如`F1`键，在`Chrome`中显示的是浏览器的帮助文档，在文档中，显示的是文档帮助。

同样的行为在不同对象上的作用不同。

允许不同类的对象对同一消息做出不同的响应。

编译时多态、运行时多态。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-12-11/26423546.jpg)

继承关系、父类引用指向子类对象

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-12-11/23734231.jpg)

**向上转型**

父类 Animal.java

	package com_animal;
	
	public class Animal {
		//属性：昵称、年龄
		private String name;
		private int month;
		
		public Animal(){
			
		}
		public Animal(String name,int month){
			this.name = name;
			this.month = month;
			
		}
		public String getName() {
			return name;
		}
		public void setName(String name) {
			this.name = name;
		}
		public int getMonth() {
			return month;
		}
		public void setMonth(int month) {
			this.month = month;
		}
		//方法：吃东西
		public void eat(){
			System.out.println("动物都吃东西");
		}
		
	}

子类 Cat.java  重写父类的 eat方法

	package com_animal;
	
	public class Cat extends Animal {
		//属性：体重
		private double weight;
		
		public Cat(){
			
		}
		public Cat(String name,int month,double weight){
			super(name,month);
			this.weight = weight;
		}
		public double getWeight() {
			return weight;
		}
		public void setWeight(double weight) {
			this.weight = weight;
		}
		
		//方法：跑动
		public void run(){
			System.out.println("小猫快乐的奔跑");
		}
		
		//方法：吃东西(重写父类方法)
		@Override
		public void eat() {
			// TODO Auto-generated method stub
			System.out.println("猫吃鱼~~");
		}
	
	}


Dog.java 重写父类的 eat方法

	package com_animal;
	
	public class Dog extends Animal {
	
		//属性：性别
		private String sex;
		
		public Dog(){
			
		}
		public Dog(String name,int month,String sex){
			this.setMonth(month);
			this.setName(name);
			this.setSex(sex);
		}
		public String getSex() {
			return sex;
		}
		public void setSex(String sex) {
			this.sex = sex;
		}
		
		//方法：睡觉
		public void sleep(){
			System.out.println("小狗有午睡的习惯");
		}
		
		//方法：吃东西(重写父类方法)
		@Override
		public void eat() {
			// TODO Auto-generated method stub
			System.out.println("狗爱吃肉~~");
		}
	}


实例化

	package com_test;
	
	import com_animal.Animal;
	import com_animal.Cat;
	import com_animal.Dog;
	
	public class Test {
		public static void main(String[] args){
			Animal one = new Animal();	
			//向上转型、隐式转型、自动转型 --》 
			// 父类引用指向子类实例，可以调用子类重写父类以及父类派生的方法，无法调用子类独有的方法
			//小类转大类
			Animal two = new Cat();
			Animal three = new Dog();
			
			one.eat();
			two.eat();
			three.eat();
			//two.run()
		}
	
	}

	动物都吃东西
	猫吃鱼~~
	狗爱吃肉~~



**向下转型**

	//向下转型，强制类型转换
		//子类引用指向父类对象，此处必须进行强转。可以调用子类特有方法
		Cat temp = (Cat)two; // (Cat)强制转换
		temp.eat();
		temp.run();
		temp.getMonth();

	*******************
	猫吃鱼~~
	小猫快乐的奔跑

**instanceof运算符**

判断某个对象是否满足某个类的实例特征

	if(two instanceof Cat){
		Cat temp = (Cat)two; // (Cat)强制转换
		temp.eat();
		temp.run();
		temp.getMonth();
		System.out.println("Two可以转换为Cat类型");
		}
	if(two instanceof Dog){
				Dog temp2 = (Dog)two;
				temp2.eat();
				temp2.sleep();
				System.out.println("Two可以转换为类型");
			}
			if(two instanceof Animal){
				System.out.println("Animal");
			}
			if(two instanceof Object){
				System.out.println("Object");
			}
			
		}



	Two可以转换为Cat类型
	Animal
	Object


**类型转换**

父类中的 静态方法static 不能被子类重写

**类型转换案例**

主人对不同动物的行为

	package com_animal;
	
	public class Master {
		/* 喂宠物
		 * 喂猫咪：吃东西后，主人会带着去玩球
		 * 喂狗狗：吃饭东西，主人会带着去睡觉
		 */
		public void feed(Cat cat){
			cat.eat();
			cat.playBall();
		}
		
		public void feed(Dog dog){
			dog.eat();
			dog.sleep();
		}
	
	}

测试

	package com_test;
	
	import com_animal.Cat;
	import com_animal.Dog;
	import com_animal.Master;
	
	public class MasterTest {
		public static void main(String[] args){
		Master master = new Master();
		Cat one = new Cat();
		Dog two = new Dog();
		master.feed(one);
		master.feed(two);
		}
	}

	猫吃鱼~~
	小猫喜欢玩线球~~
	狗爱吃肉~~
	小狗有午睡的习惯
	

方案2

	//方案2：编写方法传入动物的父类，方法中通过类型转换，调用子类的方法
		public void feed(Animal obj){
			if(obj instanceof Cat){
				Cat temp=(Cat)obj;   //强制向下转换
				temp.eat();
				temp.playBall();
			}else if(obj instanceof Dog){
				Dog temp=(Dog)obj;
				temp.eat();
				temp.sleep();
			}
		}


同一个操作行为，针对不同的参数，返回不同的结果，显示出了多态，多种形态。

	/*
		 * 饲养何种动物
		 * 空闲时间多：养狗狗
		 * 空闲时间不多：养猫咪
		 */
		//方案一
	//	public Dog hasManyTime(){
	//		System.out.println("主人时间比较多可以养狗");
	//		return new Dog();
	//	}
	//	public Cat hasLittleTime(){
	//		System.out.println("主人时间比较少可以养狗");
	//		return new Cat();
	//	}
		//方案2
		public Animal raise(boolean isManyTime){
			if(isManyTime){
				System.out.println("主人时间比较多可以养狗");
				return new Dog();
			}else{
				System.out.println("主人时间比较少可以养狗");
				return new Cat();
			}
		}

测试

	System.out.println("===============");
		boolean isManyTime=false;
		//Animal temp;
	//	if(isManyTime){
	//		temp = master.hasManyTime();
	//	}else{
	//		temp = master.hasLittleTime();
	//	}
		Animal temp=master.raise(isManyTime);
		System.out.println(temp);
		
		}


结果

	主人时间比较少可以养狗
	com_animal.Cat@44585f2a


**抽象类**

	abstract

	//抽象类：不允许实例化，可以通过向上转型，指向子类实例化
	public  abstract class Animal {


**抽象方法**


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-12-19/27902936.jpg)

包含抽象方法的一定是抽象类。抽象类中可以没有抽象方法、

抽象类和抽象方法

**接口**

单继承。

一个手机打电话，短信，音乐。

电脑，pad可以打电话，听音乐，视频。

这些类之间有相同的行为，如何进行关联，使用类。


接口。

** String **

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-1-17/52403272.jpg)

字符串的方法

长度，取值以及替换等。

字符串和byte数组之间的转换。

== equals 地址和内容

面向对象，地址问题。

内存空间

	public class d3 {

	public static void main(String[] args) {
		// == 与 equals 方法的区别
		String str1 = "imooc";
		String str2 = "imooc";
		String str3 = new String("imooc");
		System.out.println("str1和str2的内容相同?"+(str1.equals(str2)));
		System.out.println("str1和str3的内容相同?"+(str1.equals(str3)));
		
		System.out.println("str1和str2的地址相同?"+(str1 == str2));
		System.out.println("str1和str3的地址相同?"+(str1 == str3));
	}

	}

结果

	str1和str2的内容相同?true
	str1和str3的内容相同?true
	str1和str2的地址相同?true
	str1和str3的地址相同?false

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-1-17/37340145.jpg)

对象创建的内存空间地址，不同。

String的不可变性
String对象一旦创建，则不能修改，是不可变的
所谓的修改是创建了新的对象，所指向的内存空间地址不变

	String s1 = "imooc";
		s1 = "hello," + s1;
		//s1不再指向imooc所在的内存空间，而是指向了"hello,imooc"
		System.out.println("s1="+s1);

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-1-17/34717134.jpg)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-1-17/94880403.jpg)


[常量池](http://www.cnblogs.com/iyangyuan/p/4631696.html)

`StringBuilder`

	StringBuffer线程安全的，StringBuilder没有。
	str.append() 拼接
	str.delete(start,end) 删除。。开始，结束


一个类中的方法，对字符串的操作

	StringBuilder str = new StringBuilder("你好");
		//添加字符串
	//	str.append(',');
	//	str.append("imooc!");
	//	System.out.println("str="+str);
		System.out.println("str="+str.append(',').append("imooc"));
		//大小写变化
		//1:使用delete 方法删除，然后插入大写
		//System.out.println("替换后:"+str.delete(3, 8).insert(3, "iMOOC"));
		// 2、使用replace替换	
		System.out.println("替换后："+str.replace(3, 8, "iMOOC"));
		//取出数据
		System.out.println(str.substring(0,2));


** 集合 **

数组，集合。

数组固定长度，集合可以动态变化。

** List ** 列表

	有序，可重复，序列
	可以精确控制每个元素的插入位置或删除某个位置的元素
	实现类是 ArrayList和LinkedList


`ArrayList`

	底层由数组实现
	动态增长，以满足应用程序的需求
	在列表尾部插入或者删除数据非常有效
	适合查找和更新
	可以为 null


2-3

	import java.util.ArrayList
	import java.util.List

	创建列表
	List list = new ArrayList()
	list.add("")

	显示用循环
	


列表的增删改查。

**set** 是元素无序并且不可以重复的集合

	Set set = new HasjSet();
	set.add("blue");

跌代器接口

	iterator

类中的一些方法对列表，set的一些方法的处理。

	Iterator it = set.iterator();
	while(it.hasNext()){
		System.out.println(it.next());
	}


	set.contains("string")




3-11

查找

	set.contains()

删除

	set.remove()
	set.isempty()


**Map**

键值对(key-value)

HashMap 基于哈希表，允许使用null值和null键


字典的添加，删除。

这个和python中的字典一样。

商品类的一个设计

	package com.string;
	
	public class Goods {
		private String id; //商品编号
		private String name; //商品名称
		private double price; //商品价格
		
		//构造方法
		public Goods(String id,String name,double price){
			this.id = id;
			this.name = name;
			this.price = price;
		}
		// getter和setter
		public String getId() {
			return id;
		}
	
		public void setId(String id) {
			this.id = id;
		}
	
		public String getName() {
			return name;
		}
	
		public void setName(String name) {
			this.name = name;
		}
	
		public double getPrice() {
			return price;
		}
	
		public void setPrice(double price) {
			this.price = price;
		}
		
		public String toString(){
			return "商品编号:" +id+",商品名称:" +name+ ",商品价格："+price; 
		}
	}
	

商品信息管理

	package com.string;
	
	import java.util.HashMap;
	import java.util.Iterator;
	import java.util.Map;
	import java.util.Scanner;
	
	public class Goodstest {
	
		public static void main(String[] args) {
			Scanner console = new Scanner(System.in);
			//定义HashMap对象
			Map<String,Goods> goodsMap = new HashMap<String,Goods>();
			System.out.println("请输入三条商品数据");
			int i =0;
			while(i<3){
				System.out.println("请输入第"+(i+1)+"条商品信息:");
				System.out.println("请输入商品编号: ");
				String goodsId=console.next();
				System.out.println("请输入商品名称");
				String goodsName=console.next();
				System.out.println("请输入商品价格");
				double goodsPrice=console.nextDouble();
				
				Goods goods = new Goods(goodsId,goodsName,goodsPrice);
				//将商品信息添加到HashMap中
				goodsMap.put(goodsId, goods);
				i++;
			}
			
			//遍历Map输入商品
			System.out.println("商品全部信息为: ");
			//跌代器
			Iterator<Goods> itGoods = goodsMap.values().iterator();
			while(itGoods.hasNext()){
				System.out.println(itGoods.next());
			}
		}
	
	}

结果

	请输入三条商品数据
	请输入第1条商品信息:
	请输入商品编号: 
	1
	请输入商品名称
	test
	请输入商品价格
	2000.0
	请输入第2条商品信息:
	请输入商品编号: 
	02
	请输入商品名称
	bike
	请输入商品价格
	20000
	请输入第3条商品信息:
	请输入商品编号: 
	0003
	请输入商品名称
	phone
	请输入商品价格
	4000
	商品全部信息为: 
	商品编号:1,商品名称:test,商品价格：2000.0
	商品编号:0003,商品名称:phone,商品价格：4000.0
	商品编号:02,商品名称:bike,商品价格：20000.0


id不能重复，对输入id的进行判断。


需求分析，功能演示，详细设计，代码实现。

	List Set Map

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-1-25/69231880.jpg)

ArrayList:底层由数组实现，元素有序

Hashset: 元素无序和不重复

HashMap:键值对

Iterator 迭代器

hashCode()、equals()


** 集合排序 **

	sort

Collections类的sort()方法。

对数字，字符串排序

	List<Integer> list = new ArrayList<Integer>();
	List<String> list = new ArrayList<String>();


Comparator 接口

	强行对某个对象进行整体排序

	int compare()
	boolean equals(Object obj)

接口
	
java中的类，接口，增强行for循环。

Comparable接口


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-1-25/29676683.jpg)

	
** 泛型 **

	List<String> list = new ArrayList<String>(); 只能在list添加字符串

抽象类  

自定义泛型

	package com.string;
	
	public class Number<T> {
		private T num;
		public T getNum(){
			return num;
		}
		public void setNum(T num){
			this.num = num;
		}
		//测试
		public static void main(String[] args){
			Number<Integer> intNum = new Number<>();
			intNum.setNum(10);
			System.out.println("Integet:"+intNum.getNum());
			
			Number<Float> floatNum = new Number<>();
			floatNum.setNum(5.0f);
			System.out.println("Float:"+floatNum.getNum());
			
		}
	}



Integet:10
Float:5.0

自定义泛型类

	package com.string;
	
	public class TwoNumber<T,X> {
		private T num1;
		private X num2;
		public void getNum(T num1,X num2){
			this.num1 = num1;
			this.num2 = num2;
			
		}
		public T getNum1() {
			return num1;
		}
		public void setNum1(T num1) {
			this.num1 = num1;
		}
		public X getNum2() {
			return num2;
		}
		public void setNum2(X num2) {
			this.num2 = num2;
		}
		public static void main(String[] args){
			TwoNumber<Integer,Float> numOne = new TwoNumber<>();
			numOne.getNum(25, 5.0f);
			System.out.println("num1="+numOne.getNum1());
			System.out.println("num2="+numOne.getNum2());
			
		}
		
	}




自定义泛型方法

	package com.string;
	
	public class GenericMethod {
		public <T> void printValue(T t){
			System.out.println(t);
		}
	public static void main(String[] args){
		GenericMethod gm = new GenericMethod();
		gm.printValue("hello");
		gm.printValue(123);
		gm.printValue(5.0f);
	}	
	}


泛型。。

不用进行强制类型转换了。

**多线程**

进程可执行程序并存放在计算机存储器的一个指令序列，它是一个动态执行的过程

多任务

线程比进程还要小，一个进程包括多个线程。

线程的状态和生命周期

** 线程创建 **

	Thread 类

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-1-29/51852819.jpg)

Thread类和Runnable接口介绍

	package test_thread1;
	class MyThread extends Thread{
		public void run(){
			System.out.println(getName()+"该线程正在执行");
		}
	}
	public class threadtest {
	
		public static void main(String[] args) {
			//System.out.println("主线程1");
			MyThread mt = new MyThread();
			mt.start(); //启动线程，只能启动一次
			//System.out.println("主线程2");
	
		}
	
	}

	Thread-0该线程正在执行

两个线程，线程使用CPU的使用率，随机的。

	package test_thread1;
	class MyThread1 extends Thread{
		public MyThread1(String name){
			super(name);
		}
		public void run(){
			for(int i=1;i<=10;i++){
				System.out.println(getName()+"正在运行"+i);
			}
		}
	}
	public class Th2 {
	
		public static void main(String[] args) {
			MyThread1 mt1 = new MyThread1("线程1");
			MyThread1 mt2 = new MyThread1("线程2");
			mt1.start();
			mt2.start();
		}
	
	}

结果

	线程2正在运行1
	线程2正在运行2
	线程2正在运行3
	线程2正在运行4
	线程2正在运行5
	线程2正在运行6
	线程2正在运行7
	线程2正在运行8
	线程2正在运行9
	线程2正在运行10
	线程1正在运行1
	线程1正在运行2
	线程1正在运行3
	线程1正在运行4
	线程1正在运行5
	线程1正在运行6
	线程1正在运行7
	线程1正在运行8
	线程1正在运行9
	线程1正在运行10


线程创建

实现`Runnable`接口。Java不支持多继承，不打算重写Thread类的其他方法

**线程的状态**

新建、可运行(Runnable)、正在运行(Running)、阻塞(Blocked)、终止(Dead)

生命周期

获取CPU的使用权

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-1-29/56204564.jpg)

只有获取到CPU的使用权，线程才能从可执行状态转为可执行状态

调用start()方法可以使线程处于可运行状态

sleep方法应用

	Thread类的方法 
	public static void sleep(long millis)

作用：在指定的毫秒数内让正在执行的线程休眠
参数为休眠时间，单位是毫秒

join方法

	Thread类的方法

	public final void join()

作用：等待调用该方法的线程结束后才执行

join抢占资源，等它完成后，才放cpu

	package test_thread1;
	class MyThread extends Thread{
		public void run(){
			for(int i=1;i<=10;i++)
			System.out.println(getName()+"正在执行"+i+"次！");
		}
	}
	public class threadtest {
	
		public static void main(String[] args)  {
			//System.out.println("主线程1");
			MyThread mt = new MyThread();
			mt.start(); //启动线程，只能启动一次
			try {
				mt.join();
			} catch (InterruptedException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
			for(int i=1;i<=5;i++){
				System.out.println("主线程运行第"+i+"次！");
			}
			System.out.println("主线程运行结束！");
	
		}
	
	}

	Thread-0正在执行1次！
	Thread-0正在执行2次！
	Thread-0正在执行3次！
	Thread-0正在执行4次！
	Thread-0正在执行5次！
	Thread-0正在执行6次！
	Thread-0正在执行7次！
	Thread-0正在执行8次！
	Thread-0正在执行9次！
	Thread-0正在执行10次！
	主线程运行第1次！
	主线程运行第2次！
	主线程运行第3次！
	主线程运行第4次！
	主线程运行第5次！
	主线程运行结束！


调用`join()`方法可以使用线程由正在运行状态变为阻塞状态

`join()`方法的作用是等待调用该方法的线程结束后才能执行

**线程的优先级**

主线程的优先级默认 5


线程同步

多线程是通过竞争CPU时间获得运行机会的。

对象进行锁定，同一个时刻。`synchronized`关键字可以用在成员方法中，保证共享对象在同一时刻只能被一个线程访问，协调不同线程之间的工作。

**线程间通信**


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-1-30/62021376.jpg)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-1-30/9044702.jpg)



**输入输出流**

	System.out.println()

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-1-30/72564230.jpg)

输入流：从键盘接收数据

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-1-30/41172607.jpg)

**File类**

文件。`java.io.File`类对文件进行操作。 看文档

和`python`、`java`中的一样，文件的操作。。

字节输入流 `InputStream`

字节输出流 `OutStream`

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-1-30/59613257.jpg)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-1-30/19786799.jpg)

文件的复制和拷贝

字节缓冲流

	BufferedInputStream


输入输出流

 `System.currentTimeMillis()`


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-1-31/38062763.jpg)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-1-31/8163178.jpg)

字节：二进制

字符：

字符，字节的用途不同、


	InputStreamReader
	OutputStreamWriter



文件的操作以及字符的读写操作。缓冲流。

** 序列化 **

步骤：

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-1-31/28602589.jpg)

对象输入流： `ObjectInputStream`
对象输出流 `ObjectOutputStream`

对象转换字节

流的概念，File类、字节类、字符流、对象序列化


集合的使用

类、集合、数据。

列表的元素的操作，增删改查。

类中的构造方法。

