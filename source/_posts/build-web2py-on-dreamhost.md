title: web2py--在DreamHost上搭建环境
tags:
  - Python
  - Web
  - web2py
id: 416
categories:
  - 编程相关
date: 2008-12-11 16:36:26
---

Python做Web开发有很多框架，例如django，webpy，web2py......，大概比较了下，觉得[web2py](http://mdp.cti.depaul.edu/)挺合适做小一点的东西，[这里](http://mdp.cti.depaul.edu/examples/static/web2py_vs_others.pdf)有一份官方给出的web2py与其它web框架的比较，我也是因为看完了这个比较后决定学下web2py的。

首先得搭好环境，web2py是以WSGI方式运行，所以可以很方便的部署在DreamHost上，我在wiki里记录了下我搭建的过程，可以在[这里](http://cocobear.github.io/wiki/doku.php?id=web2py)看到。

网上没找到有web2py做的完整的东西，比较郁闷，想做个网站，用PHP要完全从头做，太麻烦，相应的框架我又不想去学，Python框架很多，Django好像有点麻烦，webpy不太了解，也不知道有没有成熟的应用，web2py目前看着挺不错的，可惜没什么可参考的。
## Comments

**[luguo](#4667 "2008-12-12 02:35:06"):** 一直对web编程没兴趣的说～～～～

**[可可熊](#4682 "2008-12-15 09:59:39"):** 俺是做不了的，web编程俺只能做出来玩具:-) 老大有没有认识小MM做网页漂亮点的，还懂JS，CSS的，俺想做个网站。

**[Amankwah](#4674 "2008-12-13 00:11:35"):** 你做个好用的博客程序，我们一起换？

**[liuxin](#4780 "2009-01-05 10:58:57"):** 不试试Google AppEngine的webapp，挺不错的，虽然没有django之类的快捷开发， 不过比ASP、PHP之类的，还是强很多的，有心的话，不妨基于webapp开发个MVC框架

**[可可熊](#4781 "2009-01-05 11:12:56"):** 本来想折腾折腾的，不过最近事多，就没折腾。不过做WEB开发俺少个美工:-(

**[sc](#4840 "2009-01-12 14:59:49"):** django还是不错的，成熟度和开源的数量都相对不错 web2py据说可以实现多数据库支持，方式可以django更好些，还有db合并，sum( count(field)等。 不过还在观望中。

