title: PHP调试方式
id: 152
categories:
  - 编程相关
date: 2007-07-01 20:17:46
tags:
---

这两天在写以前那个PHP五子棋的人工智能部分，由于PHP是解释执行的语言调试起来不太方便，而且在默认的情况下出错信息是不提示的，这个大概是为了方便用户的体验，毕竟一个真正运作的网站要是出现一些PHP错误信息是不安全，而且不美观。如果是在写代码的时候就得把错误警告打开，可以有以下两种方式：

*   修改php.ini文件（Linux下位于：/etc/php.ini）

	display_errors = On
	error_reporting = E_ALL & ~E_NOTICE

把display_errors,与error_reporting修改为上面所示，有关这两个变量的详细解释可以参考php.ini文件中的注释。*   修改httpd.conf文件（Linux一般位于：/etc/httpd/conf/httpd.conf）

这是最基本的调试方式，如果你想更进一步了解php程序的调试，可以参考下面这篇文章：

[PHP程序员的调试技术](http://www.ibm.com/developerworks/cn/opensource/os-debug/)
## Comments

**[Name](#886 "2007-07-01 21:51:42"):** 说实话，我个人感觉，所有的Web编程都很讨厌，而php是其中最好的。;-)

**[cocobear](#887 "2007-07-02 00:30:16"):** 呵呵，同意！

**[Name](#3185 "2008-05-16 19:52:44"):** Ϲ

**[Cherry](#6735 "2009-11-21 18:52:43"):** 你掌握的东西好多啊，要好好的向你学习了，没有经过你的允许，我先加你的链接了，可能会经常过来学习的

