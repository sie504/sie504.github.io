---
title: Django+xadmin Web 项目学习
date: 2017-05-18 15:54:24
tags:
		- Django
		 
categories:
		- Web开发
---

`Django+xadmin`的学习。


<!-- more -->



2017/05/18


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-6-14/4808993.jpg)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-6-14/48043457.jpg)

环境搭建

    pycharm、navicat、mysql、python2.7
 

virtualenv

使不同应用开发环境独立

环境升级不影响其他应用。也不会影响全局的python环境

可以防止系统中出现包管理混乱和版本冲突

使用pip命令

    C:\Users\Administrator>pip install virtualenv
    Collecting virtualenv
    Using cached virtualenv-15.1.0-py2.py3-none-any.whl
    Installing collected packages: virtualenv
    Successfully installed virtualenv-15.1.0



创建virtualenv

    C:\Users\Administrator>virtualenv testvir
    New python executable in C:\Users\Administrator\testvir\Scripts\python.exe
    Installing setuptools, pip, wheel...done.


    C:\Users\Administrator\testvir\Scripts>activate.bat
    
    (testvir) C:\Users\Administrator\testvir\Scripts>


    deactivate.bat 退出虚拟环境


安装另外一个软件

    pip install virtualenvwrapper-win


直接安装虚拟环境

    C:\Users\Administrator\testvir\Scripts>mkvirtualenv testvir2
    New python executable in C:\Users\Administrator\Envs\testvir2\Scripts\python.exe
    
    Installing setuptools, pip, wheel...done.
    
    (testvir2) C:\Users\Administrator\testvir\Scripts>



在虚拟环境安装

    C:\Users\Administrator\testvir\Scripts>workon testvir2
    (testvir2) C:\Users\Administrator\testvir\Scripts>cd ..
    
    (testvir2) C:\Users\Administrator\testvir>cd ..
    
    (testvir2) C:\Users\Administrator>pip list
    DEPRECATION: The default format will switch to columns in the future. You can u
    e --format=(legacy|columns) (or define a format=(legacy|columns) in your pip.co
    f under the [list] section) to disable this warning.
    appdirs (1.4.3)
    packaging (16.8)
    pip (9.0.1)
    pyparsing (2.2.0)
    setuptools (35.0.2)
    six (1.10.0)
    wheel (0.29.0)
    
    (testvir2) C:\Users\Administrator>pip install requests
    Collecting requests


开发基于以上的虚拟环境使用。。。


Pycharm使用

新建项目 create new project

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-18/98223542-file_1495111157811_10a14.png)

查看创建的环境是否成功

File--setting--，然后如下图

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-18/42083593-file_1495111472688_14a8c.png)

点击Run--Run运行项目，下面显示成功，访问也可以。

设置Ecipse的快捷键

File--Setting--输入 keymap--Eclipse, 点击OK

比如Ecipse的快捷键 Ctrl+h  全局搜索

可以在Run--Edit设置

目录文件设置 Mark Directoyry As  Sources Root 设置文件路径。这个设置不当会在编辑器运行的时候报错，找不到路径。。


Navicat 可以从其他数据库的表复制到另一个


#####新建项目

Tools--Run manage.py as Task

新建项目 startapp message

新建static目录。。存放静态文件。。  js/css image

新建log目录，存放日志文件

新建用户上传文件的目录 media

新建apps目录，存放所有的项目，因为app多了会乱。。。

把新建的message项目拉到apps里面。。

Settings-->Editor-->Colors & Fonts-->Font
然后在size那里调整。

在manage.py中导入apps，如果下面的编写，需要把message设置为Root路径，但是在cmd下执行的时候，会报错。

    from message import views


    (testvir2) C:\Users\Administrator\PycharmProjects\djangostart>python manage.py r
    unserver
    Traceback (most recent call last):
      File "manage.py", line 5, in <module>
    from message import views
    ImportError: No module named message


现在需要在settings.py中把项目路径apps设置为搜索路径。就不会报错。。

Shift+Tab 向前缩进


在static里面创建css文件，然后在css里面创建style.css保存样式。。


配置settings.py连接数据库


    DATABASES = {
    'default': {
    'ENGINE': 'django.db.backends.mysql',
    'NAME': "testdjango",
    'USER':"root",
    'PASSWORD':"",
    'HOST':"127.0.0.1"
    }
    }
    

windows下安装mysql-python有错误。。。

blog.csdn.net/u012882134/article/details/51934165

坑

setting.py 中 static 样式的路径
    
    STATIC_URL = '/static/'
    STATICFILES_DIRS = [
    os.path.join(BASE_DIR,'static')
    ]



![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-19/91022660-file_1495183644258_10440.png)


配置settings.py设置数据库，编写views.py,urls.py以及HTML与css的分离，地址的配置。。


#####Models

ORM:

    def book_list(request):
    	db = MySQl.db.connect(user='root',db='mydb',password='root',host='localhost')
    	cussor = db.cursor()
    	cursor.execute('select name from books order by name')
    	names = [row[0] for row in cursor.fetchall()]
    	db.close()



在views.py中，加.代表models和views同一目录

    from .models import UserMessage


之前在写入数据库中时候出现了一些配置错误，现在可以了。

写入数据

    def getform(request):
    # all_message = UserMessage.objects.filter()
    # for message in all_message:
    # print message.name
    user_meassage = UserMessage()
    user_meassage.name = "booby"
    user_meassage.message = "hello"
    user_meassage.address = "上海"
    user_meassage.email = "201@sina.com"
    user_meassage.object_id = "hello"
    user_meassage.save()
    
