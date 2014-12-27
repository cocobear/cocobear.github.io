title: shell脚本解题2
tags:
  - Shell
id: 292
categories:
  - 编程相关
date: 2008-07-15 17:12:33
---

**问题：**
如下文本

60.208.0.224
60.208.103.192
131.9.124.72
60.208.20.119
60.208.20.9
2.110.213.40
8.56.32.24
60.53.37.128
60.3.98.1

怎么先按ip的第一位sort，然后按第二位sort，然后按第三，第四

结果是这样
2.110.213.40
8.56.32.24
60.3.98.1
60.53.37.128
60.208.0.224
60.208.20.9
60.208.20.119
60.208.103.192
131.9.124.72

**解法：**
**1.**
	sort -t. -k1,1n -k2,2n -k3,3n -k4,4n

**2.**
	sed 's/\./ /g' urfile|sort -nt ' ' -k1 -k2 -k3 -k4|sed 's/ /./g'

**问题：**
httpd.conf文件中含有以下内容，要把Directory块中含有/usr/local/apache/cgi-bin这一块注释掉：
	<directory "/var/www/icons">
	    Options Indexes MultiViews FollowSymLinks
	    AllowOverride None
	    Order allow,deny
	    Allow from all
	</directory>

	<directory "/usr/local/apache/cgi-bin">
	AllowOverride None
	OptionsNone
	Orderallow,deny
	Allow fromall
	</directory>

	<directory "/var/www/icons">
	    Optioin Index
	    Allow all
	</directory>

**解法
1.**
	sed -r '/^(<directory .*cgi-bin.*)/{s/^/#/;:a;n;/^$/!s/^/#/;ta} ' httpd.conf 
今天写的最长的一行。

2.

sed  '/^</directory></directory><directory .*cgi-bin.*/,/^<\/Directory>/s/^/#/; ' httpd.conf
sed替换大全：

sed 's/foo/bar/' # 替换第一次出现
sed 's/foo/bar/4' # 替换第四次出现
sed 's/foo/bar/g' # 替换所有的出现
sed '1s/foo/bar/' # 替换第一行第一次出现</directory>
## Comments

**[luguo](#3837 "2008-07-16 17:42:28"):** 第1个的perl解法： perl -n -e 'push @foo, $_; END {print sort {pack("C4",split(/\\./,$a)) cmp pack("C4",split(/\\./,$b))} @foo;}' test.txt

**[dream](#3838 "2008-07-16 17:53:56"):** 1\. sed 's/\\./ /g' urfile|sort -nt ' ' -k1 -k2 -k3 -k4|sed 's/ /./g'，这行为什么不像解1中需要 -k1,1 -k2,2，而只是-k1 -k2, btw, 为什么要,1 ,2 ,3...看了半天manpage 没看懂。 2\. sed -r '/^(<Directory.*cgi-bin.*)/{s/^/#/;:a;n;/^$/!s/^/#/;ta} ' httpd.conf，这行有点太难懂了... orz 3\. 最后一个sed 解释，赞!

**[cocobear](#3872 "2008-07-17 09:26:55"):** perl的看着好怪啊！ 1,2,3是把每一行的内容分段进行sort；第二种解法把.替换为空格，sort默认是按空格进行分段，所以就不需要指定POS2了。

**[dream](#3873 "2008-07-17 14:25:25"):** hehe 明白了，谢谢讲解

