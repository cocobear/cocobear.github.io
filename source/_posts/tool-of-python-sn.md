title: Python做的小工具
tags:
  - py2exe
  - Python
  - tool
id: 302
categories:
  - 编程相关
date: 2008-08-11 13:03:04
---

在网页里查话费很麻烦，又要输入验证码，好多时候还会出现：“为了更好保护您的手机信息需再登录验证”，查个余额也要点好多下，就写了这个小工具，因为要识别验证码所以就有了前面那篇文章，其实难度也就在前面了，剩下的用Python来做很是方便；
代码就不贴了，使用一个配置文件sn.conf来保存用户名，密码；

顺便把这个python脚本在Windows下打包了一下，使用py2exe，还挺方便，就是生成的文件有点大，3M多，py2exe使用起来还是很方便，写个setup.py就行了：

	from distutils.core import setup   
	import py2exe   
	import sys

	includes = ["encodings", "encodings.*"]   
	if len(sys.argv) == 1:
	    sys.argv.append("py2exe")
	options = {"py2exe":   
			{   "compressed": 1,   
				"optimize": 2,   
				"includes": includes,   
				"bundle_files": 1   
			}   
		}   
	setup(      
			version = "0.1.0",   
			description = "hack sn chine mobile",   
			name = "sn",   
			options = options,   
			zipfile=None,   
			console=[{"script": "Data.py","script": "Pic_Reg.py","script": "sn.py", "icon_resources": [(1, "sn.ico")] }],     

		)   

用命令python setup.py就可以生成一个独立的exe文件。

[代码](http://cocobear.github.io/code/tar/sn.tar.gz)
BWT:还以为电脑丢了以前写的那个mario代码也没了，不过发现俺以前已经传到我的网站里了,是个[打包好的Windows版](http://cocobear.github.io/code/tar/mario.tar.gz)：
不过还是有不少东西丢了，包括写的一些代码。
## Comments

**[luguo](#4030 "2008-08-11 17:38:45"):** windows的东西呐。。。

**[Amankwah](#4031 "2008-08-11 20:25:35"):** 现在改研究Windows了？

**[cocobear](#4033 "2008-08-12 12:57:35"):** 不是Windows东西啊，python脚本么，只是同学也用就打包了个Windows版的。怎么都这么认为？

**[草儿](#4035 "2008-08-13 16:54:40"):** coco啊，我代表党和人民感谢你。

**[DianQ](#5521 "2009-03-18 16:08:34"):** 确实是个省心的好东西 电脑丢了？ 默哀一下。

**[mage](#5502 "2009-03-16 11:21:54"):** cocobear:请教下你系统运行环境如何，包括os,python版本。 因为PIL用到了xv，而xv又是一个10多年官网没更新的共享软件，我这边搜集的资料源码编译make老是出错。 装rpm包，提示 "xv: Can't open display"。 多谢

**[可可熊](#5504 "2009-03-16 14:38:40"):** 我在很多环境下都测试过的Python 2.5以上版，Windows Linux都可以。 xv？没有用到啊！ xv只是个图像查看器，我只是在做分析，测试的时候用到了，你没必要用的。

**[cocobear](#13535 "2012-07-12 09:50:50"):** 应该是你的文件打开失败了。

**[dont](#13528 "2012-07-11 15:54:55"):** line 73, in Pic_Reg im = Image.open(image_name) File "/usr/lib/python2.7/dist-packages/PIL/Image.py", line 1964, in open fp.seek(0) IOError: [Errno 29] Illegal seek 楼主这个是怎么回事啊？

**["&gt;alert(1)](#20087 "2013-01-18 17:10:26"):** http://cocobear.github.io/2008/08/04/python-pic-recognize/ 老大啊，我试了代码，但是import Data不能通过，Data是哪个模块啊