#####学习了如何存入数据，删除数据

     all_message = UserMessage.objects.filter(name='mlb',address='北京')
    #all_message.delete()
    for message in all_message:
    message.delete()
       # print message.name
    # if request.method == "POST":
    #     name = request.POST.get('name','')
    #     message = request.POST.get('message', '')
    #     address = request.POST.get('address', '')
    #     email = request.POST.get('email', '')
    #     user_meassage = UserMessage()
    #     user_meassage.name = name
    #     user_meassage.message = message
    #     user_meassage.address = address
    #     user_meassage.email = email
    #     user_meassage.object_id = "hello123"
    #     user_meassage.save()

    return render(request,'message_form.html')

取出数据

#####提取数据到页面

在Template中展示后台数据。


	<input value={{ }}>
		
		
	<input id="address" type="text" value="{{ my_message.address }}" name="address" placeholder="请输入联系地址"/>
		
	message = None
	all_messages = UserMessage.objects.filter(name='booby')
	if all_messages:
		message = all_messages[0]
		
	return render(request,'message_form.html',{"my_message":message})



#####Template if 用法


    <input id="name" type="text" value="{% if my_message.name == 'booby'%}boobytest{% endif %}" 

如果name为booby，就会变为 boobytest


	value="{% if not my_message.name == 'booby'%}boobytest{% else %} booby not test{% endif %}"

#####URL配置技巧

    url(r'^form/$'.getform,name='go_form')

html中和urls配置中，name的设置。。name、go_form。。。。urls路由，给每一个都要起一个名字。。。在templates中容易引用。。。

    <form action="{% url 'go_form' %}" method="post" class="smart-green">
    
#####问题



    Error fetching command 'collectstatic': You're using the staticfiles app without having set the STATIC_ROOT setting to a filesystem path.
    Command 'collectstatic' skipped
    

解决


    STATIC_ROOT = 'C:\Users\Administrator\PycharmProjects\django1\static'


#####快捷键

多行注释   Ctrl+/

F6单步断点


-------------------------------------------------------------
#####项目开发

django app设计

各app models设计

数据表生成与修改

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-22/11273807-file_1495430152109_12384.png)

配置虚拟环境，安装django

    pip install virtualenvwrapper-win
    mkvirtualenv mxonline
    pip install django==1.9


在PyCharm中新建项目，路径选好


    C:\Users\Administrator\Envs\mxonline\Scripts

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-22/89334206-file_1495431631108_108e0.png)

在开始Models和apps之前，先安装数据库驱动。。。

    pip install mysql-python

安装会出错，已经下载解决方案。。


    (mxonline) E:\Download>pip install MySQL_python-1.2.5-cp27-none-win_amd64.whl


配置settings.py 的数据库

    DATABASES = {
    'default': {
    'ENGINE': 'django.db.backends.mysql',
    'NAME': 'mxonline',
    'USER': 'root',
    'PASSWORD':'',
    'HOST':"127.0.0.1"
    }
    }


在Naivate 新建数据库 mxonline

在Pycharm生成默认数据表

makemigration、migrate

运行时，，Debug可以随时打断点。。


新建app

    startapp users


第一步是修改Models.py

user 表的内容

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-22/18537651-file_1495434427308_d6b8.png)

可以继承默认的表的字段

    from django.contrib.auth.models import AbstractUser

需要新添加的字段，昵称，生日，性别、区域、手机、邮箱。。

利用image的时候，需要安装pillow
    
    >pip install pillow



makemigrations的时候，，遇到错误。。。

    Error fetching command 'squashmigrations': expected an indented block (loader.py, line 160)
    Command 'squashmigrations' skipped
    Error fetching command 'sqlmigrate': expected an indented block (loader.py, line 160)
    Command 'sqlmigrate' skipped
    Error fetching command 'makemigrations': expected an indented block (loader.py, line 160)
    Command 'makemigrations' skipped
    Error fetching command 'runserver': expected an indented block (loader.py, line 160)
    Command 'runserver' skipped


因为你在一个文件中，无意中插入了一个字母m，导致该文件报错。。


扩展django默认的表

    class UserProfile(AbstractUser):
	    nick_name = models.CharField(max_length=50,verbose_name=u"昵称",default="")
	    birday = models.DateField(verbose_name=u"生日",null=True,blank=True)
	    gender = models.CharField(max_length=5,choices=(("male",u"男"),("female","女")),default="female")
	    address = models.CharField(max_length=100,default=u"")
	    mobile = models.CharField(max_length=11,null=True,blank=True)
	    image = models.ImageField(upload_to="image/%Y/%m",default=u"image/default.png",max_length=100)
	    
	    class Meta:
	    verbose_name = "用户信息"
	    verbose_name_plural = verbose_name
	    
	    def __unicode__(self):
	    return self.username


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-22/90190901-file_1495437366883_4b40.png)



![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-22/12381592-file_1495437803230_3d31.png)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-22/22095926-file_1495437907404_11164.png)

app设计

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-22/9062405-file_1495437962843_1a7.png)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-22/66698282-file_1495438052201_1458a.png)

##### 新建另外一个app courses 课程

课程和章节，一对多

章节下可能有多个视频。。

课程下有多个章节，一个章节下可能有多个视频。。

课程下面有资源下载，一对多。。。

四张表


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-22/10021600-file_1495440358444_1306a.png)


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-22/54859120-file_1495440372515_7aab.png)

数据库设计，一对一，一对多。。。

##### organization mode

CourseOrg -- 课程机构基本信息

Teacher --教师基本信息

CityDict 城市信息

##### operation

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-22/58193164-file_1495451652924_14442.png)


建立python package，把几个app拉进去。。同时把apps设置为Source root

