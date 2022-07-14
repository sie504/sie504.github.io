---
title: Flask 登录过程
date: 2017-08-16 15:54:24
tags:
		- Flask
		 
categories:
		- Web开发
---

`Flask`登录过程。

<!-- more -->



20170815

#### 管理员登录

* 定义表单
* html中使用表单
* views中请求处理，登录请求、保存会话
* 登录装饰器、访问控制


[sqlalchemy](http://flask-sqlalchemy.pocoo.org/2.1/quickstart/)

- 表单的定义

先看这个登录页面，需要几个属性，帐号，密码，登录。

 ![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-8-15/12263936.jpg)


####[flask-wtf](https://flask-wtf.readthedocs.io/en/stable/)

#### 表单的定义

定义表单以及其中的字段内容，比如帐号，label是一个标签，validators是验证，要导入模块，

		from flask_wtf import FlaskForm   #导入基类
		from wtforms import StringField,PasswordField,SubmitField  
		 #需要的字段，字符串，密码，登录
		from wtforms.validators import DataRequired,ValidationError
		#验证器DataRequired，登录的时候进行帐号验证
	
		class LoginForm(FlaskForm):
	    """管理员登录表单"""
	    account = StringField(
	        label="帐号",
	        validators=[
	            DataRequired("请输入帐号!")
	        ],
	        description="帐号",
	        render_kw={                #render_kw 额外的描述，要和html中表单中的一样，样式
	            "class":"form-control",
	            "placeholder":"请输入账号",
	            "required":"required"      #必须项，html会自动在前端判别
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

#### 表单和view

先导入，再实例化。。

	from app.admin.forms import LoginForm
	
	@admin.route("/login/")
	def login():
		form = LoginForm()
		return render_template("admin/login.html",form=form)  #把form传入里面


#### html模版中的展示

之前是这样的。`<input>`

	<input class="form-control" id="account" name="account" placeholder="请输入账号" required="required" type="text" value="">

现在使用表单

	{{ form.account }}


#### [CSRF保护](http://www.pythondoc.com/flask-wtf/csrf.html)

在配置文件中，写入 `SECRET_KEY`,

表单中 `{{ form.csrf_token }}`

#### 视图处理

在view中
	
	form = LoginForm()
	if form.validate_on_submit():  #表单验证
		data = form.data   #数据获取
	return render_template("admin/login/html")


错误消息在html中的显示。。


	{% for err in form.account.errors %}
	      <div class="col-md-12">
	       <font style="color:red">{{ err }}</font>
	        </div>
	 {% endfor %}


#### 帐号验证函数

对输入的帐号，如何进行验证，比如数据库中是否存在该帐号。

需要在表单`forms`中定义一个验证函数，这里会和数据库有交互。。

	from wtforms.validators import ValidationError
	from app.models import Admin  #导入数据库模块

	def validate_account(self,field):
		account = field.data  #表单获取的信息，获取帐号，，用户的名字。。
		admin = Admin.query.filter_by(name=account).count() #统计其中个数
		if admin == 0:
			raise ValidationError("帐号不存在") #ValidationError报错误异常



#### 密码验证函数

在`models`中Admin下面添加一个验证函数

    def check_pwd(self,pwd):
        from werkzeug.security import check_password_hash
        return check_password_hash(self.pwd,pwd)


#### 视图中，登录以及密码验证

	from flask import render_template, redirect, url_for, flash, request, session
	# 登录
	@admin.route("/login/", methods=["GET", "POST"])
	def login():
	    form = LoginForm()
	    if form.validate_on_submit():
	        data = form.data
	        admin = Admin.query.filter_by(name=data["account"]).first()
	        if not admin.check_pwd(data["pwd"]):
	            flash("密码错误！")
	            return redirect(url_for("admin.login"))
	        session["admin"] = data["account"]
	        return redirect(request.args.get("next") or url_for("admin.index"))
	    return render_template("admin/login.html", form=form)


实例化一个登录表单，判断提交是否正确，提取其中数据，进行判断验证。。第一个用户，然后判断密码是否正确，不正确返回错误，重定向到登录界面。如果正确的话，进行session会话保持。。

不理解的地方，在forms里的对用户的验证，account = field.data 。。。

#### 错误消息的显示

在前端的相应位置进行错误消息的显示。。


#### 装饰器

访问的限制

	from functools import wraps
	def admin_login_req(f):
    @wraps(f)
    def decorated_function(*args, **kwargs):
        if "admin" not in session:
            return redirect(url_for("admin.login", next=request.url))
        return f(*args, **kwargs)

    return decorated_function


装饰器写好之后，在一些视图前面加入访问控制。。

	@admin_login_req



