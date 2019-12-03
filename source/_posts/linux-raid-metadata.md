---
title: Linux下清除硬盘上的RAID metadata
tags: [linux,raid]
toc: true
comments: true
date: 2019-11-18 15:13:39
categories: Linux
description: 安装系统遇到RAID错误了！
---

![](https://asset-1258390188.cos.ap-shanghai.myqcloud.com/linux-raid.png)

<!--more-->

今天在装一台工控机的时候遇到了无法识别硬盘的问题,在RHEL 6.9下报错：

> Disks sdb contains BIOS RAID metadata, but is not part of any recognized BIOS RAID sets. Ignoring disks  sdb.



在RHEL 7.5下面是找不到硬盘，但没有上面的报错提示，这一点7.5就表现的不太好，这么重要的错误提示没有显示出来。

根据提示来看应该是硬盘上面有以前做过`RAID`的信息在里面，于是试用`fdisk`重建分区，结果还是报错，拆下来用`Windows`下的`DiskGenius`软件重建`MBR`也是不行，最后在`macOS`下重建分区格式化成了GPT的分区表，装回原来的机器，结果就好了。

装好机器后又了解了一下Linux下的软件RAID，原来它是把`metadata`写在了硬盘的最后位置，所以即使怎么重建分区，都不会删除硬盘最后面的数据，所以最根本的解决办法是把这些数据清除掉：

```shell
dd if=/dev/zero of=$YOUR_DEV bs=512 seek=$(( $(blockdev --getsz $YOUR_DEV) - 1024 )) count=1024	
```

`macOS`下的磁盘管理工具应该是清除了硬盘最后面的数据。



