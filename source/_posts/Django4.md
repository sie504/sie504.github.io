---
title: Django 搭建小Blog
date: 2017-05-12 15:54:24
tags:
		- Django
		 
categories:
		- Web开发
---

`Django`搭建小博客。

<!-- more -->
  
创建项目 


	  django-admin startproject myblog
	
	
	
	    root@Ubuntu:~/imooc/myblog# tree
	    .
	    ├── manage.py
	    └── myblog
	    ├── __init__.py
	    ├── settings.py
	    ├── urls.py
	    └── wsgi.py


创建应用

    python manage.py startapp blog


添加应用到`settings.py`中的`INSTALLED_APPS`里面。


    root@Ubuntu:~/imooc/myblog/blog# tree

    .
    ├── admin.py
    ├── admin.pyc
    ├── __init__.py
    ├── __init__.pyc
    ├── migrations
    │   ├── __init__.py
    │   └── __init__.pyc
    ├── models.py
    ├── models.pyc
    ├── tests.py
    └── views.py



`migrations`: 数据移植`(迁移)`模块

`admin.py`: 该应用的后台管理系统配置

`apps.py` 该应用的一些配置，Django-1.9以后自动生成

`models.py` 数据模块，使用ORM框架

`tests.py` 自动化测试模块，Django提供了自动化测试功能

`views.py` 执行响应代码所在模块、代码逻辑处理的主要地点、项目中大部分代码均在这里编写


#####创建第一个页面

`/blog/views.py  `    

首先引入 

	`from django.http import HttpResponse`

`index` 函数


每个响应对应一个函数，函数必须返回一个响应

函数必须存在一个参数，一般约定为request

每一个响应对应一个URL



      1 from django.shortcuts import render
      2 from django.http import HttpResponse
      3 # Create your views here.
      4 
      5 def index(request):
      6 return HttpResponse("Hello World")
    

 在 `/myblog/urls.py` 中添加 

每个URL都以url的形式写出来

url函数放在urlpatterns列表中

url函数三个参数：URL(正则)、对应方法、名称。


`import blog.views as bv` 导入模块并命名bv



	      1 from django.conf.urls import patterns, include, url
	      2 from django.contrib import admin
	      3 
	      4 import blog.views as bv
	      5 
	      6 urlpatterns = patterns('',
	      7 # Examples:
	      8 # url(r'^$', 'myblog.views.home', name='home'),
	      9 # url(r'^blog/', include('blog.urls')),
	     10 
	     11 url(r'^admin/', include(admin.site.urls)),
	     12 url(r'^index/',bv.index),
	     13 )


 流程

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-13/96859950-file_1494667208371_7f84.png)


**第二种URL配置**


     Including another URLconf
     13 1. Import the include() function: from django.conf.urls import url, include
     14 2. Add a URL to urlpatterns:  url(r'^blog/', include('blog.urls'))
    

 
修改 /myblog/urls.py
添加 include

     16 from django.conf.urls import url,include
     17 from django.contrib import admin
     18 
     19 urlpatterns = [
     20 url(r'^admin/', admin.site.urls),
     21 url(r'^index/$',include('blog.urls'))
     22 ]


在项目blog下面创建一个urls.py 写入以下代码


      1 from django.conf.urls import url
      2 from . import views
      3 urlpatterns = [
      4 
      5 url(r'^index/$',view.index),
      6  ]


访问 `http://192.168.145.230:9000/index/index/` 才出现 Hello World.

因为在/myblog/urls.py 是 /blog/urls.py的根目录。。


如果想要一个，可以修改根目录的urls，或者修改/blog/urls.py.

修改blog/urls.py。

这样可以直接访问`http://192.168.145.230:9000/index/`，可以出现Hello World


      1 from django.conf.urls import url
      2 from . import views
      3 urlpatterns = [
      4 
      5 url(r'',views.index),
      6  ]


包含其他URL

在跟urls.py中引入include

在APP目录下创建urls.py文件，格式与跟urls.py相同

