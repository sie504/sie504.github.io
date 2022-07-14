---
title: Python 文件处理
date: 2017-07-26 13:20:46
tags:
		- python
categories:
		- 编程学习

---

`Python`文件操作学习。

<!-- more -->



时间 2017/7/26

文件：python中文件是对象

一切设备都可以称对象

文件的打开方式

open

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-7-26/64221742.jpg)

读

read([size])方法         读取文件，读取size字节

readline([size])      读取一行

readlines([size])    读取完文件，返回每一行所组成的列表

文件写入方式

write(str) 将字符串写入文件

writelines()      写多行到文本




文件打开方式


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-7-26/17910483.jpg)


write写的时候，如果文件不存在可以创建。。

	In [4]: f = open('1.txt')
	
	In [5]: c = f.read()
	
	In [6]: c
	Out[6]: '111111111\n2222222\n333333333\n\n'
	
	In [7]: f.write("test")
	---------------------------------------------------------------------------
	IOError                                   Traceback (most recent call last)
	<ipython-input-7-ea377aa808ac> in <module>()
	----> 1 f.write("test")
	
	IOError: File not open for writing
	
	In [8]: f = open('2.txt','w')
	
	In [9]: f.write("test for write")
	
	In [10]: f.close()
	
	In [11]: ls
	1.txt  2.txt


如果存在这个文件的话，以`w`的方式写的话，之前的内容会被清空。。。
追加的方式打开，之前的内容还会存在。。。

	In [16]: f = open('1.txt','a')
	
	In [17]: f.write("test")
	
	In [18]: f.close()
	
	In [19]: cat 1.txt
	111111111
	2222222
	333333333
	
	test

r+/w+ 即可读又可写

	In [21]: f = open('1.txt','w+')
	
	In [22]: f.read()
	Out[22]: ''
	
	In [23]: f.write('write and read')
	
	In [24]: f.close()
	
	In [25]: cat 1.txt
	write and read
	In [26]: f = open('1.txt','r+')
	
	In [27]: f.read()
	Out[27]: 'write and read'
	
	In [28]: f.write('2222222')
	
	In [29]: f.read()
	Out[29]: ''
	
	In [30]: f.close()
	In [31]: cat 1.txt
	write and read2222222


二进制打开。。打开、读取、写入文件。。。


* 文件的读取方式

	readline(size) 		len(line) > size   return size
						len(line) < size   return len(line)



	In [47]: f = open('1.txt')
	
	In [48]: c = f.readlines()
	
	In [49]: c
	Out[49]: ['write and read2222222111111111\n', '222222222\n', '3333333333\n']


readlines() 读取缓存的

iter跌代器对文件进行操作，一行的进行操作

	In [1]: f = open('1.txt')
	
	In [2]: iter_f = iter(f)
	
	In [3]: lines = 0
	
	In [4]: for line in iter_f:
	   ...:     lines += 1
	   ...:     
	
	In [5]: lines
	Out[5]: 3



文件的写入

	In [7]: f = open('i.txt','w')
	
	In [8]: f.write('test write')
	
	In [9]: f.close()
	
	In [10]: cat i.txt
	test write
	In [11]: f = open('i.txt','w')
	
	In [12]: f.writelines('123456')
	
	In [13]: f.writelines((1,2,3,4,5))
	---------------------------------------------------------------------------
	TypeError                                 Traceback (most recent call last)
	<ipython-input-13-5997f36e60df> in <module>()
	----> 1 f.writelines((1,2,3,4,5))
	
	TypeError: writelines() argument must be a sequence of strings
	
	In [14]: f.writelines(('1','2','3'))
	
	In [15]: f.writelines(['1','2','3'])


写入的时候，要close()或者flush方法。。。

写文件的过程

	In [18]: f = open('i.txt','w')
	
	In [19]: f.write('test for write')
	
	In [20]: cat i.txt
	
	In [21]: f.flush()
	
	In [22]: cat i.txt
	test for write
	In [23]: f.close()



缓冲的大小

* 文件的关闭

将缓存同步到磁盘

Linux系统中每个进程打开文件的个数是有限的

* 文件写入和读取问题


写入文件后，必须打开才能读取写入内容

读取文件后，无法重新再次读取读过的内容

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-7-26/80429679.jpg)


* python文件指针操作

	seek(offset,[,whence]): 移动文件指针
		offset 偏移量，可以为负数
		whence:偏移相对位置
		os.SEEK_SET 相对文件起始位置  0
		os.SEEK_CUR 相对文件当前位置    1
		os.SEEK_END 相对文件结尾位置   2

