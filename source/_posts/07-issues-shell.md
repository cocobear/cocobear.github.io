title: shell脚本解题6
tags:
  - awk
  - JavaScript
  - sed
  - Shell
id: 297
categories:
  - 编程相关
date: 2008-08-19 15:42:19
---

问题：
执行命令前暂时回home目录，执行后跳回来

**解法：
1.**
	(cd ~; ls; )
使用子shell的方式去执行命令；

把这行
gs.AddMany(  [b_cls,b_bck,t_blank,b_close,b_7,b_8,b_9,b_div,b_4,b_5,b_6,b_mul,b_1,b_2,b_3,b_minus,b_0,b_dot,b_equal,b_plus, ])
进行替换，每一个元素替换为类似(b_cls,0,EXPAND)这样
**
解法1.**
s	ed 's/[b|t]_[^,]*,/(&0,EXPAND)/g'
如果使用
's/[b|t]_.*?,/(&0,EXPAND)/g'则不能得出正确的结果，因为sed不支持.*?这样的非贪婪匹配；解法中使用[^,]避免了贪婪匹配。

有如下内容,想要获取 8B E5 55 这些内容：
	[cocobear@cocobear ~]$ cat file.txt 
	00401038 8B E5                mov         esp,ebp
	0040103A 55                   push        ebp
	0040103B 8B EC                mov         ebp,esp
	0040103D 33 FF                xor         edi,edi
	0040103F 57                   push        edi
	004010A9 FF D0                call        eax
	004010AB 83 C4 03             add         esp,3

**解法1：**
	sed 's/[^ ]* \([0-9A-Z ]\+\).*/\1/' file.txt
**解法2：**
	awk -F'   ' ' {print $1}' file.txt | awk '{print $2,$3,$4,$5}'

再看一正则表达式str="uid=100(guest) gid=100(others) groups=10(users),11(floppy)"

输出guest：echo $str|sed 's/[^(]*(\([^)]*\)).*/\1/'
输出各括号里的：echo $str | awk -F'[()]' '{print $2" "$4" "$6" "$8}'

BTW：最近在使用delicious时发现添加在Opera工具栏中的“Bookmark on Delicious”挺方便：![delicious](http://l.yimg.com/hr/10317/img/register/bookmarkletOpera100.png)，看了下这个工具栏按钮的属性，发现其实就是一段JS代码，就修改了一下做了个google当前网站的小工具：

	javascript:(function(){s=window.location.href.split("//")[1].split("/")[0];f='http://www.google.com/search?hf=en&newwindow=1&q=+'+encodeURIComponent('site://'+s);a=function(){if(!window.open(f,'deliciousuiv5','location=yes,links=no,scrollbars=no,toolbar=no'))location.href=f+'jump=yes'};if(/Firefox/.test(navigator.userAgent)){setTimeout(a,0)}else{a()}})()

## Comments

**[luguo](#4052 "2008-08-19 17:41:50"):** cd后面什么都没有就是回到用户目录，无须"~"。

**[Amankwah](#4054 "2008-08-19 20:43:17"):** 嗯，我喜欢直接在命令行bash，哈哈~

