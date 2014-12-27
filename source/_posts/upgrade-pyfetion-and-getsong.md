title: "更新PyFetion && getsong"
tags:
  - getsong
  - PyFetion
  - Python
id: 585
categories:
  - Life
date: 2009-03-24 23:16:19
---

给PyFetion加入了HTTP代理的支持，HTTP发送方式中。TCP发送方式也可能会加上吧，使用[SocksiPy](http://socksipy.sourceforge.net/)这个库吧，有时间再说。

改PyFetion的时候发现urllib2的一个问题：
先看下面这段代码：


	import urllib2
	headers = [("Content-Type","application/oct-stream"),]
	opener = urllib2.build_opener()
	opener.addheaders = headers
	urllib2.install_opener(opener)
	print "after install_opener"

	ret = opener.open('http://www.dict.cn',data="word=ss")
	print ret.read()


抓包发现发送的内容为：

     POST / HTTP/1.1> 
     Accept-Encoding: identity> 
     Content-Length: 7> 
     Host: www.dict.cn> 
     Content-Type: application/x-www-form-urlencoded> 
     Connection: close> 
     
     word=ss

这里我在代码里已经指定了HTTP的header: Content-Type，但是发出去的时候却被改变了。

通过分析urllib2的代码，找到：


	if request.has_data():  # POST
	    data = request.get_data()
	    if not request.has_header('Content-type'):
	        request.add_unredirected_header(
	            'Content-type',
	            'application/x-www-form-urlencoded')
	    if not request.has_header('Content-length'):
	         request.add_unredirected_header(
	           'Content-length', '%d' % len(data))

	scheme, sel = splittype(request.get_selector())
	sel_host, sel_path = splithost(sel)
	if not request.has_header('Host'):
	    request.add_unredirected_header('Host', sel_host 
	    or host)
	    for name, value in self.parent.addheaders:
	    #这里的parent是opener对象
	    name = name.capitalize()
	    if not request.has_header(name):
	        request.add_unredirected_header(name, value)

urllib2发现如果是POST数据的话自己添加了Content-Type，接着才去追加opener对象中的headers，这时已经有Content-Type了，所以opener对象增加的Content-Type就无效了。

解决办法是创建request对象，在request对象中设置Content-Type：

 	request = urllib2.Request(url,headers=headers,data=body)

不知道是上面的示例代码写法不规范呢？还是算Python的一个小问题呢？ 

最近发现周董的不少歌挺好听，于是找个脚本来下载周董的歌（从百度mp3中），[ getsong](http://code.google.com/p/getsong)就进入了眼的视野，不过不支持下载某个歌手的全部歌曲，[俺自己加上去](http://code.google.com/p/getsong/source/list)，正在下载周董的歌：
     [cocobear@cocobear getsong]$ ./getsong.py -a 周杰伦> 
     正在下载第1首（共104首) 歌手：周杰伦 曲名：她的睫毛> 
     已经成功下载《周杰伦 - 她的睫毛》> 
     .......

不过这会儿断了，getsong好像又有bug了，今天就不折腾了，明天再整整吧，看书，睡觉了。
## Comments

**[cocobear](#5605 "2009-03-27 14:59:50"):** UI界面是指getsong还是PyFetion呢? 蟒蛇有毒,小心中毒!

**[Sutra](#5590 "2009-03-25 10:40:37"):** 不知道删除好友功能是不是必须的。因为听说有个好友人数上限问题。

**[kongove](#5878 "2009-05-09 11:04:58"):** 这个getsong很好用～我和张斌正在读源码～

**[cocobear](#5592 "2009-03-25 12:56:48"):** 这个和发短信没什么关系啊,你别加那么多人就行了啊.

**[xia_chip](#5601 "2009-03-26 23:08:35"):** 要是做个UI界面就更加爽了，最近也在研究蟒蛇！

**[草儿](#5586 "2009-03-25 08:36:37"):** 靠，现在你真勤快。

**[admin](#6237 "2009-07-14 11:46:48"):** 请问最新版的PyFetion在哪里下载呢？

**[cocobear](#6239 "2009-07-14 14:19:05"):** code.google.com/p/pytool/

