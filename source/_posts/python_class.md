---
title: Python 类
date: 2017-09-19 13:20:46
tags:
		- python
categories:
		- 编程学习

---

`Python`类学习。

<!-- more -->


#### Python面向对象

##### 面向对象是一种抽象

类、对象(个体)

属性、方法

封装、继承、多态

##### 用python定义类

`def __init__  `   构造函数

`dir()`返回对象属性

`type()`获取对象类型



##### 老式类和新式类(继承Object)

	class OldStyle:
		def __init__(self,name,description):
			self.name = name
			self.description = description
	
	class NewStyle(object):
		def __init__(self,name,description):
			self.name = name
			self.description = description
	
	if __name__ == '__main__':
		old = OldStyle('old','Old style class')
		print old
		print type(old)
		print dir(old)
		print '---------------------------------------------------'
		new = NewStyle('new','New style class')
		print new
		print type(new)
		print dir(new)


		运行结果

		<__main__.OldStyle instance at 0x000000000281F988>
		<type 'instance'>
		['__doc__', '__init__', '__module__', 'description', 'name']
		---------------------------------------------------
		<__main__.NewStyle object at 0x000000000281ACC0>
		<class '__main__.NewStyle'>
		['__class__', '__delattr__', '__dict__', '__doc__', '__format__', '__getattribute__', '__hash__', '__init__', '__module__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', 'description', 'name']


##### 定义类的属性

直接在类里定义

	class Programer(object):
		sex = 'male'
				

在构造函数里定义

每个对象中属性的值可以不一样。。。

	class Programer(object):
		def __init__(self,name,age):
			self.name = name
			self.age = age


python没提供私有属性的功能

		class Programer(object):
			def __init__(self,name,age):
				self.name = name     #公有属性
				self._age = age    #私有属性
				self.__weight = weight  #实现部分私有属性




一个实例

	class Programer(object):
		hobby = 'Play Computer'
	
		def __init__(self,name,age,weight):
			#三个属性在构造函数里面定义的
			self.name = name
			self._age = age
			self.__weight = weight
	
		#定义一个方法，获取__weight属性
		def get_weight(self):
			return self.__weight
	
	if __name__ == '__main__':
		programer = Programer('mlb504',25,60)      #实例化
		print dir(programer)           #所有属性打印一下
		print programer.__dict__		#从构造函数里面获得的属性
		print programer.get_weight()      #调用get_weight()
		print programer._Programer__weight     #访问其私有属性

		运行结果，从结果的第二行可以看出它的私有属性  _Programer__weight
		['_Programer__weight', '__class__', '__delattr__', '__dict__', '__doc__', '__format__', '__getattribute__', '__hash__', '__init__', '__module__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', '_age', 'get_weight', 'hobby', 'name']

		{'_age': 25, '_Programer__weight': 60, 'name': 'mlb504'}
		60
		60
	
##### 定义类的方法

函数和方法

函数是直接用函数名调用的

方法是类的一部分

类的方法也是类的属性

	>>> class Test(object):
	...     def test(self):
	...             pass
	... 
	>>> a = Test()
	>>> a.test
	<bound method Test.test of <__main__.Test object at 0xb710bd4c>>
	>>> a.test = '123'
	>>> a.test 
	'123'



    @classmethod 调用的时候用类名，而不是某个对象
    
    @property 像访问属性一样调用方法，不用加括号

定义类的方法，通过三种方式进行定义的。。

	class Programer(object):
		hobby = 'Play Computer'
		
		def __init__(self,name,age,weight):
			#三个属性在构造函数里面定义的
			self.name = name
			self._age = age
			self.__weight = weight
		
		@classmethod      #@classmethod 调用的时候用类名，而不是某个对象
		def get_hobby(cls):
			return cls.hobby
	
		@property    	#@property 像访问属性一样调用方法，不用加括号
		def get_weight(self):
			return self.__weight
	
		def self_introduction(self):
			print 'My Name is %s \nI am %s years old\n' % (self.name,self._age)
	
	if __name__ == '__main__':
		programer = Programer('mlb504',25,60)
		print dir(programer)
		print Programer.get_hobby()
		print programer.get_weight      #访问其中的方法就像访问属性一样
		programer.self_introduction()
    	
		运行结果。。。。
    
		['_Programer__weight', '__class__', '__delattr__', '__dict__', '__doc__', '__format__', '__getattribute__', '__hash__', '__init__', '__module__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', '_age', 'get_hobby', 'get_weight', 'hobby', 'name', 'self_introduction']
		Play Computer
		60
		My Name is mlb504 
		I am 25 years old


