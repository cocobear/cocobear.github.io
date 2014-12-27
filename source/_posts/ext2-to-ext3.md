title: Ext2到Ext3转换
tags:
  - Ext2
  - ext3
  - Linux
id: 266
categories:
  - Linux
date: 2008-04-26 18:06:19
---

在U盘上进行一次完整的文件系统创建转换，很多人应该没有用过mkfs.ext2（其实是mke2fs这个命令）这个命令来创建文件系统，大多数的时候都是在安装系统时已经完成了文件系统的创建，下面先看看如何使用mkfs.ext2来创建文件系统（这里需要root权限）：


	[cocobear@cocobear minix]$ sudo mkfs.ext2 /dev/sdb 
	mke2fs 1.40.4 (31-Dec-2007)
	/dev/sdb is entire device, not just one partition!
	Proceed anyway? (y,n) y
	Filesystem label=
	OS type: Linux
	Block size=1024 (log=0)
	Fragment size=1024 (log=0)
	31360 inodes, 125440 blocks
	6272 blocks (5.00%) reserved for the super user
	First data block=1
	Maximum filesystem blocks=67371008
	16 block groups
	8192 blocks per group, 8192 fragments per group
	1960 inodes per group
	Superblock backups stored on blocks: 
	        8193, 24577, 40961, 57345, 73729

	Writing inode tables: done                            
	Writing superblocks and filesystem accounting information: done

	This filesystem will be automatically checked every 24 mounts or
	180 days, whichever comes first.  Use tune2fs -c or -i to override.

从Filesystem label= 开始解释，mkfs.ext2可以通过-L来指定文件系统的卷标，这里我们没有指定，因此为空；
OS type: Linux：mkfs.ext2在创建文件系统时检查操作系统的类型；
Block size=1024 (log=0)：ext2文件系统是由一个个的block组成，这里给出来新创建的block的大小，1024bytes，当然也可以使用-b选项手动指定block的大小，一般情况下block大小为1024bytes或4096bytes；
Fragment size=1024 (log=0)：fragment在这里是没有意义的，因为ext2文件系统不支持fragment，我们可以直接忽略。
31360 inodes, 125440 blocks：总共生成的inodes与blocks的数量，block数量我们可以通过U盘的大小来计算出来，首先使用fdisk来得到U盘的大小，该U盘的大小为：128450560 bytes，因为我们使用的block大小为1024 bytes，因此总block数为：128450560 bytes / 1024 =125400。

6272 blocks (5.00%) reserved for the super user：在ext2文件系统的创建时候默认会为root用户保留5%的空间，这个可以-m选项来指定。

First data block=1：表示数据从第一个block开始。

Maximum filesystem blocks=67371008：一个文件系统

16 block groups：一共有16个block分组；

8192 blocks per group, 8192 fragments per group：每个分组有8192个block；

1960 inodes per group：每个组有1960个inode；

Superblock backups stored on blocks: 
        8193, 24577, 40961, 57345, 73729
超级块在以前这几个block上进行了备份。

当然在文件系统创建好后也可以使用dumpe2fs或者tune2fs来获得上面的信息，tune2fs正是我们下面需要使用的转换工具。

	[cocobear@cocobear ~]$ sudo dumpe2fs /dev/sdb 
	dumpe2fs 1.40.4 (31-Dec-2007)
	Filesystem volume name:   <none>
	Last mounted on:          <not available>
	Filesystem UUID:          d8d7cb8e-4cc0-4d91-b7e3-dcbc3f403345
	Filesystem magic number:  0xEF53
	Filesystem revision #:    1 (dynamic)
	Filesystem features:      resize_inode dir_index filetype sparse_super
	Filesystem flags:         signed directory hash 
	Default mount options:    (none)
	Filesystem state:         not clean
	Errors behavior:          Continue
	Filesystem OS type:       Linux
	Inode count:              31360
	Block count:              125440
	Reserved block count:     6272
	Free blocks:              119925
	Free inodes:              31349
	First block:              1
	Block size:               1024
	Fragment size:            1024
	Reserved GDT blocks:      256
	Blocks per group:         8192
	Fragments per group:      8192
	Inodes per group:         1960
	Inode blocks per group:   245
	Filesystem created:       Sat Apr 26 08:13:03 2008
	Last mount time:          Sat Apr 26 17:13:06 2008
	Last write time:          Sat Apr 26 17:13:06 2008
	Mount count:              1
	Maximum mount count:      31
	Last checked:             Sat Apr 26 08:13:03 2008
	Check interval:           15552000 (6 months)
	Next check after:         Thu Oct 23 08:13:03 2008
	Reserved blocks uid:      0 (user root)
	Reserved blocks gid:      0 (group root)
	First inode:              11
	Inode size:               128
	Default directory hash:   tea
	Directory Hash Seed:      f8b3e365-54e7-43b1-ad25-b3270db87684

	Group 0: (Blocks 1-8192)
	  Primary superblock at 1, Group descriptors at 2-2
	  Reserved GDT blocks at 3-258
	  Block bitmap at 259 (+258), Inode bitmap at 260 (+259)
	  Inode table at 261-505 (+260)
	  7673 free blocks, 1949 free inodes, 2 directories
	  Free blocks: 520-8192
	  Free inodes: 12-1960
	………………

