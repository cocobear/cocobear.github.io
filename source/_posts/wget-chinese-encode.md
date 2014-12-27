title: wget中文乱码
tags:
  - wget
id: 260
categories:
  - 编程相关
date: 2008-04-19 22:26:19
---

昨天打算用wget把kerneltravel上的深入分析Linux内核源码全部下载到本机，没想到使用wget下载到本地时文件名全部是乱码：

	[cocobear@cocobear ~]$ wget -r -p -k -np http://www.kerneltravel.net/kernel-book/深入分析Linux内核源码.html
	--19:52:02--  http://www.kerneltravel.net/kernel-book/%E6%B7%B1%E5%85%A5%E5%88%86%E6%9E%90Linux%E5%86%85%E6%A0%B8%E6%BA%90%E7%A0%81.html
	           => `www.kerneltravel.net/kernel-book/深�%85��%88%86�%9E%90Linux�%86%85核��%90��%81.html'
	正在解析主机 www.kerneltravel.net... 202.75.211.215
	Connecting to www.kerneltravel.net|202.75.211.215|:80... 已连接。
	已发出 HTTP 请求，正在等待回应... 200 OK
	长度：133,528 (130K) [text/html]

	100%[====================================>] 133,528       60.71K/s             

	19:52:05 (60.57 KB/s) - `www.kerneltravel.net/kernel-book/深�%85��%88%86�%9E%90Linux�%86%85核��%90��%81.html' saved [133528/133528]

