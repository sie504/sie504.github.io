---
title: 栈
date: 2018-03-21 13:20:46
tags:
		- 
categories:
		- 算法与数据结构

---

Python数据结构--栈。

<!-- more -->

#### [栈](https://interactivepython.org/courselib/static/pythonds/BasicDS/TheStackAbstractDataType.html)

栈是一种特殊的列表，插入和删除都在末尾进行。所谓的先进后出，后进先出的操作。

栈的基本操作有`push`(入栈)和`pop`(出栈)。

	Stack()    建立一个空的栈对象
	push()     把一个元素添加到栈的最顶层
	pop()      删除栈最顶层的元素，并返回这个元素
	peek()     返回最顶层的元素，并不删除它
	isEmpty()  判断栈是否为空
	size()     返回栈中元素的个数

用列表来表示栈

	>>> stack = [3,4,5]
	>>> stack.append(6)
	>>> stack.append(7)
	>>> stack
	[3, 4, 5, 6, 7]
	>>> stack.pop()
	7
	>>> stack
	[3, 4, 5, 6]
	>>> stack.pop()
	6
	>>> stack
	[3, 4, 5]

#### 代码



	#!/usr/bin/python
	
	class Stack:
	    def __init__(self):
	        self.items = []
	        
	    def isEmpty(self):
	        return self.items == []
	        
	    def push(self,item): #插入一个元素
	        self.items.append(item)
	        
	    def pop(self):
			#这个可以添加一个异常判断
	        return self.items.pop()
	        
	    def peek(self):      #返回最后一个元素，但不删除
	        return self.items[-1]
	        
	    def size(self):      #长度
	        return len(self.items)
	        
	s=Stack()
	
	print(s.isEmpty())
	s.push(4)
	s.push('dog')
	print(s.peek())
	s.push(True)
	print(s.size())
	print(s.isEmpty())
	s.push(8.4)
	print(s.pop())
	print(s.pop())
	print(s.size())      

结果

	C:\Users\Administrator\Desktop\Algo>python3 stack.py
	True
	dog
	3
	False
	8.4
	True
	2

#### 栈应用实例

了解栈的基本概念之后，看几个应用实例。

##### 括号匹配1

这个略简单。至匹配一种括号。使用了上面定义的栈类。

	#! -*- coding:utf-8 -*-
	
	from stack import Stack
	
	def parChecker(symbolString):
	    s = Stack()
	    balanced = True
	    index = 0
	    while index < len(symbolString) and balanced:
	        symbol = symbolString[index]
	        if symbol == "(":
	            s.push(symbol)    #入栈
	        else:
	            if s.isEmpty():
	                balanced = False 
	            else:
	                s.pop()   #出栈
	                
	        index = index + 1 
	
	    if balanced and s.isEmpty():
	        return True 
	    else:
	        return False 
	        
	print(parChecker('((()))'))
	print(parChecker('(()'))        


这个函数`parChecker`假定一个Stack类可用，并返回一个布尔值来确定括号字符串是否平衡。注意，平衡布尔变量初始化为True，因为在开始的时候没有其他项目。如果当前符号为`(`，那么就入栈。`pop`是从堆栈中移除一个符号，返回值没有被使用。只要表达式是平衡的并且堆栈已经完全移除，字符串就是平衡的括号序列。


##### 括号匹配2

这次匹配多个符号，`([{}])`。

唯一的变化是 16 行，我们称之为辅助函数匹配。必须检查栈中每个删除的符号，以查看它是否与当前结束符号匹配。如果不匹配，则布尔变量 balanced 被设置为 False。

一个Matches函数。

	#! -*- coding:utf-8 -*-
	
	from stack import Stack
	
	def parChecker(symbolString):
	    s = Stack()
	    balanced = True
	    index = 0
	    while index < len(symbolString) and balanced:
	        symbol = symbolString[index]
	        if symbol in "([{":
	            s.push(symbol)    #入栈
	        else:
	            if s.isEmpty():
	                balanced = False 
	            else:
	                top = s.pop()   #出栈，调用自定义函数 matches
	                if not matches(top,symbol):
	                    balanced = False
	                
	        index = index + 1 
	
	    if balanced and s.isEmpty():
	        return True 
	    else:
	        return False 
	        
	def matches(open,close):    #matches 函数
	    opens = "([{"
	    closers = ")]}"
	    return opens.index(open) == closers.index(close)   #查找的位置一样 1
	    # index 返回查找字符串的位置
	    
	print parChecker('{{([][])}()}')
	print parChecker('[{()]')


