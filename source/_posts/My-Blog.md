---
title: Github+hexo
date: 2018-01-09 18:47:07
tags:
		- hexo

categories:
		- 技术研究
---

2017年2月份在`bitcron`平台上开了一个技术博客，付费注册了一个账户，然后在10月份注册了一个域名，开始写一些技术博客。新的一年了，尝试在`Github`上再建一个`Blog`玩玩。

<!-- more -->


##### Github + hexo

- . 安装 `Git Bash`
2. 安装 Node JS
3. 安装 hexo  在`gitbash`中用`npm`工具安装就可以的。
	`npm install hexo-cli -g` 。 `hexo -v` 版本
4. 初始化 创建博客目录  

	`hexo init sie504.github.io` 
	`cd sie504.github.io` 。
	`npm install`
6.  生成静态页面

	`hexo clean`。 `hexo g`

6. 运行

	`hexo s -p 4001`

7. 发布文章

	`hexo new test`

此时会在`source/_posts`目录下生成`test.md`文件。生成一下，查看效果。

	`hexo clean`。 `hexo g` 。 `hexo s`


- 上传到`github`

	先安装 `npm install hexo-deployer-git --save`。(这样才可以将自己的文章部署到github)

- 网站配置`git`

在网站的`_config.yml`中配置deploy。

	deploy:
		type: git
		repo: <reposity url>
		brranch: [branch]	

- 执行命令（建议每次都要按照以下步骤部署）

hexo clean   #清除缓存，网页正常情况下可以忽略此条命令
hexo g生成静态网页，然后输入hexo s可以本地预览效果，最后输入hexo d上传到github上

  `hexo clean` 。 `hexo g` 。 `hexo d`

```
node_modules：是依赖包
public：存放的是生成的页面
scaffolds：命令生成文章等的模板
source：用命令创建的各种文章
themes：主题
_config.yml：整个博客的配置
db.json：source解析所得到的
package.json：项目所需模块项目的配置信息
```



##### 更换主题

将相应的主题下载到`themes`目录下面。并修改`_config.yml`里面的主题对应内容。

选择了一个主题之后，可以对这个主题进行再次设置，喜欢什么样的，就设置什么样的。

**主题配置文件**，搜索`scheme`。

	# Schemes
	#scheme: Muse
	#scheme: Mist
	scheme: Pisces
	#scheme: Gemini


**设置阅读全文**

	在首页显示一篇文章的部分内容，并提供一个链接跳转到全文页面是一个常见的需求。 NexT 提供三种方式来控制文章在首页的显示方式。 也就是说，在首页显示文章的摘录并显示 阅读全文 按钮，可以通过以下方法：
	
	在文章中使用 <!-- more --> 手动进行截断，Hexo 提供的方式 推荐
	在文章的 front-matter 中添加 description，并提供文章摘录
	自动形成摘要，在 主题配置文件 中添加：


##### 出现的问题


hexo g

	生成静态文件的时候，在写的md里面不能含有程序使用的标签。< > { } ] <div> <url>等。

如果使用的话，在代码块中使用。

hexo d

	FATAL Something's wrong. Maybe you can find the solution here: http://hexo.io/do
	cs/troubleshooting.html
	Error: spawn git ENOENT


hexo d

	Error: fatal: Not a git repository (or any of the parent directories): .git
	
	at ChildProcess.<anonymous> (E:\sie504\sie504.github.io\node_modules\hexo-ut
	il\lib\spawn.js:37:17)

在`git`终端执行命令。先删除`.deploy_git`

又遇到了错误。

	Error: fatal: The remote end hung up unexpectedly
	fatal: The remote end hung up unexpectedly
	error: RPC failed; curl 56 SSL read: error:00000000:lib(0):func(0):reason(0), er
	rno 10053
	Everything up-to-date

解决方法

	git config http.postBuffer 524288000



`Github pages`是`github`提供给用户来展示个人或者项目主页的静态网页系统。每个用户可以使用自己的`github`项目创建，上传静态页面的`html`文件，`github`会帮你自动更新你的页面。

`hexo`是一个用来生成静态界面的框架，使用`hexo`可以直接用`markdown`写文章。

`md--hexo--html--` 

问题3

	Error: Cannot find module 'cheerio'
	at Function.Module._resolveFilename (module.js:337:15)
	at Function.Module._load (module.js:287:25)

##### 标签和分类

标签是对一篇文章的记号，使用tag标识，分类是将其归到一个大类中，这个类中会有很多的性质相似的内容。

标签，方便搜索。

* 确认站点配置文件有

	tag_dir: tags

* 确认主题配置文件有

	tags: tags

分类，给文章归档。

* 确认站点配置文件有

	category_dir: categories

* 确认主题配置文件有

	categories: /categories


站点配置文件有

	archive_dir: archives
	category_dir: categories

主题配置文件

	tags: /tags/ || tags
  	categories: /categories/ || th

* 如何使用标签和分类

新建tags页面

	hexo new page tags

此时会在`source/`下生成`tag/index.md`文件

修改`source/tag/index.md`

	---
	title: tags
	date: 2018-01-05 16:20:01
	type: "tags"
	comments: false
	---

在文章中添加tags:

	---
	title: Struts2 漏洞简单汇总
	date: 2018-01-09 18:13:23
	tags:
			- 漏洞学习(标签1)
			- 标签2
	---

同样分类和标签做的步骤一样。

	hexo new page categories

修改 `source/categories/index.md`

	---
	title: categories
	date: 2018-01-05 16:24:24
	type: "categories"
	comments: false
	---

使用分类。



##### 多PC同步管理博客

纠结这个问题。把博客本地的内容记得备份就可以了。


##### 绑定域名

可以在[namesilo](https://www.namesilo.com/index.php)注册一个域名，使用国内的`DNSpod`解析服务器，然后在`namesilo`的后台修改`DNS`配置就行了，将其修改为`DNSpod`的，然后将其中的`A`和`www`记录修改为自己`Gihub Page`的ip地址。如何得到呢？`ping sie504.github.io`，得到的就是。最后在自己的初始化博客的文件夹里，新建一个`CNAME`文件，里面填写自己的域名，同步一下就可以用自己的域名访问在`Github`搭建的博客了。

###### 参考

[Hexo搭建博客教程](https://thief.one/2017/03/03/Hexo%E6%90%AD%E5%BB%BA%E5%8D%9A%E5%AE%A2%E6%95%99%E7%A8%8B/)

[Hexo 博客搭建指南](https://github.com/limedroid/HexoLearning)

[hexo从零开始到搭建完整](http://www.cnblogs.com/visugar/p/6821777.html)

[写作](https://hexo.io/zh-cn/docs/writing.html)

[配置](https://hexo.io/zh-cn/docs/configuration.html)

[GitHub+Hexo 搭建个人网站详细教程](https://zhuanlan.zhihu.com/p/26625249)

[主题](https://hexo.io/themes/)
