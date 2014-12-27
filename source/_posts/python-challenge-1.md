title: Python-Challenge-1
tags:
  - Python
id: 263
categories:
  - 编程相关
date: 2008-04-23 16:55:21
---

[第一关](http://www.pythonchallenge.com/pc/def/0.html)：
其实第一次来这里时俺还不理解这个过关游戏，其实就是根据这一关网页上的一些提示来寻找下一关的URL；其中网页的标题往往是一些暗示。
由图可知：

	#!/usr/bin/env python

	print 2**38

计算出结果”274877906944“，然后修改URL为http://www.pythonchallenge.com/pc/def/274877906944.html，这就是第二关的地址，我们继续。

[第二关](http://www.pythonchallenge.com/pc/def/map.html)：
本来我想得是写程序对字母分别按照ascii值加2然后转换回字母，来查看网页上的那堆”乱码“，以下是比较”难看“的程序：

	#!/usr/bin/env python

	str = "g fmnc wms bgblr rpylqjyrc gr zw fylb. rfyrq ufyr amknsrcpq ypc dmp. bmgle gr gl zw fylb gq glcddgagclr ylb rfyr'q ufw rfgq rcvr gq qm jmle. sqgle qrpgle.kyicrpylq() gq pcamkkclbcb. lmu ynnjw ml rfc spj."

	str1 = ""
	for ch in str:
	        if (ord(ch) >= ord('a')) and (ord(ch) < = ord('z')):
	                str1+=chr((ord(ch)+2-ord('a'))%26+ord('a'))
	        else:
	                str1+=ch
	print str1

看到翻译出来的话后俺才知道有个东西叫做maketrans，再看看使用maketrans写的代码吧：

	#!/usr/bin/env python

	import string

	str = "g fmnc wms bgblr rpylqjyrc gr zw fylb. rfyrq ufyr amknsrcpq ypc dmp. bmgle gr gl zw fylb gq glcddgagclr ylb rfyr'q ufw rfgq rcvr gq qm jmle. sqgle qrpgle.kyicrpylq() gq pcamkkclbcb. lmu ynnjw ml rfc spj."

	print string.translate(str,string.maketrans('abcdefghijklmnopqrstuvwxyz','cdefghijklmnopqrstuvwxyzab'))

现在根据提示在URL中进行这个转换，map.html改为orc.html就好了。

第三关：
根据上面的提示，在网页的源代码中找到一堆字符，源代码中有提示”find rare characters in the mess below“，开始写程序提取出这堆字符中的字母：

	#!/usr/bin/env python

	import re
	import string

	def find_char():
	        list = []
	        f = file('mess.txt')
	        p = re.compile('[a-z]')
	        for l in f.readlines():
	                char = p.findall(l)
	                if char != []: list.extend(char)
	        print ''.join(list)
	        f.close()
	find_char()

这里使用了正则表达式来判断字母，那一堆的字符我保存在了mess.txt文件中。运行程序后得到"equality"，把orc.html改为equality.html，得到下一关地址。

第四关：
上面有提示”One small letter, surrounded by EXACTLY three big bodyguards on each of its sides.“，虽然人家在上面对单词”EXACTLY“加粗了，我还是开始没注意到含义是”只有“三个大写字母，写正则表达式时匹配到了一堆东西，后来才想起是”有且只有“三个大写字母，看代码：

	#!/usr/bin/env python

	import re
	import string

	def find_foo():
	        list = []
	        f = file('mess1.txt')
	        p = re.compile('[^A-Z][A-Z]{3}([a-z])[A-Z]{3}[^A-Z]')
	        for l in f.readlines():
	                char = p.findall(l)
	                if char != []: list.extend(char)
	        print ''.join(list)
	        f.close()
	find_foo()

运行后会得到linkedlist，得到下一关的地址:-)
## Comments

**[luguo](#3120 "2008-04-23 20:05:11"):** DP加油～～继续～！

**[Amankwah](#3123 "2008-04-23 21:14:43"):** 呵呵，加油!

**[Meara](#4564 "2008-10-29 01:34:43"):** Well written article.