跟`urls.py`中url函数自二个参数改为


 	`include('blog.urls')`

注意事项

跟`urls.py`针对APP配置的URL名称，是该APP所有URL的总路径

配置URL时注意正则表达式结尾符号 `$和/`

#####Templates介绍

HTML文件

Django模版语言(Django Template Language DTL)

可以使用第三方模版(如Jinja2)


1. 在APP的根目录下创建名叫Templates的目录
2. 在该目录下创建HTML文件
3. 在views.py中返回render()

	`index.html`


		      1 <!DOCTYPE html>
		      2 <html>
		      3 <head>
		      4 <meta charset="UTF-8">
		      5 <title>Title</title>
		      6 </head>
		      7 <body>
		      8 <h1>Hello,Blog</h1>
		      9 </body>
		     10 </html>
    

在`views.py`中修改


      1 from django.shortcuts import render
      2 from django.http import HttpResponse
      3 # Create your views here.
      4 
      5 def index(request):
      6 return render(request,'index.html')
    

访问 `http://192.168.145.230:9000/blog/index/`，可以看到界面。

**DTL初步使用**

render()函数中支持一个dict类型参数

该字典是后台传递到模版的参数，键为参数名

	在模版中使用`{{ 参数名 }}`来直接使用

render第三个参数dict类型

      1 from django.shortcuts import render
      2 from django.http import HttpResponse
      3 # Create your views here.
      4 
      5 def index(request):
      6 return render(request,'index.html', {'hello': 'Hello, Blog!'})
   

	在前端index.html中。。使用`{{}}`获取值。`{{ hello }}`


      1 <!DOCTYPE html>
      2 <html>
      3 <head>
      4 <meta charset="UTF-8">
      5 <title>Title</title>
      6 </head>
      7 <body>
      8 <h1>{{ hello }}</h1>
      9 </body>
     10 </html>



#####Templates注意事项

新建一个应用blog2

    ├── blog
    │   ├── admin.py
    │   ├── admin.pyc
    │   ├── apps.py
    │   ├── __init__.py
    │   ├── __init__.pyc
    │   ├── migrations
    │   │   ├── __init__.py
    │   │   └── __init__.pyc
    │   ├── models.py
    │   ├── models.pyc
    │   ├── templates
    │   │   └── index.html
    │   ├── tests.py
    │   ├── urls.py
    │   ├── urls.pyc
    │   ├── views.py
    │   └── views.pyc
    ├── blog2
    │   ├── admin.py
    │   ├── admin.pyc
    │   ├── apps.py
    │   ├── __init__.py
    │   ├── __init__.pyc
    │   ├── migrations
    │   │   ├── __init__.py
    │   │   └── __init__.pyc
    │   ├── models.py
    │   ├── models.pyc
    │   ├── templates
    │   │   └── index.html
    │   ├── tests.py
    │   ├── urls.py
    │   ├── urls.pyc
    │   ├── views.py
    │   └── views.pyc
    ├── db.sqlite3


blog2/views.py

      1 from django.shortcuts import render
      2 from django.http import HttpResponse
      3 # Create your views here.
      4 
      5 def index(request):
      6 return render(request,'index.html')


blog2/urls.py

      1 from django.conf.urls import url
      2 from . import views
      3 urlpatterns = [
      4 
      5 url(r'^index/$',views.index),
      6  ]
    
blog2/templates/index.html

      1 <!DOCTYPE html>
      2 <html>
      3 <head>
      4 <meta charset="UTF-8">
      5 <title>Title</title>
      6 </head>
      7 <body>
      8 <h1>Hello, Blog2! </h1>
      9 </body>
     10 </html>



myblog/urls.py 

     urlpatterns = [
     21 url(r'^admin/', admin.site.urls),
     22 url(r'^blog/',include('blog.urls')),
     23 url(r'^blog2/',include('blog2.urls')),
     24 ]


访问 

	`http://192.168.145.230:9000/blog2/index/` `http://192.168.145.230:9000/blog/index/` 

