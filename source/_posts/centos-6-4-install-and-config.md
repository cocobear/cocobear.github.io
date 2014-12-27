title: CentOS 6.4安装配置-使用U盘作为安装介质
tags:
  - centos
  - Linux
id: 1012
categories:
  - Linux
date: 2013-06-06 14:25:13
---

[![centos6](http://7sbxmt.com1.z0.glb.clouddn.com/centos61-610x230.jpg)](http://c.kensou.me/blog/centos-6-4-install-and-config/centos6-2/)
前期准备:
     * IOS文件 : CentOS-6.4-x86_64-minima.iso 
     * U盘 : 因为使用最小化的安装, 所以1G的U盘就够用了 
     * UltraISO: 最好使用9.X的版本, 新版本制作的U盘启动支持的主板比较多 


CentOS 6系统使用U盘安装会遇到一些问题:
* 首先是如果直接把官方下载的IOS用UltraISO写入U盘启动时会遇到错误, U盘启动后会停在下面的画面: 


     Press the <enter> key to begin the installation process
 
解决方法: **用记事本打开这个文件syslinux\syslinux.cfg，把第一行default vesamenu.c32 替换为 default linux timeout 600 label linux kernel vmlinuz append initrd=initrd.img 这样修改之后，系统启动的时候就会跳过安装菜单选项界面，直接进入到语言选择界面**
* 使用上面的方法修改ISO文件, 重新用UltraISO写入U盘, **同时需要把官方下载的原始ISO文件拷贝到U盘中一份**, 如果你错误的把你修改过的ISO拷贝到U盘则会遇到错误:

     Unable to read group information from repositories. This is a problem with the generation of your install tree

* 顺利进入安装过程后,需要注意的是在分区结束后, 要修改一下安装GRUB的位置, 不要把GRUB安装在你的U盘上.

安装完成后需要做一些基本的配置: 

* **网络**
使用下面的命令编辑网卡的配置文件: 
     vi /etc/sysconfig/network-scripts/ifcfg-eth0

    
    DEVICE=eth0
    HWADDR=00:24:1D:7F:39:7F
    TYPE=Ethernet
    UUID=040e48f1-9712-42bb-8ad4-942ec0a5a5e5
    ONBOOT=yes
    NM_CONTROLLED=no
    BOOTPROTO=static
    IPADDR=192.168.1.11
    NETMASK=255.255.255.0
    GATEWAY=192.168.1.1


使用下面的命令编辑DNS的配置文件:

     vi /etc/resolv.conf

    
    nameserver 61.134.1.5
    nameserver 8.8.8.8


修改完这些配置文件后重起网络:

     service network restart

重起后更新一下系统:

     yum -y update

如果习惯ntsysv这样的系统启动配置工具,需要手动安装一下,mini盘里默认没有这样的软件包, 或者你可以使用chkconfig命令行程序来配置启动:

     yum  -y install　　ntsysv

mysql导出所有数据库:

     mysqldump --opt --user=root --all-databases > mysql_all.sql
## Comments

**[Amankwah](#25350 "2013-06-06 20:08:10"):** 怎么又开始装CentOS了？

**[可可熊](#25351 "2013-06-06 21:00:33"):** 当服务器用么。

**[Amankwah](#25352 "2013-06-07 11:09:30"):** 改行做SA了？

**[可可熊](#25353 "2013-06-07 16:55:39"):** 不知道SA是啥！

**[Amankwah](#25354 "2013-06-08 11:47:43"):** System Administrator，你这就是这个节奏啊！！

