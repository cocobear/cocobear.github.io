title: 读完了Programming in Lua (first edition)
tags:
  - Lua
id: 613
categories:
  - Life
date: 2009-04-08 16:19:11
---

花了差不多两周的时间读完了[Lua Programming in Lua (first edition) ](http://www.lua.org/pil/index.html)，没找到pdf格式的，一直在官网在线看的，不是很舒服。看的似乎有点慢，里面的英文很简单，讲的内容难点也不多，也没有太多的代码去动手实践，似乎是受别人打击了:-)（看到人家博客里写道：“花两天的时间过了一遍PIL”）。

Lua作为一个脚本语言在很多地方与Python有相似之处，不过小巧了很多，数据类型、表达式、语句等都很精简，但是这并不影响Lua成为一个流行的脚本语言，灵活的table成就了Lua的强大，table可以演化出list,dict，更强大的是table可以实现OO，Lua并没有在语言级提供OO的实现，不过Lua的作者在[PIL中](http://www.lua.org/pil/16.html)展示了使用table完全可以实现近乎完美的OO编程，我觉得这是对整天鼓吹OO思想的人一个极大讽刺，OO其实没什么，C语言中使用结构体，函数指针也可以有模有样的进行OO编程。

Lua中也有很多函数式编程语言的影子，所以很多人把Lua和Scheme归为了近亲，不过俺不了解Scheme，对函数式编程也是一知半解，只能凑个热闹。

Lua之所以在游戏中使用比较广泛，是因为它可以很好的和C/C++结合在一起，可以很方便的互操作，通过使用Stack的方式。还有一个很重要的原因，Lua提供的是搭积木的原料，而不是搭好的建筑，所以你可以很灵活的用自己的方式去写程序。比如你可以用table实现一个完整的OO系统，然后在OO上面写程序(这里有一点很重要，基于table实现OO是很轻量的，而不是语言级的OO)；你也可以用metatable来实现Python中很强大的filter,map,reduce。

Lua中的table有点类似道家学说里面的"道","道生一，一生二，二生三，三生万物"，而table就是Lua中的“道”。

还得提一点Lua是完全用ANSI C编写的。

     cocobear@0-0 /home/cocobear/Codes/lua-5.1.4/src $ wc *.c *.h -l | tail -1> 
      17002 total
## Comments

**[草儿](#5694 "2009-04-09 09:11:12"):** 00?什么是00？

**[luguo](#5722 "2009-04-12 22:16:37"):** 我花了一个下午就把这本书的前三部分给看完了。。。。。 因为有饭局，所以第四部分没看完就赶场子去了。。。。 我是不是有点bt？ 另外，我是坐在书城里白看的。。。。-_-

**[Amankwah](#5727 "2009-04-14 07:47:07"):** 呵呵，都玩Lua了啊～确实好东西

