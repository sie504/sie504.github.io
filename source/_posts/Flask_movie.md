---
title: Flask 构建微电影
date: 2017-07-31 15:54:24
tags:
		- Flask
		 
categories:
		- Web开发
---

`Flask`微电影网站学习过程

<!-- more -->

时间 2017/07/31

[Flask](https://www.shiyanlou.com/courses/29)

[官网](http://flask.pocoo.org/)

[中文](http://dormousehole.readthedocs.io/en/latest/)

[知乎专栏flask](https://zhuanlan.zhihu.com/flask)

##### 前言

按照[Python Flask 构建微电影视频网站](http://coding.imooc.com/class/124.html)，一步步的来学习使用`Flask`编写一个简单的Web应用，记录一下学习的过程。

断断续续算是做了下了，源码上传到`github`。[下载地址](https://github.com/sie504/A-movie-of-flask)

##### 环境安装

* virtualenv      

安装virtualenv

	 	apt-get install python-virtualenv  

创建项目

	root@Ubuntu:~# mkdir myproject
	root@Ubuntu:~# cd myproject/
	root@Ubuntu:~/myproject# virtualenv venv
	New python executable in venv/bin/python
	Installing setuptools, pip...done.


激活相应环境

	root@Ubuntu:~/myproject# . venv/bin/activate
	(venv)root@Ubuntu:~/myproject# 


激活virtualenv中的flask

	$ git clone https://github.com/shiyanlou/flask
	Initialized empty Git repository in ~/dev/flask/.git/
	$ cd flask
	$ virtualenv venv --distribute
	New python executable in venv/bin/python
	Installing distribute............done.
	$ . venv/bin/activate
	$ python setup.py develop
	...
	Finished processing dependencies for Flask



#### Flask运行及调试模式

简单访问

		  1 from flask import Flask
		  2 app = Flask(__name__)
		  3 
		  4 @app.route('/')
		  5 def hello_world():
		  6     return 'Hello world!'
		  7 
		  8 if __name__ == '__main__':
		  9     app.run()
	

访问

	(venv)root@Ubuntu:~/myproject/flask# python hello.py 
	 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
	127.0.0.1 - - [31/Jul/2017 16:22:04] "GET / HTTP/1.1" 200 -


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-7-31/56279982.jpg)

这段代码的做了什么？

1. 首先导入了类Flask。这个类的实例化将会是我们WSGI应用。第一个参数是应用模块的名称。如果使用的是单一的模块，第一个参数应该使用`__name__`。因为如果它已单独应用启动或作为模块导入，名称将会不同。
1. 接着创建一个该类的实例，我们传递给它模块或包的名称，这样Flask才会知道去哪里寻找模版、静态文件等等。
2. 接着使用装饰器`route()`告诉Flask哪个URL才能触发我们的函数
3. 定义一个函数，该函数也是用来给待定函数生成URLs，并且返回我们想要显示在用户浏览器上的信息。
4. 最后用`run()`启动本地服务器来运行我们的应用。`if __name__ == '__main__'`确保服务器只会在该脚本被Python解析器直接执行的时候才会运行，而不是作为模块导入的时候。。


* 外部可见服务器

	需要修改 app.run(host='0.0.0.0')


* 调试模式

run()方法十分适用启动一个本地开发服务器。。

开启调试模式

	app.debug = True
	app.run()

或者

	app.run(debug=True)


#### 路由

`route`装饰器是用于把一个函数绑定到一个URL上。类似与Django的urls的设置。。直接在一个文件里面了，路由和函数。。

		  1 from flask import Flask
		  2 app = Flask(__name__)
		  3 
		  4 @app.route('/')
		  5 def hello_world():
		  6     return 'Hello world!'
		  7 
		  8 @app.route('/hello')
		  9 def hello():
		 10     return 'flask'
		 11 
		 12 if __name__ == '__main__':
		 13     app.run(host='0.0.0.0')

访问

	http://192.168.145.226:5000/hello


变量规则

	为了给URL增加变量的部分，需要把一些特定的字段标记成`<variable_name>`。这些特定的字段作为参数传入到函数中。也可以指定转换器通过规则`<converter:variable_name>`将变量转换为特定的数据类型。

	  #! -*- coding:utf-8 -*-
	  2 from flask import Flask
	  3 app = Flask(__name__)
	
	 13 @app.route('/user/<username>')
	 14 def show_user_profile(username):
	 15     #显示用户的名称
	 16     return 'User %s' % username
	 17 
	 18 @app.route('/post/<int:post_id>')
	 19 def show_post(post_id):
	 20     #显示提交整型的用户"id"的结果，注意"int"将输入的字符串转换为整型数据
	 21     return 'Post %d' % post_id
	 22 if __name__ == '__main__':
	 23     app.run(host='0.0.0.0')


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-7-31/76205052.jpg)


* 唯一URLs 重定向行为 

	当用户访问url时，后面最好不要写入/

* 构造URL

使用函数`url_for()`来针对一个特定的函数构造一个URL，它能够接受函数作为第一个参数，以及一些关键字参数，每一个攻击者参数对应与URL规则的变量部分。


	>>> from flask import Flask,url_for
	>>> app = Flask(__name__)
	>>> @app.route('/')
	... def index(): pass
	... 
	>>> @app.route('/login')
	... def login(): pass
	... 
	>>> @app.route('/user/<username>')
	... def profile(username): pass
	... 
	>>> with app.test_request_context():
	...     print url_for('index')
	...     print url_for('login')
	...     print url_for('login',next='/')
	...     print url_for('profile',username='mlb')
	... 
	/
	/login
	/login?next=%2F
	/user/mlb


* HTTP方法

默认情况下路由只会响应GET请求，但是能够通过给route()装饰器提供methods参数来改变。。

#### 静态啊文件及渲染模块

* 静态文件

动态的web应用需要静态文件，比如html、css、javascript文件。在包中或模块旁边创建一个名为static的文件夹，在应用中使用`/static`即可访问。

给静态文件生成URL，使用特殊的static端点名：

	url_for('static',filename='style.css')

这个文件应该存储在文件系统的`static/style.css`

* 渲染模块

可以使用方法`render_template()`来渲染模版，所有需要做的就是提供模版的名称以及你想要作为关键字参数传入模版的变量。

		#! -*- coding:utf-8 -*-
		from flask import Flask
		from flask import render_template
		
		app = Flask(__name__)
		
		@app.route('/hello/')
		@app.route('/hello/<name>')
		
		def hello(name=None):
		    return render_template('hello.html',name=name)
		
		if __name__ == '__main__':
		    app.run(host='0.0.0.0')


vim templates/hello.html

		<!doctype html>
		<title>Hello from Flask</title>
		{% if name %}
		    <h1>Hello {{ name }}!</h1>
		{% else %}
		    <h1>Hello World<h1>
		{% endif %}


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-1/29004914.jpg)

静态文件放在 static目录中，模板文件放在templates目录下。


#### 重定向、响应、会话和扩展

#### Flask 项目1

0803

`render_template`引用html，传递相应的参数到前端。。。

templates  html脚本，同样一些简单的逻辑。。

static 静态文件

模版的一些继承，，

	{% extends "base.html" %}
	{% block content %}
	{% endblock %}


配置文件 `config.py`

	CSRF_ENABLED = True 
	SECRET_KEY = '...'  #配置CSRF激活的时候使用，一个加密的令牌


表单。。

Form的子类。。把表单的域定义成类的变量。其中的意思就是把表单中一些变量设置为类中的变量，比如用户名，密码，邮箱，

从wtforms中导入字段类型，比如TextField、PasswordField。。

	form.hidden_tag()模板参数将被替换一个隐藏字段，用来实现在配置中激活的CSRF保护

在html模版中，调用在表单类中的字段。。

表单、模版，通过视图将表单的数据传递到模版，即前端。。。

数据库

同样是在`config.py`中进行设置。。

数据库模型设计

存储在数据库中的数据以类的集合显示。。ORM层做的是将这些类创建的对象映射到适合的数据库表的行。。

类，继承`db.Model`

0804

#### Flask 构建微电影视频网站

* 学会使用整型、浮点型、路径型、字符串型正则表达式路由转化器
* 学会时候post、get请求、上传文件、cookie获取与响应，404处理
* 学会使用模版自动转义、定义过滤器、定义全局上下文处理器、Jinja2语法、包含、继承、定义宏
* 使用flask-wtf定义表单模型、字段类型、字段验证、视图处理表单、模版使用表单
* 使用flask-sqlachemy定义数据库模型、添加数据。修改数据、查询数据、删除数据、数据库事件、数据迁移
* 使用蓝图优化项目结构，实现前台和后台业务逻辑
* flask的部署方法
* 

##### 结构

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-4/61839159.jpg)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-4/98881381.jpg)

目录结构分析

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-4/89167512.jpg)

