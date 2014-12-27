title: 历时5天的php+mysql留言板完成
id: 165
categories:
  - 编程相关
date: 2007-07-31 23:53:23
tags:
---

先做个介绍，开发环境是apache2.2.4+php5.2.2+F7+vim7.0+opera9.1，代码统计：
	[cocobear@cocobear guest]$ wc *.php config/*.php
	   98   219  2947 admin.php
	  202   548  6099 index.php
	   83   243  3908 install.php
	  122   278  3737 write.php
	   34    63   823 config/config-sample.php
	    8    26   429 config/footer.php
	  178   283  2696 config/header.php
	  725  1660 20639 总计

一共725行，不是很多，不过功能还算完善的，主要特点有：

*   可以实现从网页安装
*   支持管理员回复留言
*   支持管理员删除留言
*   分页功能完整，可自定义每页显示留言数
*   界面简洁.在Opera9.1,Firefox2.0下显示基本完全一致

主要的功能全部实现了，可以满足一般用户的需求，不过还是有很多地方需要完善的，比如对留言内容的检查，简单的检查打算用JS完成，以后可能会加上关键词过滤，IP过滤以防止垃圾留言。为了使界面在Opera与Firefox全都能正常显示，花了一整天的时间，浏览器之间的差异真让人郁闷，IE还没去试，不管它了。

大家可以在这里试用[可可熊的留言板](http://cocobear.github.io/guest/)
[源代码下载](http://cocobear.github.io/code/tar/guest.tar.gz)

原始的用户名为:cocobear  密码:ffffff
如果需要修改用户名在config.php(安装完成后)或者config-sample.php(未安装时)，修改相应的admin_name值，修改密码请在admin_pass后输入你想使用的密码的md5值，可以在[这里](http://www.cmd5.com/)计算md5值。
## Comments

**[windflush](#1380 "2007-08-04 19:31:17"):** 表示一下支持。

**[kongove](#1489 "2007-08-22 12:42:32"):** 那个不能下载呀.

**[cocobear](#1521 "2007-08-27 11:16:52"):** 已经修正了下载地址，感谢kongove.

