title: 管理服务器的感受
tags:
  - Linux
id: 54
categories:
  - Linux
date: 2007-04-08 06:20:04
---

这几天一直在管理一些站点，服务器是Debain的系统，我一般是使用ssh远程登录进行一些操作，觉得Linux作服务器确实是很方便的，拿一个简单的例子来说，你如果要建一个论坛，如果你是使用的MS主机的话必然需要先从Discuz下载论坛程序，然后在本机解压，最后在使用Flashfxp一类的工具上传到服务器，这样一来，效率必然会降了许多。而Linux主机直接可以ssh登录进去，然后使用wget下载程序，接着使用rar解压，这样安装一个论坛就是几分钟的时间。同时直接在服务器上下载文件速度是“相当的快”，我下载一个程序最少都260K，有时还达到400K的速度。

Linux服务器还有一个很重要的特长就是有很多实用的工具，象awk,vi,sed,等等。这些工具使得你的好多事情可以得到高效的解决，例如文件编码的转换，可以使用下面的命令：


	`find src -type d -exec mkdir -p utf8/{} \;
	find src -type f -exec iconv -f GBK -t UTF-8 {} -o utf8/{} \;
	mv utf8/* src
	rm -fr utf8`

替换文件中的字符（下面这个替换很多时候会很有用）：

	`sed -i 's/gb2312/utf8/' *`

把所有的php3文件改为php文件：

	`rename 's/.php3$/.php/' *`

…………

其实还有很多其实的功能。

下午的时候需要把一个朋友的站点移到我的服务器上，因为他以前的服务器的MS的主机，所以打包的时候先把所有文件从服务器下载，然后又用rar压缩，最后又上传到他的服务器（其实他也可以直接把这个压缩包传到我的服务器上，不过因为他弄这个的时候我不知道，所以又多费了一些功夫），轮到我的时候就快了，使用wget下载他的压缩包，267K的速度，一会儿的功夫就完了，然后解压，一切正常，接下来就是导入数据库了，以前还没干过这事，找了一下如何导入MySql数据的资料，原来是这么的简单：

	mysql -u username -ppassword -h hostname databasename < backup.sql

一般情况下hostname是不需要的，因为好多服务器的mysql和apache是一块的，但偶这个服务器的mysql在别的服务器上，所以必须填这一项了。接下来又遇到了问题，因为有50项的sql数据备份，如果一条一条搞命令那不是累死了吗？ 想到了awk这个伟大的工具，于是写了下面的代码

ls pw_* | awk '{print "mysql -u username -ppassword -h hostname databasename < "$1"" }'
这条语句少了个分号，郁闷！

因为命令比较长，不能在一行显示，结果不能正确执行（每次输入到行末的时候接下来就覆盖该行的提示符了），也不知道是不是服务器shell的原因，只好使用vi把这句命令写到脚本tmp.sh里，然后执行：

`bash tmp.sh > import.sh`

接下来继续执行：

`bash import.sh
`

好了，数据库的导入就自动开始了

PS：我这次写的脚本不是上面那样的-p后面没有写password，因为不知道它使用密码竟然用这样的格式，很怪异的一种：

`mysql -u username -ppassword -h hostname`

密码是紧跟着-p选项的，可想而知，我在导入50个数据备份输入的50次密码（痛苦啊！）。

仅是这几天有限的时候内使用Linux主机的感受。

[mysql使用手册](http://dev.mysql.com/doc/refman/5.1/zh/using-mysql-programs.html)