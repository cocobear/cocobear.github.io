title: Linux系统下E680使用USB网模式
tags:
  - E680
  - Linux
id: 253
categories:
  - Linux
date: 2008-03-31 22:19:32
---

草儿同学写了一篇关于在Win下面E680的USB网模式使用，俺就说一说Linux下的使用吧。
对于目前主流的发型版来说驱动问题应该不会有的，2.6.12以后的内核应该都可以直接识别E680的，首先在手机上把连接电脑的模式设计为USB网，然后连接数据线，系统会识别到有新的网卡，在我的网络配置中可以看到：
Motorola PCS EZX GSM Phone(USBLAN)，设备名一般是usb0，我们先激活它，

	[cocobear@cocobear ~]$ sudo ifconfig usb0 192.168.1.1
然后ping一下手机：

	[cocobear@cocobear ~]$ ping 192.168.1.2
	PING 192.168.1.2 (192.168.1.2) 56(84) bytes of data.
	64 bytes from 192.168.1.2: icmp_seq=1 ttl=64 time=3.59 ms

接下来我们就可以对E680进行操作了，可以使用telnet登录，用户名root，密码为空：

	[cocobear@cocobear ~]$ telnet 192.168.1.2
	Trying 192.168.1.2...
	Connected to 192.168.1.2.
	Escape character is '^]'.

	MontaVista Linux Consumer Electronics Edition 3.0
	Linux/armv5tel 2.4.20_mvlcee30-mainstone

	(none) login: root
	Linux 192.168.1.11 2.4.20_mvlcee30-mainstone #7 Fri Feb 13 15:39:51 CST 2004 armv5tel unknown

	MontaVista Linux Consumer Electronics Edition 3.0

为了文件传输方便我们还是使用smb服务把E680挂载到电脑上：

	[cocobear@cocobear webpage]$ sudo mount.cifs //192.168.1.2/system /mnt/o/ username=root

这里我们需要使用mount.cifs，而不是平常使用的mount，这时我们就把E680当作普通的一个文件夹来进行操作了。

如果你的系统使用的是utf8（使用locale查看），那么会出现乱码的现象，[这里](http://linux.chinaunix.net/bbs/viewthread.php?tid=888016)有一个解决办法，我没有去试，应该可以的，但是很不方便，根据文中提到的原理我们完全可以这样搞定：

	[cocobear@cocobear soft]$ export LC_ALL=zh_CN.GB18030

这里你需要把Terminal关掉，重新打开一次，如果你使用的是gnome-terminal，可以在菜单中把当前显示的编码改为GB18030,这样访问E680中的文件就不会是乱码了：
	[cocobear@cocobear ~]$ cd /mnt/o/mmc/mmca1/soft/
	[cocobear@cocobear soft]$ ls
	quicksms.jar           快捷方式更改.mtf     删自带主题1_0.mpkg
	YYwb_V0.1.3_For_E680i  快速短信（20080303)
	[cocobear@cocobear soft]$ 

如果你想在图形界面中使用的话可以在窗口管理器的菜单中找到：打开位置这个选项，在这里输入：
smb://192.168.1.2/system
你就可以很方便的进行操作了，但这时乱码还会出现:-(

很不幸,俺发现没有权限对挂载的文件系统进行操作,郁闷,暂时还没有找到解决的办法:-(

Update 08.04.01：
今天又试了试发现在图形界面（nautilus)中各种复制、删除都正常，但命令行下就出错了：

`[cocobear@cocobear soft]$ cat >d
bash: d: 权限不够`

很奇怪的错误，乱码问题在nautilus中依然存在。
## Comments

**[Amankwah](#3057 "2008-03-31 22:51:01"):** 好，E680不错~网上买的？

**[草儿](#3062 "2008-04-01 20:55:06"):** 挂载的权限问题就是很奇怪，我在WIN下就老是出这问题，好像是随机的，而且不确定。很难说。

**[cocobear](#3059 "2008-04-01 09:20:47"):** 西安买得.

**[luguo](#3061 "2008-04-01 10:03:34"):** 行啊～～高科技啊～！