蓝图构建项目目录

flask-sqlalchemy 

##### 3章：数据库的设置

数据库的日志

继承db.Model

都是分析功能，设置数据库。。外键的关联。。。

用户表，日志表，标签、电影、预告、评论、电影收藏、权限以及角色数据模型设计。。

定义角色数据模型设置

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-4/27828109.jpg)






	<hr/>

####环境的配置

虚拟化环境的安装

	C:\Users\Administrator>python3 -m pip install virtualenv


创建虚拟环境

	C:\Users\Administrator>python3 -m virtualenv test

激活虚拟环境

	C:\Users\Administrator>cd C:\Users\Administrator\test\Scripts

	C:\Users\Administrator\test\Scripts>activate.bat

	(test) C:\Users\Administrator\test\Scripts>


虚拟环境使用

	创建虚拟环境 virtualenv venv
	激活虚拟环境 source venv/bin/activate
	退出虚拟环境	deactivate



flask的安装

	安装前检测 pip freeze
	安装flask pip install flask




创建flask项目


5-2 视频的重音


20170807

#### 前后台项目目录分析

##### 前台(home)

* 数据模型: models.py
* 表单处理： home/forms.py
* 模版目录 templates/home
* 静态目录 static


##### 后台(admin)

* 数据模型: models.py
* 表单处理： home/forms.py
* 模版目录 templates/home
* 静态目录 static

