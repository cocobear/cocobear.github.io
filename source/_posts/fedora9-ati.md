title: Fedora中ATI显示驱动问题
tags:
  - ATI
  - Fedora
  - Linux
  - ssh
id: 285
categories:
  - Linux
date: 2008-05-30 16:25:34
---

Fedora9中由于Xorg版本过新的原因（刚开始我还以为是内核版本过新的原因），最新的ATI显卡驱动没办法使用，因此3D效果无法开启，在我的机子上不仅如此，而且连在runlevel 5 下使用Ctrl+ALT+F1这样切换终端都出现问题（不支援）。

忍了几天，今天终于解决掉了，我是按照**[这篇文章](http://www.fedoraforum.org/forum/showthread.php?t=189227)**来做的，既然Xorg版本过新，那就直接换个低版本的Xorg（Linux不就有这个好处吗^_^）。

详细的我就不写出来了，说下大概过程：
首先是为系统添加Fedora8的源，可以直接下载[这个文件](http://www.linux-ati-drivers.homecall.co.uk/fedora8.repo)，然后在Fedora9 yum配置文件中把xorg*  linuxwacom*  rhpxl*  mesa*/" 相关的软件包排除掉，卸载Xorg，重新安装一次，这时由于Fedora9中已经排除了Xorg相关的软件包，因此yum就会从Fedora8的源中安装这些包，这样就得到了downgrade的效果。

文章的最后还给出来如果恢复到系统默认Xorg的办法，这里就不提了。

现在我的Fedora9完全没有问题了，^_^

btw:记个东西，ssh到内网的电脑：
首先得有个公网的IP，然后在内网（我公司的电脑）的电脑上使用下面命令：
[cocobear@cocobear ~]$ ssh -f -N -R 1986:localhost:22 cocobear@208.113.171.94
（208.113.171.94这个IP是我网站的IP，一会儿会提到为什么用这个IP）
接着在有公网IP的电脑上登录这台内网机器：

因为我另一边要在宿舍来连公司的这台电脑，而宿舍的电脑也在内网，因此需要使用一个具有公网IP的跳板电脑，我先在宿舍ssh到我这个网站的服务器上：
[cocobear@cocobear ~]$ ssh -l cocobear cocobear.github.io
然后在服务器上连接
[crystallight]$ ssh cocobear@localhost -p 1986
呵呵，这样就可以从宿舍电脑ssh到公司了，一会儿回宿舍看看公司这边电脑东西下完了没^_^
## Comments

**[kongove](#3354 "2008-06-02 12:41:01"):** dreamhost确实不错，上面有好多软件可以方便使用。 用w3m在上面浏览网页也不错，懒得用代理。

**[crazyfranc](#3347 "2008-06-01 12:06:35"):** TO DREAM： DREAMHOST 不贵的。

**[dream](#3342 "2008-05-31 20:02:37"):** 羡慕啊，有dreamhost很贵吧，我也很想有一个～～：）

**[Amankwah](#3336 "2008-05-31 00:08:53"):** 这个ssh方案好！可以对实验室不适用~

**[luguo](#3334 "2008-05-30 22:42:52"):** 行啊，用dreamhost做中转啊～！