##### 类的继承

继承的子类

- 会继承父类的属性和方法
- 也可以自己定义，覆盖父类的属性


用`super()`调用父类的方法

		class A(object):
			def method(self,arg):
				pass
		
		class B(A):
			def method(self.arg):
				super(B,self).method(arg)


用类名调用父类的方法（没能很好的体现父类的继承。。）

		class A(object):
			def method(self,arg):
				pass
		
		class B(A):
			def method(self,arg):
				A.method(arg)



子类的类型判断

- isinstance 判断父类
- issubclass  判断子类

一个继承的实例

		class Programer(object):
			hobby = 'Play Computer'
			
			def __init__(self,name,age,weight):
				#三个属性在构造函数里面定义的
				self.name = name
				self._age = age
				self.__weight = weight
			
			@classmethod
			def get_hobby(cls):
				return cls.hobby
		
			@property 
			def get_weight(self):
				return self.__weight
		
			def self_introduction(self):
				print 'My Name is %s \nI am %s years old\n' % (self.name,self._age)
		
		
		class BackendProgramer(Programer):       #继承Programer
			def __init__(self,name,age,weight,language):
				super(BackendProgramer,self).__init__(name,age,weight)
				self.language = language
		
		
		if __name__ == '__main__':
			programer = BackendProgramer('mlb504',25,60,'python')
			print dir(programer)
			print programer.__dict__
			print type(programer)
			print isinstance(programer,Programer)    #属于它的父类





##### 类的多态

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-6-29/98230404.jpg)


多态的要素

- 继承
- 方法重写

		class Programer(object):
			hobby = 'Play Computer'
			
			def __init__(self,name,age,weight):
				#三个属性在构造函数里面定义的
				self.name = name
				self._age = age
				self.__weight = weight
			
			@classmethod
			def get_hobby(cls):
				return cls.hobby
		
			@property 
			def get_weight(self):
				return self.__weight
		
			def self_introduction(self):
				print 'My Name is %s \nI am %s years old\n' % (self.name,self._age)
		
		
		class BackendProgramer(Programer):       #继承Programer
			def __init__(self,name,age,weight,language):
				super(BackendProgramer,self).__init__(name,age,weight)
				self.language = language
		
		
			def self_introduction(self):
				print 'My name is %s\nMy favorite language is %s' % (self.name,self.language)
		
		
		def introduce(programer):
			if isinstance(programer,Programer):   #是否是父对象
				programer.self_introduction()
		
		if __name__ == '__main__':
			programer = Programer('cly',24,55)
			backend_programer = BackendProgramer('mlb504',25,60,'python')
			introduce(programer)
			introduce(backend_programer)


	
		My Name is cly 
		I am 24 years old

		My name is mlb504
		My favorite language is python


##### Python Magic Method

Magic Method给类添加魔法。。

比如方法名的前后有两个下划线`def __init__(self):`

##### Python面向对象- 对象的实例化

对象的创建和实例化

常用的类定义

	class Person(object):
		def __init__(self,name,age):
			self.name = name
			self.age = age
	
	person = Person('mlb',25)




对象实例化的过程

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-6-30/24835246.jpg)

回收对象

__del__()、

首先调用new方法，用来返回一个Programer对象，然后将对象交给__init__进行设置，，

	class Programer(object):
	
		def __new__(cls,*args,**kwargs):
			print 'call __new__ method'
			print args
			return super(Programer,cls).__new__(cls,*args,**kwargs)
	
		def __init__(self,name,age):
			print 'call __init__ method'
			self.name = name
			self.age = age
	
	if __name__ == '__main__':
		programer = Programer('mlb',25)
		print programer.__dict__

		运行结果

		call __new__ method
		('mlb', 25)
		call __init__ method
		{'age': 25, 'name': 'mlb'}


##### Python面向对象- 类与运算符

比较运算符

	__cmp__(self,other)
	__ep__(self,other)
	__lt__(self,other)
	__gt__(self,other)

数字运算符

	__add__(self,other)
	__sub__(self,other)
	__mul__(self.other)
	__div__(self,other)

