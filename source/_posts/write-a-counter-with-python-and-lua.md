title: Python与Lua分别实现一个计数器
tags:
  - Lua
  - Python
id: 591
categories:
  - Life
date: 2009-03-31 12:47:15
---




```
	>   function newCounter ()
	>>     local i = 0
	>>     return function ()   -- anonymous function
	>>              i = i + 1
	>>              return i
	>>            end
	>>   end
	>   
	>   c1 = newCounter()
	> print(c1())
	1
	> print(c1())

2
```


Lua同Python类似，也可以嵌套定义函数，不过Lua嵌套函数可以访问上层闭包函数的局部变量，而在这个内嵌函数中这些变量不是全局变量，也不是局部变量，而是一种upvalue，与C语言中的static修饰的变量类似，所以在这里可以利用这个特性完成这个计数器。

Python:

```
  def counter(last=[1]):
  ...     next = last[0] + 1
  ...     last[0] = next
  ...     return next
  ... 
  counter()
  2
  counter()
  3
```

今天看Lua的时候觉得Python也应该有比较简单的办法写个计数器，找到[一篇文章](http://www.ibm.com/developerworks/library/l-pycon.html)介绍这个小技巧，其中有一句话：

     However, if named parameters are given mutable default values, the parameters can act as persistent memories of previous invocations. Lists, specifically, are handy mutable objects that can conveniently even hold multiple values.

Lua作为一个嵌入式（嵌入到其它语言中）的脚本语言，在很多地方与Python类似，这几天在看Programming in  Lua 。
     this is quote text
## Comments

**[草儿](#5626 "2009-03-31 19:25:41"):** 靠，你的文章对我来说越来越深奥了，都快成天书了。

**[tocer](#6659 "2009-11-04 11:11:44"):** 用 itertools.count 算不算？ from itertools import count counter = count(1) for i in range(10): print counter.next()

**[cocobear](#6663 "2009-11-05 14:42:43"):** 没用过这个库 不错

