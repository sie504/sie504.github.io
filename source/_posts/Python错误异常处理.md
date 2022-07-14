---
title: Python 异常处理
date: 2017-07-19 13:20:46
tags:
		- python
categories:
		- 编程学习

---

`Python`异常处理学习。

<!-- more -->


2017/7/19

[Python错误和异常](http://www.imooc.com/learn/457)

#### 错误和异常

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-7-19/86330832.jpg)

错误

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-7-19/93205306.jpg)

异常

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-7-19/2074392.jpg)

错误和异常区别

编写的代码出现语法错误或者逻辑错误，程序是不能正常运行的。异常是在程序运行后，碰到特殊情况而是程序不能继续运行。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-7-19/15304830.jpg)

#### python常见错误

* a: **NameError**

变量没有定义就被使用

	In [1]: a
	---------------------------------------------------------------------------
	NameError                                 Traceback (most recent call last)
	<ipython-input-1-60b725f10c9c> in <module>()
	----> 1 a
	
	NameError: name 'a' is not defined


* if True **SyntaxError**

语法错误。。

	In [2]: if a
	  File "<ipython-input-2-73bf85fc5fba>", line 1
	    if a
	        ^
	SyntaxError: invalid syntax


* f = open('1.txt') : **IOError**

文件处理错误

	In [4]: f = open('test1.txt')
	---------------------------------------------------------------------------
	IOError                                   Traceback (most recent call last)
	<ipython-input-4-2659d8b9b5b7> in <module>()
	----> 1 f = open('test1.txt')
	
	IOError: [Errno 2] No such file or directory: 'test1.txt'



* 10/0 **ZeroDivisionError**

除数为0的错误

	In [5]: 10 / 0
	---------------------------------------------------------------------------
	ZeroDivisionError                         Traceback (most recent call last)
	<ipython-input-5-3e699e72a4ab> in <module>()
	----> 1 10 / 0
	
	ZeroDivisionError: integer division or modulo by zero

* a = int('dd') **ValueError**

类型错误

	In [8]: a = int('a100')
	---------------------------------------------------------------------------
	ValueError                                Traceback (most recent call last)
	<ipython-input-8-c5ee1e4f90b8> in <module>()
	----> 1 a = int('a100')
	
	ValueError: invalid literal for int() with base 10: 'a100'



* Ctrl+c 错误 **KeyboardInterrupt** 

	In [9]: import time
	
	In [10]: for i in range(10):
	   ....:     time.sleep(2)
	   ....:     
	^C---------------------------------------------------------------------------
	KeyboardInterrupt                         Traceback (most recent call last)
	<ipython-input-10-ab722fa29748> in <module>()
	      1 for i in range(10):
	----> 2     time.sleep(2)
	      3 
	
	KeyboardInterrupt: 


#### try -- except 异常处理

使用try捕获要处理的代码，其中except用来处理异常。。
自己可以设置，是那些类型的异常，来进行处理。。Exception可以设置。。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-7-19/63844858.jpg)


	  1 #!/usr/bin/python
	  2 
	  3 a
	  4 print "come on"

运行结果，可以看到出现了,NameError的错误。

	root@Ubuntu:~/show_me_code/XTeach# python error.py 
	Traceback (most recent call last):
	  File "error.py", line 3, in <module>
	    a
	NameError: name 'a' is not defined

捕获一下异常

	  1 #!/usr/bin/python
	  2 try:
	  3     a
	  4 except NameError,e:
	  5     print "catch Error:", e
	  6 
	  7 print "come on"

可以看到运行结果，把异常捕获到，进行处理，后面的内容还是可以进行运行的。
其中的e就是捕获到的异常错误信息。

	root@Ubuntu:~/show_me_code/XTeach# python error.py 
	catch Error: name 'a' is not defined
	come on

这个异常的是因为a没有被定义，现在定义一个a。

	  1 #!/usr/bin/python
	  2 a = 0
	  3 try:
	  4     a
	  5 except NameError,e:
	  6     print "catch Error:", e
	  7 
	  8 print "come on"

