title: 一些杂事
tags:
  - Life
id: 218
categories:
  - Life
date: 2007-11-02 23:12:52
---

没想到俺去深信服面试时的那个人就是深信服的何总，越来越对这个公司感兴趣了，决定开始写Web Server，整体的思路已经有了，因为本来就有写这个的打算，而且写过FTP Server。两个礼拜应该能完成，不过能做到什么程度就不敢保证了。下面是题目要求：

**1\. 编写Windows下或者Linux下的http服务器，不得抄袭别的Web服务器。要求：支持多个并发用户访问，使用配置文件配置根目录等选项。支持CGI，最好支持简单的脚本(语法可以类似asp或者php，实现一些简单的语法就可以了)。给出简单说明和设计文档。**

了解了一下CGI：
CGI的作用接受来自于Web客户端的要求，启动对应的程序，将这个程序的执行结果返还给Web客户端。

感觉实现起来难度应该不是很大，应该要用到管道等东西。

支持简单的脚本还没有什么思路，实现个echo就行吗？还是需要实现再多一些，比如一个for循环，如果太多了就相当于实现一种脚本语言，同时写个解释工具，难度可不一般啊！！谁有什么好的建议吗？

cocobear.cn的PR值到3了，上半年似乎一直是0。
SuperMario又得拖一拖了。
## Comments

**[DDR物语](#2306 "2007-11-08 16:20:34"):** mxpc.cn/ 4 呵呵……

**[wind](#2245 "2007-11-05 10:42:44"):** pr值： cocobear.cn 3 wangcong.org 2 amankwahly.cn 1 windflush.cn 0 cocobear.cn/blog 1 wangcong.org/blog 1 amankwahly.cn/blog 1 windflush.cn/blog 1

**[luguo](#2220 "2007-11-03 17:19:12"):** 你只解释php中几个最简单的语句不就行了吗？！汗～～

**[wind](#2221 "2007-11-03 17:28:40"):** faint

**[草儿](#2223 "2007-11-04 01:04:47"):** 加油~

**[cocobear](#2216 "2007-11-03 13:49:16"):** 貌似就是新发明一个，不过不需要多完整，只要简单的一些就行。 但不知道需要多简单？

**[Amankwah](#2213 "2007-11-03 11:05:23"):** 支持脚本能不能用已有的脚本语言？

**[luguo](#2214 "2007-11-03 11:44:33"):** ms楼上废话啊～不支持已有的还让dp同学自己发明一个新的不成？！

**[cocobear](#2248 "2007-11-05 17:39:20"):** wind小同学怎么faint了？

