title: shell脚本解题1
tags:
  - Shell
id: 289
categories:
  - 编程相关
date: 2008-07-10 13:28:24
---

问题：请教如何得到一个目录树的最深目录路径？

解法：
1.
	find -type d | awk '{if( max < gsub(/\//,"/")) {max= gsub(/\//,"/");maxp=$0"/"}  else if (max == gsub(/\//,"/")) maxp=maxp"\n"$0"/"} END{print maxp}'

2.
	find -type d -printf "%d %p\n" | sort -nrk1 | awk 'NR==1{a=$1}a==$1{print $2}'

问题：我想将包含read的行放在echo单词的后面

解法：
[bash]sed '/read/{h;d};/echo/{G;s/\(.*echo\)\(.*\)\n\(.*\)/\1\3\2/}'  urfile 

问题：怎么copy一个目录中的某些文件，但保持目录结构？

比如说，我想copy

sourcedir\a\b\c.java
sourcedir\a\b\c.cs
sourcedir\a\b\d.java
sourcedir\a\b\d.cs

到

destdir\a\b\c.java
destdir\a\b\d.java

解法:
1.
	rsync -av --include='*.java' --filter='hide,! */' ./sourcedir/ ./destdir

2.
[bash]cp -r b dest;find dest -type f ! -name "*.java" | xargs rm
我的解法
3.
	tar -c * | tar -xf- --wildcards -C destdir *.java
4.
	find sourcedir -name "*.java"|xargs -I {} cp  --parents {} destdir

请教：如何枚举目录中所有没有子目录的目录的路径？

解法:
1.
	find -type d -printf "%h\t%p\n" | awk 'BEGIN{FS="\t"}{a[$1]++;a[$2]++}END{for(i in a)if(a[i]==1)print i}'

2.
	find -type d -ls|awk '$4==2{print $NF}'

问题:
有这样一个字符串  MG001:MobileLove:MGLoveCMD:down:  每个字段以冒号分割
STRING="MG001:MobileLove:MGLoveCMD:down:"
我想把其中的每个字段分别赋值给另外几个变量，比如第一个字段赋值给SERV_CODE ，第二个赋值给
APPLICATION.............

解法:
	IFS=: read SERV_CODE APPLICATION ... <<<"$STRING"
## Comments

**[cocobear](#3682 "2008-07-14 11:09:47"):** 1\. man xargs: -I replace-str Replace occurrences of replace-str in the initial-arguments with names read from standard input. Also, unquoted blanks do not terminate input items; instead the separator is the newline character. [cocobear@cocobear test]$ find b -name "*.java"|xargs ls {} ls: 无法访问 {}: 没有那个文件或目录 b/08/07/1.java [cocobear@cocobear test]$ find b -name "*.java"|xargs -I{} ls {} b/08/07/1.java 2\. <<<这个我前面说过了，你看看bash手册，那里面提到了。 3\. read就没有这样的用法；参考<<<。

**[dream](#3675 "2008-07-12 22:20:11"):** 对不起，有些看懂了，有些还没看懂，能不能帮我再解释一下，真的想学啊～～ 呵呵 1\. 在你的例子4中，"find sourcedir -name "*.java"|xargs -I {} cp --parents {} destdir"。xargs -I {} 是什么意思？{}是指find所找到的内容，那么-I是什么意思？man xargs里没有-I的解释。 2\. IFS=: read SERV_CODE APPLICATION ... <<<"$STRING"中，<<<是什么意思？ 以前从没有见过，< 不是作为输入的符号吗？怎么变成<<<了？ 3\. 我 echo "$STRING"| IFS=: read q w e r t 为什么赋不成值？ 谢谢了~ :)

**[luguo](#3632 "2008-07-10 20:32:17"):** 我更喜欢： OLDIFS=$IFS; ....; IFS=$OLDIFS; 最后一个也可以用cut嘛！

**[luguo](#3633 "2008-07-10 22:43:00"):** 第1个问题，我的解法： $ find -type d | awk -F'/' 'BEGIN {m=0;l="" } {if(NF>m){m=NF; l=$0}} END {print l;}' 当然，若有多个最深的它只能得到其中一个。

**[cocobear](#3644 "2008-07-11 11:19:40"):** to:dream 你可以看看find手册， {} 就是代表前面find找到的结果。xargs是用来执行标准输入的内容 Here Strings A variant of here documents, the format is: <<<word The word is expanded and supplied to the command on its standard input. 我也是学习中，很多都是别人写的脚本。

**[dream](#3621 "2008-07-10 16:38:13"):** 请问： 1\. find sourcedir -name "*.java"|xargs -I {} cp --parents {} destdir 中， xargs -I {} 是什么意思？ 2\. cp --parents {} destdir 中 {} 是什么意思？ 你的解法中, awk用的很多啊 ～ 你真是厉害的bash高手～～～

**[dream](#3622 "2008-07-10 16:43:17"):** 还有一个问题： 3\. IFS=: read SERV_CODE APPLICATION ... <<<"$STRING" 中，为什么要用<<<，而不是<<?

**[dream](#3623 "2008-07-10 16:46:21"):** 我 echo "$STRING"| IFS=: read q w e r t 为什么赋不成值？

**[dream](#3836 "2008-07-16 17:33:27"):** 哈哈，这回全明白了，太感谢了！