项目文件的分布

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-7/94122699.jpg)

把所有的目录建立在了一个app下面，里面包括前后端的各自目录，以及静态文件的划分。。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-7/93483568.jpg)

#### [蓝图构建项目目录](http://docs.jinkan.org/docs/flask/blueprints.html)

蓝图：一个应用中或跨应用制作应用组件和支持通用的模式

蓝图作用：将不同的功能模块化、构建大型应用、优化项目目录、增强可读性。易于维护。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-7/54990449.jpg)


便于管理不同功能模块，以及和路由的对应。。

 首先是定义
 

	`/admin/__init__.py`
	
		from flask import Blueprint
		
		admin  = Blueprint("admin",__name__)
		
		import app.admin.views


然后注册

在`app/__init__.py`中注册

	from app.admin import admin  as admin_blueprint
	app.register_blueprint(admin_blueprint,url_prefix="/admin/")


调用

在 `admin/views.py` 中写函数


		from . import admin
		
		@admin.route("/")
		def index():
		    return "<h1 style='color:red'>This is admin</h1>"


在入口文件中启动

`manage.py`

	from app import app
	
	if __name__ == '__main__':
	    app.run()


#### [数据库 SQLAlchemy](http://www.pythondoc.com/flask-sqlalchemy/)


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-7/97985107.jpg)

安装数据库连接依赖包、定义mysql数据库连接


	`__tablename__` 定义数据库的名称


	pip install flask-sqlalchemy    安装模块


打开`models.py`


	class User(db.Model):
	    __tablename__ = "user"
	    id = db.Column(db.Integer,primary_key=True) #编号
	    name = db.Column(db.String(100),unique=True) #昵称
	    pwd = db.Column(db.String(100)) #密码
	    email = db.Column(db.String(100),unique=True) #邮箱
	    phone = db.Column(db.String(11),unique=True) #手机
	    info = db.Column(db.Text) #个性简介
	    face = db.Column(db.String(255),unique=True,default=datetime.utcnow)
	    uuid = db.Column(db.String(255),unique=True) #唯一标志符
	
	    def __repr__(self):
	        return "<User %r>" % self.name


外键关系的关联。。。

首先在一个数据库中。。

关联的一个用户

	user_id = db.Column(db.Integer, db.ForeignKey('user.id'))  # 所属用户

在该表中进行设计

	moviecols = db.relationship("Moviecol", backref='user')  # 电影收藏外联关系       Moviecol是和和其关联的表

#### 前台页面

在前台引入静态文件以及路由等其他资源。。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-7/39177381.jpg)

静态文件引入 

	`{{ url_for('static',filename='文件路径')}}`