出现的结果都是一样的，为什么blog2的页面没有出现？


Django按照INSTALLED_APPS 中添加顺序查找Templates，如果不同应用下的模版相同，就会出现冲突。解决办法，把每个应用下的模版都用不同的名字，太麻烦。

解决Templates冲突方案：

在APP的TEmplates目录下创建以APP名为为名称的目录

将html文件放入新创建的目录下。同时修改views.py 中的路径。


    root@kali:~/django/myblog/blog2/templates# tree
    .
    └── blog2
    └── index.html
    
    1 directory, 1 file



同时修改 views.py中的render参数的路径

    def index(request):
      6 return render(request,'blog2/index.html')


#####Models

通常，一个Model对应数据库的一张数据表

Django中Models以类的形式表现

它包含了一些基本字段以及数据的一些行为


ORM 对象关系映射(Object Relation Mapping)

实现了对象和数据库之间的映射

隐藏了数据访问的细节，不需要编写SQL语句


	  1 from __future__ import unicode_literals
	  2 
	  3 from django.db import models
	  4 
	  5 # Create your models here.
	  6 
	  7 class Article(models.Model):
	  8     title = models.CharField(max_length=32,default='Title')
	 9     content = models.TextField(null=True)
	

**生成数据表**

命令行进入manage.py同级目录

执行python manage.py makemigrations app(可选)    数据迁移

执行 python manage.py migrate 迁移动作


    root@kali:~/django/myblog# python manage.py makemigrations
    Migrations for 'blog':
      blog/migrations/0001_initial.py:
    - Create model Article
    root@kali:~/django/myblog# python manage.py migrate
    Operations to perform:
      Apply all migrations: admin, auth, blog, contenttypes, sessions
    Running migrations:
      Applying contenttypes.0001_initial... OK
      Applying auth.0001_initial... OK
      Applying admin.0001_initial... OK
      Applying admin.0002_logentry_remove_auto_add... OK
      Applying contenttypes.0002_remove_content_type_name... OK
      Applying auth.0002_alter_permission_name_max_length... OK
      Applying auth.0003_alter_user_email_max_length... OK
      Applying auth.0004_alter_user_username_opts... OK
      Applying auth.0005_alter_user_last_login_null... OK
      Applying auth.0006_require_contenttypes_0002... OK
      Applying auth.0007_alter_validators_add_error_messages... OK
      Applying auth.0008_alter_user_username_max_length... OK
      Applying blog.0001_initial... OK
      Applying sessions.0001_initial... OK



会自动给添加主键。。。


页面呈现数据

后台步骤

views.py中    

	import models
	
	article = models.Article.objects.get(pk=1)
	
	render(request,page, {'article':article})

      1 from django.shortcuts import render
      2 from django.http import HttpResponse
      3 from . import models
      4 # Create your views here.
      5 
      6 def index(request):
      7 	article = models.Article.objects.get(pk=1)
      8 	return render(request,'blog/index.html',{'article':article})





前端操作

模版可直接使用对象以及对象的. 操作


		{{ article.title }}

      1 <!DOCTYPE html>
      2 <html>
      3 <head>
      4 <meta charset="UTF-8">
      5 <title>Title</title>
      6 </head>
      7 <body>
      8 <h1>{{ article.title }}</h1>
      9 <h3> {{ article.content }}</h3>
     10 </body>
     11 </html>

    
访问可以看到内容

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-16/3631910-file_1494930833165_9494.png)


在数据库中，添加的内容。

    root@kali:~/django/myblog# python manage.py shell
    Python 2.7.9 (default, Mar  1 2015, 12:57:24) 
    [GCC 4.9.2] on linux2
    Type "help", "copyright", "credits" or "license" for more information.
    (InteractiveConsole)
    >>> from blog.models import Article
    >>> Article.objects.create(title='Hello World',content='Come on')
    

#####Admin