运行结果，就不会出现捕获的异常了。

	root@Ubuntu:~/show_me_code/XTeach# python error.py 
	come on


* try-except 捕获异常分析

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-7-19/80586903.jpg)

	  1 try:
	  2     undef
	  3 except:
	  4     print "catch error"

结果

	root@Ubuntu:~/show_me_code/XTeach# python error2.py 
	catch error


	  1 try:
	  2    if  undef
	  3 except:
	  4     print "catch error"

这次不能捕获到异常了。

	root@Ubuntu:~/show_me_code/XTeach# python error2.py 
	  File "error2.py", line 2
	    if  undef
	            ^
	SyntaxError: invalid syntax


case1： 可以捕获异常，因为是运行时错误

case2：不能捕获异常，因为是语法错误，运行前错误。。

py文件生成字节码，，

* try--except 捕获异常分析

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-7-19/97288280.jpg)

设置错误的类型。。

	  1 try:
	  2    undef
	  3 except NameError, e:
	  4     print "catch error", e

设置的错误类型是名字没有被定义。。

	root@Ubuntu:~/show_me_code/XTeach# python error2.py 
	catch error name 'undef' is not defined


设置的捕获错误类型为IOError

	  1 try:
	  2    undef
	  3 except IOError, e:
	  4     print "catch error", e

但是捕获不到。。

	root@Ubuntu:~/show_me_code/XTeach# python error2.py 
	Traceback (most recent call last):
	  File "error2.py", line 2, in <module>
	    undef
	NameError: name 'undef' is not defined


case 3: 可以捕获异常，因为设置捕获NameError异常

case4: 不能捕获异常，因为设置IOError，不会处理NameError。。

**关于异常的处理，首先这个程序能够正常运行，异常捕获需要程序先能运行为前提。捕获的类型也要一一对应，，比如IOError不能处理NameError的异常。。**


* try--except 用例

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-7-19/75246145.jpg)

这个例子，会出现问题。

	  1 #!/usr/bin/python
	  2 
	  3 import random
	  4 
	  5 num = random.randint(0,100)
	  6 
	  7 while True:
	  8     guess = int(raw_input("Enter 1~100: "))
	  9     if guess > num:
	 10         print "guess Bigger:", guess
	 11     elif guess < num:
	 12         print "guess Smaller:", guess
	 13     else:
	 14         print "Guess OK, Game Over"
	 15         break
	 16     print "\n"
	
输入 89a 的时候

	root@Ubuntu:~/show_me_code/XTeach# python error2.py 
	Enter 1~100: 80
	guess Bigger: 80
	
	
	Enter 1~100: 89a
	Traceback (most recent call last):
	  File "error2.py", line 8, in <module>
	    guess = int(raw_input("Enter 1~100: "))
	ValueError: invalid literal for int() with base 10: '89a'
	
如何处理这个异常问题。。

加入一个异常处理，还是出现了 guess Bigger 80,为了美观，需要添加continue

	  1 #!/usr/bin/python
	  2 
	  3 import random
	  4 
	  5 num = random.randint(0,100)
	  6 
	  7 while True:
	  8     try:
	  9         guess = int(raw_input("Enter 1~100: "))
	 10     except ValueError, e:
	 11         print "Enter 1~100"
	 12     if guess > num:
	 13         print "guess Bigger:", guess
	 14     elif guess < num:
	 15         print "guess Smaller:", guess
	 16     else:
	 17         print "Guess OK, Game Over"
	 18         break
	 19     print "\n"



运行结果

	root@Ubuntu:~/show_me_code/XTeach# python error2.py 
	Enter 1~100: 80
	guess Bigger: 80
	
	
	Enter 1~100: 70a
	Enter 1~100
	guess Bigger: 80
	
	
	Enter 1~100: 
        