tell()读取所在位置。。。

	In [2]: f = open('1.txt','w')
	
	In [3]: f.write('0123456789abcdef')
	
	In [4]: f.close()
	
	In [5]: cat 1.txt
	0123456789abcdef
	In [6]: import os
	
	In [7]: f = open('1.txt','r+')
	
	In [8]: f.tell()
	Out[8]: 0L
	
	In [9]: f.read(3)
	Out[9]: '012'
	
	In [10]: f.read(3)
	Out[10]: '345'

	In [11]: f.tell()
	Out[11]: 6L
	In [13]: f.seek(0,os.SEEK_SET)

	In [14]: f.tell()
	Out[14]: 0L


* 文件属性

	file.fileno()   文件描述符
	file.mode()    文件打开权限
	file.encoding:文件编码格式
	file.closed:文件是否关闭
	文件标准输入： sys.stdin
	文件标准输出： sys.stdout
	文件标准错误： sys.stderr

	In [22]: f = open('1.txt')
	
	In [23]: f.fileno
	Out[23]: <function fileno>
	
	In [24]: f.fileno()
	Out[24]: 7
	
	In [25]: f.mode
	Out[25]: 'r'
	
	In [26]: f.closed
	Out[26]: False


	In [28]: import sys
	
	In [29]: type(sys.stdin)
	Out[29]: file
	
	In [30]: sys.stdin.mode
	Out[30]: 'r'
	
	In [31]: sys.stdin.fileno()
	Out[31]: 0
	
	In [32]: a = raw_input(":")
	:00000
	
	In [33]: a
	Out[33]: '00000'
	
	In [34]: sys.stdout.mode
	Out[34]: 'w'
	
	In [35]: sys.stdout.write('1000')
	1000
	In [36]: print '1111111'

文件命令行参数

	  1 #!/usr/bin/python
	  2 
	  3 import sys
	  4 if __name__ == '__main__':
	  5     print len(sys.argv)
	  6     for arg in sys.argv:
	  7         print arg


	root@Ubuntu:~/show_me_code/XTeach/file# python arg.py 0 1 2 3
	5
	arg.py
	0
	1
	2
	3

把汉字转换为utf-8,进行写入文件，，，

	In [1]: a = unicode.encode(u'木了','utf-8')
	
	In [2]: a
	Out[2]: '\xe6\x9c\xa8\xe4\xba\x86'


使用codecs模块提供方法创建指定编码格式文件。。

	open(fname,mode,encoding,errors,buffering):
	使用指定编码格式打开文件。。

	In [3]: import codecs
	
	In [4]: f = codecs.open('test.txt','w','utf-8')
	
	In [5]: f.write(u'米')
	
	In [6]: f.close()
	
	In [7]: cat test.txt
	米


* Linux文件系统简介

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-7-26/78191643.jpg)


open、write、read

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-7-26/90575233.jpg)

* os模块打开文件

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-7-26/46833136.jpg)

	In [8]: import os
	
	In [9]: fd = os.open('immo.txt',os.O_CREAT | os.O_RDWR)
	
	In [10]: ls
	1.txt  2.txt  arg.py  immo.txt*  i.txt  test.txt
	
	In [11]: n = os.write(fd,'immo')
	
	In [12]: l = os.lseek(fd,0,os.SEEK_SET)
	
	In [13]: l
	Out[13]: 0L
	
	In [14]: str = os.read(fd,5)
	
	In [15]: str
	Out[15]: 'immo'
	
	In [16]: os.close(fd)


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-7-26/78603138.jpg)


os模块可以创建文件以及目录，类似底层命令的操作。。。

* os.path模块方法

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-7-26/5103583.jpg)

* 练习


`ConfigParser`

	In [26]: import ConfigParser
	
	In [27]: ConfigParser.
	ConfigParser.ConfigParser
	ConfigParser.DEFAULTSECT
	ConfigParser.DuplicateSectionError
	ConfigParser.Error
	ConfigParser.InterpolationDepthError
	ConfigParser.InterpolationError
	ConfigParser.InterpolationMissingOptionError
	ConfigParser.InterpolationSyntaxError
	ConfigParser.MAX_INTERPOLATION_DEPTH
	ConfigParser.MissingSectionHeaderError
	ConfigParser.NoOptionError
	ConfigParser.NoSectionError
	ConfigParser.ParsingError
	ConfigParser.RawConfigParser
	ConfigParser.SafeConfigParser
	ConfigParser.re
	
	In [27]: cfg = ConfigParser.ConfigParser()
	
	In [28]: cfg.
	cfg.OPTCRE          cfg.getfloat        cfg.read
	cfg.OPTCRE_NV       cfg.getint          cfg.readfp
	cfg.SECTCRE         cfg.has_option      cfg.remove_option
	cfg.add_section     cfg.has_section     cfg.remove_section
	cfg.defaults        cfg.items           cfg.sections
	cfg.get             cfg.options         cfg.set
	cfg.getboolean      cfg.optionxform     cfg.write
	
	In [28]: cfg.