##### 会员登录页面的搭建

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-7/66157786.jpg)

##### 会员注册页面的搭建

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-7/63362936.jpg)

#### 会员中心页面搭建

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-7/70002630.jpg)

	{% include "home/menu.html" %}  引用包括其他的html



前端里面的 active 激活。。。  用到了javascript的一些代码，

#### 电影列表页面搭建

#### 搜索布局

#### 电影详情页面

#### 后台页面


设计路由的时候

0811

激活的操作

点击某个链接的激活状态。。

* 标签管理页面搭建

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-11/46920605.jpg)

一段script代码，显示激活状态


		<script>
		    $(document).ready(function () {
		        $("#g-8").addClass("active");
		        $("#g-8-2").addClass("active");
		    })
		</script>


以上只是对页面的一个展示，让html和后台或者前台的结合，接下来要进入正式的学习了，业务的真正处理。。
20170811

20170814

如何将写的代码生成数据库表。。

在最后加入


		if __name__ =="__main__":
	    	db.create_all()

使用pymysql模块。。。

执行		python3 models.py 就会在数据库中生成对应的表


密码生成 

	from werkzeug.security import generate_password_hash

和数据库的链接。。


	import pymysql
	app.config["SQLALCHEMY_DATABASE_URI"] = "mysql+pymysql://root:@127.0.0.1:3306/movie"

	if __name__ =="__main__":
    	db.create_all()


* 错误页面的显示设置

同样是一个html文件，需要在app的init里面设置。。。

	@app.errorhandler(404)
	def page_not_found(error):
	    return render_template("home/404.html"),404


* 管理员登录

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-14/61408317.jpg)


模型admin、表单LoginForm、请求方法 GET、POST、访问控制 无

	http://127.0.0.1:5000/admin/login/

验证的字段有那些，帐号和密码。。

使用表单认证。。。

* 表单认证 flask_wtf。。。这个需要学习

