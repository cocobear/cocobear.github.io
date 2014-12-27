title: fc5下从源码编译安装gmplayer(mplayer)
tags:
  - Fedora
  - Linux
  - mplayer
id: 20
categories:
  - Linux
date: 2007-04-06 19:17:21
---

Thursday, 30\. November 2006, 13:35:22


也适用于fc系列,只是在一些细节上有些不同

[mplayer官方网站](http://www.mplayerhq.hu mplayer)

首先下载源码包，解码器，皮肤

源码包：http://www1.mplayerhq.hu/MPlayer/releases/MPlayer-1.0rc1.tar.bz2
解码器：http://www1.mplayerhq.hu/MPlayer/releases/codecs/all-20061022.tar.bz2
皮肤：http://www.mplayerhq.hu/MPlayer/skins/Blue-1.6.tar.bz2

确认你已经有上面的文件，建立一个临时目录：player,把上面的压缩文件解压到player中(假设你当前的目录为player)：
tar -jxvf MPlayer-1.0rc1.tar.bz2
tar -jxvf all-20061022.tar.bz2
tar -jxvf Blue-1.6.tar.bz2

 先安装解码器，直接拷贝到/usr/lib/codecs目录下面就可以:
mv all-20061022 /usr/lib/codecs
［注意：如果原来已经有codecs文件夹你可以考虑一下是否覆盖］

接下来开始编译源码包：
cd MPlayer-1.0rc1/
使用./confiure --help查看一下编译时的一些配置
这里偶就不贴出来了，N长！
根据你的情况选择一些东西，我这里用的是:
 ./configure --enable-gui --enable-largefiles --enable-menu --enable-smb --language=zh_CN --enable-xmms
如果没有错误继续
make
make install
其实make 的时间可能会挺长的，根据你机子的速度
如果./configure 的时候出现错误，一般是缺少一些库，你可以根据提示去rpmfind.net或者rpm.bhone.net下载并安装。

安装完成后输入mplayer应该不会有问题，但是如果要使用图形界面就得继续安装皮肤和字体

 一般输入gmplayer后会得到下面的提示：

[skin] 文件 (/usr/local/share/mplayer/skins/default/skin) 不可读

或者是找不到皮肤
不可以直接把Blue这个皮肤复制到上面提示的目录中，即使这样做了仍然会得到：

[skin] 文件 (/usr/local/share/mplayer/skins/default/skin) 不可读

的错误，也不是权限的问题，网上有很多提问关于上面的错误，偶自己也不知道为什么会这样，不知道哪位知道告诉偶一声。

以下是我的解决办法：
在~/.mplayer目录下面建立/Skin目录

把皮肤文件移动到default
mv Blue ~/.mplayer/Skin/default

此时执行gmplayer,你会看到亲切的蓝色皮肤了，不过应该还会有以下的错误：

New_Face failed.Maybe the font path is wrong.
Please supply the text font file(~/.mplayer/subfont.ttf).

解决办法：

随便找一个ttf字体拷贝到.mplayer目录下面就可以了，只要名字是subfont.ttf就可以了。

cd /usr/share/fonts/chinese/TrueType/
cp ukai.ttf /home/cocobear/.mplayer/subfont.ttf

OK,No problem!，这时候你可以随便找一个视频文件来播放了！

Enjoy it !

PS:原文我用的是essential-20061022.tar.bz2,没搞明白这个单的意思：essential（必要的），呵呵，所以大家还是装all-20061022这个包，全一点好：）

## Comments

**[BRN](#1202 "2007-07-23 22:03:29"):** 推薦這一篇,覺得寫的很清楚,所以每次linux重灌後,都跑來這看一下^^"

**[BRN](#1303 "2007-07-29 10:43:49"):** 恩 我是台灣人. 另外,跟原著說一下,skin的問題,只要把解壓縮出來的 skin內容複製,然後放入/usr/local/share/mplayer/skins/default 這樣就好了. ex:我用的是Blue,將裡面檔案複製起來(不是複製blue資料夾),放入/usr/local/share/mplayer/skins/default 這樣就好囉^^

**[cocobear](#1226 "2007-07-24 19:48:06"):** TO:楼上 兄弟台湾人吗？用繁体字?