##### 括号匹配3

	>>> ord('('),ord(')')
	(40, 41)
	>>> ord('['),ord(']')
	(91, 93)
	>>> ord('{'),ord('}')
	(123, 125)

假设一个表达式包含三种括号`()`、`[]`、`{}`，嵌套顺序是任意的。

__正确格式__

	{()[()]},[{({})}]

__错误格式__

		
	[(]),[()),(()}

编写一个函数，判断一个表达式字符串，括号匹配是否正确。


1. 创建一个空栈，用来存储尚未找到的左括号；
1. 遍历字符串，遇到左括号则压栈，遇到右括号则出栈一个左括号进行匹配；
1. 在第二步骤过程中，如果空栈情况下遇到右括号，说明缺少左括号，不匹配；
1. 在第二步骤遍历结束时，栈不为空，说明缺少右括号，不匹配；

代码

	# -*- coding:utf-8 -*-
	
	LEFT = {'(','[','{'}   #左括号
	RIGHT = {')',']','}'}  #右括号
	#注意：这个比较的字符串必须是对称的，例如[{({})}]，从最里面的{}进行左右分隔
	# 如果不是对称的，在其中的左右边一定先会有一对匹配的括号且它们中间没有其他符号
	def match(expr):
	    '''    
	    :param expr:  检测的字符串
	    :return: 返回结果
	    '''
	    stack = [] #创建一个栈
	    for b in expr:   #迭代传过来的所有字符串。遍历完之后，会结束这个for循环，执行return not stack
	        if b in LEFT:   #如果当前字符串在左括号内
	            stack.append(b)  #把当前符号b进行入栈
	        elif b in RIGHT:   #如果在右括号
	            if not stack:
	                return False
	            # a = ord(b) - ord(stack[-1])
	            # print a
	            if not stack or not 1 <= ord(b) - ord(stack[-1]) <=2:
	                # 如果当前栈为空
	                # 如果右括号的减去左括号的值不是小于等于2，大于等于1。匹配的括号的ASCII值，右减左两种情况，1，2
	                return False
	            stack.pop() #删除左括号
	    return not stack
	result = match('[(}){()}]')
	print result


##### 迷宫

用一个二维数组表示一个简单的迷宫，用0表示通路，用1表示阻断，老鼠在每个点上可以移动相邻的东南西北四个点，设计一个算法，模拟老鼠走迷宫，找到从入口到出口的一条路径。

如图所示

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-3-23/16438349.jpg)

出去的正确线路如图中的红线所示

思路

1. 用一个栈来记录老鼠从入口到出口的路径
1. 走到某点后，将该点左边压栈，并把该点值置为1，表示走过了；
1. 从临近的四个点中可到达的点中任意选取一个，走到该点；
1. 如果在到达某点后临近的4个点都不走，说明已经走入死胡同，此时退栈，退回一步尝试其他点；
1. 反复执行第二、三、四步骤直到找到出口；