例子

	In [30]: import ConfigParser
	
	In [31]: cfg = ConfigParser.ConfigParser()
	
	In [32]: cat 1.txt
	[userinfo]
	name = zhangsan
	pwd = abo
	
	[study]
	python_base = 15
	linux_base = 15
	
	
	In [33]: cfg.read('1.txt')
	Out[33]: ['1.txt']
	In [35]: cfg.sections()
	Out[35]: ['userinfo', 'study']

	In [36]: for se in cfg.sections():
	   ....:     print se
	   ....:     print cfg.items(se)
	   ....:     
	userinfo
	[('name', 'zhangsan'), ('pwd', 'abo')]
	study
	[('python_base', '15'), ('linux_base', '15')]



修改内容

	In [37]: cfg.set('userinfo','pwd','123456')
	
	In [38]: for se in cfg.sections():
	    print se
	    print cfg.items(se)
	   ....:     
	userinfo
	[('name', 'zhangsan'), ('pwd', '123456')]
	study
	[('python_base', '15'), ('linux_base', '15')]


可以插入。删除。。

	In [39]: cfg.set('userinfo','email','123456@sina.com')
	
	In [40]: for se in cfg.sections():
	    print se
	    print cfg.items(se)
	   ....:     
	userinfo
	[('name', 'zhangsan'), ('pwd', '123456'), ('email', '123456@sina.com')]
	study
	[('python_base', '15'), ('linux_base', '15')]

删除
	
	In [41]: cfg.remove_option('userinfo','email')
	Out[41]: True
	
	In [42]: for se in cfg.sections():
	    print se
	    print cfg.items(se)
	   ....:     
	userinfo
	[('name', 'zhangsan'), ('pwd', '123456')]
	study
	[('python_base', '15'), ('linux_base', '15')]


一个小例子

	  1 [userinfo]
	  2 name = mulibo
	  3 pwd = 123456
	  4 
	  5 [study]
	  6 python_base = 15
	  7 linux_base = 15
	  8 

代码

	#!/usr/bin/python
	# -*- coding:utf-8 -*-
	import os
	import os.path
	import ConfigParser
	
	'''
	1: dump ini
	2: del section
	3: del item
	4: modify item
	5: add section
	6: save modify
	'''
	
	class student_info(object):
	    def __init__(self,recordfile):
	        self.logfile = recordfile      #记录文件名字
	        self.cfg = ConfigParser.ConfigParser()   
	        #使用ConfigParser操作文件，创建对象
	
	    def cfg_load(self):
	        self.cfg.read(self.logfile)      
	
	    def cfg_dump(self):
	        se_list = self.cfg.sections()    #通过section把文件显示出来
	        print "===============>"
	        for se in se_list:
	            print se
	            print self.cfg.items(se)
	        print "<================"
	    
	    def delete_item(self,section,key):
	        #删除一个条目
	        self.cfg.remove_option(section,key)
	
	    def delete_section(self,section):
	        #删除一个section
	        self.cfg.remove_section(section)
	    
	    def add_section(self,section):
	        self.cfg.add_section(section)
	
	    def set_item(self,section,key,value):
	        self.cfg.set(section,key,value)
	
	    def save(self):
	        #保存文件
	        fp = open(self.logfile,'w')
	        self.cfg.write(fp)
	        fp.close()
	
	if __name__ == '__main__':
	    info = student_info('1.txt')
	    info.cfg_load()
	    info.cfg_dump()
	    info.set_item('userinfo','pwd','abc')
	    info.cfg_dump()
	    info.add_section('login')
	    info.set_item('login','2017-0726','20')
	    info.cfg_dump()
	    info.save()


运行结果

	root@Ubuntu:~/show_me_code/XTeach/file# python file.py 
	===============>
	userinfo
	[('name', 'mulibo'), ('pwd', '123456')]
	study
	[('python_base', '15'), ('linux_base', '15')]
	<================
	===============>
	userinfo
	[('name', 'mulibo'), ('pwd', 'abc')]
	study
	[('python_base', '15'), ('linux_base', '15')]
	<================
	===============>
	userinfo
	[('name', 'mulibo'), ('pwd', 'abc')]
	study
	[('python_base', '15'), ('linux_base', '15')]
	login
	[('2017-0726', '20')]
	<================


