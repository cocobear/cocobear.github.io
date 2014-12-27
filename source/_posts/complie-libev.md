title: libev编译篇
tags:
  - C
  - libev
id: 508
categories:
  - 编程相关
date: 2009-02-07 23:17:13
---

先给出libev的主页[http://software.schmorp.de/pkg/libev.html](http://software.schmorp.de/pkg/libev.html)，libev是一个高性能的事件驱动模型，与libevent类似，不过设计更为小巧，简洁。

[libevent](http://en.wikipedia.org/wiki/Libevent)有成功的应用--memcached，libev是一个比较新的项目，代码比较少，所以这几天来研究下这个。

首先从CVS中下载最新的代码：

     cvs -z3 -d :pserver:anonymous@cvs.schmorp.de/schmorpforge co libev
CVS代码中有autogen.sh文件，执行该文件会生成configure，我机子上出了点问题，需要首先运行
     automake --add-missing
然后就是./configure && make && make install 了。
安装好后会有一个提示：
     Libraries have been installed in:> 
        /usr/local/lib> 
     
     If you ever happen to want to link against installed libraries> 
     in a given directory, LIBDIR, you must either use libtool, and> 
     specify the full pathname of the library, or use the `-LLIBDIR'> 
     flag during linking and do at least one of the following:> 
        - add LIBDIR to the `LD_LIBRARY_PATH' environment variable> 
          during execution> 
        - add LIBDIR to the `LD_RUN_PATH' environment variable> 
          during linking> 
        - use the `-Wl,--rpath -Wl,LIBDIR' linker flag> 
        - have your system administrator add LIBDIR to `/etc/ld.so.conf'> 
     
     See any operating system documentation about shared libraries for> 
     more information, such as the ld(1) and ld.so(8) manual pages.

需要注意一下，因为默认这个库安装在了/usr/local/lib/里，所以运行程序时有可能会提示找不到libev.so这个动态库。我们需要在编译时加上-LLIBDIR参数，然后设置LD_RUN_PATH这个环境变量。

安装好为了测试libev，可以使用[lighttz](http://www.zenebo.com/word/asynchronous-programming/lighttz-a-simple-and-fast-web-server/)，其实我是从这里知道的libev，下载C文件，使用下面的命令来编译：

     LD_RUN_PATH=/usr/local/lib/> 
     export LD_RUN_PATH> 
     gcc -LLIBDIR -o lighttz lighttz.c -lev