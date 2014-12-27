title: 使用awk消除行号
id: 41
categories:
  - 编程相关
date: 2007-04-06 19:18:12
tags:
---

在读ABS(Advanced Bash-Scripting Guide)的时候，遇到书中的示例代码如下:
	   1 #!/bin/bash
	   2 # broken-link.sh
	   3 # Written by Lee bigelow >ligelowbee@yahoo.com< 4 # Used with permission.
	   5
	   6 #A pure shell script to find dead symlinks and output them quoted
	   7 #so they can be fed to xargs and dealt with
	   8 #eg. broken-link.sh /somedir /someotherdir|xargs rm
	   9 #
	  10 #This, however, is a better method:
	  11 #
	  12 #find "somedir" -type l -print0|\
	  13 #xargs -r0 file|\
	  14 #grep "broken symbolic"|
	  15 #sed -e 's/^\|: *broken symbolic.*$/"/g'
	  16 #
	  17 #but that wouldn't be pure bash, now would it.
	  18 #Caution: beware the /proc file system and any circular links!
	  19 ###################################
	  20
	  21
	  22 #If no args are passed to the script set directorys to search
	  23 #to current directory.  Otherwise set the directorys to search
	  24 #to the agrs passed.
	  25 ####################
	  26 [ $# -eq 0 ] && directorys=`pwd` || directorys=$@
	  27
	  28 #Setup the function linkchk to check the directory it is passed
	  29 #for files that are links and don't exist, then print them quoted.
	  30 #If one of the elements in the directory is a subdirectory then
	  31 #send that send that subdirectory to the linkcheck function.
	  32 ##########
	  33 linkchk () {
	  34     for element in $1/*; do
	  35     [ -h "$element" -a ! -e "$element" ] && echo \"$element\"
	  36     [ -d "$element" ] && linkchk $element
	  37     # Of course, '-h' tests for symbolic link, '-d' for directory.
	  38     done
	  39 }
	  40
	  41 #Send each arg that was passed to the script to the linkchk function
	  42 #if it is a valid directoy.  If not, then print the error message
	  43 #and usage info.
	  44 ################
	  45 for directory in $directorys; do
	  46     if [ -d $directory ]
	  47 then linkchk $directory
	  48 else
	  49     echo "$directory is not a directory"
	  50     echo "Usage: $0 dir1 dir2 ..."
	  51     fi
	  52 done
	  53
	  54 exit 0

想复制下来自己执行一下，无奈每行都有行号，不能直接做为shell脚本执行，当然不甘心了，于是想到强大的awk，写了如下awk脚本:
	{
	i=2;
	while (i >= NF) {
	ORS=" ";
	if (i == NF) {
	ORS="\n";
	}
	print $i;
	i++;}
	}
其实很简单，只要实现不打印第一个字段的内容，就可以完成消除行号。似乎应该有更简单的awk脚本，更是暂时没有想到,所以用了这个笨办法，从第二个字段开始打印，直到NF(当前记录的字段总数)，默认的print打印字段后是输出一个“\n”,于是做了一个简单的判定，只有当打印最后一个字段后才输出"\n",否则使用" "(空格)作为print的额外输出。
不知道哪位对awk比较熟悉，如何能使这个脚本更简单(用一行语句就搞定),麻烦告诉我一声
同样,这个脚本可以处理使用"cat -n"(该命令作用是给每一行加行号) 命令生成的文件
有关awk的参考文章:[FROM](http://fanqiang.chinaunix.net/program/shell/2005-03-30/3068.shtml)
## Comments

**[cocobear](#3470 "2008-07-01 09:52:23"):** sed正则不错。

**[ninesuns](#3457 "2008-06-27 14:03:11"):** sed -s 's/^[0-9]\\{1,\\}//' filename