admin/admin888


    root@kali:~/django/myblog# python manage.py createsuperuser
    Username (leave blank to use 'root'): admin
    Email address: 12344@sina.com
    Password: 
    Password (again): 
    The password is too similar to the email address.
    This password is too short. It must contain at least 8 characters.
    This password is too common.
    This password is entirely numeric.
    Password: 
    Password (again): 
    Superuser created successfully.
    

设置settings.py的LANGUAGE_CODE 为 zh-Hans可以后台为中文。。

**细心 zh-Hans 而不是 zh_Hans** 

配置应用

在应用下项目blog/admin.py中引用自身的models模块

编辑

	  admin.py: admin.site.register(models.Article)

      1 from django.contrib import admin
      2 from models import Article
      3 # Register your models here.
      4 
      5 admin.site.register(Article)


数据库中的内容显示在了后台

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-16/79155408-file_1494931997616_941f.png)


看下图不方便

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-16/52070534-file_1494932210980_4e48.png)

修改数据默认显示名称

在Article类下添加一个方法

根据Python版本选择

	 __str__(self) 或__unicode__(self)

return self.title

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-16/62259153-file_1494932434730_c3d8.png)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-16/1822432-file_1494932462004_1f77.png)


####博客页面设计
#####页面概要
######博客主页面
主页面内容：文章标题列表，超链接。发表博客按钮（超链接）

**列表编写思路**

1. 取出数据库中所有文章对象 

2. 将文章对象打包成列表，传递到前端

3. 前端页面把文章以标题超链接的形式逐个列出

模版For循环


	    {% for xx in xxs %}
	    
	    HTML语句
	    
	    {% endfor %}


######博客文章内容页面
######博客撰写页面


后台代码

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-16/62791115-file_1494933636107_6b74.png)

前端代码

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-16/83058915-file_1494934176067_d524.png)

访问，可以看到相应的博客文章已经显示出来。。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-16/36144841-file_1494934205777_152c4.png)


文章显示出来了，但是没有链接，点击其中要看到全部的文章。。

超链接的目的地。。

#####博客文章页面
######页面内容

**标题
文章内容
修改文章按钮（超链接）**

**URL参数的传递**
参数写在响应函数中的request后，可以有默认值

URL正则表达式：

	 r'^/article/(?P<article_id>[0-9]+)/$'

URL正则中的组名必须和参数名一致

首先视图函数

/blog/views.py

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-16/74592015-file_1494935556807_17e0d.png)


然后模版 article_page.html

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-16/61288311-file_1494935655252_8bf1.png)

最后 urls.py设置

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-16/97038282-file_1494935734788_6d18.png)

现在可以看到每一个文章的具体标题以及内容了，但是必须知道具体位置。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-16/74062942-file_1494935838895_15dd6.png)



#####博客中超链接设置

比如新文章 new blog到编辑页面的链接

文章到article_page页面的链接。

超链接如何编写。


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-16/36144841-file_1494934205777_152c4.png)


**Django中超链接**

超链接目标地址

href后面是目标地址

template中可以用

	 "{% url 'app_name:url_name' param %}"

其中app_name和url_name都在url中配置。。

**url函数的名称参数**

跟urls，写在include()的第二个参数位置，`namespace = 'blog'`

应用下则写在url()的第三个参数位置，`name = 'article'`

主要取决于是否使用include引用了另一个url配置文件。。我们的这种情况使用了include引用了另一个url配置文件。。


/myblog/urls.py 

第三个参数 namespace='blog'


     url(r'^blog/',include('blog.urls',namespace='blog')),


在项目 /blog/urls.py中第三个参数 name

 	`url(r'^article/(?P<article_id>[0-9]+)$',views.article_page,name='article_page'),`


	修改index.html中的`<a>`标签里的，`href`


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-16/84321378-file_1494938617461_122ea.png)

比如我们点击，名为 "第一篇文章"的标题就会直接跳转到具体的文章页面


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-16/36144841-file_1494934205777_152c4.png)



