title: 再发个Python的小脚本
tags:
  - Python
  - url
id: 303
categories:
  - 编程相关
date: 2008-08-11 16:42:12
---

很多时候我们会遇到类似这样的字符串“%E5%8F%AF%E5%8F%AF%E7%86%8A”，如果你稍有经验就应该知道这是URL编码的汉字.

URL编码的产生是因为在URL只有少量的字符可以被使用:
"...Only alphanumerics [0-9a-zA-Z], the special characters "$-_.+!*'()," [not including the quotes - ed], and reserved characters used for their reserved purposes may be used unencoded within a URL."

[这里](http://www.blooberry.com/indexdot/html/topics/urlencoding.htm)有一篇介绍URL编码的文章。

为了方便把“%E5%8F%AF%E5%8F%AF%E7%86%8A”这样的字符串转换为我们可读字符串，我写了这个小工具；同时可以做反向的转换。例如把"可可熊"转换为“%E5%8F%AF%E5%8F%AF%E7%86%8A”。

做个示例,使用该程序在命令行下获取“\”的URL编码：
[cocobear@cocobear sn]$ ./url2read.py -r ""'\'""
GBK:
%5C
UTF8:
%5C

注意这里由于存在shell的解释，需要这样把“\”围起来。

这个脚本可以很好的在Linux与Windows下使用，贴代码：

	#!/usr/bin/env python
	#coding: utf-8 
	#author:        cocobear
	#version:       0.1

	import urllib
	import sys,getopt,re
	__doc__ = """Usage:
	            ./url2read.py -h
	            ./url2read.py -r ftp://cocobear.github.io/中国
	            ./url2read.py http://cocobear.github.io/%E4%B8%AD%E5%9B%BD
	        """

	def url2read(s):

	    s = urllib.unquote(s)
	    try: 
	            s = s.decode('utf-8')
	    except UnicodeDecodeError:
	            s = s.decode('gbk')
	    finally:
	            print s.encode(sys.stdin.encoding)

	def read2url(s):
	    head = ''
	    g = re.search('^(http|ftp://)(.*)',s)
	    if g:
	        head = g.group(1)
	        s = g.group(2)
	    gbk = urllib.quote(s.decode(sys.stdin.encoding).encode('gbk'))

	    utf8 = urllib.quote(s.decode(sys.stdin.encoding).encode('utf-8'))
	    if gbk == utf8:
	        print head+gbk
	        return 0
	    else:
	        print "UTF8:\n"+head+utf8
	        print "GBK:\n"+head+gbk
	        return 0

	def main(argv=None):
	    f = False
	    if len(sys.argv) < 2:
	        print __doc__
	        return 1
	    try:
	        opts,args = getopt.getopt(sys.argv[1:],"h,r",["help","reverse"])
	    except getopt.error,msg:
	        print msg
	        print __doc__
	        return 1
	    for o,a in opts:
	        if o in ("-h","--help"):
	            print __doc__
	            return 0
	        if o in ("-r","--reverse"):
	            f = True
	    for arg in args: 
	        if f:
	            return read2url(arg)
	        else:
	            return url2read(arg)

	if __name__ == "__main__":
	    sys.exit(main())


<a href="http://cocobear.github.io/code/url2read.py">代码下载
## Comments

**[luguo](#4029 "2008-08-11 17:37:00"):** 恩，这个简单易懂～

**[Amankwah](#4032 "2008-08-11 20:27:37"):** 这个不错~

**[shuge.lee](#4234 "2008-09-14 21:03:36"):** yuki@yukilib /home/share/code/python_note $ ./url2read.py http://www.shuge.org/中文 File "./url2read.py", line 12 SyntaxError: Non-ASCII character '\xe4' in file ./url2read.py on line 13, but no encoding declared; see http://www.python.org/peps/pep-0263.html for details yuki@yukilib /home/share/code/python_note $ 怎么回事？

**[可可熊](#4244 "2008-09-16 11:28:01"):** 使用参数-r，需要反转。

