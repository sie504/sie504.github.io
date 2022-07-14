---
title: Web安全资料汇集
date: 2018-04-11 18:13:23
tags:
		- 
categories:
		- Web安全资料汇集
---

收集网上分享的技术文章，对不同的研究内容进行汇集整理。持续更新。

<!-- more -->

#### 他山之石

[优质 安全文章 收藏  ](https://github.com/tom0li/collection-document)

[Web-Security-Learning](https://github.com/CHYbeta/Web-Security-Learning)



#### 逻辑漏洞资料


逻辑漏洞是指由于程序逻辑不严或逻辑复杂，导致一些逻辑分支不能够正常处理或处理错误。

1. 任意密码修改
2. 越权访问
3. 密码找回
4. 支付金额
5. 等...

[Web安全 -- 逻辑漏洞讲解](https://xz.aliyun.com/t/2011)

[分享一个近期遇到的逻辑漏洞案例](http://www.freebuf.com/vuls/151196.html)

[逻辑漏洞小结](https://tom0li.github.io/2017/07/17/%E9%80%BB%E8%BE%91%E6%BC%8F%E6%B4%9E%E5%B0%8F%E7%BB%93/)

[我的越权之道](http://cb.drops.wiki/drops/tips-727.html)

[逻辑漏洞之密码重置](https://mp.weixin.qq.com/s/Lynmqd_ieEoNJ3mmyv9eQQ)

[逻辑漏洞之支付漏洞](https://mp.weixin.qq.com/s/w22omfxO8vU6XzixXWmBxg)

[密码找回逻辑漏洞总结](http://cb.drops.wiki/drops/web-5048.html)

[代码审计之逻辑上传漏洞挖掘](http://cb.drops.wiki/drops/papers-1957.html)

[应用程序逻辑错误总结](http://cb.drops.wiki/drops/papers-1418.html)

[在线支付逻辑漏洞总结](http://cb.drops.wiki/drops/papers-345.html)

[Web安全测试中常见逻辑漏洞解析（实战篇）](http://www.freebuf.com/vuls/112339.html)

[密码逻辑漏洞小总结](http://www.mottoin.com/87224.html)

[逻辑漏洞](http://wyb0.com/posts/logical-loophole/)

[十大漏洞之逻辑漏洞](http://lyx.dropsec.xyz/2016/04/18/%E5%8D%81%E5%A4%A7%E6%BC%8F%E6%B4%9E%E4%B9%8B%E9%80%BB%E8%BE%91%E6%BC%8F%E6%B4%9E/)


#### PHP 反序列化

漏洞成因是程序没有对用户输入的反序列化字符串进行检测，导致反序列化过程的数据被恶意控制，造成代码执行等一些漏洞。

[PHP里面的魔术方法。](https://secure.php.net/manual/zh/language.oop5.magic.php)

PHP 将所有以 `__`（两个下划线）开头的类方法保留为魔术方法。

	__construct() 方法在每次创建新对象时会被自动调用
	
	__destruct() 方法在使用 exit() 终止脚本运行时也会被自动调用
	
	__toString() 方法在一个类被当成字符串时应怎样回应。例如 echo $obj; 应该显示些什么。

可控点，魔术方法的里面的具体操作。序列化`payload`，反序列化执行。




[最通俗易懂的PHP反序列化分析](http://www.notyeat.com/2018/03/26/php-unserialize/)

[浅谈php反序列化漏洞](https://chybeta.github.io/2017/06/17/%E6%B5%85%E8%B0%88php%E5%8F%8D%E5%BA%8F%E5%88%97%E5%8C%96%E6%BC%8F%E6%B4%9E/)

[PHP反序列化漏洞](https://www.anquanke.com/post/id/86452)

[神奇的php反序列化](https://www.anquanke.com/post/id/84705)

[PHP反序列化漏洞成因及漏洞挖掘技巧与案例](https://www.anquanke.com/post/id/84922)

[SugarCRM v6.5.23 PHP反序列化对象注入漏洞分析](https://paper.seebug.org/39/)


[PHP反序列化漏洞详细教程及实例（上）](https://zhuanlan.zhihu.com/p/32553774)

[PHP反序列化漏洞详细教程及实例（下） Typecho 反序列化漏洞分析](https://zhuanlan.zhihu.com/p/32601669)

[由Typecho 深入理解PHP反序列化漏洞](https://zhuanlan.zhihu.com/p/33426188)

[ROP 面向返回编程](https://zh.wikipedia.org/wiki/%E8%BF%94%E5%9B%9E%E5%AF%BC%E5%90%91%E7%BC%96%E7%A8%8B)

[ROP系统攻击思想](http://spd.dropsec.xyz/2016/12/02/ROP%E7%B3%BB%E7%BB%9F%E6%94%BB%E5%87%BB%E6%80%9D%E6%83%B3/)

[PPT-Utilizing Code Reuse/ROP in PHP Application Exploits - owasp](https://www.owasp.org/images/9/9e/Utilizing-Code-Reuse-Or-Return-Oriented-Programming-In-PHP-Application-Exploits.pdf)

[PPT--Practical PHP Object Injection - Insomnia Security](https://www.insomniasec.com/downloads/publications/Practical%20PHP%20Object%20Injection.pdf)


#### Java反序列化

[Java 反序列化原理及漏洞利用](https://zhuanlan.zhihu.com/p/30045174)

[Java反序列化漏洞研究](https://www.cnblogs.com/alert123/p/5124637.html)

[Java反序列化漏洞学习实践一：从Serializbale接口开始，先弹个计算器](http://www.polaris-lab.com/index.php/archives/447/)

[浅谈Java反序列化漏洞修复方案.md](https://github.com/Cryin/Paper/blob/master/%E6%B5%85%E8%B0%88Java%E5%8F%8D%E5%BA%8F%E5%88%97%E5%8C%96%E6%BC%8F%E6%B4%9E%E4%BF%AE%E5%A4%8D%E6%96%B9%E6%A1%88.md)


[java反序列化工具ysoserial分析](http://drops.xmd5.com/static/drops/papers-14317.html)

[深入分析Java的序列化与反序列化](http://www.hollischuang.com/archives/1140)

[Java 序列化](http://www.runoob.com/java/java-serialization.html)

[Java对象的序列化和反序列化](http://www.cnblogs.com/xdp-gacl/p/3777987.html)

[Java反序列化漏洞从入门到深入](https://xz.aliyun.com/t/2041)

#### 代码执行

[php代码/命令执行漏洞](https://chybeta.github.io/2017/08/08/php%E4%BB%A3%E7%A0%81-%E5%91%BD%E4%BB%A4%E6%89%A7%E8%A1%8C%E6%BC%8F%E6%B4%9E/)


#### 一句话和菜刀

[Kali下常见webshell管理工具汇总](https://www.ohlinge.cn/kali/kali_webshell.html)

[Weevely（php菜刀）工具使用详解](https://blog.csdn.net/SmalOSnail/article/details/53010483)

[中国菜刀20160620初体验](https://www.secpulse.com/archives/47940.html)

[如何编写webshell管理工具菜刀](https://zhuanlan.zhihu.com/p/27187537)

[Webshell进化史与中国菜刀](https://www.secfree.com/article-149.html)

[如何全面防御Webshell？](https://zhuanlan.zhihu.com/p/24722552)

[php webshell分析和绕过waf技巧](https://www.anquanke.com/post/id/85083)

[一类混淆变形的Webshell分析](https://www.anquanke.com/post/id/98889)

[Webshell安全检测篇](http://cb.drops.wiki/drops/papers-10807.html)

[Webshell-Part1&Part2](http://cb.drops.wiki/drops/papers-12598.html)

[基于语义分析的Webshell检测技术研究](http://ris.sic.gov.cn/CN/abstract/abstract221.shtml)

[webshell检测－日志分析](http://tanjiti.lofter.com/post/1cc6c85b_10c4e356)

[php一句话学习](https://bbs.ichunqiu.com/thread-26365-1-1.html?from=sec)

[Secwiki——Webshell](https://www.sec-wiki.com/news/search?wd=webshell)

[那些强悍的PHP一句话后门](https://www.uedbox.com/good-php-door/)

[浅析一句话后门中eval与assert执行条件与原理](http://www.vuln.cn/8395)


[php一句话后门的几种变形分析[preg_replace函数]](http://www.vuln.cn/8314)

[php代码审计之preg_replace引发的phpmyadmin(4.3.0-4.6.2)命令执行漏洞](http://blog.topsec.com.cn/ad_lab/audit-defanse-2/)

[打造自己的中国菜刀](https://www.t00ls.net/articles-40874.html)

[菜刀工作原理分析](https://www.t00ls.net/viewthread.php?tid=28388)

[菜刀（Chopper）的通信协议分析](https://www.t00ls.net/thread-25587-1-1.html)

#### 安全会议PPT

[Black Hat USA 2017 议题PPT下载(部分)](https://www.blackhat.com/us-17/briefings.html)

