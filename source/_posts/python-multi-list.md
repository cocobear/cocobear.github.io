title: 一篇文章引发的思考
tags:
  - Python
id: 262
categories:
  - 编程相关
date: 2008-04-23 13:05:28
---

先贴一段代码:

	#!/usr/bin/env python

	import sys
	import random

	m = [[0 for x in range(128)] for y in range(128)]

	for i in range(1000):
	        m[random.randint(0,127)][random.randint(0,127)] = 1

	f = file('foo.pbm','w')
	sys.stdout = f

	print "P1\n128 128"
	for i in range(128):
	        for j in range(128):
	                print m[i][j],
	        print
	f.close()

再扯一扯题外话，[这里](http://blog.codingnow.com/2008/04/quasi-random_sequences.html)，有一个问题，"掷 30 次硬币，至少出现一次“连续 7 次正面”的概率有多少？"，本来以为挺简单的，不过似乎又不是那么简单，谁来算一算告诉俺怎么算。

上面的文章中有C写的一段代码，生成一个pbm的图像，挺有意思的，俺就顺手写了一个Python版的，调用了文件操作就不用手动重定向了。

上面的Python代码中用到了一句生成”二维数组“，这里用这个词有点不太准备，因为Python里面是没有数组的，只有list。

map1 = [[0 for x in range(128)] for y in range(128)]
看起来是一种比较简单的实现一个”二维数组“的方法，不过还有更简单的：

map2 = [[0]*128]*128
map1 == map2的结果是True，不过第二种定义方式存在问题，请看[这里](http://www.cnblogs.com/yoshow/archive/2007/05/15/747453.html)。

还有一种定义方式：
map3 = map(lambda _: [0] * 128, range(128))
这里lambda中 ”_“其实只是一个普通的变量，接下来就好理解了，这种方式也比较可靠。
## Comments

**[luguo](#3118 "2008-04-23 20:00:40"):** 怎么感觉最后那种方式走了Perl路线？？ ;-)

**[luguo](#3119 "2008-04-23 20:03:32"):** 还有，map是Python的内置函数啊！你怎么用它来命名list呢？

**[cocobear](#3121 "2008-04-23 20:08:35"):** Perl的路线？ 我觉得很pythonic的。

**[Amankwah](#3124 "2008-04-23 21:17:13"):** 现在专攻Python?

**[cocobear](#3125 "2008-04-23 21:36:08"):** Python是个好东东。

**[cocobear](#3122 "2008-04-23 20:12:58"):** 那个map变量已经改了:-)