加入continue

	  1 #!/usr/bin/python
	  2 
	  3 import random
	  4 
	  5 num = random.randint(0,100)
	  6 
	  7 while True:
	  8     try:
	  9         guess = int(raw_input("Enter 1~100: "))
	 10     except ValueError, e:
	 11         print "Enter 1~100"
	 12         continue
	 13     if guess > num:
	 14         print "guess Bigger:", guess
	 15     elif guess < num:
	 16         print "guess Smaller:", guess
	 17     else:
	 18         print "Guess OK, Game Over"
	 19         break
	 20     print "\n"


加入continue之后。。后面的代码就不执行了。。

	root@Ubuntu:~/show_me_code/XTeach# python error2.py 
	Enter 1~100: 90
	guess Bigger: 90
	
	
	Enter 1~100: 45a
	Enter 1~100
	Enter 1~100: 45
	guess Smaller: 45
       


* try-except 处理多个异常

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-7-19/16512962.jpg)

	  1 #!/usr/bin/python
	  2 try:
	  3     f = open('1.txt')
	  4     line = f.read(2)
	  5     num = int(line)
	  6     print "read num=%d" % num
	  7 except IOError,e:
	  8     print "catch Error:", e


不能存在1.txt，运行。

	root@Ubuntu:~/show_me_code/XTeach# python error.py 
	catch Error: [Errno 2] No such file or directory: '1.txt'


添加一个`1.txt`，可以运行成功。。

	root@Ubuntu:~/show_me_code/XTeach# python error.py 
	read num=11


把1.txt的内容改变一下。。`aa11111`

运行时报错。。。

	root@Ubuntu:~/show_me_code/XTeach# python error.py 
	Traceback (most recent call last):
	  File "error.py", line 5, in <module>
	    num = int(line)
	ValueError: invalid literal for int() with base 10: 'aa'


多个捕获异常

	  1 #!/usr/bin/python
	  2 try:
	  3     f = open('1.txt')
	  4     line = f.read(2)
	  5     num = int(line)
	  6     print "read num=%d" % num
	  7 except IOError,e:
	  8     print "catch Error:", e
	  9 except ValueError, e:
	 10     print "catch Error:", e

可以看到能捕获到异常

	root@Ubuntu:~/show_me_code/XTeach# python error.py 
	catch Error: invalid literal for int() with base 10: 'aa'


使用多个except可以处理多个异常类

* try-except--else使用

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-7-19/59541946.jpg)

如果没有异常，执行else语句中的代码。。

	  1 #!/usr/bin/python
	  2 try:
	  3     f = open('1.txt')
	  4     line = f.read(2)
	  5     num = int(line)
	  6     print "read num=%d" % num
	  7 except IOError,e:
	  8     print "catch Error:", e
	  9 except ValueError, e:
	 10     print "catch Error:", e
	 11 else:
	 12     print "No Error"


再修改1.txt里面的内容，执行结果。。。

	root@Ubuntu:~/show_me_code/XTeach# python error.py 
	read num=11
	No Error

* try--finally 语句

finally语句会被执行的。。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-7-19/7307467.jpg)

	  1 try:
	  2     f = open('1.txt')
	  3     print int(f.read())
	  4 finally:
	  5     print "file close"
	  6     f.close()
	
	 
执行结果

	root@Ubuntu:~/show_me_code/XTeach# python error.py 
	1111111
	file close

修改 1.txt中的内容 qq11111

再次执行程序。。可以看到finally的语句被执行了。。

	root@Ubuntu:~/show_me_code/XTeach# python error.py 
	file close
	Traceback (most recent call last):
	  File "error.py", line 3, in <module>
	    print int(f.read())
	ValueError: invalid literal for int() with base 10: 'qq1111111\n'


`try--finally的作用`

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-7-19/83141630.jpg)



* try--except--finally

无论如何都是要执行finally的。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-7-19/94052699.jpg)

	  1 try:
	  2     f = open('1.txt')
	  3     line = f.read(2)
	  4     num = int(line)
	  5     print "read num=%d" % num
	  6 except IOError, e:
	  7     print "catch IOError:", e
	  8 except ValueError, e:
	  9     print "catch ValueError:", e
	 10 finally:
	 11     print "close file"
	 12     f.close()

