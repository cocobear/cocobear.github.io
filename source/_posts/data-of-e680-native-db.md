title: "读取E680(i,g)的native.db文件"
tags:
  - bsddb
  - E680
  - Python
id: 462
categories:
  - Life
date: 2009-01-13 16:51:35
---

E680手机使用了[Berkeley DB](http://www.oracle.com/technology/documentation/berkeley-db/db/index.html)，关于这个数据库的一些信息可以看我给的链接。

很早以前就有打算要写个Python读取native.db（不知道这个的请[G](http://www.google.com)之）中联系人，短信的程序，不过没能成功，这两天又接触到了[bsddb](http://docs.python.org/library/bsddb.html)，于是今天分析了下这个native.db，没想到原来挺简单的。Berkeley DB本来就是一个很简单的数据库，只是在E680中系统把多个数据库同时存放在了一个文件中native.db，所以不可以直接使用bsddb.btopen来直接打开这个文件。由于Berkeley DB中只是一种类似Python中dict的数据库（key=value）所以要存放手机联系人（有很多字段，如地址，电话...），就需要使用多个数据库，然后每个数据库使用索引来与其它数据库建立联系。按照一般的想法应该是多个数据库的key是作为索引，然后value分别存放各种信息，但E680中的native.db恰好相反。

其实这样的结构也是很简单的，但是我在写代码的时候遇到一个问题，从某个数据库中读取一个key，然后使用bsddb中的get方法去取这个key对应的value，结果确是None，以前好像就是卡在了这一块，似乎与这个value的值有关系，这个value是二进制数据：“< x03x00x00”，不理解为什么这里不直接用整数作为索引而用这么奇怪的数据，或者这里是某种编码？

先不理这个，不管这个value是什么，使用sorted把每个数据库按照其value排序,下面是所有代码:

读取E680联系人:


	#!/usr/bin/env python
	# -*- coding=utf-8 -*-
	#Using GPL v2
	#Author: cocobear.cn@gmail.com

	"""
	获取数据库名:
	db = bsddb.db.DB()
	db.open('native.db',bsddb.db.DB_UNKNOWN,bsddb.db.DB_RDONLY)
	db.keys()
	"""

	import bsddb

	db_file = 'native.db'    
	databases = [('姓名','780_contact_table.first name'),('手机1','780_contact_table.mobile1'),('手机2','780_contact_table.mobile2'),('家庭1','780_contact_table.home1'),('家庭2','780_contact_table.home2'),('工作1','780_contact_table.work1'),('工作1','780_contact_table.work1'),]
	databases = [('姓名','780_contact_table.first name'),('手机1','780_contact_table.mobile1')]
	out = []

	for database in databases:
	    #进行单文件多数据库操作 需要每次新建一个环境
	    #因此下面这行在for循环里
	    db =bsddb.db.DB()
	    db.open(db_file,database[1],bsddb.db.DB_UNKNOWN,bsddb.db.DB_RDONLY)
	    #每次读一个数据库
	    #原本数据库中的value是系统的id key是具体有意义的内容
	    #下面这行把db.items()得到的list进行反转 为以后有序输出
	    out.append(sorted(db.items(),key=lambda (k,v): (v,k)))
	    db.close()

	#格式化输出
	for i in databases:
	    print i[0]+":",
	print

	for i in range(len(out[0])):
	    for j in range(len(out)):
	        #如果输出内容属于电话号码需要反转
	        #根据databases中各位置含义来来判断
	        if j in [1,2,3,4,5,6]:
	            print out[j][i][0].decode('utf16')[::-1],
	        else:
	            #非ascii字符使用utf16进行解码
	            print out[j][i][0].decode('utf16'),
	    print

不到50行就搞定了:-)
## Comments

**[草儿](#4855 "2009-01-14 14:23:14"):** 楼上在那个那个什么啊，那个了半天还不知道是啥呢…… coco，啥时候咱换HTC的吧~

**[1069](#4857 "2009-01-15 00:44:53"):** 我开始用HTC的了，wm的用起来顺手，呵呵。

**[Amankwah](#4849 "2009-01-13 23:08:24"):** 我还是不知道～发现智能手机，目前还是Symbian最哪个啥，接下来是WM，Linux还不够哪个啥啊～还需要努力啊～

**[wind](#4848 "2009-01-13 18:15:34"):** 我貌似还没用写过一个和我的手机有关的手机程序，呵呵