首先卸载掉U盘，然后使用下面的命令：


	[cocobear@cocobear ~]$ sudo tune2fs -j /dev/sdb
	tune2fs 1.40.4 (31-Dec-2007)
	Creating journal inode: done
	This filesystem will be automatically checked every 31 mounts or
	180 days, whichever comes first.  Use tune2fs -c or -i to override.

然后我们再来看一下文件系统信息：

	[cocobear@cocobear ~]$ sudo dumpe2fs  /dev/sdb
	dumpe2fs 1.40.4 (31-Dec-2007)
	Filesystem volume name:   <none>
	Last mounted on:          <not available>
	Filesystem UUID:          d8d7cb8e-4cc0-4d91-b7e3-dcbc3f403345
	Filesystem magic number:  0xEF53
	Filesystem revision #:    1 (dynamic)
	Filesystem features:      has_journal resize_inode dir_index filetype sparse_super
	Filesystem flags:         signed directory hash 
	Default mount options:    (none)
	Filesystem state:         not clean
	Errors behavior:          Continue
	Filesystem OS type:       Linux
	Inode count:              31360
	Block count:              125440
	Reserved block count:     6272
	Free blocks:              115812
	Free inodes:              31348
	First block:              1
	Block size:               1024
	Fragment size:            1024
	Reserved GDT blocks:      256
	Blocks per group:         8192
	Fragments per group:      8192
	Inodes per group:         1960
	Inode blocks per group:   245
	Filesystem created:       Sat Apr 26 08:13:03 2008
	Last mount time:          Sat Apr 26 17:13:06 2008
	Last write time:          Sat Apr 26 17:55:15 2008
	Mount count:              1
	Maximum mount count:      31
	Last checked:             Sat Apr 26 08:13:03 2008
	Check interval:           15552000 (6 months)
	Next check after:         Thu Oct 23 08:13:03 2008
	Reserved blocks uid:      0 (user root)
	Reserved blocks gid:      0 (group root)
	First inode:              11
	Inode size:               128
	Journal inode:            12
	Default directory hash:   tea
	Directory Hash Seed:      f8b3e365-54e7-43b1-ad25-b3270db87684
	Journal size:             4113k

	Group 0: (Blocks 1-8192)
	  Primary superblock at 1, Group descriptors at 2-2
	  Reserved GDT blocks at 3-258
	  Block bitmap at 259 (+258), Inode bitmap at 260 (+259)
	  Inode table at 261-505 (+260)
	  5113 free blocks, 1948 free inodes, 2 directories
	  Free blocks: 520-5632
	  Free inodes: 13-1960


从这里“Filesystem features:      has_journal resize_inode dir_index filetype sparse_super”我们可以看到已经加入了ext3的日志功能。

## Comments

**[luguo](#3129 "2008-04-27 11:07:05"):** tune2fs很强大嘛～～没用过的说～！

**[kongove](#3130 "2008-04-27 14:22:20"):** 我前两天刚在Unix技术手册上看过这个，我准备在虚拟机上创建一第二块硬盘练习一下。