在settings.py下面设置 apps路径


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-22/97477669-file_1495454935346_709c.png)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-22/40104656-file_1495454995066_b8fe.png)


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-22/22160738-file_1495455027303_11b4.png)


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-22/91575917-file_1495455111375_175b8.png)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-22/98756619-file_1495455147154_bbb.png)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-22/97600057-file_1495455186946_18596.png)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-22/71999268-file_1495455231754_fcd0.png)

#####后台管理系统

权限管理、少前端样式、快速开发、

django admin也是一个app。。

    manage.py@MxOnline > createsuperuser

用户名和密码登录。。。

设置中文，setting.py...

    LANGUAGE_CODE = 'zh-hans'
    
    TIME_ZONE = 'Asia/Shanghai'
    
    USE_TZ = False


app，users中的admin.py注册到后台。。。


    from django.contrib import admin
    
    # Register your models here.
    
    from .models import  UserProfile
    
    class UserProfileAdmin(admin.ModelAdmin):
    	pass
    
    admin.site.register(UserProfile,UserProfileAdmin)


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-23/86413304-file_1495517951876_81a4.png)



**问题**：通过以上就可以把自己写的用户信息的数据库注册到后台里面，在后台可以看到和设计数据库的字段一样的。。。

在使用Django Admin后台添加用户时出现报错，在后台添加新的用户，报错。。这个问题你在网上找到了答案，可以测试的时候，没有测试成功。。细心和耐心不够。。。

    IntegrityError at /admin/users/userprofile/add/
    (1452, 'Cannot add or update a child row: a foreign key constraint fails (`mxonline`.`django_admin_log`, CONSTRAINT `django_admin_log_user_id_c564eba6_fk_auth_user_id` FOREIGN KEY (`user_id`) REFERENCES `auth_user` (`id`))')
    

一个bug，在某些django版本中，使用自定义User后，在创建用户的时候，数据库中的django_admin_log不能正确的写入，原因是，这个表中有一个外键约束，针对的是auth_user表，但所由于自定义了User，使用的是自定义用户，则外键约束失败，解决方法是，删除外键约束，然后重新添加一个针对自定义User表id的约束即可

  `CONSTRAINT django_admin_log_user_id_c564eba6_fk_auth_user_id FOREIGN KEY (user_id) REFERENCES users_userprofile (id)`


在settings.py的DATABASES板块中添加以下代码取消外键检查。。


     'OPTIONS':{'init_command':'SET foreign_key_checks=0'}


##### xadmin

    pip install xadmin

在settings.py中添加apps

	'xadmin',
    'django-crispy-forms'

改变urls.py

pip uninstall xadmin
    
**源码安装**

安装源码，将xadmin拷贝进去，并新建目录extra_apps<将xadmin放进去，设置Source root..

#####users app的model注册到xadmin后台


新建py模版，settings--template--File and COde Template--Python 

__date__ - '$DATE $TIME'

新建一个adminx.py把邮箱验证码添加进去。。。

    import xadmin
    
    from .models import  EmailVerifyRecord
    
    class EmailVerifyRecordAdmin(object):
    	pass
    
    xadmin.site.register(EmailVerifyRecord,EmailVerifyRecordAdmin)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-23/21831040-file_1495530591065_13b71.png)

没有log表，因为新的xadmin的数据表没有迁移。。

ImportError: No module named xadmin

路径设置一下，，

    sys.path.insert(0,os.path.join(BASE_DIR,'extra_apps'))



adminx.py中的字段

list_display列表显示、search_fields 搜索、、list_filter筛选字段、

在每个app里面新建adminx.py,然后在里面把models注册到后台里面。
其中的字段就是models中定义的字段。。。

apps：users、courses、operation、organization。

把后台数据表注册到后台之后，可以看到相应的内容。。。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-24/85572886.jpg)

##### 后台全局配置

比如左上角以及最下面的名字修改。。。把app的名字改为中文，，，

users/adminx.py 设置修改，添加代码。。。。 主题代码

	from xadmin import views

    class BaseSetting(object):
    	enable_themes = True
    	use_bootswatch = True
    
    xadmin.site.register(views.BaseAdminView,BaseSetting)


左上角

    class GlobalSettings(object):
    	site_title = "504后台管理系统"
    	site_footer = "504Comeon"
    
    xadmin.site.register(views.CommAdminView,GlobalSettings)

左侧导航栏。。

    menu_style = "accordion"

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-24/94334736.jpg)

app下apps.py中修改中文名字。。

    verbose_name = u"用户操作"

__init__.py 添加引用。。

	default_app_config= "operation.apps.OperationConfig"

可以看到，后台的名字为中文。。。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-24/96914339.jpg)

主题的显示，在用户里面注册了一个主题。。

#### 前台业务逻辑

#### 用户登录

首先前端的index.html放在template目录下面。。

新建static文件，将静态文件拷贝进来。。

配置urls.py

     url('^$',TemplateView.as_view(template_name="index.html"),name="index")
    
前端的css找不到，需要在settings.py配置路径。。。


    url('^$',TemplateView.as_view(template_name="index.html"),name="index")  # template_name 指向静态文件

问题，静态文件加载不了，500错误。元组，如果只有一个元素，一定加逗号。。

    STATICFILES_DIRS  = (
    os.path.join(BASE_DIR,"static"),
    )

##### 配置登录界面
urls.py


    url('login/^$',TemplateView.as_view(template_name="index.html"),name="login")

在index.html前端，修改登录 中的href 为login



