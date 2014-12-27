title: 看一段python的代码
id: 256
categories:
  - 编程相关
date: 2008-04-29 21:43:38
tags:
---


	def xselections(items, n):
	    if n == 0:
	        yield []
	    else:
	        for i in xrange(len(items)):
	            for ss in xselections(items, n-1):
	                yield [items[i]] + ss
	for l in xselections(xrange(10), 3):
	    print l

上面这段代码实现了从x数中取出y个数的全排列，xselections的实现用到了递归和yield，yield这个语句比较有意思：
“yield跟return一样会返回值，但是不一样的是return之后就退出函数了，而yield在丢出一次值给调用者之后还会返回继续运行生成器函数，并随着yield的再次调用丢出随后的生成值”
## Comments

**[luguo](#3138 "2008-04-30 13:05:46"):** 恩，以前见识过yield的威力了～～

