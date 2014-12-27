title: shell脚本解题5
tags:
  - awk
  - Shell
id: 295
categories:
  - 编程相关
date: 2008-07-24 17:37:36
---

问题：
随机输出目录下5个文件：

**解法：
1.**

	#! /bin/bash
	arr=($(ls *.sql.gz))
	len=$((${#arr[@]}+1))

	for((i=1;i&lt;6;i++))
	do
	        RANDOM=$(($$+i))
	        echo ${arr[$((RANDOM%len))]}
	done
这种方法有可能会产生重复的文件。
**2.**
	ls * 'BEGIN {srand()} {A[NR]=$0;} END {for(i=0;i&lt;5;++i) { n=int(rand()*FNR)+1; print A[n]} }'
同样这个也有可能重复。
**3.**
	ls *sql.gz | awk 'BEGIN{srand()}{a[rand()" "NR]=$0}END{for(i in a) {print a[i];if(++j==5) break}}'

将600个同一目录下的文件随机分成3份，每份200个：

**解法：
1.**
	ls | awk 'BEGIN{srand();b[1]="path1";b[2]="path2";b[3]="path3"}{a[rand()" "NR]=$0}END{for(i in a){if(k++%200==0) j++;print "mv "a[i]" "b[j]}}' | sh

对这样的代码进行缩进:
	void func1()
	{
	if()
	{
	printf();
	}
	return;
	}

	void func2()
	{
	if()
	{
	printf();
	}
	return;
	}

**解法:
1.**
	awk '/}/{flag--}{if(flag>0) for(i=1;i< =flag;i++) printf "\t";print}/{/{flag++}'

打印字符串hello上下两行,不允许使用grep.

解法:
1.
	awk -v n=2 '{a[NR%n]=$0}/hello/{for(i=NR-n+1;i< =NR;i++) if(i>0) print a[i%n];i=0;while(i<n && getline){print;i++}}'

查看23号15点到24号13点之间的系统日志：
sudo cat /var/log/messages | awk '$2" "$3 >= "23 15:00:00" && $2" "$3 <= "24 13:00:00"' 