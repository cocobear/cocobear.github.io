title: 代码高亮插件--使用黑色背景
tags:
  - ig_syntax_hilite
  - sed
id: 398
categories:
  - Linux
date: 2008-11-21 17:11:56
---

ig_syntax_hilite这个代码高亮的插件默认使用的是白色背景，所以[小林子](http://windflush.cn/blog/)同学说看不清楚代码，俺就试着把ig_syntax_hilite插件所有颜色代码全部反转了一下，例如：
#ffffff黑色转换为#000000

ig_syntax_hilite使用的是[geshe](http://qbnz.com/highlighter/)，在geshe目录下有很多与语言相关的php加亮文件，颜色代码就在这里生成，使用sed命令进行替换：

[ 代码一 ](#)


	sed -r '/color *: *#[0-9a-f]{6};/{h;s/.*#([^;]+);.*/\1/;y/0123456789abcdef/fedcba9876543210/;G;s/(.*)\n(.*#)[^;]+(;.*)/\2\1\3/}' urfile


[ 代码二 ](#)


	find ./ -iname abap.php | { while read i;do sed -rn 'h;s/.*color[ \t]*[=:][ \t]*#([a-fA-F0-9]+).*/\1/;ta;p;d;:a y/0123456789abcdefABCDEF/fedcba9876543210543210/;G;s/(.*)\n(.*color[ \t]*[=:][ \t]*#)([a-fA-F0-9]+)(.*)/\2\1\4/p' $i >/tmp/tmp__;echo fuck;\cp -f /tmp/tmp__ $i;echo fuck;rm -f /tmp/tmp__;done; }


上面的代码不是俺写的，具体可以查看[CU论坛](http://bbs3.chinaunix.net/thread-1316725-1-1.html)
俺什么时候也能把sed用到这种境界也就满足了:-)

上面简单的一句话脚本还是很有用的，上次俺花了一天的时间去修改主题的配色，其实如果要求不高的话用这脚本一下就搞定了:-)

update:08-12-31
还得把一个颜色表示为单词的也换一下:

	sed -i 's/black/white/g' *
