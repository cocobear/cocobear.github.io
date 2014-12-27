title: Jbrush 刷校内网人气
id: 150
categories:
  - 编程相关
date: 2007-06-26 20:12:17
tags:
---

申明一点，仅供学习研究之用。本来的打算也只是玩玩的。

本来是打算用Java写个校内网刷人气的工具在我的服务器上运行，这样可以一天24小时不停的刷，哈哈。不过很可惜写完后发现程序在服务器上运行一会儿就会被杀掉，这个应该是Dreamhost的限制吧，要是没有这个限制那么……呵呵，大家可以想像一下可以在服务器上做些什么事情。

还是用的Httpclient这个包，其实最值得一提的是验证码的识别（校内目前的设置是访问100个人需要填一次验证码）， 我在网上搜索了好长时间也没有什么结果，最后在[编程中国的论坛](http://bbs.bc-cn.net)里得到神vLinux飘飘x的帮助，在这里表示十分的感谢。由于校内网目前的验证十分简单，因此识别起来也不是很难，使用模板匹配的方式，建立十个数字的模板，然后通过对比分析图片中数字与模板的匹配程度，最接近1的为识别出来的数字。

没有图形界面，因为打算是在服务器上运行的，用户名、密码通过Jbrush.conf这个配置文件来获得，还包括留言内容、所要刷的校内网起始ID与结束ID。

留言不能为中文，这里有点问题，还没有解决。

如果哪位朋友有自己的服务器可以试着在服务器上挂着刷，呵呵。

主程序：[Jbrush.java](http://cocobear.github.io/code/Jbrush/Jbrush.java)

验证码识别：[CodeHacker.java](http://cocobear.github.io/code/Jbrush/CodeHacker.java)

验证码模板：[CodeData.java](http://cocobear.github.io/code/Jbrush/CodeData.java)

配置文件：[Jbrush.conf](http://cocobear.github.io/code/Jbrush/Jbrush.conf)

[打包下载](http://cocobear.github.io/code/tar/Jbrush.tar.gz)
## Comments

**[luoluo](#795 "2007-06-28 09:45:10"):** 好佩服你

**[huiyal](#809 "2007-06-28 21:34:38"):** 很不错,向你学习~~

**[cocobear](#742 "2007-06-26 20:25:56"):** 1 如何将字串 String 转换成整数 int? A. 有两个方法: 1). int i = Integer.parseInt([String]); 或 i = Integer.parseInt([String],[int radix]); 2). int i = Integer.valueOf(my_str).intValue(); 注: 字串转成 Double, Float, Long 的方法大同小异. 2 如何将整数 int 转换成字串 String ? A. 有叁种方法: 1.) String s = String.valueOf(i); 2.) String s = Integer.toString(i); 3.) String s = "" + i; 注: Double, Float, Long 转成字串的方法大同小异.

**[cocobear](#748 "2007-06-26 22:38:00"):** http://xiaonei.com/ImOnline.do

**[Name](#749 "2007-06-27 00:04:11"):** 1\. 貌似上面的解释地球人皆知，多余得很 2\. 好的图片验证都会带“干扰”，这很难处理 3\. 写个SYN攻击的程序不是更有技术含量？当然了，也更邪恶啊！

**[Name](#750 "2007-06-27 00:05:40"):** 如果校內开源的话，我可以给它打个补丁。哈哈～

**[草儿](#751 "2007-06-27 01:48:22"):** 所以就说忙活的几天没有达到预期效果啊～ 不过可以在自己机子上挂MS也不错～