##### 登录进去
##### 用户登录的后台逻辑
users/views.py

    def login(request):
    	if request.method == "POST":
    		pass
    	elif request.method == "GET":
    		return  render(request,"login.html", {})#{}字典那些变量需要传递也前端，空的时候，不做处理。
    
    

urls.py

	url('^login/$', login, name="login")


打一个断点，看请求的变量。指向Url的地址。。。
当我们从主页面点击登录的时候，由于是get请求 ，所以会跳转到登录login.html页面。。


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-24/95119231.jpg)

后台登录之前，login.html的input表单。。。

action中的地址。/login/，表单中的 username以及password会传递到后台。。。

但是点击击立刻登录的时候，已经输入用户名以及密码的时候，是post请求，可以看到断点。。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-24/84156983.jpg)

可以看到token的保护。。。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-24/56157267.jpg)


数据发过来了，我们要取这个数据。。。然后验证一下数据是否合法。。

    from django.contrib.auth import authenticate  认证、、



当认证成功后，返回首页或者个人中心，这里没有个人中心，返回首页，需要修改首页的index.html源码。。。

inedx.html有两个div。。

一个登录和注册，一个判断用户登录之后显示。。可以看到现在这两个是同时存在的。。。
在返回html之前需要进行判断，显示那一段，，，html中不允许写入大量的逻辑。。。允许某个对象调用其中的方法。。。




![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-24/96818477.jpg)


学会看错误，然后便于修改。。。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-24/51297554.jpg)



[http://alanhou.org/django/](http://alanhou.org/django/)
    
应该是数据库配置的问题，字段的设置错误。。。

[https://stackoverflow.com/questions/1253459/mysql-error-1452-cannot-add-or-update-a-child-row-a-foreign-key-constraint-fa](https://stackoverflow.com/questions/1253459/mysql-error-1452-cannot-add-or-update-a-child-row-a-foreign-key-constraint-fa)

[http://www.jianshu.com/p/8769329eed4d](http://www.jianshu.com/p/8769329eed4d)

##### 用户邮箱登录
自定义后台认证方法。。。
在setting.py  田间变量认证。。。

    AUTHENTICATION_BACKENDS = (
    'users.views.CustomBackend',
    )


    class CustomBackend(ModelBackend):  #继承backend类
    def authenticate(self, username=None, password=None, **kwargs):
    try:
    user = UserProfile.objects.get(username,username) #只查询 username
    if user.check_password(password):   #明文加密，user password作对比
    return user
    except Exception as e:
    return None
    
    
重定义登录方式，用户名以及邮箱来进行登录。。。

views.py


    return render(request,"login.html",{"msg":"用户名或者密码错误"}) 


login.html

	<div class="error btns login-form-tips" id="jsLoginTips">{{ msg }}</div>

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-25/93526408.jpg)




6-1，2，3问题

index.html中.今天一下午对前台业务逻辑，怎么实现的，没有处理好，费了很多的时间来处理。比如元组的一个元素要加单引号，其他的一些后台和前台的结合的配置。。


里面的`if`忘记写了，基础不牢固，把`templates`里面的python逻辑学精。。。。

一个是inedx.html中\{if 的判断\}，当用户输入是真的时候返回那个页面，假的时候如何选择，其实这是基本的知识，但是由于自己不懂，才花费很多的时间来处理。。。。基础知识花时间多去学习。。。。

6.3 主要是进行用户名和密码进行登录验证，可以登录进来，如有错误，会提示用户名或者密码错误。。。

6.4 完善用户登录

之前的配置是基于函数，这次要基于类，继承。。基于类来login。。

在users下面创建forms.py 表单文件。。。

6.5 登录的逻辑，通过form进行验证。。并在界面显示这个字段错误，或者用户名密码错误。。。

完善login的用户提示。。。以下验证信息是form完成的。。。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-25/47735557.jpg)

forms.py

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-25/83683344.jpg)


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-25/74354467.jpg)


**6.6** Session和cookie

Session和cookie

cookie浏览器本地存储方式，，

	{key,value}

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-25/34436735.jpg)


服务器把id分配给用户，id=1，用户请求的时候，cookie中的id=1

带上cookie就是有状态的请求。。

cookie本地存储，把服务器给我们的信息保存到本地，请求的时候再给服务器。。。这样完成用户信息的保存。。。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-25/20990902.jpg)


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-25/43131305.jpg)

保存在数据库中的session

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-25/23960851.jpg)

浏览器中的sessionid

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-25/7288776.jpg)

当请求的时候，服务器不用判断用户名以及密码了。。。

有一个django.contrib.sessions app对request,response来进行拦截。。到数据库进行查找。。默认配置好的。。。

cookie本地存储的，浏览器的。。。
cookie不安全，不能把所有的信息保存到本地。。

[Session的本质](http://www.freebuf.com/articles/web/10369.html)

正是基于这个考虑，服务器返回id的时候，用到了session的机制，session的机制根据用户名和密码生成一段字符串，这个字段是有过期时间的，session是由服务器生成的，存储在服务器端，发给用户，用户把它存储在cookie当中，下一次的请求的时候，用户在cookie中带上这个sessionid，带回到服务器，服务器通过sessionid在数据库中查询这个用户是那个用户，就可以对这个用户做标记了。。

6.7 用户注册

首先urls.py 配置，把前端的注册代码拷贝到templates..

在views.py写入类。。。


	    class RegisterView(View):
	    	def get(self,request):
	    		return render(request,"register.html",{})
    


把index.html中的注册链接，指向register..


	<a style="color:white" class="fr registerbtn" href="{% url 'register' %}">注册</a>


点击首页的注册，就可以跳转到注册页面。。。

	对之前的../改为 /static/css的时候，可以是使用{% %}来引用、、、

首先在index.html中 

	{% load staticfiles %}
	href="{% static 'css/reset.css' %}">

静态文件都这么做，，，

点击注册，返回的页面。。。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-5-25/19989099.jpg)