运行结果

	root@Ubuntu:~/show_me_code/XTeach# python error.py 
	catch ValueError: invalid literal for int() with base 10: 'qq'
	close file

这次运行一下，`2.txt`不存在。。

	  1 try:
	  2     f = open('2.txt')
	  3     line = f.read(2)
	  4     num = int(line)
	  5     print "read num=%d" % num
	  6 except IOError, e:
	  7     print "catch IOError:", e
	  8 except ValueError, e:
	  9     print "catch ValueError:", e
	 10 finally:
	 11     print "close file"
	 12     f.close()

出现的结果。。捕获到了异常，但是`f.close()`有异常，因为`open('2.txt')`异常后，没有对f进行赋值。。

	root@Ubuntu:~/show_me_code/XTeach# python error.py 
	catch IOError: [Errno 2] No such file or directory: '2.txt'
	close file
	Traceback (most recent call last):
	  File "error.py", line 12, in <module>
	    f.close()
	NameError: name 'f' is not defined

解决方案，在添加一个异常处理。。

	  1 try:
	  2     f = open('2.txt')
	  3     line = f.read(2)
	  4     num = int(line)
	  5     print "read num=%d" % num
	  6 except IOError, e:
	  7     print "catch IOError:", e
	  8 except ValueError, e:
	  9     print "catch ValueError:", e
	 10 finally:
	 11     try:
	 12         print "close file"
	 13         f.close()
	 14     except NameError, e:
	 15         print "catch Error:", e


对f没有打开情况，进行一个异常处理。。捕获两个错误。。

	root@Ubuntu:~/show_me_code/XTeach# python error.py 
	catch IOError: [Errno 2] No such file or directory: '2.txt'
	close file
	catch Error: name 'f' is not defined


* try--except--else--finally

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-7-19/76575654.jpg)

	  1 try:
	  2     f = open('1.txt')
	  3     line = f.read(2)
	  4     num = int(line)
	  5     print "read num=%d" % num
	  6 except IOError, e:
	  7     print "catch IOError:", e
	  8 except ValueError, e:
	  9     print "catch ValueError:", e
	 10 else:
	 11     print "No Error"
	 12 finally:
	 13     try:
	 14         print "close file"
	 15         f.close()
	 16     except NameError, e:
	 17         print "catch Error:", e

执行结果

	root@Ubuntu:~/show_me_code/XTeach# python error.py 
	read num=11
	No Error
	close file

* with 语句

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-7-19/11837013.jpg)

with 对一个文件进行操作

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-7-19/35033059.jpg)

实例演习，with和open一个文件

	  1 try:
	  2     f = open('1.txt')
	  3     print "in try f.read():", f.read(2)
	  4 except IOError, e:
	  5     print "catch IOError:", e
	  6 except ValueError, e:
	  7     print "catch ValueError:", e
	  8 finally:
	  9     f.close()
	 10 print "try-finally:",f.closed
	 11 
	 12 with open('1.txt') as f1:
	 13     print "in with f1.read:", f1.read(2)
	 14 
	 15 print "with :", f1.closed

运行结果，关闭状态。。

	root@Ubuntu:~/show_me_code/XTeach# python error.py 
	in try f.read(): 11
	try-finally: True
	in with f1.read: 11
	with : True

with虽然可以关闭文件，但对关闭文件的错误异常没有处理。。

	  1 import os
	  2 try:
	  3     f = open('1.txt')
	  4     print "in try f.read():", f.read(2)
	  5     f.seek(-10,os.SEEK_SET)
	  6 except IOError, e:
	  7     print "catch IOError:", e
	  8 except ValueError, e:
	  9     print "catch ValueError:", e
	 10 finally:
	 11     f.close()
	 12 print "try-finally:",f.closed
	 13 
	 14 with open('1.txt') as f1:
	 15     print "in with f1.read:", f1.read(2)
	 16     f.seek(-10,os.SEEK_SET)
	 17 
	 18 print "with :", f1.closed

