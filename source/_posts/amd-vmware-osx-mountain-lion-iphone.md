title: 在AMD的PC机上使用Vmware安装Mac OS Mountain-Lion进行iPhone(iOS 6)开发
tags:
  - AMD
  - iPhone
  - MAC
  - vmware
id: 972
categories:
  - Life
date: 2013-05-11 18:36:16
---

　　Android的开发环境很方便搭建，不管是用Windows还是Linux（Mac上面还真不清楚）。 iPhone的开发环境就麻烦多了，为了验证个想法，写个小程序Android上面很快就可以完成，iPhone还得折腾半天，尤其是使用的还是AMD的CPU，折腾了几次，今天终于可以在Vmware里跑的Mac OS 上面跑Xcode了，前面先安装了一个Snow Leopard（10.6）可是不支持最新的iOS 6，也没办法升级到最新的系统，只好重新再安装新的系统，光是下载镜象以及Xcode的安装包就等了好长时间（8M的网还是不给力）。

　　这里就不贴图了，下面链接里面有比较详细的说明，请大家按照链接一中的步骤安装系统。

　　我开始是在[链接一](http://www.hankcs.com/appos/amd_mac_vmware.html "安装步骤")看到这个Vmware的镜象文件，里面有安装的步骤，不过文章没有在显示的位置给出[链接二镜象的来源](http://www.souldevteam.net/blog/2013/02/06/os-x-mountain-lion-vmware-image-amd/ "Vmware镜象来源")，只有在后面的链接里面提到。

　　这个镜象的来源是国外的一个团队：**Soul Dev Team**

　　以下是我机器的配置，拿出来给需要使用该镜象包的同学作参考：

     CPU AMD Athlon（速龙） II X4 640 > 
     主板 技嘉 GA-880G-UD3H (AMD 880G/980G (RS880P) + SB750/SB710) > 
     内存 8 GBytes > 
     显卡 ATI/AMD Radeon HD 4250 (RS880) > 
     硬盘 希捷 ST31000528AS > 
     显示器 冠捷 TFT1980 > 
     网卡 瑞昱 Semiconductor RTL8168/8111 PCI-E 千兆以太网 NIC > 
     声卡 ATI/AMD SB600 - High Definition 音频设备 控制器

　　总结下这个Mac OS Mountain Lion 10.8.3镜象包吧：

　　首先使用起来很简单，基本上不需要做任何事情，下载后直接解压，打开，然后如果不出意外的话你就可以使用了，不过速度有点慢，打开程序的时候有明显的卡顿现象，镜象默认使用了1G的内存，以及一个CPU核，我多分配了一些，大家可以根据自己机器的配置适当的在Vmware中修改Mac的配置，同时打开显卡的加速，之后再次启动虚拟机，能明显感觉到速度快了一些，不过还是有些卡，由于是在Terminal下一个diff命令能卡1，2秒，比较奇怪。原来是因为显卡驱动的问题，安装了Soul Dev Team贴子下面评论中提到的[vmsvga2](http://sourceforge.net/projects/vmsvga2/files/ "vmsvga2")，感觉速度快多了。强烈建议都安装一下，不过在安装显卡驱动之前一定要对虚拟做个快照（snapshot）,否则出现问题无法进入系统就麻烦了。

　　Mountain Lion有一点很不错，刚才重起了一次，重起前打开的东西都完整的显示在屏幕上面了，之前打的的Terminal被我最大化了，重起后这个Terminal是先显示出来，然后完成了一次类似我点最大化时出现的效果，赞一个感觉工作台是跳到我面前了，而不是象Windows的休眠一样，死寂沉沉的。

　　放个截图吧，都讲究图文并茂么：
[![desktop](http://7sbxmt.com1.z0.glb.clouddn.com/desktop-610x487.jpg)](http://c.kensou.me/blog/amd-vmware-osx-mountain-lion-iphone/desktop/)
Update: 
[Xcode 4.6.2开发免99美金证书真机调试](http://blog.csdn.net/fightingbull/article/details/8059651)
## Comments

**[草儿](#24413 "2013-05-13 15:01:39"):** 难怪离线里那么多OSX，嘿嘿 我一直用的virutalbox，前段时间也想虚拟机里搞个玩玩，可惜一直启动不了，郁闷

**[Amankwah](#24513 "2013-05-15 15:32:18"):** 可以考虑直接搞个黑青苹果装上？

**[可可熊](#24514 "2013-05-15 15:52:42"):** AMD的CPU啊, 怕啃不动黑苹果.

