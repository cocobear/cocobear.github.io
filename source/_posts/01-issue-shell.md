title: shell脚本解题
tags:
  - awk
  - sed
  - Shell
id: 287
categories:
  - 编程相关
date: 2008-07-07 10:27:55
---

以下内容来自http://bbs.chinaunix.net/thread-1189933-1-1.html

现有数据，每一行中各个段间的分隔为多个空格，如下
　　AAB    BB    CCC
      A2B    CB    ABC
如何实现，转换成下面的格式
AAB|BB|CCC|
A2B|CB|ABC|

awk解法：
**1.**
	[cocobear@cocobear ~]$ awk 'BEGIN{OFS="|"} {$1=$1;print}' test.txt 
	AAB|BB|CCC
	A2B|CB|ABC
结果最后少一个"|"
**2.**
	[cocobear@cocobear ~]$ awk '{print $1"|"$2"|"$3"|"}' test.txt 
	AAB|BB|CCC|
	A2B|CB|ABC|

**3.**
	[cocobear@cocobear ~]$ awk -v OFS='|' 'NF++' test.txt 
	AAB|BB|CCC|
	A2B|CB|ABC|

sed解法：
**1.**
	[cocobear@cocobear ~]$ sed 's/^ \+//;s/ \+/|/g;s/ *$/|/;' test.txt 
	AAB|BB|CCC|
	A2B|CB|ABC|

**2.**
	[cocobear@cocobear ~]$ sed 's/\s\+//;s/\s\+\|$/|/g' test.txt 
	AAB|BB|CCC|
	A2B|CB|ABC|
**3.**
	[cocobear@cocobear ~]$ sed 's/^[[:blank:]]\+//;s/[[:blank:]]\+\|$/|/g' test.txt 
	AAB|BB|CCC|
	A2B|CB|ABC|

如何把一行的第一个字母换成大写?

文件如下: test.txt
	#-----------------------------#
	This is  line number 1

	THIS IS Line Number 2

	ThiS is Line Number THREE

	this is line Number four
	#-----------------------------#

解法：
	sed 's/^\(.\)/\u\1/'
	sed 's/[a-z]/\u&/'

& 指代替前面查找的关键字即[a-z]      \1是指前面用\(..\)定义的第一个标签
## Comments

**[cocobear](#3557 "2008-07-07 16:36:17"):** 表示匹配一个或者多个字符。

**[dream](#3553 "2008-07-07 11:08:09"):** sed 's/ \\+/|/g' 中\\+是什么意思？

**[dream](#3620 "2008-07-10 16:21:35"):** 那 .* 不是也可以达到目的吗？

