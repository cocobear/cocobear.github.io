title: 折腾Archlinux
tags:
  - archlinux
  - xorg
id: 567
categories:
  - Linux
date: 2009-03-11 15:37:14
---

公司新拿来几台电脑，头让俺换一台用用，趁这个机会就想装个Archlinux试试，没想到折腾两天了，还是用不起来。

整个硬盘用ghost对拷了一下，Windows分区拷过去都正常，以前用的主Linux分区无法使用，以前这个分区就有问题，使用正常，但从Windows下无法访问，而且引导文件也放在另一个ext3分区上。

用光盘安装的Archlinux，300多M，base很快就安装好了，pacman也用的挺顺手，不过记得要加几个国内速度快一点的源，然后使用rankmirrors来重排一下源的顺序。不过在安装X的时候出问题了（我主板集成HD3200显卡），折腾了好久，手动改了几次xorg.conf文件，最后显卡驱动用xf86-video-ati，在xorg.conf文件里加上了1680x1050这个分辩率。好不容易(openbox+fbpanel+rox)启动后分辩率正常了，也可以自由切换到其它终端，但是安装ibus的时候又出问题了，使用的是yaourt，实在是折腾累了，浏览器里的字体也很不好看，uxvrt不好用，不能粘贴，不能用标签，字体怪怪的也不知道怎么改，问题一大堆啊，有点像刚用Fedora的时候，要真正能使用系统得折腾好久，但现在实在是懒得折腾了，看着[人家](http://picasaweb.google.com/kekenba/Openbox#5263326767131605058)漂亮的Archlinux桌面，只能叹息了。

俺自己的那台电脑更是郁闷，是Intel GMA 3100的显卡，本来说Linux应该支持挺好的，但在Archlinux里安装xf86-video-intel驱动后，X启动报错，如果使用vesa驱动能进入X，但是切换到终端再切换回X后就花屏了，昨天折腾了一晚上也不搞定，看来只能回到Fedora了。

不过话说回来，Archlinux速度确实挺快的，连启动X估计就10秒多点，Archlinux+openbox这样轻量级的组合确实不错，如果能在细节上完美些的话还真不想换回Fedora了。

切换到Windows下写的这篇文章，一会儿再整整Archlinux，不过俺自己那台机器要能让X在Archlinux上跑起来估计悬！
## Comments

**[Amankwah](#5638 "2009-04-01 21:10:08"):** 能够折腾的日子，真的幸福，羡慕一下～

**[volans](#5447 "2009-03-11 17:58:28"):** 我靠，太巧了，我也在鼓捣arch，往t43上装……

**[草儿](#5449 "2009-03-11 20:00:31"):** 靠，终于见你露头了。 你上次说什么测试终端QQ是什么意思？又捣鼓出一个什么玩意儿？

**[Fung](#6184 "2009-06-28 21:37:12"):** 我也在用arch，第一次失败了，后来在不得不在CentOS下看了好几天的教程，现在忍不住手痒又装了，不过幸好这次成功了，我的是arch+fluxbox，nvidia的显卡,xorg.conf也是弄了半天，后来还是直接拷的centos里的，然后在那个基础上改的。终于还是成功了，挺高兴的，效果还不错。 一直知道你编程很牛，希望有机会能指点指点一下阿。。。能交个朋友不？

**[Amankwah](#25356 "2013-06-08 11:49:15"):** 草儿的空间木有了？

**[Amankwah](#25357 "2013-06-08 11:50:40"):** 我也在BenQ T131上装ARCH了……Gentoo在低配电脑上足够蛋疼，e17玩玩