可以看到运行的异常

	root@Ubuntu:~/show_me_code/XTeach# python error.py 
	in try f.read(): 11
	catch IOError: [Errno 22] Invalid argument
	try-finally: True
	in with f1.read: 11
	Traceback (most recent call last):
	  File "error.py", line 16, in <module>
	    f.seek(-10,os.SEEK_SET)
	ValueError: I/O operation on closed file


虽然with可以关闭文件，但对关闭的异常没有进行处理。。。

	import os
	
	try:
	    f = open('1.txt')
	    print "in try f.read():", f.read(2)
	    f.seek(-10,os.SEEK_SET)
	except IOError, e:
	    print "catch IOError:", e
	except ValueError, e:
	    print "catch ValueError:", e
	finally:
	    f.close()
	print "try-finally:",f.closed
	
	try:
	    with open('1.txt') as f1:
	        print "in with f1.read:", f1.read(2)
	        f.seek(-10,os.SEEK_SET)
	except ValueError, e:
	    print "in with catch  ValueError:", e
	    print "with :", f1.closed

运行结果。。。

	root@Ubuntu:~/show_me_code/XTeach# python error.py 
	in try f.read(): 11
	catch IOError: [Errno 22] Invalid argument
	try-finally: True
	in with f1.read: 11
	in with catch  ValueError: I/O operation on closed file
	with : True


with语句实质是上下文管理

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-7-19/93566498.jpg)

编写一个类实现with

	  1 #!/usr/bin/python
	  2 
	  3 class Mycontex(object):
	  4     def __init__(self,name):
	  5         self.name = name
	  6 
	  7     def __enter__(self):
	  8         print "__enter__"
	  9         return self
	 10 
	 11     def do_self(self):
	 12         print "do_self"
	 13 
	 14     def __exit__(self,exc_type,exc_value,traceback):
	 15         print "__exit__"
	 16         print "Error:", exc_type, "info:", exc_value
	 17 
	 18 if __name__ == '__main__':
	 19     with Mycontex('test context') as f:
	 20         print f.name
	 21         f.do_self()

运行结果

	root@Ubuntu:~/show_me_code/XTeach# python with.py 
	__enter__
	test context
	do_self
	__exit__
	Error: None info: None


with的使用

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-7-19/86527961.jpg)

* raise 和 assert

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-7-19/34870286.jpg)

assert 语句

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-7-19/2319377.jpg)

* 标准异常和自定义异常

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-7-19/22018523.jpg)

Built-in Exceptions

* 自定义异常

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-7-19/20839219.jpg)


自定义异常示例

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-7-19/81567658.jpg)

	In [5]: class FileError(IOError):
	   ...:     pass
	   ...: 
	
	In [6]: raise FileError, "Test FileError"
	---------------------------------------------------------------------------
	FileError                                 Traceback (most recent call last)
	<ipython-input-6-822ad3f0675d> in <module>()
	----> 1 raise FileError, "Test FileError"
	
	FileError: Test FileError

	In [7]: try:
	   ...:     raise FileError, "Test FileError"
	   ...: except FileError, e:
	   ...:     print e
	   ...:     
	Test FileError



[内建异常](https://docs.python.org/2/library/exceptions.html?highlight=exception#module-exceptions)

产生异常，抛出异常、捕获异常。。分析异常。。

	  1 #!/usr/bin/python
	  2 
	  3 class CustomError(Exception):
	  4     def __init__(self,info):
	  5         Exception.__init__(self)
	  6         self.errorinfo = info
	  7         print id(self)
	  8 
	  9     def __str__(self):
	 10         return "CustionError: %s" % self.errorinfo
	 11 try:
	 12     raise CustomError("test CustomError")
	 13 except CustomError, e:
	 14     print "ErrorInfo:%d,%s" %(id(e),e)


运行结果

	root@Ubuntu:~/show_me_code/XTeach# python except.py 
	3071267020
	ErrorInfo:3071267020,CustionError: test CustomError