#####博客编辑页面
######页面内容
**标题编辑栏，文章内容编辑区域、提交按钮**

先是templates创建edit_page.html

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-17/95752806-file_1495019124002_7d00.png)

然后views.py视图函数

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-17/43933525-file_1495019200917_a510.png)

最后urls.py

    url(r'^edit/$',views.edit_page),

访问页面

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-17/6459609-file_1495019309637_15cd4.png)

表单响应函数

编辑响应函数

	使用request.POST['参数名'] 获取表单数据

	models.Article.objects.create(title,content) 创建对象


views.py 修改文章的函数，返回的页面为博客的主页。。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-17/25266863-file_1495020456772_fcf.png)

urls.py的设置

     url(r'^edit/action$',views.edit_action,name='edit_action'),
    

	edit_page.html 其中的`{% csrf_token %}` 防止csrf攻击，在每个表单中都要加入。


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-17/68180731-file_1495020649433_d6d5.png)


可以看到新添加的文章

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-17/96829333-file_1495020669057_13288.png)

新文章的链接

点击主页的新文章链接，可以直接跳转到

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-17/36506424-file_1495021590840_cc53.png)

index.html

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-17/20644843-file_1495021525002_16ae0.png)

#####修改文章链接

思路两个编辑页面：新文章为空。。。 修改文章有内容，且修改之后返回自己的页面。。

修改文章页面有文章对象，文章的ID，传递文章ID，后台代码根据ID进行修改。。

新建文章的ID设置为0,文章的ID是从1开始的，修改的时候判断一下，修改文章的时候，判断ID为0.



先修改响应函数 views.py

edit_page函数，加入了判断 article_id，并判断。。是否等于 0

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-18/98103695-file_1495094336534_4a05.png)


然后修改 urls.py 中的edit

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-18/9637440-file_1495094513530_71e2.png)

修改前端页面，article_page.html..修改文章的超链接。点击该链接来挑战到修改文章的页面，并且该页面还要显示该文章的具体内容。。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-18/99531679-file_1495095001466_2ac3.png)

修改index.html 中的 添加一个0， 新文章显示的为0

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-18/38122510-file_1495095090864_483e.png)

在主页点击新文章直接跳转到 /edit/0

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-18/18181117-file_1495095201551_8ce6.png)


现在主页面的可以添加新文章了，但是在文章页面如何来修改文章。。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-18/6702209-file_1495095434948_b441.png)

需要在edit_page.html中，添加一个判断，根据什么判断呢，就是根据article_id是不是0,0就是添加新文章，不是0就是修改已有的文章。。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-18/43213503-file_1495095912746_8f83.png)


	把article_id放在一个隐藏的`<input>`标签里。。


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-18/66828190-file_1495096074709_6487.png)


修改views.py中的edit_action

判断article_id 等于0，就是创建一个新的对象。不是0就是修改文章。。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-18/25290306-file_1495096242626_8cd2.png)


在文章页面点击修改文章时候，就直接可以跳转到如下界面。。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-18/80400500-file_1495096426202_1266b.png)

#####Templates过滤器

	模版中 `{{ value|filter }}`

修改之前的edit_page.html  去除了 else判断。。添加红色的内容。。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-18/89432856-file_1495097103299_a5c9.png)


#####Admin

创建admin配置类

    class ArticleAdmin(admin.ModelAdmin)

注册 admin.site.register(ArticleAdmin)

在后台界面，显示其他字段

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-18/94289424-file_1495098079620_c019.png)

在后台中，其他的字段可以显示了。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-18/59438614-file_1495098120284_d581.png)

修改models.py

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-18/51946545-file_1495098337792_10110.png)

数据移植

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-18/29016641-file_1495098426792_2e40.png)

还有要在admin.py中加入显示时间字段。。。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-18/90453396-file_1495098589677_98a2.png)

后台admin的过滤器、
admin.py中添加

    list_filter = ('pub_time',)

在后台中，可以选择时间。。。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-18/72668470-file_1495099023973_182c6.png)


