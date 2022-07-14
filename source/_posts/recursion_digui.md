---
title: 递归
date: 2018-04-27 13:20:46
tags:
		- 
categories:
		- 算法与数据结构

---

递归学习。

<!-- more -->


#### [递归](https://baike.baidu.com/item/%E9%80%92%E5%BD%92)

递归

方法内部自己调用自己。函数的定义中使用函数自身的办法。

[递归算法详解](https://chenqx.github.io/2014/09/29/Algorithm-Recursive-Programming/)

[精通递归程序设计](https://www.ibm.com/developerworks/cn/linux/l-recurs.html)

[递归算法](https://blog.csdn.net/wangjinyu501/article/details/8248492)

[递归算法总结](https://www.jianshu.com/p/0c5db522eabb)

递归算法是一种直接或者间接调用自身函数或者方法的算法。递归算法的实质是把问题分解成规模缩小的同类问题的子问题。

递归算法解决问题的特点：

- 递归就是方法里面调用自身
- 使用递归的策略中，必须有一个明确的递归结束条件，称为递归出口
- 运行效率较低

案例

	def fact(n):
	    if n == 1:
	        return 1
	    else:
	        return n * fact(n-1)
	
	print fact(4)


只要初始值大于零，这个函数就能够终止。停止的位置称为基线条件(base case)。基线条件是递归程序的最底层位置，在此位置没有必要再进行操作，可以直接返回一个结果。所有递归程序必须至少有一个基线条件，而且必须确保它们最终达到某个基线条件；否则程序将永远运行下去，直到程序缺少内存或者栈空间。


执行过程

	def factorial(n):
	    print("factorial has been called with n = " + str(n))
	    if n == 1:
	        return 1
	    else:
	        res = n * factorial(n-1)
	        print("intermediste result for ",n," * factorial(",n-1,"): ",res)
	        return res
	
	print(factorial(5))

结果

	factorial has been called with n = 5
	factorial has been called with n = 4
	factorial has been called with n = 3
	factorial has been called with n = 2
	factorial has been called with n = 1
	('intermediste result for ', 2, ' * factorial(', 1, '): ', 2)
	('intermediste result for ', 3, ' * factorial(', 2, '): ', 6)
	('intermediste result for ', 4, ' * factorial(', 3, '): ', 24)
	('intermediste result for ', 5, ' * factorial(', 4, '): ', 120)
	120


#### 案例

##### Fibonacci Sequence

代码

	def fibonacci(n):
	    if n == 1:
	        return 1
	    elif n == 2:
	        return 1
	    elif n > 2:
	        return fibonacci(n-1) + fibonacci(n-2)
	
	for n in range(1,11):
	    print(n,":",fibonacci(n))

结果

	(1, ':', 1)
	(2, ':', 1)
	(3, ':', 2)
	(4, ':', 3)
	(5, ':', 5)
	(6, ':', 8)
	(7, ':', 13)
	(8, ':', 21)
	(9, ':', 34)
	(10, ':', 55)


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-4-25/53011983.jpg)


`Memoization`  记忆，备忘


	fibonacci_cache = {}
	
	def fibonacci(n):
	    # if we have cached the value,then return it
	    if n in fibonacci_cache:
	        return fibonacci_cache[n]
	
	    # Compute the Nth term
	    if n == 1:
	        value = 1
	    elif n == 2:
	        value = 1
	    elif   n > 2:
	        value = fibonacci(n-1) + fibonacci(n-2)
	
	    fibonacci_cache[n] = value
	    return value
	
	for n in range(1,5):
	    print(n,":",fibonacci(n))
	    


结果

	(1, ':', 1)
	(2, ':', 1)
	(3, ':', 2)
	(4, ':', 3)


`print fibonacci_cache`

	{1: 1, 2: 1, 3: 2, 4: 3}


functools


##### Hanoi

汉诺塔

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-4-25/8166017.jpg)


从左到右三个柱子，大盘子在下面，小盘子在上，借助B柱将A柱所有的盘子移到C上面。且大盘子只能在小盘子的下面。


最基本的情况是，一个盘子，从`A` 移到 `C`。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-4-27/31832186.jpg)

使用中间杆将塔从起始杆移动到目标杆的步骤。

1. 使用目标杆将 `height-1` 个塔移动到中间杆
2. 将剩余的一个塔移到目标杆
3. 使用起始干将 `height-1` 个塔从中间杆移到目标杆

较大的盘子保留在栈的底部。可以使用递归的三个步骤，处理任何更大的盘子。识别基本条件。最简单的汉诺塔是一个盘子的塔，这种情况下，只需要将一个盘子移动到


	# fromPole 原始位置
	# toPole  目标位置
	# wiehPole 借助位置
	def moveTower(height,fromPole,toPole,withPole):
	    if height >= 1:
	        moveTower(height-1,fromPole,withPole,toPole)
	        moveDisk(fromPole,toPole)
	        moveTower(height-1,withPole,toPole,fromPole)
	
	def moveDisk(fp,tp):
	    print("moving disk from",fp,"to",tp)
	
	moveTower(3,"A","C","B")

结果

	('moving disk from', 'A', 'to', 'C')
	('moving disk from', 'A', 'to', 'B')
	('moving disk from', 'C', 'to', 'B')
	('moving disk from', 'A', 'to', 'C')
	('moving disk from', 'B', 'to', 'A')
	('moving disk from', 'B', 'to', 'C')
	('moving disk from', 'A', 'to', 'C')


代码2

	def move(n,a,buffer,c):
	    if(n == 1):
	        print(a,"->",c)
	        return
	    move(n-1,a,c,buffer)
	    move(1,a,buffer,c)
	    move(n-1,buffer,a,c)
	
	move(3,"a","b","c")


结果

	('a', '->', 'c')
	('a', '->', 'b')
	('c', '->', 'b')
	('a', '->', 'c')
	('b', '->', 'a')
	('b', '->', 'c')
	('a', '->', 'c')


![](https://pic2.zhimg.com/v2-59507a712f50971601587cc970f75904_r.jpg)


##### 整数转换为任意进制字符串

将数字`769`转换为三个单个位数字，`7`，`6`，`9`，将其转换为字符串。

数字小于 `10` 是一个基本情况。(基线条件)

知道基本情况是什么，意味着整个算法可以分成三个部分。

1. 将原始数字减少为一些列单个位数字
2. 使用查找将单个数字转换为字符串
3. 将单个字符串连接在一起形成最终结果

下一步找到改变其状态的方法并向基本情况靠近。使用余数的整数除法为其提供了一个明确的方向，

整数除法 。 `769` 除以 `10` 得到 `76`，余数 `9`。

余数是小于基数(10)的数字，可以通过查找立即转换为字符串。

得到的商小于原始数字，并让我们可以靠近小于基数的单个数字的基本情况。现在要做的是将`76`转换为字符串表示。

再次使用商和余数分别得到 `7` 和 `6`的结果。最后将问题减少到转换 `7`，这时可以很容易做到，因为 `n<base(7 < 10)`的基本条件。

转换过程，余数位于图右框中。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-4-26/98125622.jpg)


	def toStr(n,base):
	    convertString = "0123456789ABCDEF"
	    if n < base:
	        return convertString[n]
	    else:
	        return toStr(n//base,base) +' '+ convertString[n%base]
	
	print toStr(769,10)


结果

	7 6 9



##### 视频学习

[6. Recursion and Dictionaries](https://www.youtube.com/watch?v=WPSeyjX1-4s)


