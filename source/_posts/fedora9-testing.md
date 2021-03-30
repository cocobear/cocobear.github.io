title: Fedora9试用
tags:
  - Fedora
  - Linux
id: 277
categories:
  - Linux
date: 2008-05-15 14:59:34
---

刚在公司电脑上装了Fedora9，安装完成后发现X显示有问题，重启后进入X时显示器提示“不支援输入”，这个显示器是AOC的，查了下这种情况应该是刷新率与显示器的冲突吧，但是Fedora9中xorg.conf 文件已经不包含屏幕分辨率、刷新率的配置，而是自动进行检测，本来应该说是更智能了，但遇到这样的情况都没办法手动解决问题了，按照以往的经常，我把机子上Ubuntu的xorg.conf 文件拷了过来，覆盖掉原来的文件，X终于正常了，但屏幕的分辨率变为了：1152x864，刷新率只有60Hz，而且使用Ctrl+Alt+num 切换到其它终端时显示“无信号”，太奇怪了。用了Fedora系统这么久还是头一次遇到这样的情况。

现在正在下ATI 的显卡驱动，不知道会不会解决这个问题。

在[这里](http://ati.amd.com/support/drivers/linux64/radeonprevious-linux64.html)下载驱动，安装好后可以设置正确的分辨率，1280x1024，但是其它终端仍然“无信号”，而且ati的那个控制中心没办法使用。

找到一篇[文章](http://blog.hxsd.com.cn/blog/linlin911911/article/i28813.html)，正在使用上面的方法重新安装驱动，里面有一点:yum install kmod-fglrx要加--enablerepo=livna-testing参数,，否则是无法找到kmod-fglrx这个包的。

公司的网速好慢……
## Comments

**[cocobear](#3176 "2008-05-16 10:37:10"):** 就是用的官方的驱动，好像是内核版本过高的问题，郁闷。

**[cocobear](#3177 "2008-05-16 10:38:27"):** 8.04我也装了，到是没有这个问题，不过还是习惯用Fedora了。

**[dream](#3183 "2008-05-16 18:41:36"):** 想订阅你的博客，不过没找到RSS或ATOM， 你有什么联系方式吗？..即时的

**[crazyfranc](#3180 "2008-05-16 15:11:24"):** 据说网络设置也很让人不爽.格了装回8吧.

**[luguo](#3174 "2008-05-15 18:32:25"):** 你动作真快～！我还没下呢～

**[Amankwah](#3175 "2008-05-15 18:56:15"):** 用显卡的官方驱动会好些，应该。 不过还是Ubuntu8.04的体验好得很，你可以试试，哈哈。

**[草儿](#3187 "2008-05-16 21:25:29"):** 就你和葫芦装F9了，偏偏你们俩都说用着不爽，连带我也不敢尝鲜了…… 继续F7。

**[cocobear](#3197 "2008-05-18 10:11:41"):** 我用GTALK： cocobear.cn[dot]gmail[x]com RSS可以看到吧？ 我用opera直接提示feed的地址的，用抓虾也可以看到。
