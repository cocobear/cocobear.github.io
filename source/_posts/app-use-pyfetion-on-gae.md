title: GAE上使用PyFetion做一个应用
tags:
  - gae
  - PyFetion
  - Python
id: 759
categories:
  - 编程相关
date: 2009-12-15 14:22:14
---

GAE上有一个cron jobs的功能，类似于Linux下的crontab，可以实现在指定规则的时间里去运行程序。利用这个功能和飞信的短信功能就完成一些简单的小应用，比如天气预报，服务器监控，股票大盘实时行情提醒等等。

拿服务器监控来说，我们先注册一个GAE账户，然后创建一个Application，下载一份GAE 的SDK，接下来就写代码了。
(以上过程请自行google之)

monitor.py:

	#!/usr/bin/env python
	# -*- coding: utf-8 -*-
	#MIT License
	#By : cocobear.cn@gmail.com
	#

	from google.appengine.api import urlfetch
	from PyFetion import *

	def get():
	    print('<html><body>')
	    url = 'http://61.236.244.162'
	    result = urlfetch.fetch(url)
	    if result.status_code == 200:
	        print('OK')
	    else:
	        fetion = PyFetion('136xxxx','123456','HTTP')
	        i = 0
	        while True:
	            try:
	                fetion.login(FetionOnline)
	                fetion.send_sms('服务器掉了!')
	                fetion.logout()
	            except:
	                if i > 5:
	                    break
	                continue
	    print('</body></html>')

	if __name__ == '__main__':
	    get()


很简单的代码，使用GAE的函数urlfetch.fetch来访问这个地址，如果失败调用PyFetion来发短信给自己做通知。

app.yaml:

	application: pythoning
	version: 1
	runtime: python
	api_version: 1

	handlers:
	- url: /console/.*
	  script: $PYTHON_LIB/google/appengine/ext/admin
	  login: admin

	- url: /monitor
	  script: monitor.py

	- url: /
	  static_files: index.html
	  upload: index.html


这里指定url映射的规则，访问pythoning.appspot.com/monitor就会执行monitor.py脚本。application需要填写你申请GAE应用时的名称，也就是这个二级域名pythoning，version：当前代码的版本。

index.html:

首页你可以做别的事，比如放个人主页，然后把自己域名指过来。


cron.yaml:

	cron:
	- description: watch http server 
	  url: /monitor
	  schedule: every 1 minutes
	  timezone: Asia/Shanghai


关键的一个文件，对monitor这个url进行定时执行，这里是每分钟执行一次; 如果你是写天气预报，就可以写为 7:00 every day，每天早上7点发一次天气预报。

写完这些以后，运行命令:
ls demos/test
app.yaml cron.yaml index.html monitor.py PyFetion.py
python appcfg.py update demos/test
把你的代码提交到GAE服务器上，你可以在GAE的后台Cron Jobs看到：

     /monitor> 
     watch http server	 every 1 minutes (Asia/Shanghai) > 
     2009/12/15 14:06:50 on time Success

在GAE上使用r49版本的PyFetion，请注释掉from select import select,如果只是像上面做提醒的话最后去掉下面的代码(824行):

	self.get_personal_info()
	if not self.get_contactlist():
	    d_print("get contactlist error")
	    return False
	self.get("compactlist",self.__uri,self.contactlist.keys())
	response = self.send()
	code = self.get_code(response)
	if code != 200:
	    return False
	#self.get("PGGetGroupList",self.__uri)
	#response = self.send()

	self.get_offline_msg()


这里是获得了好友列表，然后给这些好友发一个上线的通知，如果你不想每次提醒的时飞信上的好友都看到你上线，那么就去掉这些，也会加快发送短信的速度。更进一步的优化，你可以去掉TCP通信的相关代码。

ok, 发挥你的想象力，play with GAE and PyFetion
## Comments