也是表单的提交。。。

captcha,第三方插件验证码。。，安装。。。。下面的链接为设置验证码的。。

[http://django-simple-captcha.readthedocs.io/en/latest/usage.html#installation](http://django-simple-captcha.readthedocs.io/en/latest/usage.html#installation)


    pip install  django-simple-captcha==0.4.6


犯错误的地方，render的参数，第三个参数，字典形式。。。

	return render(request,"register.html",{'register_form':register_form})


6.9，10 邮箱发送，，

这个是注册页面，另外有邮箱激活功能，其实注册的时候逻辑和登录的类似，包括其中前端页面的修改，报错信息之类的。。views一些逻辑，forms表单，前端的错误信息显示..

学到用户是否被激活、、、


##### 授课机构    6/1号

共有头部底部的，template...


	{%   include  %}

模版继承的机制。。

首先在template中新建一个base.html

把课程机构拷贝过来。。。

在base.html中加入

	{% load staticfiles %}

定义一个父模版，其中的头和底固定的。。。

在title里面。。

	<title>{% block title %}课程机构列表 - 慕学在线网{% endblock %}</title>

block的用处，，，静态文件有共有的。。。

    {% block custom_css %}{% endblock %}用户可以加入自定义的css，也可以包含全局的。。。

js也是这样的。。。

    {% block custom_bread %}
    <section>
    <div class="wp">
    <ul  class="crumbs">
    <li><a href="index.html">首页</a>></li>
    <li>课程机构</li>
    </ul>
    </div>
    </section>
    {% endblock %}



7-1 base页面的定制。。对html模版的引用以及定制。。。

在org-list.html中继承模版。。。

	{% extends 'base.html' %}
	{% block title %}课程机构列表 - 幕学在线网{% endblock %}
	#{只替换其中的title，其他的保留下来#}
    

7-2 页面以及页尾的继承。。中间自己的内容需要自己编写。。。模版的继承


7-3 课程列表的逻辑编写

动态数据，从后台读取。编写view

所在地区、

数据models编写完之后，可以完善view

每个课程机构都是一个dl

7-4 将自己写的数据库里面的内容发送到前端，如何配置路径以及将数据库里的动态内容显示到前端。。。

7-5 显示分页功能的设计。。。

pure pagination

    https://github.com/jamespacileo/django-pure-pagination     
 
 根据其中的文档写出自己的逻辑。来具体的怎么分页。。分页功能主要使用的是上面的那个库，根据这个文档的说明，来进行分析研究。。。

7-6  筛选。。。

城市的筛选，机构类别的筛选。。。


	<a href="?order_by="><span class="active">全部</span></a>
	
	<a href="?ct=pxjg"><span class="">培训机构</span></a>
	
	<a href="?ct=gx"><span class="">高校</span></a>
	
	<a href="?ct=gr"><span class="">个人</span></a>


在views里面和前端都需要修改。。。

右侧的授课机构排名。。。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-6-1/24358973.jpg)

排序关联，在选择北京市的时候，还可以进行课程数的选择。。。

7-7

表单提交的完成

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-6-1/10796034.jpg)

UserAsk表单的提交

之前的是先创建一个forms.py，现在通过models.py来转换为forms。。

其中的UserAsk在operation里面，但是填写表单在organization的里面。。。forms.py

django url中的include使用。。在每个app可以自己配置url。。这个设置主要是根目录和相对目录。为了减少主urls.py的复杂，在相应的apps里面创建新的urls.py

url的分发。。。

ModelForm在一定条件下比Form方便。。。兼具了Model和Form的功能。。。内容可以直接存储到数据库。。

在用户咨询表单添加内容。。。

在我要学习咨询的表单中提交内容，可以在数据库中看到，利用script和ajax来响应。。。

7-9 机构详情。。。

点击课程--跳转到课程机构页面。。。

点击这个图片，就可以跳转到对应的机构页面‘

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-6-2/8380597.jpg)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-6-2/54891398.jpg)

7-10 机构课程的页面显示

7-11 机构介绍显示。。。一般都是左边的框架不同，所以这几个页面可以通过继续模版。。。

面包屑的修改。。。


	{% block page_path %}机构教师{% endblock %}


7-12 收藏页面的设置

页面的显示已经完成，在这一节做收藏的实现。。。

收藏功能。异步完成，ajax...

加url..


    RemovedInDjango110Warning: django.core.context_processors is deprecated in favor of django.template.context_processors.
      __import__(name)

出现这个错误，遇到的问题，用户未登录的情况下，可以收藏成功。。。。

这次的问题，主要是没有耐心听下去课程，遇到问题可以先放下，把这节的课程听完。


收藏功能，各个页面都要显示出来是收藏。。。。


8-1 公开课

页面显示最新、最热、参与人数。。。。

课程详情，开始学习下面的所有视频，章节和评论。。资料展示以及下载。。。

把前端的页面拷贝过来。。。course-list.html,首先写url,,

在主urls.py中，include..

base.html的继承。。。course-list.html继承base的头和尾部的内容，然后其中的面包屑，以及内容需要独有的html。。。

    {% block title %}公开课列表 - 幕学在线网{% endblock %}
    {% block custom_bread %}
    {% endblock %}
    {% block content %}
    {% endblock %}
    


	让数据库中的数据动态的展示，需要在后台添加数据。。。在前台页面作展示。。。
	
	需要修改在页面显示数据的具体位置，比如\<h2\>等一些字段的显示。。。
	
