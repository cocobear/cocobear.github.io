title: 加快Linux的启动速度
tags:
  - boot
  - Linux
id: 87
categories:
  - Linux
date: 2007-05-07 13:26:28
---

有很多写关于加快Windows启动速度的文章，大概就是取消无用的启动项目，取消预读，可以通过修改注册表，以及使用msconfig配置工具。

Linux当然也可以通过一些配置来加快启动的速度，我就拿我使用的Fedora core 5为例，说明如何加快Linux的启动速度。

开机时的BIOS自检就不在我们的讨论范围之内，一般加快BIOS自检速度可以通过设置BIOS中的一些参数来实现。接下来的BIOS把控制权交给MBR，我机子的GRUB是装在MBR上的，所以接下来就是GRUB的引导了，因为我使用的是双系统，所以启用了GRUB的选择菜单，时间为3秒，如果你只有一个系统，就可以把启动菜单隐藏，把等待时间设置为0,这样GRUB引导的时候就不会有延迟。

timeout=0
#splashimage=(hd0,2)/boot/grub/splash.xpm.gz
#hiddenmenu

把timeout设置为0,不使用背景图片，不使用引导菜单。

接下来就是加载内核了，如果我们到自己的需求十分清楚，可以自己编译内核，把一些无用的模块从内核中去掉，这样也可以加快启动的速度，当然这要求你对系统比较熟悉，而且你要做好失败的准备，因为我自己编译过两次内核都失败了。其实对于普通用户使用各自发行版编译好的内核也是可以的，和自己编译的内核加载速度不会有很大差距，编译内核只是给高级用户的一个选择。

最后就是初始化系统，我们尽量不要把自动挂载分区（可以通过编译/etc/fstab文件来实现），自动拨号（图形界面有这个选项，配置文件为

	/etc/sysconfig/network-scripts/ifcfg-XXX

XXX为你配置的拨号连接名称），这个当然也是仁者见仁，如果你想要获得最快的启动的速度，不防照我说的去做。

Fedora core 5默认启动的服务有很多，有很多服务普通的用户是根本用不到的，正是这些自启动的服务很大的影响了系统启动速度。编缉启动项目可以有很多种办法，图形界面下可以使用

System->Administration->Server Settings->Sevices

来添加、删除启动项目，注意Edit RunLevel菜单，不同的Level具有不同的启动项目。命令图形界面可以使用ntsysv。当然你也可以完全手动修改/etc/rc5.d里面的文件来实现停止相关的服务（K开关的文件为不启动，S开关的文件为启动。）

我在图形界面下使用的启动项目只有：

	*acpid
	 autofs
	 crond
	*haldaemon
	*messagebus
	*network
	 readahead
	 readahead_early
	*syslog

加*号的是必须启动的服务，readahead可以加快启动速度，关于服务的详细说明，可以参数下面这篇文章:

[Services in Fedora Core 5](http://www.mjmwired.net/resources/mjm-services-fc5.html)

最后如果你使用的是图形界面可以通过修改登录界面，配置为自动登录，这样也可以加快启动速度:)

我们从启动计算机到最后的登录完整的描述了如何加快Linux的启动速度，希望对大家有所帮助，有什么更好的想法也可以提出。
## Comments

**[cocobear](#46 "2007-05-08 17:00:21"):** 上面那篇文章对这些服务介绍的很详细，其实大部对于普通用户来说都是没用的，对于咱们这些老机子还是能关就关了吧:)

**[草儿](#47 "2007-05-08 15:19:50"):** 你的速度的确快，比我的快多了 不过以前看过介绍相关服务的文章，大多都是建议开启，自己对系统也不是太熟悉，就没敢乱动 呵呵，看来以后得多多学习了

**[wangcong](#48 "2007-05-07 15:25:48"):** 下面几个我都没舍得关，你确定它们没用？ anacron apmd auditd Avahi-daemon cpuspeed gpm lm_sensors smartd sysstat xinetd

**[cocobear](#49 "2007-05-07 16:38:11"):** 没用的，你可以看上面那篇文章的，偶的机子现在启动很快。

