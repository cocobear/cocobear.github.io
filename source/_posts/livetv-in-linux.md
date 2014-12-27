title: linux下的网络电视
tags:
  - Linux
  - sopcast
id: 21
categories:
  - Linux
date: 2007-04-06 19:17:24
---

Thursday, 30\. November 2006, 13:38:53

一些从windows下转来的用户可能要问到linux下有网络电视软件吗？回答是肯定的，这里介绍一款比较不错的网络电视：sopcast，主页是：http://www.sopcast.com/cn 

官方的windows版本已经出到了1.0.1版了，但是linux版只有0.9.6版的，官方似乎也提供了gui界面的sopcast但是偶没有用，好象不是很好安装似的，推荐一个比较好的gui，gsopcast（a 
gtk front-end of p2p TV 
sopcast)，作者主页：http://lianwei3.googlepages.com/home2 

下面是偶自己fc5安装的过程： 

下载最新版本的gsopcast: 
[http://lianwei3.googlepages.com/gsopcast-0.2.9.tar.bz2](http://lianwei3.googlepages.com/gsopcast-0.2.9.tar.bz2) 

以及sp-sc: 
[http://download.sopcast.org/download/sp-sc.tgz](http://download.sopcast.org/download/sp-sc.tgz) 

安装gsopcast: 

#tar jxf gsopcast-*.tar.bz2 

#cd gsopcast-* 
#cd src  [这个文件需要有root权限]

#make 

#make install 

安装sp-sc： 

直接把压缩文件解压，把sp-sc文件复制到/usr/bin或者/usr/local/bin中，只要是PATH中包含的路径就可以了。 
如果提示缺少stdc++5库，从以下地址下载： 
[http://www.sopcast.com/download/libstdcpp5.tgz](http://www.sopcast.com/download/libstdcpp5.tgz) 

安装完成后在终端输入： 
gsopcast就可以看电视的（播放列表的显示根据网络情况得一些时间），选择你喜欢的频道双击就可以播放了。当然，前提是你要有一款播放器的，这里推荐mplayer. 

ps:在这里可以下载中文补丁包：http://gxgg2000.51.net/softshare/web/gsopcast.htm
