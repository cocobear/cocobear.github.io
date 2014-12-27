title: 一个shell脚本
id: 72
categories:
  - 编程相关
date: 2007-04-17 15:09:42
tags:
---

在Fedora core 5下测试成功，目前的脚本只是针对XXXX的帐号，稍做修改可以试用于各种ADSL拨号。使用需要root仅限，请先修改/etc/syslog.conf文件，在最后添加：
	# cocobear add
	loca12.*                                                /var/log/ppp.bug
	daemon.*                                                /var/log/ppp.bug

本程序仅供学习研究之用，对于其它非法使用造成的后果本作者均不承担任何责任！

[查看代码](http://cocobear.github.io/code/html/ppp.html "html")
[下载代码](http://cocobear.github.io/code/pppold.sh "code")
## Comments

**[cocobear](#15 "2007-05-12 09:58:04"):** update1:已经更新，新版本可以在下面的地址查看 [ADSL脚本更新](http://www.cocobear.cn/blog/?p=98)

**[草儿](#16 "2007-04-18 16:41:33"):** 不错 终于把他搞出来了 庆祝一下

