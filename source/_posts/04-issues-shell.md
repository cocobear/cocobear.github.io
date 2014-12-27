title: shell脚本解题3
tags:
  - awk
  - sed
  - Shell
id: 291
categories:
  - 编程相关
date: 2008-07-16 13:15:42
---

**问题：**
如何将命令的输出信息按行放入到数组里面?

**解法：
1.**
	n=0
	while read line;do 
	        array[$n]="$line"
	        ((n++))
	done < <(traceroute 192.168.1.1 -n)

	echo ${array[0]}


问题：
sed 可以同时匹配多个条件？

比如
file:
AAA  BBB  CCC DDD
AAA  BBB  DDD
CCC DDD 

sed能实现，同时匹配AAA和CCC就打印,在一条命令中

解法：
1.
	sed -n '/AAA/{/CCC/p}'

同时匹配kobe和james:
	sed -n '/kobe/{/james/p}'        
	awk '/kobe/&&/james/{ print $0 }' 

匹配kobe或james:
	sed -n '/\(kobe\|james\)/p'
	awk '/kobe/||/james/{ print $0 }'
	seq 5|sed '$!N;$!D'
## Comments

**[luguo](#3834 "2008-07-16 16:33:19"):** 恩，第一个中那个重定向用得不错。 我的第2个的解法： perl -ne 'print if /AAA/ && /CCC/;' test.txt

**[wind](#3876 "2008-07-17 19:48:30"):** 娃最近怎么天天脚本呢？

**[cocobear](#3887 "2008-07-18 09:18:57"):** 那你说我做什么啊。这儿有个驱动，你帮我写写吧。我不会。

