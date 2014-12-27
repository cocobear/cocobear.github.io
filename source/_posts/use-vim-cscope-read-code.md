title: 使用vim+cscope阅读源码
tags:
  - cscope
  - Linux
  - Vim
id: 204
categories:
  - Linux
date: 2007-10-12 17:43:54
---

vim与cscope安装就不说了，一般的发行版都会有的。不过如果你是源码编译的vim，请使用--enable-cscope选项。

-R: 在生成索引文件时，搜索子目录树中的代码
-b: 只生成索引文件，不进入cscope的界面
-q: 生成cscope.in.out和cscope.po.out文件，加快cscope的索引速度
-k: 在生成索引文件时，不搜索/usr/include目录
-i: 如果保存文件列表的文件名不是cscope.files时，需要加此选项告诉cscope到哪儿去找源文件列表。可以使用“-”，表示由标准输入获得文件列表。
-I dir: 在-I选项指出的目录中查找头文件
-u: 扫描所有文件，重新生成交叉索引文件
-C: 在搜索时忽略大小写
-P path: 在以相对路径表示的文件前加上的path，这样，你不用切换到你数据库文件所在的目录也可以使用它了。

在使用cscope之前需要先生成一个数据库，你可以使用cscope-indexer（如果多个目录你可以使用-R选项），它会在当前目前下生成一个cscope.files的文件，这个文件包含了cscope需要生成索引的全部文件，因为cscope-indexer不会自动查到cpp,java后缀的文件，因此最后使用find来生成cscope.files文件：

[cocobear@cocobear src]$ find ./ -name "*.c" -or -name "*.h" -or -name "*.cpp" > cscope.files

上面的命令会把当前目录下所有.c,.h,.cpp文件列出并写入cscope.files文件中。接着使用cscope -bq来生成索引引。接着你就可以使用vim来打开一个文件来浏览代码了。使用cs(cscope写)命令来实现函数的调用，定义查找：

s: 查找C语言符号，即查找函数名、宏、枚举值等出现的地方
g: 查找函数、宏、枚举等定义的位置，类似ctags所提供的功能
d: 查找本函数调用的函数
c: 查找调用本函数的函数
t: 查找指定的字符串
e: 查找egrep模式，相当于egrep功能，但查找速度快多了
f: 查找并打开文件，类似vim的find功能
i: 查找包含本文件的文

例如:cs find c do_cscope 可以用来查找项目中调用了do_cscope函数的函数，在vim会以一个列表的形式列出所有相关的内容，你可以输入数字来选择。

当然如果你的源码中只含有.c,.h文件，你可以直接使用

cscope -Rbq

来生成索引文件。

如果你有兴趣的话可以在vim里输入:cs help来查看更多cscope的信息。

BTW：总觉得在kscope里面看代码不爽的很，还是喜欢vim。
## Comments

**[cocobear](#1970 "2007-10-13 23:24:40"):** luguo不是专用滴。

**[cocobear](#1973 "2007-10-14 19:25:05"):** 同理，cocobear也是～～

**[luguo](#1967 "2007-10-13 15:45:29"):** 博主怎么用起我的ID来了？？

**[wind](#1968 "2007-10-13 23:14:31"):** ctags和cscope是相对的，不算taglist。

**[Amankwah](#1962 "2007-10-12 21:39:39"):** 哦，cscope是这样用的啊？和tags+taglist的区别在什么地方？

**[luguo](#1964 "2007-10-13 10:15:33"):** 我也搞不明白taglist,

**[kongove](#3031 "2008-03-23 22:34:15"):** cscope-indexer的选项是 -r

**[dream](#2839 "2008-01-02 13:51:08"):** cscope和ctags比，有什么区别？

**[cocobear](#2843 "2008-01-02 22:13:32"):** cscope和ctags相比，感觉cscope更灵活一些，功能更多一些。

