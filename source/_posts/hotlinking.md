title: 盗链
tags:
  - blog
id: 156
categories:
  - 互联网
date: 2007-07-04 11:33:57
---

昨天晚上收到[dreamhost](http://www.dreamhost.com)的邮件,说我服务器上的一个网站受到攻击,流量过大,已经关闭.没过多久又收到[草儿](http://hao150.cn)的网站也受到来自221.217.198.202的攻击.我上去看了一下[草儿网站](http://hao150.cn)的日志:

	221.217.198.202 - - [03/Jul/2007:06:12:10 -0700] "GET /download/E680g/PST_7.2.3_
	GENERAL.rar HTTP/1.1" 301 609 "http://www.hao150.cn/blog/2007/05/10/\xe6\x91\xa9
	\xe6\x89\x98\xe7\xbd\x97\xe6\x8b\x89e680g\xe5\xba\x94\xe7\x94\xa8\xe7\xa0\x94\xe
	7\xa9\xb6/" "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1)"

省略N条

	221.217.198.202 - - [03/Jul/2007:06:22:23 -0700] "GET /download/E680g/PST_7.2.3_
	GENERAL.rar HTTP/1.1" 206 12009 "http://www.hao150.cn/blog/2007/05/10/\xe6\x91\x
	a9\xe6\x89\x98\xe7\xbd\x97\xe6\x8b\x89e680g\xe5\xba\x94\xe7\x94\xa8\xe7\xa0\x94\
	xe7\xa9\xb6/" "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1)"

	[crystallight]$ tail -10000 access.log |awk '{print $1}' | sort | uniq -c|sort|grep 221.217.198.202
	    490 221.217.198.202

从昨天06:12:10到06:22:23,上面这个IP一共连接达490次.估计应该是被哪个网站盗链了,

看来如果上传文件的话得做防盗链的工作了.

正好我这几天访问统计中有很多伊朗的IP,莫非也被盗链了,去看了下日志:

	217.218.238.196 - - [03/Jul/2007:00:14:42 -0700] "GET /gfw/index.php?q=aHR0cDovL
	3MudXM2LnVzL2Vlay5naWY%3D HTTP/1.1" 200 1522 "http://cocobear.github.io/gfw/index.php?q
	=aHR0cDovL2F2aXpvb24uY29tL2ZvcnVtLzExXzcxNzFfNTY4Lmh0bWw%3D" "Mozilla/4.0 (compa
	tible; MSIE 6.0; Windows NT 5.1; SV1)"

全是这样的访问,看来代理被人哪个网站给盗链了,果然被王聪说中了.马上换个地址,目前的代理地址为:

[](http://cocobear.github.io/gfw-gfw)在线代理

以后会经常更换的,请及时来我的博客查看最新代理地址.

看下我的access.log文件统计:

	403 85.185.37.3
    448 85.185.226.210
    453 62.60.223.87
    541 217.219.119.2
    612 82.99.234.6
    633 85.185.229.51
    975 80.191.195.20
   1242 217.218.156.66
   2592 211.97.245.18

仅列出超过400的,从昨天凌晨现到今天早上,真不爽.
## Comments

**[cocobear](#904 "2007-07-04 13:10:52"):** 你不是说过小心被google搜到吗?这次是被别人搜到了:-)

**[cocobear](#905 "2007-07-04 13:15:18"):** http://www.myipneighbors.com/ 一个不错的网站,可以反查一个IP上有多少个网站 TO:wangcong,看下你的IP,吓死我了竟然有215个网站在上面!!!!

**[Name](#903 "2007-07-04 12:52:30"):** 好像前几天也有一次你的网站上不了，而且不光你的，咱们小组的.cn的都上不了。不过大约过了十几分钟就好了，估计是.cn域名服务器的问题。 BTW：我什么时候说过你被盗链了？

**[草儿](#907 "2007-07-04 14:39:10"):** 很有必要做好防盗链工作，要不以后就赔大了

**[windflush](#908 "2007-07-04 19:00:18"):** OH MY GOD 把俄吓坏咧

**[Amankwah](#920 "2007-07-05 10:32:55"):** 貌似代理会被别人滥用是我给你说的…… 我依然建议你弄个身份验证，谁想用，跟你要用户名和密码！

**[cocobear](#921 "2007-07-05 13:46:01"):** 呵呵，给忘了，我还以为是王聪给说的。 “身份验证”，不错的建议，以后再说吧。