分页功能。。页面的分页，代码是通用的，首先在view中

###### 对课程进行分页

        try:
            page = request.GET.get('page', 1)
        except PageNotAnInteger:
            page = 1

        p = Paginator(all_courses, 3, request=request)  # 5每页的数量

        courses = p.page(page)


然后就是html

	    <div class="pageturn">
	    <ul class="pagelist">
	    {% if all_courses.has_previous %}
	     <li class="long"><a href="?{{ all_courses.previous_page_number.querystring }}">上一页</a></li>
	    {% endif %}
	    
	    {% for page in all_courses.pages %}
	    {% if page %}
	    {% ifequal page all_courses.number %}
	     <li class="active"><a href="?{{ page.querystring }}">{{ page }}</a></li>
	    {% else %}
	     <li><a href="?{{ page.querystring }}" class="page">{{ page }}</a></li>
	    {% endifequal %}
	    {% else %}
	    <li class="none"><a href="">...</a></li>
	    {% endif %}
	    {% endfor %}
	    {% if all_courses.has_next %}
	     <li class="long"><a href="?{{ all_courses.next_page_number.querystring }}">下一页</a></li>
	    {% endif %}
	    
	    
	    </ul>
	    </div>


排序功能和课程机构的类似。。。

sort,

公开课右边的热门课程推荐。。。

		{{ hot_course.degree }}   难度的显示。。。但是难度是多选项，可以使用下面的这个显示。。。
	
	
	    {{ hot_course.get_degree_display }} 专门针对choice显示的。。


30min的视频---100min完成。。。         3倍呀。。。。。

8-2 课程详情页

首先 html的拷贝。。。

在前端显示中没有章节数，如何统计。。可以在models中定义一个函数，def 章节数。。。

课程点击之后，把其中的点击数量加一。。。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-6-5/16061124.jpg)



8-3 课程详情下面的部分内容以及右部分，授课机构。。

相关课程的推荐。。代码的编写。。。按照点击数做相关推荐。。首先在course.models字段里面添加一个tag..

可以看到界面的收藏。。。需要js代码来完成。。。

8-4-5 点击开始学习，进入学习页面，显示，章节，每一章的内容以及其中的具体学习内容。比如视频。。

章节信息，评论信息。。

把其中的html拷贝到template里面。。先考虑继承base.html

其中的视频没有在页面显示的原因，是因为在models中添加def函数的时候，应该在对应的数据表中添加对应的def函数。。。

在这个浪费了许多的时间，其中的原因还是不明白其中的原理，为什在在models下的加入def函数，可以显示数据。。。

资源下载。。。

view中是将数据取出来，并展示在页面。

思考，课程的遍历 

	{%  for a in aa %}  {% endfor %}

把内容遍历出来后，利用数据库的一些属性a.b来将其显示在页面。。。



    class CourseInfoView(View):
    	"""
    	课程章节信息
    	"""
    	def get(self,request,course_id):
    		course = Course.objects.get(id=int(course_id))
		    all_resources = CourseResource.objects.filter(course=course)
		    return render(request, "course-video.html", {
		    "course": course,
		    "course_resources":all_resources
    
    })



比如，在view中使用class来建立视图，取数据库中的数据 `course = Course.objects.get(id=int(course_id))` 然后将其显示在某个前端页面中

    return render(request, "course-video.html", {"course": course,｝)

字典形式，键是名字，值就是从数据库中取到的值。。因为取到的值是多个的，所以在前端需要进行遍历一个个的显示。。比如说取的是那一个数据库，就可以使用其中的属性，在页面来显示。。



	如果数据库中一个字段是多选的，比如难易程度，在前端显示的时候 `{{ course.get_degree_display }}`

需要 .get_字段_display


8-6 课程评论。。。。

首先配置 url

    url(r'^comment/(?P<course_id>\d+)/$',CourseInfoView.as_view(),name="course_info"),

写view，首先一个get方法、、、

    class CommentsView(View):
    	def get(self,request,course_id):
			return render(request,"course-video.html",{})


    all_comments = CourseComments.objects.all()    取出数据库的全部字段
	all_comments = CourseComments.objects.get()      可能抛出异常
	all_comments = CourseComments.objects.filter()  这几个方法需要整理


url 跳转的时候
    
    href="{% url 'course:course_comments' course.id  %}"    后面加入 course.id

	url(r'^info/(?P<course_id>\d+)/$',CourseInfoView.as_view(),name="course_info"), url配置的，(?P<course_id>\d+)


你只是在后台把评论写了进来，在前端如何进行添加评论呢？傻了吧。。。新建view

添加评论的view，先判断是否登录，然后取数据存入到数据库中。。。。



    class AddCommentsView(View):
	    """
	    用户添加课程评论
	    """
	    def post(self,request):   #post方式来提交数据
            if  not request.user.is_authenticated():
			    #判断用户登录状态
			    return HttpResponse('{"status":"fail","msg":"用户未登录"}', content_type="application/json")
			    
			    course_id = request.POST.get("course_id",0)   #取出评论课程的id
			    commtents = request.POST.get("comments","")  #取出评论的内容
    			if course_id > 0 and commtents: #判断评论的id存在吗和有内容吗，然后保存
				    course_comments = CourseComments() #实例化
				    course = Course.objects.get(id=int(course_id))
				    course_comments.course = course#评论课程
				    course_comments.comments = commtents   #把评论内容传递到数据库
				    course_comments.user = request.user  #那个人评论的
				    course_comments.save()   #保存到数据库中
				    return HttpResponse('{"status":"success","msg":"添加成功"}', content_type="application/json")
    
    		else:
   				 return HttpResponse('{"status":"fail","msg":"添加失败"}', content_type="application/json")


    

url的配置添加课程评论

    url(r'^add_comment/$',AddCommentsView.as_view(),name="add_comment"), #不需要(?P<course_id>\d+)/，因为在post已经传递


添加评论在前端需要ajax以及javascript的设置。。

    <script type="text/javascript">
    //添加评论
    $('#js-pl-submit').on('click', function(){
    var comments = $("#js-pl-textarea").val()
    if(comments == ""){
    alert("评论不能为空")
    return
    }
    $.ajax({
    cache: false,
    type: "POST",
    url:"/course/add_comment/",
    data:{'course_id':10, 'comments':comments},
    async: true,
    beforeSend:function(xhr, settings){
    xhr.setRequestHeader("X-CSRFToken", "5I2SlleZJOMUX9QbwYLUIAOshdrdpRcy");
    },
    success: function(data) {
    if(data.status == 'fail'){
    if(data.msg == '用户未登录'){
    window.location.href="login.html";
    }else{
    alert(data.msg)
    }
    
    }else if(data.status == 'success'){
    window.location.reload();//刷新当前页面.
    }
    },
    });
    });
    
    </script>



总结8.6用户，提交评论，如何来提交呢，首先有添加用户评论的url，在view设置函数来如何取数据，进行判断用户是否登录，等这些逻辑，取出数据存入到数据库中，然后根据前端页面JavaScript/ajax的动态结合，判断是否存入输入，用户输入不能为空等操作，这些完成之后，就是评论的在页面的展示，同样的原理，for循环，一个个的展示出来。。

加油！


8-7   右下角的，学习该课程的同学还学习过什么课程。 课程关联的设置。。。

在view中，COurseInfoView中，课程章节信息中，

有一个判断，在课程页面里，如果需要开始学习的话，需要进行验证，用户是否登录，没有登录直接返回到登录页面。。。

总结。这个逻辑没有学好。关联一个用户学习的课程，比如学习了该课程，还学习了其他的课程也显示出来。。。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-6-6/57962898.jpg)

