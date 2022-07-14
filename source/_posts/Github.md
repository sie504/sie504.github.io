---
title: Git学习记录
date: 2017-07-09 18:47:07
tags:
		- git

categories:
		- 技术研究
---

`Github`和`Git`的学习。

<!-- more -->




#### GitHub使用

学习一个新技能，遇见问题是正常的。遇见问题解决问题，一点点的来，不急不躁。耐心，坚持！

Git是由Linux之父Linus Tovalds 为了更好的管理linux内核开发而创立的分布式版本控制/软件配置管理软件。
Git是一个管理你的代码的历史记录的工具。

##### 准备工作

1. 下载  [github for windows](https://github-windows.s3.amazonaws.com/GitHubSetup.exe)
2. 注册  [github帐号](https://github.com/)
3. 登录到github for windows。



#### 创建第一个代码库

点击左上角的+号，建立一个仓库。



##### Add 功能 
如果本地有工程，就可以使用Add添加
##### Clone 功能
克隆的意思。
如何使用Clone功能，就是将在浏览器上已经创建好的项目导入本地，换句话就是下载到本地。
##### Create 功能
创建一个代码库。
Name填写你的仓库名字，Local path写在你要保存到本地路径。

这里填写First，来创建第一个我们的respository。

#### 开始使用第一个代码库
##### 修改第一个代码库中的内容
来到刚刚创建的代码库在本地的位置，就是刚刚在local path的地址路径。
右键选择Open in Exploer。这样就可以转到刚刚的路径下。
在里面新建一个文档。在里面编辑。
此时的github就会变成这个样子(Changes)



你会发现此时的github会出现刚刚编辑的内容
并且前面会有蓝色标识，这个蓝色标识是什么作用呢？
其实这个蓝色标识是提示你会上传改变的文本。(蓝色提示你要改变的内容)

Summary：改动的总结，可以理解为标题，必填
Description：可以理解详细概况，选填

这次我们只选择第一个修改对象，也就是学习Github，这行修改，之后点击Commit to master.
summary为修改标题
Description：描述

切换到History目录下


会发现改变了。
这次把第二行Git添加上去。。



会在History目录下发生这样的改变，会在History目录下形成一天时间线，来指出修改的标题和内容。同时会把修改的内容用绿色标识标出。

打开本地的文本，删除第一行的内容。此时github发生了变化。



此时的红色标识标识删除。
填好Summary和Description并点击Commit to master
这样就会删除了一行。同时在History目录下多了一条时间轴。红色标识。

####上传与同步
##### 上传
此时，当我们打开github网页，就会发现此时你修改的内容并没有出现在这里，这是因为你没有进行同步。只是在本地进行了修改。此时我们需要点击右上角的publish.



此时就可以把本地的内容上传到网页上。

##### 同步

当你的代码上传后就会发现，原理的publish变为了Sync.
点击Sync同步代码库。
5/3/2017 3:36:14 PM 

#### 分支的使用
##### 创建分支
创建第一个分支取名为“new masterh”，点击Create new branch创建第一个分支。


发现此时的分支已经切换到了我们刚刚创建的分支new masterch


来修改new masterch分支上的内容。
我们依然打开test.txt进行编辑，输入以下内容。
`创建第一个分支`
打开github进行，填写Summary和Description
之后点击Commit to new-masterh
在History目录下，可以看到两条主线，分别是master和new-masterh，并且在new-masterh的分支下又出现一个蓝色的实线空心圈和一个虚线空心圈。
实心圈表示当前的节点，空心圈表示下一次修改时的节点。


红线表示的部分就是当前的分支


##### 切换分支

点击红色画线部分就会出现分支的列表。
点击master就会切换到master分支。

#####上传/同步分支
这个操作和同步仓库是一个操作，点击Publish/Sync上传或同步分支。
##### 删除分支
首先把分支切换到你要删除的分支下，如要删除new masterh,将分支切换到new masterh点击右上角齿轮就会出现Delete new masterh。



点击Delete new masterh就会出现一个对话框，询问删除的内容



第一个是yes,Delete both是将本地与网页全部删除
第二个是Delete local only 仅仅是删除本地
第三个是取消

##### 合并两个分支
将一个分支与master分支进行合并。
首先把分支切换到master下，点击Update from new-branch进行分支的合并。


此时查看history目录发现





##### 小技巧
1. 一个小技巧。 fork,在阅读别人的项目的时候，点击这个fork按钮，就可以把别人的项目变成自己的。在自己的目录下，可以任意修改。便于标记记录。
1. 删除项目。进入项目里面，选择设置，然后delete this repository
2. 测试Github

    root@Ubuntu:~/learngit# ssh git@github.com
    PTY allocation request failed on channel 0
    Hi sie504! You've successfully authenticated, but GitHub does not provide shell access.
    Connection to github.com closed.
1. 设置Git全局用户配置
    
    fatal: 远程 origin 已经存在。
    git remote rm origin

全局配置的时候记得配置上自己github的账户以及邮箱。。。
1. fork：创建源项目代码库的分支，并拷贝到自己的帐号中
2. star 关注别人项目更新
3.


#### git 基本操作

Git是分布式版本控制系统。
版本控制系统？如果你用Word编写一个文档，向删除一个段落，又怕将来想恢复找不回怎么办？有办法，先把当前文件另存为一个新的Word文件，改到一定程度，再另存一个新文件，这样一直改下去。

Git分布式系统版本控制系统，集中式版本控制系统和分布式控制系统的区别。。







问题1. `fatal: pathspec 'readme.txt' did not match any files`

路径的问题，你应该将路径cd到仓库的路径。

##### 创建版本库
版本库又叫仓库，repository，可以简单理解为一个目录，这个目录里的所有文件都可以被Git管理，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以跟踪历史，或者在将来某个时刻还原。

选择一个合适的地方，创建一个空目录：
```root@Ubuntu:~# mkdir learngit
root@Ubuntu:~# cd learngit/
root@Ubuntu:~/learngit# pwd
/root/learngit```

第二步，通过git init命令把这个目录变为Git可以管理的仓库：
```root@Ubuntu:~/learngit# git init
初始化空的 Git 版本库于 /root/learngit/.git/```

在learngit目录下，创建一个readme.txt文件，内容如下：

```Git is a version control system.
  2 Git is free software.```


**第一步**:用`git add`告诉Git，把文件添加到仓库：

`# git add readme.txt`

执行上面的命令，没有任何显示，这就对了。Unix的哲学是"没有消息就是好消息" 

**第二步** 用命令 `git commit -m "" `告诉Git，把文件提交到仓库。

工作区和暂存区

**第三步** 推送 `git push`  从远程获取 `git pull `

[http://www.liaoxuefeng.com]()


[http://www.yiibai.com/git/git_fetch.html](http://www.yiibai.com/git/git_fetch.html)

#### 问题



1. 无法推送一些引用到 'git@github.com:sie504/Something_py.git'
提示：更新被拒绝，因为远程版本库包含您本地尚不存在的提交。这通常是因为另外
提示：一个版本库已向该引用进行了推送。再次推送前，您可能需要先整合远程变更
提示：（如 'git pull ...'）。
提示：详见 'git push --help' 中的 'Note about fast-forwards' 小节。

	[http://www.it610.com/article/4921283.htm](http://www.it610.com/article/4921283.htm)


1.   请输入一个提交信息以解释此合并的必要性，尤其是将一个更新后的上游分支
   合并到主题分支。
   以 '#' 开头的行将被忽略，而且空提交说明将会终止提交。
	


1. 您的分支领先 'origin/master' 共 2 个提交。
  （使用 "git push" 来发布您的本地提交）



[github本地修改与远程同步](http://blog.csdn.net/gonghuiyun19910610/article/details/46673897)


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-7-3/51261492.jpg)



[Fork + Pull模式](http://blog.csdn.net/stormbjm/article/details/14455903)



#### [GIT学习](http://www.imooc.com/learn/208)

2017/7/18

版本管理工具。。

发展 `cvs--svn---git---github`

git的下载和安装




	git clone 

提交     git status 状态

关于代码的冲突、、冲突是怎么造成的。。

同一时间，同一代码。你也改了，我也改了代码。。

更新被拒绝。。git pull 合并冲突。。

git diff 查看差别。。

回到过去

	git log 
	git reset --hard
	git reflog

建立里程碑

分支开发之分支合并

分支开发到master。。

多人合作的一些经验

整理好自己的工作区。。。

并行的项目，使用分支开发

遇到冲突的时候，搞明白冲突的原因，千万不要随意修改别人的代码

产品发布后，记得打tag。。


----------

20170914


GIT技能书

[闯过这 54 关，点亮你的 Git 技能树](https://codingstyle.cn/topics/51)


__test__

>Github新建一个项目
> git clone git@github.com:sie504/Django_mooc.git 下载下来
> 
> git add readme.txt
> 
> git commit -m ""告诉Git
> 
> 推送 git push 
> 
> git pull


__回滚某个版本__

首先把项目`clone`到其他文件夹下。


	git clone git@github.com:spring-guides/gs-messaging-stomp-websocket.git

进入该项目的文件夹下面

回滚到某个版本

	git checkout 6958af0b02bf05282673826b73cd7a85e84c12d3
 

#### 其他

2017/7/17 慕课 github

版本控制系统

初始化项目的时候，创建一个readme.md

commit 做一个版本。。 版本信息。。

	1 parent ad75b54 commit 41664494fdf29df404539965c7c5efba74956c7f    版本号


分支操作介绍

一个新的版本，在主分支边上的。。。

[合并分支](http://gitbeijing.com/merge.html)

gitbeijing.com

合并分支的冲突问题。。。修改了通过一个地方。。。

	<<<<<<HEAD
	冲突标记符
	========


[团队协作流程](http://gitbeijing.com/flow/)


给一个队友添加写权限。。
在settings--collaborators---

[贡献开源项目](http://gitbeijing.com/fork_flow.html)

fork一下，

wiki知识库

[github搭建网站](http://gitbeijing.com/pages.html)

gitlab可以下载源码进行安装



#### 参考

[https://www.zhihu.com/question/20070065](https://www.zhihu.com/question/20070065)

[http://blog.csdn.net/tangbin330/article/details/9128765](http://blog.csdn.net/tangbin330/article/details/9128765)

[http://youngxhui.github.io/2016/05/03/GitHub-for-Windows%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B(%E4%B8%80)/](http://youngxhui.github.io/2016/05/03/GitHub-for-Windows%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B(%E4%B8%80)/)
