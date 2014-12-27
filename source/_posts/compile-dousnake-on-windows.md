title: 编译了个Win下的DouSnake
id: 217
categories:
  - 编程相关
date: 2007-11-02 09:34:17
tags:
---

SDL是跨平台的开发库，因此移植到WIN下只需要重新编译一次就可以了，源代码我基本没做修改，只是把SDL的包含方式改为了：

#include <SDL/SDL.h> 

因为我不知道在WIN下如何使用-I/usr/include/SDL

使用的是Dev-C++ 4.9.2.2编译的，还做了个图标，呵呵，练练手，为了SuperMario的WIN版，没办法啊，有人期待WIN的啊！

[源码下载](http://cocobear.github.io/code/tar/DouSnakeSrc_win.rar)
[游戏下载](http://cocobear.github.io/code/tar/DouSnake_win.rar)
## Comments

**[crazyfranc](#2236 "2007-11-04 18:51:34"):** 老大说话真含糊

**[Amankwah](#2198 "2007-11-02 10:26:21"):** 你用SDL是从什么弄起的？

**[cocobear](#2199 "2007-11-02 12:01:38"):** 从什么弄起？什么意思？

