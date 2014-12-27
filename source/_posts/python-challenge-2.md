title: Python-Challenge-2
tags:
  - Python
id: 264
categories:
  - 编程相关
date: 2008-04-24 20:31:26
---

第五关：
不多说什么了，点点图片就可以发现了，直接贴代码上来：

	#!/usr/bin/env python

	from urllib import urlopen
	import re

	id = 12345

	while id:
	        url = "http://www.pythonchallenge.com/pc/def/linkedlist.php?nothing=%s" % id
	        page = urlopen(url)
	        text = page.read()
	        print text
	        id = ''.join(re.findall('and the next nothing is ([0-9].*)',text))
	        print id

中间停了一下，出来个提示“Yes. Divide by two and keep going.”，刚开始还以为是分成两组，其实是除以2然后继续，那就手动把上面的id初始化为92118/2=46059。再运行一次程序就得到下一关的地址：peak.html。

第六关：
网页的源码中有一个提示“peak hell sounds familiar ”，我读了半天也没觉得熟悉，后来作了个弊，查了一下Python的函数，发现了pickle这个函数，peak,hell连读一下就是pickle了，这都可以。把网页改为pickle.html，会提示“yes! pickle!”，看来函数是找到了。然后在源代码中找到banner.p，获得这个文件里的内容，看起来乱七八糟的一些东西，其实就是使用pickle生成对象的一个序列，可以直接保存在文件里，关于pickle[这里](http://wiki.renyangok.cn/python-pickle)有一个简单的介绍，我也是现学的，贴代码：

	#!/usr/bin/env python

	import pickle
	import sys

	f = file('banner.p')
	p = pickle.load(f)

	print p
	for a in p:
	        for b in a:
	                x,y = b
	                while y:
	                        sys.stdout.write(x)
	                        y=y-1
	        print
	f.close()

从文件中会得到一个很长的list，仔细看看又是由好多个list组成，而这些list中数字之和都为95，根据banner.p这个文件名可以想到这是个字符图片，在上面的程序中我打印出了这个图片（有一点需要注意，你得把终端可显示的字符设置为至少95,不然会提前自动折行，这样就没办法看到这个banner了）：
得到下一关地址：channel.html