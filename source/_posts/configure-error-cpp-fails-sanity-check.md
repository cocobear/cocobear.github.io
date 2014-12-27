title: configure时遇到的问题
tags:
  - configure
  - Life
id: 314
categories:
  - 编程相关
date: 2008-10-10 11:28:29
---

编译一个游戏的客户端时遇到了点问题：
configure: error: C preprocessor "/lib/cpp" fails sanity check 

首先确定是否安装了gcc-c++，如果没有则安装之；然后检查config.log文件，如果提示找不到limits.h文件，解决的办法是：
yum install kernel-headers

更多讨论见：[http://www.linuxquestions.org/questions/linux-software-2/configure-error-c-preprocessor-libcpp-fails-sanity-check-124961/page3.html#post2974284](http://www.linuxquestions.org/questions/linux-software-2/configure-error-c-preprocessor-libcpp-fails-sanity-check-124961/page3.html#post2974284)

################################显眼的分隔线#######################################################
出来冒个泡，好久没更新了，今天遇到这个问题发现很多人在写相关的解决办法时很不全面，而我刚开始搜解决办法的时候也是被迷惑了好久。总结下经验吧，首先是要按照configure时的提示看看config.log文件，会有很有用的信息在里面，还有一点，把google的搜索语言改为英文，我的浏览器已经是默认设置了，中文的技术资料质量还是太差了。

顺便想起昨天偶尔去GameRes逛了下，发现了几个有意思的小游戏，结果下下来的文件中全是一堆垃圾，描述里面还说什么开源，评论里面也是一堆赞美的话，真是悲哀啊！！
## Comments

**[kongove](#4446 "2008-10-11 09:55:43"):** Linux下我就玩“地下恐龙”和superTux。

**[Amankwah](#4443 "2008-10-10 23:39:57"):** 楼上也学会“冏”了？ Linux就玩玩Ubuntu源里带的游戏就OK了，哈哈～

**[luguo](#4441 "2008-10-10 16:28:03"):** 我在opera里早就把google的改成英文的了～～不过最近一直在用firefox，所以还得手工修改～～冏

