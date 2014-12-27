title: Fedora9中的gcc
tags:
  - gcc，F9
id: 282
categories:
  - 编程相关
date: 2008-05-22 16:18:31
---

Fedora9中默认安装的gcc版本是4.3，（Ubuntu8.04还只是4.2.1）由于gcc本身的变化，在Fedora9中源码编译一些软件的时候会出错，比如eva。我原来写的程序在gcc 4.2.1中可以顺利编译，今天在F9中测试的时候就出错了：

test.cpp:38: error: ‘memcpy’ was not declared in this scope

gcc的官方有针对这种情况的说明：
[http://gcc.gnu.org/gcc-4.3/porting_to.html](http://gcc.gnu.org/gcc-4.3/porting_to.html)
gcc 为了加快编译的速度，减少了对头文件的检查，因此得手动包含所有相关的头文件。这样做可以确保程序员在写代码的时候意识到自己需要哪些头文件，而不是交给编译器去处理。不过同时也带来了不少麻烦，许多以前写的代码都没办法在gcc 4.3中编译通过。

我在f8（VM虚拟机中）中使用gcc 4.2.1编译一个动态链接库时完全正常，但在F9（AMD64 Dou）中使用gcc 4.3编译就无法通过了提示：

/usr/bin/ld: test.o : relocation R_X86_64_32 against `a local symbol' can not be used when making a shared object; recompile with -fPIC
test.o: could not read symbols: Bad value

不知道这是gcc 4.3的问题，还是双核64系统的问题。只好再装一个低版本的gcc，下载了gcc 4.2.4的源码包，没想到编译时又出错了：
/usr/include/gnu/stubs.h:7:27: error: gnu/stubs-32.h: 没有那个文件或目录
又google了半天终于找到了[答案](http://groups.google.com/group/gnu.gcc.help/browse_thread/thread/e0e6913fdf385839)，缺少：glibc-devel-32bit，但是一直找不到和我系统glibc-devel（2.8）匹配的glibc-devel-32bit,只能找到一个2.5的rpm包，只好在安装时使用了--nodeps选项。

参考[这篇文章](http://www.linuxdiyf.com/bbs/thread-91900-1-1.html)，把新编译的gcc安装好，并且重新设置PATH，再次编译前面的程序，结果仍然一样，看来确实是64位系统的问题了。
## Comments

**[Kermit](#3272 "2008-05-25 15:07:24"):** 没用过，我还在默默地等待 OpenSuse 11。8、9月份的事情吧，呵呵

**[crazyfranc](#3312 "2008-05-28 10:50:31"):** 没想到GCC居然会忽略64位系统，汗

**[cocobear](#3250 "2008-05-23 10:09:41"):** 大家都没用过4.3，确实是有这样的问题，比较郁闷。

**[wind](#3247 "2008-05-23 02:45:25"):** 如果这样的话不是要造成以前的代码很多不可用？ 这种程度的不兼容应该是很严重的问题吧？gcc这次行动这么帅？

**[Amankwah](#3242 "2008-05-22 22:53:18"):** gcc4.3这么麻烦？

**[luguo](#3243 "2008-05-23 00:08:12"):** gcc 4.3把以前的警告提升为错误了？