**[flyingzl](#7262 "2010-02-02 22:59:13"):** 这页面背景可刺激死我的眼睛，呵呵。。

**[oo](#6917 "2009-12-23 22:00:37"):** gae 又发不了飞信啦，每次都错误！！

**[bones7456](#6863 "2009-12-15 14:32:18"):** 呵呵，这个挺好玩的。

**[草儿](#6864 "2009-12-15 14:34:52"):** 搞的不错，越来越有意思了

**[xbwee](#7787 "2010-04-11 21:46:05"):** 最近在GAE上使用报错，module object has no attribute socket PyFetion.py line 464 google之，一些人说可能是当前目录下有同名的 ‘socket’文件导致出错，我确定上传的目录中没有这个文件。错误只能是 GAE 的 Python 运行环境了，这麻烦了~~

**[david](#6865 "2009-12-15 16:57:23"):** 正是我想要的

**[小卒](#6867 "2009-12-15 19:02:53"):** 最近1周多，在GAE上的get_offline_msg()不能用了，是飞信协议修改了吗？我的一个应用，如果那样我的一个应用就废了，可可熊有办法吗？

**[可可熊](#6868 "2009-12-15 19:30:50"):** 嗯，HTTP协议不能那样获得消息了，因为本来就没有官方走HTTP方式的飞信，所以我也没有什么办法。以前那样也是一种凑合的方法。

**[damon](#6873 "2009-12-15 22:30:07"):** 哈哈··· 不错 终于发出来了··

**[evlos](#6876 "2009-12-15 22:46:56"):** 帅呆了 ~ ( ⊙o⊙ )

**[Solrex](#6877 "2009-12-15 23:19:53"):** Great work!

**[flourishing](#7738 "2010-04-02 16:52:09"):** 能支持http proxy 吗

**[flourishing](#7739 "2010-04-02 16:52:37"):** 建议调一下样式，几乎都看不见文章的标题。

**[cocobear](#8009 "2010-05-04 15:37:26"):** 405的错误是只有在GAE上面使用才会出现，应该是移动做的针对IP的限制，405一般只能重试。

**[cocobear](#7794 "2010-04-12 13:29:51"):** 注释掉socket那行 在GAE上使用HTTP方式 用不到socket模块

**[sinosure](#7998 "2010-05-03 13:37:25"):** 谢谢指导，搞定了，但好像不稳定，有时候报错： (405, 'Http error') Traceback (most recent call last): File "/base/python_runtime/python_lib/versions/1/google/appengine/ext/webapp/__init__.py", line 513, in __call__ handler.post(*groups) File "/base/data/home/apps/forumfly2009/1.341680310453677646/helloworld.py", line 513, in post fetion.login(FetionOnline) File "/base/data/home/apps/forumfly2009/1.341680310453677646/PyFetion.py", line 829, in login response = self.send() File "/base/data/home/apps/forumfly2009/1.341680310453677646/PyFetion.py", line 400, in send response = self.__sendSIPP() File "/base/data/home/apps/forumfly2009/1.341680310453677646/PyFetion.py", line 453, in __sendSIPP raise PyFetionSocketError(405,'Http error') PyFetionSocketError: (405, 'Http error') I 05-02 10:26PM 23.591 This request caused a new process to be started for your application, and thus caused your application code to be loaded for the first time. This request may thus take longer and use more CPU than a typical request for your application.

**[sinosure](#7973 "2010-04-29 11:29:08"):** GAE上也遇到socket问题了，但不知道怎么调整，怎么调整构造函数，能否详细说一下，或者给出一份调整后的版本呢，谢谢了

**[可可熊](#7979 "2010-04-29 21:42:37"):** To: sinosure 直接注释掉import socket，然后使用HTTP模式。

**[xbwee](#7801 "2010-04-14 00:16:23"):** 哈哈，多谢提醒！PyFetion 类默认连接方式是TCP，只要在构造函数中指定为HTTP就可以了

**[Yao Wei](#8141 "2010-05-26 13:36:35"):** 我在本机上测试是好的，但是一部署到GAE上，立马就有错误，一开始是urllib2的问题，后来我试了google.appengine.api的 urlfetch还是有问题 不知道有什么解决方法没有。 njustyw@sina.com

**[文](#8285 "2010-07-06 20:53:41"):** 可否提供源码？在PHP上的可以收到但有乱码出现

**[cocobear](#8286 "2010-07-07 09:54:44"):** 源码都贴出来了 :-(

**[oppih](#8255 "2010-06-28 14:57:36"):** 你好，我想用PyFetion做一个自动处理接收到的飞信内容的问题，想向你请教怎么做方便，不知道是否有空帮助研究研究？

**[cocobear](#9006 "2010-11-29 09:17:27"):** 现在可以用 更新协议了

**[小江恩](#9001 "2010-11-28 21:48:02"):** 请问现在还能用吗？不是飞信换了协议吗？

