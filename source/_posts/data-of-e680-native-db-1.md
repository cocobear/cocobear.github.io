title: "读取E680(i,g)的native.db文件-续"
tags:
  - E680
  - Python
id: 485
categories:
  - 编程相关
date: 2009-02-01 10:49:45
---

上次[写到读E680联系人](http://c.kensou.me/blog/2009/01/13/data-of-e680-native-db/)，后来又增加了读短信，并且做了一些代码上的修改，代码放在了google code里，使用的方法是需要读数据库（native.db）中的什么内容就在databases这个list中添加相应的数据库名，获取数据库名的方法也在代码的首部给出了。

[地址](http://code.google.com/p/pytool/source/browse/trunk/read_e680_db.py)

PyFetion也放在了这里，以后Python的脚本就统一用google code管理，这样就方便我在多台电脑修改代码了。

前几天看到一篇文章比较SQLite与Bdb的文章，提到了Bdb中数据存放是使用的原始数据，所以在读到短信中的日期时需要使用struct.unpack来进行处理。

本来以为Bdb也应该是代码很小七的，类似与SQLite，没想到竟然有十多兆的代码:-(
[可可熊的Python仓库：](http://code.google.com/p/pytool)
http://code.google.com/p/pytool

-------------------------google code 中svn使用-------------------------------------
 svn checkout https://pytool.googlecode.com/svn/trunk/ pytool --username cocobear.cn
 svn status
?      cang.py
 svn add cang.py 
A         cang.py
 svn ci -m"add cang.py"
## Comments

**[草儿](#4992 "2009-02-03 16:10:31"):** 里面内容真空，好歹你把以前写的说明文字也一块儿cp过去啊。

**[dcy](#6067 "2009-06-08 01:06:14"):** 哈哈，我的也是E680

**[dcy](#6153 "2009-06-19 00:36:47"):** 支持大牛写个chm的横屏阅读器，网上只看到竖屏的……

**[dcy](#6154 "2009-06-19 00:38:44"):** 忘了说是E680的chm横屏阅读器……

