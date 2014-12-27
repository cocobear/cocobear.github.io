title: sed的几个选项
tags:
  - sed
id: 290
categories:
  - 编程相关
date: 2008-07-11 17:07:24
---

sed中有些选项不太好理解,今天就顺一顺:

几个命令:
n 
把下一行输入读入模式空间中.当前行被发送到标准输出,下一行将成为当前行.控制将被转移给n后面所跟的命令,而不是回到脚本开始处.

N
把下一行输入行添加到模式空间的内容之后;这两行文本用一个插入的换行符分隔开.

p
打印出寻址到的行.除非使用命令行选项-n,否则该命令会导致输出相同的行.

这里有一段对D的解释
函数参数 D 表示删除 pattern space 内的第一行资料。其指令格式如下:   

    [address1,address2]D 

对上述格式有下面几点说明 :   

函数参数 D 最多配合两个位址参数。  
函数参数 D 与 d 的比较如下 :  
当 pattern space 内只有一资料行时 , D 与 d 作用相同。  
当 pattern space 内有多行资料行时  
D 表示只删除 pattern space 内第一行资料 ; d 则全删除。  
D 表示执行删除後 , pattern space 内不添加下一笔资料 , 而将剩下的资料重新执行 sed script ; d 则读入下一行後执行 sed script。

几个选项:
-e script
执行后面的脚本

-n 
禁止自动打印模式空间的内容

例子:

	[cocobear@cocobear test]$ cat 1
	1
	2
	3
	4
	5
	6
	7
	8
	9

	[cocobear@cocobear test]$ sed -e 'n' -e 'd' 1
	1
	3
	5
	7
	9
sed读入第一行内容,执行命令'n',把第二行读入模式空间,向标准输出第一行内容,第二行成为当前行,然后执行后面的命令'd','d'命令删除模式空间内容(也就是第二行内容),该行处理完成,接下来处理第三行,依此进行.

	[cocobear@cocobear test]$ sed -e 'n' -n -e 'p' 1
	2
	4
	6
	8
sed读入第一行内容,执行命令'n',把第二行读入模式空间,向标准输出第一行内容,但由于使用了-n选项,所以这里就没有输出;第二行成为当前行,然后执行后面的命令'p','p'命令输出模式空间内容(也就是第二行内容),该行处理完成,接下来处理第三行,依此进行.

	[cocobear@cocobear test]$ sed -e 'N' -e 'd' 1
	9
sed读入第一行内容,执行命令'N',把第二行添加到模式空间中,然后执行后面的命令'd','d'命令删除模式空间内容(也就是第一+第二行内容),该行处理完成,接下来处理第三行,依此进行.

	[cocobear@cocobear test]$ sed -e 'N' -n -e 'p' 1
	1
	2
	3
	4
	5
	6
	7
	8
sed读入第一行内容,执行命令'N',把第二行添加到模式空间中,然后执行后面的命令'p','p'命令打印模式空间内容(也就是第一+第二行内容),由于-n选项,处理完后sed不会自己再次打印该行处理完成,接下来处理第三行,依此进行.处理完第七行后,使用'N'命令时出错,sed退出.
## Comments

**[dream](#3676 "2008-07-13 01:30:44"):** hehe 谢谢分享

**[luguo](#3646 "2008-07-11 17:47:45"):** http://www.catonmat.net/blog/wp-content/plugins/wp-downloadMonitor/user_uploads/sed.stream.editor.cheat.sheet.txt

