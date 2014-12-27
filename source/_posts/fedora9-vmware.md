title: Fedora9安装vmware
tags:
  - Fedora
  - Linux
  - vmware
id: 279
categories:
  - Linux
date: 2008-05-19 09:32:25
---

刚开始公司的机子上没办法装Fedora8，只好用vmware在XP下装了个Fedora8，现在F9出来了，在本机装了一个，以后就可以不用进XP了，不过F9似乎有些不太稳定，听说vmware也有linux版的,找了一个新6.0.3.build.80004版本的vmware，没想到安装时又出问题了，似乎又是因为内核版本过高的原因。

[这里](http://bbs.archlinux.org/viewtopic.php?id=47704)有一个解决方案，有人根据最新的内核写的vmware的补丁。

首先下载：
http://blender6xx.ic.cz/pub/vmware/vmblock.patch
http://blender6xx.ic.cz/pub/vmware/vmmon.patch
http://blender6xx.ic.cz/pub/vmware/vmnet.patch
这几个补丁，然后安装使用vmware-install.pl 开始安装vmware，当编译出错时把这几个补丁打上去，需要打补丁的文件在：/usr/lib/vmware/modules/source/ ，有几个tar的包，先解压了，然后进入解压后的目录, 用
patch -Np1 < modulename.patch 打好补丁，然后再把打好补丁的包再打包一次。

完成后使用 vmware-config.pl 继续安装，中间可能会缺少一些库，如:
libX11.so.6
libXtst.so.6
libXext.so.6
libXrender.so.1
libz.so.1
都可以在rpmfind.net里找到。

接下来一切都会顺利进行。把windows下的Fedora8拷过来直接就可以运行了，呵呵。
## Comments

**[Amankwah](#3207 "2008-05-19 11:56:06"):** 可以考虑用VBOX，听说挺不错的？

**[kongove](#3262 "2008-05-24 10:14:26"):** 相当不错，听梅延涛分析什么无缝接入、内存释放、共享文件…… 我感觉用着确实很方便。