关联课程，取到一个用户学习过课程的id，然后遍历取出来。。。

		#取出所有课程id
        course_ids = [ user_couser.course.id for user_couser in all_user_courses]
        #获取学习过该课程的用户学习的其他课程
        releate_courses = Course.objects.filter(id__in=course_ids).order_by("-click_nums")[:5]


8-8   课程播放页面

video.js      课程js播放器     视频播放需要引用特地的js来在线播放。。。

七牛云上传视频文件。。。。

对象存储，新建存储空间，django_video,,内容管理，上传文件。。。



9-1 课程教师

页面的跳转，，以及将数据在页面显示出来。。。

表的建立以及外键。。。。

收藏的功能。。。

分页功能，以及收藏功能，。代码可以共用、、、、
分页前端的代码可以共用。。。

页面老师的排序功能的设置，，，在前端的参数里面 


    sort = request.GET.get('sort', "")
    if sort:
    	if sort == "hot":
    		all_teachers = all_teachers.order_by("-click_nums")



右侧的讲师排行榜

另外对人气等一些选项之间的排序选中的，标记

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-6-6/95409888.jpg)



    <li {% if sort == '' %}class="active"{% endif %}><a href="?sort=">全部</a> </li>
    <li {% if sort == 'hot'%}class="active"{% endif %}><a href="?sort=hot">人气 &#8595;</a></li>
    

    class="active"


总结思考，view里面对需要排序内容字段的取出，排序，然后在前端显示出来。。

对与分页，在前端有通用的代码。。。

9-2 讲师详情页

跳转入门的意思就是在href设置跳转到特定的url

讲师排行榜。。。。

点击讲师的时候往哪里跳转、点击课程的时候往那里跳转。。。  链接的跳转。。。

最后一个授课讲师的一个收藏以及授课机构的一个收藏。。。

后台的收藏逻辑通用。。。前端ajax使用

比如课程详情以及教师详情中，会有收藏功能，需要js以及ajax通用。。。。

已收藏刷新一下变为收藏，前端页面太容易变化，需要在view里面进行判断。。。。

判断老师是否被收藏，has_teacher_faved需要被传递到前端

    has_teacher_faved = False #定义是否收藏的默认值为False

    if UserFavorite.objects.filter(user=request.user,fav_type=3,fav_id=teacher.id): #如果查询到，就是收藏状态

    	has_teacher_faved = True


在前端进行判断。。。

	{% if  has_teacher_faved %}已收藏{% else %}收藏{% endif %}



10-1 slice       配置全局导航。。。。

pycharm 全局搜索快捷键 Ctrl+H

首页的搜索功能。。。首页的跳转页面的href的设置。。。

设置完之后，没有成为选中的状态。。。在base.html没有办法判断那个选中。。

最简单的方法在view中传入、、、

在view中 `current_nav = "teacher"`     传入前端 
	
	  "current_nav":current_nav
	
	
	       <li {% if current_nav == 'teacher' %}class="active"{% endif %}>
	    								<a href="{% url 	'org:teacher_list' %}">授课教师</a>



通过Url链接来判断，`http://127.0.0.1:8000/org/teacher/list/` 进入了那个页面。。。

request.path|slice    相对地址的前几位。。。

    <li {% if request.path|slice:'6' == 'course' %}class="active"{% endif %}>

	
	总结：全局配置功能的设置，通过href的跳转来完成的，，
	`<a href="{% url 'index' %}">首页</a>`这种形式来进行跳转，因为之前在url里面配置了Url的正则，通过url配置的name来进行跳转。。


