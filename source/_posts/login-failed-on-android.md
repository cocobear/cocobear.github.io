title: Android系统GOOGLE账号无法登录
tags:
  - android
  - google
id: 861
categories:
  - Life
date: 2010-12-14 10:32:22
---

Android 系统上google账号无法登录，解决办法：
* 手机上安装R.E.管理器.
* 创建一个hosts文件写入内容：


     127.0.0.1		    localhost> 
     74.125.93.113 android.clients.google.com
这个IP如果不能用了，可以使用nslookup查找一个IP替换

* 他用R.E.用这个hosts文件替换掉/etc/hosts
## Comments

**[Amankwah](#9097 "2010-12-14 22:14:38"):** 貌似昨天是，Andorid自带的电子市场不能上，我的帐号在手机上登录是正常的～

**[小杨](#9717 "2011-03-12 20:27:23"):** 手机上登陆争产，市场不能上很正常。 android链接google账户似乎会传递很多东西。

