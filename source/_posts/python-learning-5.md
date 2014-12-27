title: Python学习笔记五
id: 223
categories:
  - 编程相关
date: 2007-11-22 17:16:34
tags:
---

**高效的使用Python:**
	 (Mon,Tue,Wed,Thu,Fri,Sat,Sun) = range(7)
	Mon
	0
	 Sun
	6
比C语言里使用enum来实现要直观、简单多了吧！


	 li = [1, 9, 8, 4]
	 [elem*2 for elem in li]      
	[2, 18, 16, 8]
可以从右至左的看这个表达式!


	 params = {"server":"mpilgrim", "database":"master", "uid":"sa", "pwd":"secret"}
	 params.items()
	[('server', 'mpilgrim'), ('uid', 'sa'), ('database', 'master'), ('pwd', 'secret')]
	["%s=%s" % (k, v) for k, v in params.items()] 
	['server=mpilgrim', 'uid=sa', 'database=master', 'pwd=secret']
这个例子与上面的例子有点类似，只是更复杂了一些，Dictionary的items操作会返回一个由Tuple组成的List，可从第二、三行代码看出，然后再结合上面例子中给出的方法就可以实现这个快速、简单的解析。
Dictionary还有keys,values两个操作，它们都返回一个List。

def info(object, spacing=10, collapse=1):
上面这个函数定义，如果在C++里想在调用info函数时对第一个参数与第三个参数使用给定的值，是无法做到的，而在Python里则可以，比起C++里的重载更灵活！

info(collapse=15, object=odbchelper) 
说白了Python里的参数只不过是个Dictionary

	'' and 'b'  
	''
	'a' and 'b' and 'c'
	'c'


如果某个值为假，则 and 返回第一个假值；
所有值都为真，所以 and 返回最后一个真值，'c'。


	 'a' or 'b'          
	'a'
	 '' or 'b'  
如果有一个值为真，or 立刻返回该值；
如果所有的值都为假，or 返回最后一个假值。

	a = "first"
	b = "second"
	1 and a or b 
	'first'
	0 and a or b 
	'second'

上面这个例子是不是有点类似C语言中的三元运算呢？
## Comments

**[luguo](#2435 "2007-11-23 13:43:22"):** Python还能写2

**[luguo](#2436 "2007-11-23 13:44:29"):** -_-b 怎么小于号后面的都显示不出来？！留言居然不能转换大于小于号～～

**[cocobear](#2439 "2007-11-23 18:30:39"):** 应该是被过滤掉了，这种字符比较敏感。

**[Kermit](#2457 "2007-11-24 18:42:57"):** # luguo: 于07年11月23日 13点44分 辛苦敲键盘： -_-b 怎么小于号后面的都显示不出来？！留言居然不能转换大于小于号～～ 貌似很多blog都这样，呵呵。用个html的转义字符就行了。 我一般都在openoffice里把blog写好，再调整字体，存到html文件里，然后把源码复制到blog中，这样就可以有彩色的blog了，哈哈

**[Amankwah](#2459 "2007-11-24 21:31:45"):** 这样：>？

**[cocobear](#2460 "2007-11-24 21:48:17"):** 楼上写blog好麻烦啊。要是我的机子你写个blog得多久啊！

**[vvoody](#4097 "2008-08-26 13:12:12"):** 嘿嘿，三元运算不错，谢谢 ;-)

