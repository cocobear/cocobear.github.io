title: PHP留言板更新
id: 168
categories:
  - 编程相关
date: 2007-08-31 15:05:29
tags:
---

更新内容有:

*   过滤了一些HTML代码,如DIV TABLE META等,不过像A STRONG等这些代码还是可用的
*   增加了记忆留言者用户名和信息的功能,这样方便每次留言里输入自己的信息
*   限制了留言的时间间隔,两次留言时间最少为30秒
*   源代码中增加了头部的说明注释
*   修改了数据库中user_info的长度,以前为20,填我自己的邮箱都不够
*   配置文件中增加了时区选项 其实以前就有,只是没有写到配置文件中
*   一些页面布局的修改

似乎就这么多了吧.
关于换行的问题,我试着使用了PHP中的函数nl2br(),但是似乎没有作用,暂时大家可以使用br换行吧:-)

新版本的[代码下载](http://cocobear.github.io/code/tar/php_gb1.0.tar.gz)
欢迎有好的建议给我!
## Comments

**[草儿](#1547 "2007-09-01 02:29:37"):** 修改的不错 UP！！

**[wind](#1575 "2007-09-03 19:02:51"):** 连cnenc都跑过来，熊的博客推广做得不错。

**[noad](#1576 "2007-09-03 19:26:02"):** 我没做推广， 你认识cnenc？

**[cocobear](#1570 "2007-09-02 11:19:19"):** TO LS 能不能说一下什么情况？不太理解。

**[cnenc](#1558 "2007-09-02 00:06:15"):** 你要把所有的HTML标识都去调才对。。。 不然你会后悔的！！！

**[nothing](#1561 "2007-09-02 00:31:51"):** 例如A这样的不用吧WORDPRESS里也没有过滤啊 [cocobear.cn](http://cocobear.cn)

**[cnenc](#1562 "2007-09-02 01:01:47"):** 只有当你感受到痛的时候，你才会知道。。。

**[Amankwah](#1553 "2007-09-01 11:05:43"):** 你昨天原来在做这个工作？

**[luguo](#1545 "2007-08-31 18:02:08"):** 对php没兴趣，都是jsp惹得祸～～！

**[cocobear](#1546 "2007-09-01 00:44:26"):** 你不是还觉得PHP不错吗？ 我对JSP印象也很不好，竟然大型网站好多用JSP，真郁闷。
