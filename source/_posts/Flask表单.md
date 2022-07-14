---
title: Flask 表单
date: 2017-09-04 15:54:24
tags:
		- Flask
		 
categories:
		- Web开发
---

`Flask`表单

<!-- more -->


[Flask表单：表单数据的验证与处理](https://zhuanlan.zhihu.com/p/23605845)

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

首先html中的`input`可以去掉。。用什么替换。。 

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

在前端的`input`进行修改，进行表单的实现。

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


* 唯一性的确认


		tag = Tag.query.filter_by(name=data["name"]).count() #数量的统计
		        if tag == 1:
		            flash("名称已经存在")
		            return redirect(url_for('admin.tag_add'))


*  [flash 消息闪现](http://docs.jinkan.org/docs/flask/patterns/flashing.html)

 闪烁，对结果的一个提示，这个也会在前端显示出来。。`get_flashed_messages()`


	 {% for msg in get_flashed_messages() %}
	 {% endfor %}      


* 入库


		tag = Tag(
	            name=data["name"]
	        )
	        db.session.add(tag)
	        db.session.commit()
	        flash("添加标签成功！", "ok")
	        return redirect(url_for('admin.tag_add'))


#### 表单，提取数据，判断，前端错误消息显示、入库

这几个基本的操作步骤要理解了，这样碰见其他的类似操作，流程化就可以来了。。

1：根据前端具体的表单，定义表单类。。其中的`name、label、validators、description、render_kw（样式内容）`

2：在views对应的视图导入表单类，实例化，在前端进行替换，使用定义的表单生成相应的input，csrf的防御。

3:进行数据的验证，是否重复等一些列逻辑的处理

4：入库

5：后端处理，在前端的错误信息的提示，时刻存在的。


#### 表单中的数据类型


	__all__ = (
	    'BooleanField', 'DecimalField', 'DateField', 'DateTimeField', 'FieldList',
	    'FloatField', 'FormField', 'IntegerField', 'RadioField', 'SelectField',
	    'SelectMultipleField', 'StringField',
	)


比如上传文件类型 

#### 0823 获取数据

WTForms提供的静态方法.data返回一个以字段名(filed name)和字段值(field value)作为键值对的字典。。

`DataRequired` 验证器只是简单地检查相应域提交的数据是否是空

#### 0903

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-9-3/82082288.jpg)

#### 接收表单数据

`validate_on_submit `

#### 数据库的设置  0904

	'primary_key'
	A_I Auto_increment 自动增加


删除很好的，只要把请求的id在数据库中查找进行删除，。。

修改就有一定一定的问题了。。。


	def auth_edit(id=None):
	    form = AuthForm()
	    auth = Auth.query.get_or_404(id)
	    if form.validate_on_submit():
	        data = form.data
	        auth.url = data["url"]
	        auth.name = data["name"]
	        db.session.add(auth)
	        db.session.commit()
	        flash("修改权限成功!", "ok")
	        redirect(url_for('admin.auth_edit',id=id))
	    return render_template("admin/auth_edit.html",form=form,auth=auth)  #将auth传递到前端，是展示的时候有提示


修改也会有设计到表单的处理，那就处理一下。接收表单，对其中修改的id，进行查找，获取其中的数据，将原始数据传递到前端，需要对其中的修改的数据进行接收并重新的存储提交到数据库。


#### 


表单中涉及多选框的时候，用那一个字段 SelectMultipleField

一个错误，html表单里面的一个错误。。

	<form role="form" method="post">  元素之间没有逗号的。。。浪费了很多时间


多选框的在表单的定义，以及在后端的取出和前端的展示。。