10-2 首页的搜索功能的设置。。。。跳到各个链接。。。


在url后面加入关键词值，

ORM的like数据库功能，在数据表某个字段 比如 `name__icontains`           两个下划线。。。

    from django.db.models import Q


授课讲师搜索的时候，没有搜索成功。。。可以找到一条，但是找不全。。。。


10-3 个人信息页面

radio的选项

    &nbsp;<input type="radio" name="gender" value="female" {% if request.user.gender == 'female' %}checked="checked"{% endif %}>女</label>



这一段代码是显示的那个X号，来退出修改。。。

    <div class="close jsCloseDialog"><img src="{% static 'images/dig_close.png' %}"/></div>


10-4 修改头像和密码

新建url,写class view，然后将 class导入，url中。。配置html。。。

上传图片

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-6-7/8137189.jpg)


总结：修改头像，表单的引用。。。。Model.form 可以直接保存，既有model又有form的功能。。。



10-5 修改密码。。。

	修改密码和重置密码的步骤类似
	
	js中的url必须是路径，不像html中的 {%  url  %}


问题：当输入的密码小于5位的时候，没有显示必须输出的密码大于5位。。。







django的最新xadmin源码有个重大bug 大家要注意 这个bug可能会引起某些地方的bug，
大家如果是下载的最新的xadmin源码 注意一下这个地方 如果使用的是django1.9可能会引起问题
大家可以先把这个地方的逻辑注释掉 或者直接将这个变量改为false

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-6-7/36323117.jpg)


修改密码遇到的问题，当输入两个不一样的密码的时候，显示密码不一致。。
但是输入两个一样的密码的时候，就不能修改成功。。。

    TypeError: 'SimpleLazyObject' object is not callable，这个问题是方法的调用，比如request.user()、request.user的区别。。。
    


10-5 总结，修改头像和密码，和头部的全局配置。。。

10-6 修改邮箱。。。

两个接口，点击获取验证码的时候，需要向用户新的邮箱发送验证码。。。和用户的邮箱和验证码是否匹配。。。

获取验证码的接口。。。


    class SendEmailCodeView(LoginRequiredMixin,View):



总结 10-6 只是发送了验证码。。。


10-7 进行修改邮箱的view修改

    class UpdateEmailView(LoginRequiredMixin,View):

接下来表单的提交。。。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-6-8/97895571.jpg)


    return HttpResponse(json.dumps(modify_form.errors), content_type="application/json")        json.dumps 不需要'' 包裹。。。。


然后显示到页面上面。。。

数据库的外键的作用。。。。继承其他数据表的内容。。。

页面的跳转要学习，，那个要跳那个页面，以及后面显示的参数问题，id的选项。。。

10-9 我的收藏


比如课程机构，授课教师，公开课。。。

	</div>    <span>


10-11 个人消息显示。。。。

消息的显示，如果用户有什么消息，可以在页面的头部显示。。。但是现在没有。。。这段代码就是显示页面顶部的消息。


    <a href="">
    <div class="msg-num"><span id="MsgNum">{{ requst.user.unread_nums }}</span></div>
      </a>


在user的models中显示的消息函数。。。

    def unread_nums(self):
        # 获取用户未读消息的数量
        from operation.models import UserMessage
        return UserMessage.objects.filter(user=self.id).count()



11-1 登出和点击数以及收藏数完善

课程点击数的收藏。。。

现在的排序功能实现好了，可以根据其中的最热门以及参与人数来进行相应的排序了。后台对其中的大小。。

收藏数量，有一个接口。。。

取消收藏。。。。

问题：：：授课机构的收藏有问题。。。。点击的收藏不能收藏进去。。。



	<li class="{% if forloop.counter|divisibleby:5 %}five{% endif %}">    

一行五个显示

在debug=False的时候，第三方静态文件是通过服务器来代理的。。所以需要设置一下。。



13章：xadmin的进阶开发

注册以及权限管理。。。


后台管理系统权限的使用。。。。

图标，icons的修改。。。

某些字段在后台只读。。。`readonly_fields`

在后台添加课程的时候，比如课程机构，进行搜索状态的。。

13-4自定义列表返回数据，同一个model注册两个管理器。。

新增课程的时候，添加章节。。。课程里面嵌套章节，只能一层嵌套。。。


13-6  xadmin插件的开发。。。

把富文本编辑器集成到后台。。

xadmin.readthedoc.io

写一个xadmin的插件。。在源代码中添加，plugins

后台的富文本。。


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-6-12/50717424.jpg)


图片不能上传成功。。。

    Forbidden (CSRF token missing or incorrect.): /ueditor/controller/
    [12/Jun/2017 20:02:58] "POST /ueditor/controller/?imagePathFormat=courses%2Fueditor&filePathFormat=courses%2Fueditor&imageMaxSize=1204000&action=uploadimage HTTP/1.1" 403 2266



插件二，excel


14。。部署、、、

nginx+uswgi


href的跳转，以及对应的id的编写，，`是_还是.`
跳转，如果后面有参数的话，比如具体那个数字，章节的问题，如何写href....



##### 继承的机制 

比如一些html的头部以及底部的内容有一样的地方，可以把共用的写为base.html..

##### 子模版的重写 

不同页面继承base的时候，有的情况下，需要修改继承的内容，比如title的修改等等，，

    <title>{% block title %}课程机构解表{% endblock %} </title>

	重载
	`css <% block custom_css %>{% endblock %}`  同时可以包含已有的css

js也是一样的道理

	<% block custom_js %>{% endblock %}

面包屑 

	<% block custom_bread %>{% endblock %}

content 内容的重载 

	<% block content %>{% endblock %}
