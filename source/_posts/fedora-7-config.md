title: F7安装后的配置
tags:
  - config
  - Fedora
id: 130
categories:
  - Linux
date: 2007-06-07 02:00:09
---

3j001-qxcpos-f2ney640-1-f2nffdow-38
不知道大家安装完后做的第一件事是什么，我做的第一件事是调整scim的激活热键，因为打算搜索个东西，结果发现没办法用快捷键把scim调出来，于是在“scim设置”－－“全局设置”中添加“开关键”，设置为ctrl+space。顺便删除一些没用的输入法、语言。

如果你不喜欢scim，想要安装fcitx，那么首先要做的是

rpm -qa | grep scim

然后删除scim相关的软件包，kill掉所有的scim进程，

killall -o scim*

接下来就可以安装fcitx了，你可以选择源码编译，也可以直接使用二进制包，我使用的是二进制包，但是安装后无法激活，找了些相关的文章，但仍然没有成功，就没再继续研究下去，直接用scim了，反正以前不用scim是因为它有些bug，但是F7里暂时没发现，就将就着用；而且正好使用scim不用担心和opera有冲突了。

第二件事就是源加livna、freshrpms的yum源：

rpm -ivh http://rpm.livna.org/livna-release-7.rpm

rpm -ivh http://ftp.freshrpms.net/pub/freshrpms/fedora/linux/7/freshrpms-release/freshrpms-release-1.1-1.fc.noarch.rpm

或者你可以使用fedora-cn的镜像源：

rpm -ivh ftp://ftp.fedora.cn/pub/fedora-cn/linux/7/i386/fedora-cn-release-7-2.y0.noarch.rpm

因为我装许多软件都使用的是yum，所以刚装完系统就把这些源设置好。

接着安装必备的软件：

浏览器是必不可少的，当然F7已经自带有firefox2.0，不过我还是喜欢opera，所以从我以前的软件备份里找到opera，使用rpm包安装。上面的几个源里似乎没有opera，所以如果要安装opera的话可以去[opera官方](http://www.opera.com)下载。

yum install chmsee	//chm阅读器也是不可缺少的

yum install bluefish	//一个不错的web编辑器，不过我目前似乎更习惯使用vim

yum install xmms xmms-mp3 xmms-wma

这个不用说了吧，要注意的是后面两个必须装的，不然无法播放歌曲。

这里顺便提一点，F7中的xmms菜单已经可以完美显示中文了，歌曲列表只要你在“首选项”－－“字体”中选择合适的中文字体就可以了。

yum install mplayer mplayer-gui mplayer-fonts mplayerplug

建议上面几个都装上，尤其是最后一个可以使firefox在线播放百度的mp3，还有其它一些在线视频。

yum install stardict

星际译王，也少不了的，不过词典似乎无法安装，我是直接使用以前的软件备份中的rpm包安装的，可以在[星际译王主页](http://stardict.sourceforge.net)去下载。

yum install rar		//不可缺少的一个。

yum install gvim	//似乎安装的时候没有这个

yum install eva		//如果你觉得QQ对你来说还是必须的话。

到此为止，该装的软件应该装完了，接着应该对系统进行一些配置、优化。有关F7启动项目的优化可以参考下面的文章：

http://www.linuxsir.org/bbs/showthread.php?t=304624

或者参考我以前写的[加快Linux的启动速度](http://cocobear.github.io/?p=89)一文。

关于字体我觉得F7已经做得非常好了，可以不做任何修改，可能只有在opera下面字体不是很好，可以通过安装新的字体来解决。笔记本推荐使用雅黑字体。

F7默认的主题、图标都挺不错的，如果你想继续美化的话可以去[FOR GNOME](http://www.gnome-look.org)或者[FOR KDE](http://www.kde-look.org)看看。
## Comments

**[cocobear](#318 "2007-06-11 00:15:20"):** **TO：草儿** 我这里好着了啊，是不是你少了源。 或者你可以直接去livna下。 那个我看过，仍然不行。

**[草儿](#326 "2007-06-11 17:56:34"):** **Re:cocobear** 那个livna的源我装了，可能是插件名打错了，等下我直接去看看

**[草儿](#316 "2007-06-10 23:31:28"):** 那个mplayer在线播放插件安装时应用 yum install mplayer mplayerplug-in， 照你所说的yum install mplayerplug会出现找不到提示的 opera里实现在线播放的可以参照一下： http://zh.gentoo-wiki.com/HOWTO_Make_mplayerplug-in_work_with_Opera 我没试，有时间的话可以试一下

**[DDR物语](#2006 "2007-10-17 07:13:23"):** 有linux下的rm或rmvb播放器吗？

**[DDR物语](#2007 "2007-10-17 07:14:09"):** 我新装的F7播放不了rm或rmvb 提示没有安装相应的插件

**[cocobear](#2008 "2007-10-17 12:15:10"):** 可以看我写的Mplayer的安装，要从官方下一个解码器包。

**[DDR物语](#1971 "2007-10-13 23:37:47"):** 呵呵……谢谢学长啊！节省了我很多时间啊！

**[komac](#1954 "2007-10-12 03:34:26"):** 我喜欢用ubuntu。不过FC系列以前一直用。后来发现ubuntu的apt机制真的很不错，我是个懒人，就此就蜗居在ubuntu的屋檐下再也不想换了。

**[cocobear](#1976 "2007-10-14 20:22:45"):** 呵呵，也方便我忘了去查阅，两全其美:-) 俺记性不太好。

