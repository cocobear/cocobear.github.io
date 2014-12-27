title: console与x-window自由切换
tags:
  - Linux
  - X
id: 37
categories:
  - Linux
date: 2007-04-06 19:18:02
---

Friday, 29\. December 2006, 15:46:07


有时候我们开机的时候也许没有必要进入X-window界面。比如，我开机只是为了编译一个东西，或者是写一段程序，我可以在console下可以更高效的完成，例如编译软件的时候使用console会大大的提高编译效率。(甚至我用vi写程序的时候仍然可以使用mplayer播放歌曲。)

安装完一个发行版后，一般默认启动的是x-window，我们可以通过更改
/etc/inittab
中的
id:5:initdefault:
这一行来实现开机默认启动X还是console。(3为console,5为X-window)

当然这有点不太现实，因为我们无法在开机的时候决定这个文件的内容，所以最好的办法是如果能在grub中选择。

下面是实现的办法：
更改/etc/grub.conf文件


	title Fedora Core (X-window)
	        root (hd0,2)
	        kernel /boot/vmlinuz-2.6.18-1.2239.fc5 ro root=LABEL=/12 rhgb quiet vga=0x305
	        initrd /boot/initrd-2.6.18-1.2239.fc5.img
	title Fedora Core (text console)
	        root (hd0,2)
	        kernel /boot/vmlinuz-2.6.18-1.2239.fc5 ro root=LABEL=/12 rhgb quiet vga=0x305 3
	        initrd /boot/initrd-2.6.18-1.2239.fc5.img


其实很简单的，新增加一个启动的项目，在kernel最后一行加一数字3，进入level3模式(多用户console模式)

vga=0x305代表启动vga图形驱动模式，设置分辨率为0x305(1024x768)

觉得1024x768下的console更cool!
![](http://files.myopera.com/cocobear/blog/1Screenshot.png)

关于linux启动可以参考：[Linux's boot process explained](http://my.opera.com/cocobear/blog/2006/12/28/linux-s-boot-process-explained)