[1](http://www.ttlsa.com/python/python-flask-wtf-and-wtforms/)

[手册](https://wtforms.readthedocs.io/en/latest/)

比如定义一个登录的表单。。其中的class 和html中的一致。。



	from flask_wtf import FlaskForm
	from wtforms import StringField,PasswordField,SubmitField #表单类型
	from wtforms.validators import DataRequired  #验证字段
	
	class LoginForm(FlaskForm):      #定义表单类
	    """管理员登录表单"""
	    account = StringField(
	        label="帐号",
	        validators=[
	            DataRequired("请输入帐号!")
	        ],
	        description="帐号",
	        render_kw={
	            "class":"form-control",
	            "placeholder":"请输入账号",
	            "required":"required"
	        }
	
	    )
	    pwd = PasswordField(
	        label="密码",
	        validators=[
	            DataRequired("请输入密码!")
	        ],
	        render_kw={
	            "class": "form-control",
	            "placeholder": "请输入密码",
	            "required": "required"
	        }
	    )
	    submit = SubmitField(
	        '登录',
	        render_kw = {
	            "class":"btn btn-primary btn-block btn-flat",
	        }
	    )



把表单处理好之后，需要在view里面进行设置、、

首先导入，实例化，和html的相结合。。

首先html中的input可以去掉。。用什么替换。。
 
	{{ form.account }}

访问表单可以自动生成

	<input class="form-control" id="account" name="account" placeholder="请输入账号" required="required" type="text" value="">

在表单下添加

	 {{ form.csrf_token }}

在前端访问时候可以看到 csrf的保护

	<input id="csrf_token" name="csrf_token" type="hidden" value="IjM5Mjk5NmU4NWRkMDc1NWFlMzhkYmJjNTM1MWU2YmMwMTFkNjBiNmIi.DHMAfA.aNKDXI2d9tXTwoFJ5H7mmcXEaqc">


定义好之后，视图的处理，在view中


显示错误信息。。

验证登录密码之类的信息。。

	flash是一个消息的闪现

	登录密码错误的话，显示密码错误，重新返回登录页面。。
	正确的话，建立一个session会话。。

* 标签管理 0816

首先定义表单

	class TagForm(FlaskForm):
	    name = StringField(
	        label="名称",
	        validators=[
	            DataRequired('请输入标签!')
	        ],
	        description="标签",
	        render_kw={
	            "class": "form-control",
	            "id": "input_name",
	            "placeholder": "请输入标签名称！"
	        }
	    )
	
	    submit = SubmitField(
	        '添加',
	        render_kw={
	            "class": "btn btn-primary",
	        }
	    )

然后传入view中
先实例化，然后传入到参数中。

在前端的input进行修改，进行表单的实现。

有表单的地方，可能就会和数据库的结合。

	#数据的获取
    if form.validate_on_submit():
        data = form.data

在前端的错误信息的显示。。

	{% for err in form.name.errors %}
                        <div class="col-md-12">
                        <font style="color:red">{{ err }}</font>
                    </div>


错误信息

	{% for err in form.字段.errors %}

	<div class="col-md-12">
                        <font style="color:red">{{ err }}</font>
                    </div>
	{% endfor %}

这个错误信息，是在表单定义中的


	 validators=[
	            DataRequired('请输入标签!')
	        ],


唯一性的确认


	tag = Tag.query.filter_by(name=data["name"]).count() #数量的统计
	        if tag == 1:
	            flash("名称已经存在")
	            return redirect(url_for('admin.tag_add'))

[flash 消息闪现](http://docs.jinkan.org/docs/flask/patterns/flashing.html)

 闪烁，对结果的一个提示，这个也会在前端显示出来。。get_flashed_messages()

	 {% for msg in get_flashed_messages() %}
	 {% endfor %}      


入库


		tag = Tag(
	            name=data["name"]
	        )
	        db.session.add(tag)
	        db.session.commit()
	        flash("添加标签成功！", "ok")
	        return redirect(url_for('admin.tag_add'))


#### 表单，提取数据，判断，前端错误消息显示、入库

这几个基本的操作步骤要理解了，这样碰见其他的类似操作，流程化就可以来了。。

1：根据前端具体的表单，定义表单类。。其中的name、label、validators、description、render_kw（样式内容）

2：在views对应的视图导入表单类，实例化，在前端进行替换，使用定义的表单生成相应的input，csrf的防御。

3:进行数据的验证，是否重复等一些列逻辑的处理

4：入库

5：后端处理，在前端的错误信息的提示，时刻存在的。

#### 标签的管理，增删改查、分页。。

对于没有涉及到表单的地方，但是对数据库内容要进行删除或者修改，这里和数据库的操作。

对于一些页面的显示，多少页以及分页的操作。。分页技术的实现。。

在标签列表里面进行操作，分页的操作。。

在views中

	#标签列表
	@admin.route("/tag/list/<int:page>/",methods=["GET"])
	@admin_login_req
	def tag_list(page=None):
	    if page is None:
	        page = 1
	    page_data = Tag.query.order_by(
	        Tag.addtime.desc()
	    ).paginate(page=page,prt_page=1)       #paginate分页操作
	
	    return render_template("admin/tag_list.html",page_data=page_data) #page_data传入模版中




修改的问题，修改的是内容已经在数据库中了。。

如果去判断的话，那名字一定存在。。。

	#编辑标签
	@admin.route("/tag/edit/<int:id>/", methods=["GET", "POST"])
	@admin_login_req
	def tag_edit(id):
	    form = TagForm()
	    tag = Tag.query.get_or_404(id)
	
	    if form.validate_on_submit():
	        data = form.data
	        tag_count = Tag.query.filter_by(name=data["name"]).count()  # 数量的统计
	        if  tag.name != data["name"] and tag_count == 1:     #多了一个判断，必须要修改一下
	            flash("名称已经存在", "err")
	            return redirect(url_for('admin.tag_edit'))
	        tag.name = data["name"]
	        db.session.add(tag)
	        db.session.commit()
	        flash("修改标签成功！", "ok")
	        return redirect(url_for('admin.tag_edit'))
	
	    return render_template("admin/tag_edit.html", form=form,tag=tag)


多一个判断。。。

	tag_count = Tag.query.filter_by(name=data["name"]).count()  # 数量的统计
	        if  tag.name != data["name"] and tag_count == 1:  


#### 电影管理

添加电影，同样是根据后台的界面，设置表单，表单传入view中 ，设置html，以及读取数据，报错信息。。

上传的设置。。。

表单的上传。。

添加电影、电影列表。。

这个和之前的标签管理挺类似的，只不过添加电影多了一个对文件的处理，因为添加电影的时候，一些大的文件不适合存入到数据库中，可以存在服务器上的某一个文件中。。。处理文件的上传。。

对于涉及到参数的地方 


	`/movie/list/<int:page>/`
	
		def tag_list(page=None):     定义一个参数


删除的时候 id。。。

关联的删除。。。


* 添加电影

文件那个input type="file"    就是上传文件的。。

* 列表 删除 编辑

电影列表细节问题

	page_data = Movie.query.join(Tag).filter(    #对表的一个关联。。。
            Tag.id == Movie.tag_id
        ).order_by(                     
            Movie.addtime.desc()
        ).paginate(page=page,per_page=10)



对电影的修改，即编辑。对已有的数据库进行修改，需要进行一些判断，首先是数据库中数据的改变，然后涉及到文件的改变。。


    if form.validate_on_submit():
        data = form.data
        movie_count = Movie.query.filter_by(title=data["title"]).count()
        if movie_count == 1 and movie.title != data["title"]:   #判断片名以及数目
            flash("片名已经存在","err")
            return redirect(url_for('admin.movie_edit', id=id))


	movie.star = data["star"]
        movie.tag_id = data["tag_id"]
        movie.info = data["info"]
        movie.title = data["title"]
        movie.area = data["area"]
        movie.length = data["length"]
        movie.release_time = data["release_time"]
        db.session.add(movie)
        db.session.commit()
        flash("修改电影成功!","ok")
        return redirect(url_for('admin.movie_edit',id=id))
    return render_template("admin/movie_edit.html", form=form,movie=movie)


#### 预告管理 0823

这个功能和电影管理类似，也是上传文件，编辑，修改等这些操作，异曲同工。会了电影管理的逻辑，这个逻辑自然也应该可以的。。

表单实现过程

1. 首先根据表单，设置表单类。。
2. 在view中，导入数据库类以及表单类
3. 相应的逻辑函数中，实例化表单，然后传递到html中
4. 判断表单提交，获取数据
5. 前端html中修改表单标签，使用定义的类来定义
6. 错误信息的显示
7. form的设置，如果有上传文件，`method="post" enctype="multipart/form-data"`

表单实现完之后，逻辑的处理。涉及到数据库。。表单的上传。。

1. 获取HTTP数据
2. 传入数据库
3. 就这两步，但其中需要的逻辑判断。。(逻辑判断。。。)

删除和编辑

删除其实很好删除，从数据库中查找该数据，直接删除就可以了。

	preview = Preview.query.get_or_404(int(id))
    db.session.delete(preview)
    db.session.commit()


不理解的地方，对其中取出的数据内容的定义。。

	 preview = Preview.query.get_or_404(int(id)) //preview 就是表类的一个实例化
	 form.title.data = preview.title  #修改标题名字

案例

	from app.models import Admin, Tag, Movie,Preview
	preview = Preview.query.get_or_404(9)
	preview
	<Preview 'testr2'>
	preview.id
	9
	preview.title
	'testr2'
	preview.logo
	'2017082318021088e33b3370bd4ea39541e8ee60e28685.png'


#### 会员管理 0824

#### 评论管理

表的关联 join 表的过滤 filter 排序 order_by

		page_data = Comment.query.join(
		        Movie
		    ).join(
		        User
		    ).filter(
		        Movie.id == Comment.movie_id,
		        User.id == Comment.user_id
		    ).order_by(
		        Comment.addtime.desc()
		    )


#### 后台管理员的管理以及访问控制 7-3-4



#### 0827 评论管理 收藏管理

#### 修改密码

定义表单，然后在view实例化表单，在前端重置表单。。

表单数据，和数据库的交互。

验证旧密码，在表单定义一个方法。。验证旧密码的一个函数。。

在表单里面对旧密码的验证。。

    def validate_old_pwd(self,field):
        from flask import session
        pwd = field.data
        name = session["admin"]
        admin = Admin.query.filter_by(
            name=name
        ).first()
        if not admin.check_pwd(pwd):
            raise ValidationError("旧密码错误")


在view里面实现密码的改变。。使用加密函数。。从表单中获取的对应的用户，以及对其密码的修改，从表单中获取新密码,data..

	form = PwdForm()
	    if form.validate_on_submit():
	        data = form.data
	        admin = Admin.query.filter_by(name=session["admin"]).first()
	        from werkzeug.security import generate_password_hash
	        admin.pwd = generate_password_hash(data["new_pwd"])
	        db.session.add(admin)
	        db.session.commit()
	        flash("修改密码成功，请重新登录！","ok")
	        redirect(url_for('admin.logout'))
	    return render_template("admin/pwd.html",form=form)


#### 日志管理


上下文处理器。。将模版中的内容全局展示。。

时间管理上下文

	# 上下文处理器
	@admin.context_processor
	def tpl_extra():
	    data = dict(
	        online_time=datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
	    )
	    return data
	


* 操作日志

flask 获取ip地址


* 前台会员注册 0831



* 0902 基于角色的访问控制

权限管理 。。

第七章添加角色这里除出了很多的错误，一个是前端页面的错误信息显示不出来，重新拷贝一份后，可以显示错误信息了。但是添加数据的时候，又不能添加到数据库中。是什么原因？在这里卡了一个下午，好好思考找到这个问题的原因并解决这个问题。细节，原理。。。


数据库错误信息。。。数据库添加错误的原因是，你设置的数据库的为整数型，而输入的为字符型。。

	sqlalchemy.exc.IntegrityError: (pymysql.err.IntegrityError) (1062, "Duplicata du champ '0-0' pour la clef 'PRIMARY'") [SQL: 'INSERT INTO auth (name, url, addtime) VALUES (%(name)s, %(url)s, %(addtime)s)'] [parameters: {'name': 'list', 'url': '/admin/add/list', 'addtime': datetime.datetime(2017, 9, 2, 17, 22, 55, 50462

学会看错误信息，不要心急。。`sqlalchemy.exc.IntegrityError:`


错误信息没有显示的原因：

	if form.validate_on_submit(): 先进性这个判断。。

需要学习的内容，错误信息的展示，为什么会有错误信息，`DataRequired`。什么时候这个才会提示，表单的处理是如何进行的。

`DataRequired` 验证器只是简单地检查相应域提交的数据是否是空


#### 0906

涉及到多选项的表单，取出的数据要进行字符类型的转换。。字符串和列表的转换。。

	'1,2,3' ---> [1,2,3]

	如何来进行转换
	>>> a = '1,2,3'
	>>> type(a)
	<type 'str'>
	>>> b = a.split(',')
	>>> b
	['1', '2', '3']
	>>> type(a)
	<type 'str'>
	>>> type(b)
	<type 'list'>
	>>> map(lambda v:int(v),b)
	[1, 2, 3]
	

#### 添加管理员，验证重复密码。。

form里面有专门的字段验证。EqualTo


数据库的关联。。


#### 访问权限控制



定义权限装饰器

角色对应的权限。。


auths 权限

role 角色   什么角色对应什么样的权限。。


#### 0906 会员的注册

表单的字段，选择器 有选择字段，提交有提交字段等

表单信息的验证：密码两次是否相等Equalto、数据字段的验证等。。邮箱的验证 email


#### 0909 会员登录

登录进来其中，会话机制，session..


退出  session  研究 session 的用法

	session.pop(user,None)


登录的装饰器

8-4-5 修改密码，会员登录日志


0911

学习9章 电影模块的实现

0914 实现分页

模糊查询。。。

		movie_count = Movie.query.filter(
		        Movie.title.ilike('%' + key + '%')
		    ).count()
		    page_data = Movie.query.filter(
		        Movie.title.ilike('%' + key + '%')
		    ).order_by(
		        Movie.addtime.desc()
		    ).paginate(page=page, per_page=5)

一个单词的错误，session

__0919__


	{{ v.content | safe }}  safe的用途


__0920__

电影的弹幕

article/19536

[动手实战实现Redis数据库主从同步](http://www.imooc.com/article/19536)


	`dplayer.js.org`


关键字搜索，保留关键字。。。


#####问题

python items

flask-sqlalchemy.api.html#id4

class=disabled

对于其中出现字符串的内容一定要添加 单引号或者双引号。。

下上文处理器。。。


----------

[count number of rows in flask templates](https://stackoverflow.com/questions/17739264/count-number-of-rows-in-flask-templates)


轮播图哪一节





[flask request对象](http://www.cnblogs.com/harrychinese/p/flask_tips.html)


0920

遇到的问题，在收藏电影的时候，用户的个人页面里面，显示不了电影收藏的情况。。