逻辑运算符

	__or__(self,other)
	__and__(self,other)

	一个实例

	class Programer(object):
	
		def __init__(self,name,age):
			self.name = name
			if isinstance(age,int):
				self.age = age
			else:
				raise Exception('age must be int')
	
		def __eq__(self,other):
			if isinstance(other,Programer):
				if self.age == other.age:
					return True
				else:
					return False
			else:
				raise Exception('The type of object must be Programer')
	
		def __add__(self,other):
			if isinstance(other,Programer):
				return self.age + other.age
			else:
				raise Exception('The type of object must be Programer')
	
	
	if __name__ == '__main__':
		p1 = Programer('mlb',24)
		p2 = Programer('hcx',22)
		print p1 == p2
		print p1 + p2

	运行结果

	False
	46

##### Python面向对象- 类的展现

转换为字符串


    __str__、__repr__、__unicode__


展现对象属性

	__dir__


python的大多数内建函数由魔术方法支持的。。。

	class Programer(object):
	
		def __init__(self,name,age):
			self.name = name
			if isinstance(age,int):
				self.age = age
			else:
				raise Exception('age must be int')
	
		def __str__(self):
			return '%s is %s years old' %(self.name,self.age)
	
		def __dir__(self):
			return self.__dict__.keys()
	
	
	if __name__ == '__main__':
		p = Programer('mlb',25)
		print p
		print dir(p)


	mlb is 25 years old
	['age', 'name']



##### Python面向对象- 类的属性



##### 类和实例

数据的封装


##### 类的学习 


[Python的类和对象 入门级讲解（简单粗暴）](https://zhuanlan.zhihu.com/p/26617823)

	a = 2

a作为标签，挂在对象(数值2)上。。


对象 2  object

类     class

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-9-18/75762696.jpg)


__self 参数__

[Pythons self parameter](https://www.youtube.com/watch?v=BhBeVfrMh_g)

	class DemoClass:
	    def demo_method(self):
	        print("test")
	        print(id(self))
	
	my_object = DemoClass()
	print(id(my_object))
	my_object.demo_method()

结果

	40086088
	test
	40086088



定义一个类，实例化，并调用其中的方法。

	class DemoClass:
	    def demo_method():
	        print("test")
	my_object = DemoClass()
	my_object.demo_method()

出现错误。`my_object.demo_method()`不需要参数，但是给了1个。

	 my_object.demo_method()
	TypeError: demo_method() takes no arguments (1 given)


__self代表类的实例，而非类__

	class DemoClass:
	    def demo_method(self):
	       print(self)
	       print(self.__class__)
	
	t = DemoClass()
	t.demo_method()

结果

	<__main__.DemoClass instance at 0x00000000024EAA48>
	__main__.DemoClass

其实也可以不必写成`self`，改为其他字符也可以。

	class DemoClass:
	    def demo_method(this):
	       print(this)
	       print(this.__class__)
	
	t = DemoClass()
	t.demo_method()


替换为`this`，一样可以运行成功。

__self不写的结果__

定义一个类，实例化，并调用其中的方法。

	class DemoClass:
	    def demo_method():
	        print("test")
	my_object = DemoClass()
	my_object.demo_method()

出现错误。`my_object.demo_method()`不需要参数，但是给了1个。

	 my_object.demo_method()
	TypeError: demo_method() takes no arguments (1 given)

没有定义参数但是却传入了一个参数。

类的实例以及调用方法的两种形式。

	class DemoClass:
	    def demo_method(self):
	       print(self)
	
	
	t = DemoClass()
	t.demo_method()
	#上面两个和下面的性质一致的
	DemoClass.demo_method(t)

__self类的实例__

	Class A:
		def methodA(self,arg1,arg2):
			#do something

and `ObjectA` 是类的一个实例。

	ObjectA = A()
	ObjectA.methodA(arg1,arg2)

	== 等价于
	ClassA.methodA(ObjectA,arg1,arg2)





#### 参考

[Python中self的含义](http://www.cnblogs.com/jessonluo/p/4717140.html)

[What is the purpose of self?](https://stackoverflow.com/questions/2709821/what-is-the-purpose-of-self?utm_medium=organic&utm_source=google_rich_qa&utm_campaign=google_rich_qa)


