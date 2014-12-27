title: Linux下rar文件解压错误
tags:
  - Linux
  - rar
id: 51
categories:
  - Linux
date: 2007-04-06 19:18:37
---

Monday, 26\. March 2007, 07:00:56

昨天下了本电子书，是rar压缩的，打开时提示出错，无法解压，缺少lib什么的，还以为是文件下载出错了，于是重新换了个地方又下载了一遍，结果还是同样的错误。觉得可能出在本机rar版本的问题，从网上下了一个 Linux下较新的版本3.6,安装后可以正常解压，给大家提个醒，有些网站可能为了提高压缩比(也许是)，使用了较高版本的rar，大家都不能正常解压的，可以试着更新一下rar的版本。

[rar 3.6 for linux](http://www.rarlab.com/rar/rarlinux-3.6.0.tar.gz)

一个有趣的项目：

[http://www.physics.ox.ac.uk/jpc/Demo.html](http://www.physics.ox.ac.uk/jpc/Demo.html)

前提是你的浏览器支持java

ps: firefox支持java,在.mozilla/plugins/目录下做个/XXX/jre/plugin/i386/ns7/libjavaplugin_oji.so的链接。