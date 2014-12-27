title: 前几天写了写Python
tags:
  - Python
id: 273
categories:
  - 编程相关
date: 2008-05-10 10:23:10
---


	#!/usr/bin/env python

	import sys
	import os
	import re

	TEST = "test"
	def find_dir(original_dir,l):
	        while True:
	                for f in os.listdir(original_dir):
	                        #print f,
	                        f = original_dir+"/"+f
	                        if os.path.isdir(f):
	                                l.append(f)
	                                find_dir(f,l)
	                return 

	def file_copy(original_dir,num):
	        global TEST
	        while num:
	                test_dir = TEST+str(num)
	                for f in os.listdir(original_dir):
	                        #print f,
	                        f = original_dir+"/"+f
	                        if os.path.isfile(f):
	                                print "cp",f,re.sub(original_dir,test_dir,f)
	                l = []
	                find_dir(original_dir,l)
	                for i in l:
	                        for j in os.listdir(i):
	                                j = i+"/"+j
	                                if os.path.isfile(j):
	                                        print "cp",j,re.sub(original_dir,test_dir,j)
	                num-=1
	                print 

	def dir_copy(original_dir,num):
	        global TEST
	        while num:
	                print "mkdir",
	                test_dir = TEST+str(num)
	                l = []
	                find_dir(original_dir,l)
	                for i in l:
	                        print test_dir,re.sub(original_dir,test_dir,i),

	                num-=1
	                print
	        return

	def main(argv=None):
	        if argv is None:
	                argv = sys.argv
	        if len(sys.argv) != 2:
	                return(usage())
	        num = 5
	        print "echo \"Starting create directories\""
	        print "date +%T.%N"
	        dir_copy(argv[1],num)
	        print "echo \"Starting copy files\""
	        print "date +%T.%N"
	        file_copy(argv[1],num)
	        print "echo \"Recursive directory stats\""
	        print "find . -print -exec ls -l {} \\;"
	        print "du -s *"
	        print "date +%T.%N"

	        print "echo \"Scanning each file\""
	        print "find . -exec grep kangaroo {} \\;"
	        print "find . -exec wc {} \\;"
	        print "date +%T.%N"

	def usage():
	        print "\n%s [Your File name]\n" % sys.argv[0]
	        return 1

	if __name__ == "__main__":
	        sys.exit(main())


帮小林子写的测试文件系统性能的，递归目标目录，进行文件复制操作，这个脚本只是为了生成相应的shell脚本。

还有一个公司用的脚本，就不贴了。
前天在公司写程序，用C语言，发现手生的很，觉得要是用python写的话几行就搞定的，C得写半天，郁闷啊，俺被python给害了……

昨天在公司装了个ubuntu8.04，二十分钟就装好了，等F9出来后再装个F9，^_^

BTW：scim-python这个输入法不错，无论你用拼音还是五笔都是一个不错的选择。
## Comments

**[crazyfranc](#3159 "2008-05-10 20:06:08"):** 你们公司是搞python的？

**[luguo](#3160 "2008-05-10 21:26:20"):** 咋用python去生成bash脚本而不是去直接写呢？

**[cocobear](#3173 "2008-05-14 18:37:01"):** python写顺手了，感觉shell在许多地方比较“怪异”，呵。