google了半天也没有找到解决方案，似乎也有不少人碰到这个问题。仔细想了想应该是wget在处理url时的问题，于是下载了最新的[wget源代码](http://www.sfr-fresh.com/unix/www/wget-1.11.1.tar.gz/)开始找问题。

基本看完了[main.c](http://www.sfr-fresh.com/unix/www/wget-1.11.1.tar.gz:a/wget-1.11.1/src/main.c)这个文件，后来在[url.c](http://www.sfr-fresh.com/unix/www/wget-1.11.1.tar.gz:a/wget-1.11.1/src/url.c)这个文件里找到了wget如何处理保存的文件名，url_file_name()这个函数。

url_file_name()在根据url的形式判断该保存为什么样的文件名，并进行了多方面的考虑，最终该函数调用了append_uri_pathel()，该函数会判断url中的特殊字符，例如空格等，如果遇到这些字符wget把它进行转义，而问题就出在这里，append_uri_pathel()函数是通过FILE_CHAR_TEST (*p, mask)这一句来判断该字符是否为特殊字符，而同时它会认为中文也是特殊字符，然后按照转换空格之类的方式对中文进行转义，这样就会造成中文乱码的情况，知道了问题所在我在append_uri_pathel()函数对特殊字符的判断中排除了中文字符。

接下来重新编译wget，再试一次：

	[cocobear@cocobear src]$ ./wget -r -p -k -np http://www.kerneltravel.net/kernel-book/深入分析Linux内核源码.html
	--2008-04-19 20:16:34--  http://www.kerneltravel.net/kernel-book/%E6%B7%B1%E5%85%A5%E5%88%86%E6%9E%90Linux%E5%86%85%E6%A0%B8%E6%BA%90%E7%A0%81.html
	Resolving www.kerneltravel.net... 202.75.211.215
	Connecting to www.kerneltravel.net|202.75.211.215|:80... connected.
	HTTP request sent, awaiting response... 200 OK
	Length: 133528 (130K) [text/html]
	Saving to: `www.kerneltravel.net/kernel-book/深入分析Linux内核源码.html'

	74% [===========================>           ] 98,832      79.7K/s      

可以看到wget可以正确的把中文的网页保存下来了:-)

我的locale是utf-8，如果使用gbk的话wget上面的网页会得到404错误，因为gbk环境下wget发出来GET请求是按照gbk编码的方式进行转义，而服务器对接收到的GET请求中的URL是按照UTF-8的方式进行“解码”，因此会得到404的错误。（早上测试的时候还是这样的，但下这会写这篇文章的时候gbk环境下使用wget也正常了，郁闷）

以下是我修改后的url.c与原文的diff:
	[cocobear@cocobear ~]$ diff -u wget-1.11.1/src/url.c Codes/wget-1.11.1/src/url.c 
	--- wget-1.11.1/src/url.c       2008-04-19 11:42:24.000000000 +0800
	+++ Codes/wget-1.11.1/src/url.c 2008-04-19 11:56:49.000000000 +0800
	@@ -1332,11 +1332,10 @@
	   /* Walk the PATHEL string and check how many characters we'll need
	      to quote.  */
	   quoted = 0;
	-  for (p = b; p < e; p++) {
	-    if (FILE_CHAR_TEST (*p, mask) && !((*p | 0x0fffffff) == 0xffffffff)) {
	+  for (p = b; p < e; p++)
	+    if (FILE_CHAR_TEST (*p, mask) && !((*p | 0x0fffffff) == 0xffffffff))
	                   ++quoted;
	-              }
	-  }
	+  
	   /* Calculate the length of the output string.  e-b is the input
	      string length.  Each quoted char introduces two additional
	      characters in the string, hence 2*quoted.  */

以上仅仅是我自己对wget的一些分析、看法，如有错误请路过的指正一下。

BTW：
(1):http://www.sfr-fresh.com这个网站不错，有不少源代码在上面可以在线看。
(2):深入分析Linux内核源码这本书打包下载(html格式):http://cocobear.github.io/download/Understand_Linux_kernel.tar.bz2

Update1(08.4.20):
这个问题是wget对国际化的字符支持不好，目前google summer code已经有人在这方面进行改进，期待。我的解决方案只是临时的，只能针对utf-8环境。
## Comments

**[sept](#4339 "2008-09-20 00:14:56"):** 你好，我也遇到同样的问题，在百度google也没找到方法。看到你有解决方法很兴奋，无奈水平有限，没看出来该修改url.c的哪里？能否在详细说一下，或者能否把改完的发给我，谢谢

**[luguo](#3113 "2008-04-19 22:37:34"):** 晕，我今天搜索时也正好搜到那个网站上面的代码～～真巧啊！！

**[kongove](#3114 "2008-04-19 23:14:15"):** 能不能用个脚本把文件URL的后边截取下来，用wget的-O 选项支持输出文件名？

**[kongove](#3115 "2008-04-19 23:20:00"):** Thanks for your share of ulk.

**[cocobear](#3116 "2008-04-20 08:44:24"):** 单个文件下载你可以使用脚本截取，如果递归下载你都没办法知道可能下载的文件。

**[cocobear](#3828 "2008-07-16 08:57:03"):** to:leon 原理上还是一样的，不过windows使用的是gb2312编码，针对编码处理的地方应该不太一样。 如果要完全解决这个问题，还得wget官方做不少东西毕竟国际化的支持是件不小的工作。

**[leon](#3811 "2008-07-15 18:39:11"):** cocobear:您好。看到您解决了这个问题，简直是五体投地。我现在不是使用linux，只能够使用wget的windows版或者cygwin版，请问应该怎么解决问题？请高手出手相救。谢谢。

**[leon](#3842 "2008-07-16 19:13:06"):** cocobear: 感谢您的回答。请问应该如何修改代码才能够正确获得中文文件名？如果需要，我可以把windows下的代码发给您。谢谢

**[Simxinzi](#3918 "2008-07-21 09:51:06"):** Win下该如何修改url.c这个文件呢，不知道需要修改哪里才能解决UTf-8编码的文件下载的问题，望不吝赐教,感激不尽。QQ:9961459

**[cocobear](#3919 "2008-07-21 10:27:16"):** WIN下的实现和linux下不一样，而且win使用的是gb2312具体修改我不太清楚。 不知道你是使用的是wget的win版一还是cywin中的wget？

**[Simxinzi](#3920 "2008-07-21 10:52:37"):** 谢谢斑竹的回复，我使用的是wget的win版 访问中文URL时因为编码问题，出现403错误 Wget编码默认为非utf-8 而服务器Apache编码为UTF-8 比如说下载这样一个文件 此中文路径可直接访问并可下载： http://www.****.com/QQ幻想/music/M001.ogg Wget却访问成： http://www.****.com/QQ%BB%C3%CF%EB/music/M002.ogg 以下路径也可以下载： http://www.****.com/QQ%e5%b9%bb%e6%83%b3/music/M002.ogg 应该是一个编码的问题：Win下的源码下载地址： http://www.christopherlewis.com/WGet/wget-1.11.4s.zip

**[Simxinzi](#3921 "2008-07-21 10:55:03"):** 你的思路应该能解决问题，即不转换中文路径，仅转换空格等其他字符，但就是不知道该修改哪里

**[cocobear](#3922 "2008-07-21 14:17:59"):** 同样的位置,你试着改了,编译一下.我没有环境编译.

**[j](#3982 "2008-08-02 08:59:25"):** \- for (p = b; p < e; p++) { \- if (FILE_CHAR_TEST (*p, mask) && !((*p | 0×0fffffff) == 0xffffffff)) { \+ for (p = b; p < e; p++) \+ if (FILE_CHAR_TEST (*p, mask) && !((*p | 0×0fffffff) == 0xffffffff)) ++quoted; \- } \- } \+ 这两段代码功能完全一样呀，只是少了｛｛｝｝

**[leon](#3925 "2008-07-22 13:39:32"):** 我成功了，呵呵

**[leon](#3926 "2008-07-22 13:40:04"):** 昨天加你的QQ你没有通过。不然可以把文件发给你了。

**[leon](#3927 "2008-07-22 13:43:02"):** 不好意思，刚才没有看清楚。你的问题跟我的不一样。我的是下载UTF8编码文件乱码。现在解决了。你这个就不知道了，呵呵

**[leon](#3928 "2008-07-22 13:49:29"):** 你要找到URL请求的代码，在wget发送请求前做一下转换就好了。

**[Simxinzi](#3931 "2008-07-22 19:29:39"):** leon 麻烦再加一下，谢谢了，我也是和你一样的问题，下载UTF-8编码文件乱码，下载UTF-8路径找不到文件或乱码

**[Simxinzi](#3932 "2008-07-22 19:29:59"):** leon 麻烦再加一下，谢谢了，我也是和你一样的问题，下载UTF-8编码文件乱码，下载UTF-8路径找不到文件或乱码 QQ:9961459

**[可可熊](#4377 "2008-09-28 09:50:26"):** url.c修改1332行附近。

**[wmk](#6090 "2009-06-09 05:21:05"):** wget -r -np -nH --cut-dirs=3 http://www.kerneltravel.net/kernel-book/%E6%B7%B1%E5%85%A5%E5%88%86%E6%9E%90Linux%E5%86%85%E6%A0%B8%E6%BA%90%E7%A0%81.html \--cut-dirs=3 可以解决问题。

**[cocobear](#6096 "2009-06-09 09:15:11"):** To: wmk 不行的，不知道你试了没。 [root@ibmfedora8 ~]# wget --version GNU Wget 1.10.2 (Red Hat modified)

**[arthur1989](#7023 "2009-12-26 22:16:47"):** 求一份修改后的wget..谢谢

**[angoo-mart](#7045 "2010-01-01 00:37:42"):** 我也遇到这样的问题，请问一下，windows下的wget要怎么改？我参考的代码是wget-1.12的，我很想把这个问题解决掉，请各位牛人指点指点。我的QQ是43904930. 如果谁有已经修改好的，windows下可以运行的wget.exe就更好，麻烦提供一下。谢谢各位，谢谢cocobear

**[cocobear](#7050 "2010-01-01 21:40:53"):** windows下应该是在cygwin里用wget的吗？修改的方法也应该是一样的。 在url.c里面，我上面已经写出来了。

**[cocobear](#7028 "2009-12-29 09:57:41"):** To:arthur1989 上面有需要修改的地方，完整的修改过的包我现在没有了。

**[demonoid](#7749 "2010-04-04 09:27:01"):** !((*p | 0×0fffffff) == 0xffffffff)) 这句“0×0fffffff”里 应为“x”，你错写成乘号了。

**[fedora](#9907 "2011-04-23 17:41:58"):** 不过改代码的想法非常好

**[fedora](#9906 "2011-04-23 17:40:40"):** \--restrict-file-names=nocontrol 加上这个参数就行了

**[jinfeng](#11398 "2011-10-20 13:46:12"):** 我需要一份wget文件，是解决Ascii乱码的，谢谢

**[jinfeng](#11399 "2011-10-20 13:46:56"):** 我的QQ号是515331954

**[jinfeng](#11397 "2011-10-20 13:45:42"):** 有没有人在啊？

**[Star Brilliant](#23216 "2013-04-13 18:55:52"):** wget 有一个参数 --restrict-file-names=nocontrol 能解决这个问题。