代码

	>>> maze = [[0]*7 for _ in range(7)]
	>>> maze
	[[0, 0, 0, 0, 0, 0, 0], [0, 0, 0, 0, 0, 0, 0], [0, 0, 0, 0, 0, 0, 0], [0, 0, 0,
	0, 0, 0, 0], [0, 0, 0, 0, 0, 0, 0], [0, 0, 0, 0, 0, 0, 0], [0, 0, 0, 0, 0, 0, 0]
	]
	>>> walls = [
	... (1,3),
	... (2,1),(2,5),
	... (3,3),(3,4),
	... (4,2),
	... (5,4)
	... ]
	>>> for i,j in walls:
	...     print i,j
	...
	1 3
	2 1
	2 5
	3 3
	3 4
	4 2
	5 4
	>>> for i in range(7):
	...     maze[i][0] = maze[i][-1] = 1
	...     maze[0][i] = maze[-1][i] = 1
	...
	>>> maze
	[[1, 1, 1, 1, 1, 1, 1], [1, 0, 0, 0, 0, 0, 1], [1, 0, 0, 0, 0, 0, 1], [1, 0, 0,
	0, 0, 0, 1], [1, 0, 0, 0, 0, 0, 1], [1, 0, 0, 0, 0, 0, 1], [1, 1, 1, 1, 1, 1, 1]
	]
	>>> for i,j in walls:
	...     maze[i][j] = 1
	...
	>>> maze
	[[1, 1, 1, 1, 1, 1, 1], [1, 0, 0, 1, 0, 0, 1], [1, 1, 0, 0, 0, 1, 1], [1, 0, 0,
	1, 1, 0, 1], [1, 0, 1, 0, 0, 0, 1], [1, 0, 0, 0, 1, 0, 1], [1, 1, 1, 1, 1, 1, 1]
	]

maze

	[1, 1, 1, 1, 1, 1, 1]
	[1, 0, 0, 1, 0, 0, 1]
	[1, 1, 0, 0, 0, 1, 1]
	[1, 0, 0, 1, 1, 0, 1]
	[1, 0, 1, 0, 0, 0, 1] 
	[1, 0, 0, 0, 1, 0, 1]
	[1, 1, 1, 1, 1, 1, 1]


里面被包围的才是迷宫。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-3-23/67345560.jpg)

	#!/usr/bin/python
	# -*- coding:utf-8 -*-
	def initMaze():
	    maze = [[0] * 7 for _ in range(5+2)] #用列表解析创建一个7*7的二维数组
	    walls = [   #记录墙的位置
	        (1,3),
	        (2,1),(2,5),
	        (3,3),(3,4),
	        (4,2),
	        (5,4)
	    ]
	    
	    for i in range(7):  #把迷宫的四周设置为墙
	        maze[i][0] = maze[i][-1] = 1
	        maze[0][i] = maze[-1][i] = 1 
	        
	    for i,j in walls:  #把所有墙的点设置为1
	        maze[i][j] = 1
	    return maze
	    
	    
	def path(maze,start,end):
	    '''
	    param maze：迷宫
	    param start: 起始点
	    param end: 结束点
	    return: 行走的每个点
	    '''
	    
	    i,j = start # 分解起始点的坐标
	    ei, ej = end  #分解结束点的左边
	    stack = [(i,j)] #创建一个栈，并让老鼠站在起始点的位置
	    maze[i][j] = 1 #走过的路置为1
	    while stack:    #栈不为空的时候继续走，否则退出
	        i,j = stack[-1] #获取当前老鼠所在的位置
	        if(i,j) == (ei,ej): #如果老鼠找到了出口
	            break      
	        for di,dj in [(0,-1),(0,1),(-1,0),(1,0)]: #左右上下
	            if maze[i+di][j+dj] == 0:   #如果当前点可走
	                maze[i+di][j+dj] = 1 #把当前点置为1
	                stack.append((i+di,j+dj))  #把当前的位置添加到栈里面
	                break
	        else:  #如果所有的点都不能走
	            stack.pop()    #退回上一步
	    return stack
	    
	Maze = initMaze() #初始化迷宫
	result = path(maze=Maze,start=(1,1),end=(5,5)) #老鼠开始走迷宫
	print result
	    
 结果

   
    [(1, 1), (1, 2), (2, 2), (3, 2), (3, 1), (4, 1), (5, 1), (5, 2), (5, 3), (4, 3),
 	(4, 4), (4, 5), (5, 5)]


#### 参考

[Python数据结构——栈](https://segmentfault.com/a/1190000003937981)

[Data Structures](https://docs.python.org/2/tutorial/datastructures.html#more-on-lists)

[Python算法实战系列之栈](http://python.jobbole.com/87581/)
