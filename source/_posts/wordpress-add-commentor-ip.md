title: wordpress添加查询留言者IP地址功能
tags:
  - wordpress
id: 143
categories:
  - 互联网
date: 2007-06-22 19:43:39
---

在Wordpress后台管理中，留言管理里面可以看到访问者的IP信息，不过可惜的Wordpress默认连接到的网站查询的是网站信息，这一点对于大部分的用户是没用的，不过如果把这个改为IP地址查询的话就比较有用了。

&nbsp;

我用grep查询了wp-admin目录下的所有文件，有以下几个文件包含了

     admin-functions.php

     edit.php

     edit-comments.php

     moderation.php

&nbsp;

“http://ws.arin.net/cgi-bin/whois.pl?queryinput=”这个字符串。把这些文件中的字符串替换为“http://www.ip.cn/ip.php?q=”就可以了，如果你还想要新窗口打开，那么在前边加上“target="_black"”。不过经过我的测试似乎只修改admin-functions.php就可以了。

上面这几个不同的文件分别代表在不同页面的IP链接地址，比如moderation.php这个就是指待通过页面的IP地址链接，还是把这几个文件都改一下为好。
## Comments

**[Amankwah](#388 "2007-06-22 22:19:23"):** 咋弄的？？咋弄的？

**[草儿](#391 "2007-06-22 22:57:59"):** 嗯，试试 我的那个IP估计有问题，如果可以的话帮我换一个

**[cocobear](#393 "2007-06-22 23:09:11"):** **TO:Amankwah** 说得很明白了啊！

**[Anna](#854 "2007-06-29 20:32:16"):** 没学计算机系真好

**[cocobear](#856 "2007-06-30 00:53:36"):** to: Anna 不明白什么意思？

