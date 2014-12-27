title: Fedora 9 通过E680G/I手机实现GPRS上网
tags:
  - E680
  - Fedora
  - GPRS
  - Linux
id: 316
categories:
  - Linux
date: 2008-10-16 10:43:06
---

**电脑通过E680G/I手机实现GPRS上网**

E680是一款摩托的Linux系统手机，在Windows下可以通过自带的MPT工具包实现与电脑连接的GPRS上网，在Linux下也是比较方便的，以前我也用过，只是没记下来，今天再用的时候还得看别人写的文章，还是自己记一下。

我用的是Fedora 9，一般的Linux系统都应该可以，大同小异。首先是手机端设置连接模式为调制解调器(Modem)，通过数据线连接到电脑后(也可以用蓝牙，我这里没有环境)，Fedora 9会识别到有新的调制解调器：

[ 发现新硬件: ](#)


	usb 4-3: new full speed USB device using ohci_hcd and address 2
	usb 4-3: configuration #1 chosen from 1 choice
	usb 4-3: New USB device found, idVendor=22b8, idProduct=3802
	usb 4-3: New USB device strings: Mfr=1, Product=2, SerialNumber=0
	usb 4-3: Product: Motorola USB Modem
	usb 4-3: Manufacturer: Motorola
	cdc_acm 4-3:1.0: ttyACM0: USB ACM device
	usbcore: registered new interface driver cdc_acm
	drivers/usb/class/cdc-acm.c: v0.25:USB Abstract Control Model driver for USB modems and ISDN adapters


它对应的设备就是/dev/ttyACM0。接下来用wvdial这个拨号工具，如果没有这个包的话请自行安装，wvdial有一个配置文件/etc/wvdial.conf,(使用root进行下面的操作)编辑这个文件,：

[ 添加以下内容:](#)


	[Dialer Defaults]
	Init1 = at+cgdcont=1,"ip","cmnet"
	Phone = *99***1#
	Modem = /dev/ttyACM0
	Username = "cmnet"
	Password = "cmnet"
	Carrier Check = no
	Baud = 460800
	Auto DNS = on


如果已经有Dialer Defaults这一块则覆盖之。这里要注意使用的是cmnet，最好是手机包月不分cmnet和cmwap那种的，不然收费是很贵的。修改完该文件后还需要对/etc/ppp/options文件进行修改，添加一行：
:192.168.0.254
这个修改比较奇怪，E680系列的Linux手机都需要这样:-(，不然用wvdial拨号时会在/var/log/messages中产生下面的错误：

[ 错误信息：](#)


	Oct 16 09:56:25 cocobear pppd[18141]: pppd 2.4.4 started by root, uid 0
	Oct 16 09:56:25 cocobear pppd[18141]: Using interface ppp0
	Oct 16 09:56:25 cocobear pppd[18141]: Connect: ppp0 < --> /dev/ttyACM0
	Oct 16 09:56:36 cocobear pppd[18141]: Remote message: Welcome to Motorola A760 Software Modem!
	Oct 16 09:56:36 cocobear pppd[18141]: PAP authentication succeeded
	Oct 16 09:56:44 cocobear pppd[18141]: LCP terminated by peer (^@^@^@^@^@^@)
	Oct 16 09:56:44 cocobear pppd[18141]: Modem hangup
	Oct 16 09:56:44 cocobear pppd[18141]: Connection terminated.
	Oct 16 09:56:44 cocobear pppd[18141]: Exit.


修改完这个文件后就可以开始拨号了，直接使用wvdial命令:

[ 拨号信息：](#)


	[root@cocobear cocobear]# vim /etc/wvdial.conf 
	[root@cocobear cocobear]# vim /etc/ppp/options 
	[root@cocobear cocobear]# wvdial
	--> WvDial: Internet dialer version 1.60
	--> Cannot get information for serial port.
	--> Initializing modem.
	--> Sending: at+cgdcont=1,"ip","cmnet"
	OK
	--> Modem initialized.
	--> Sending: ATDT*99***1#
	--> Waiting for carrier.
	CONNECT
	--> Carrier detected.  Waiting for prompt.
	~[7f]}#@!}!}!} }< }!}$}%\}"}&} } } } }#}$@#}%}&j}?} } }'}"}(}"?}3~
	--> PPP negotiation detected.
	--> Starting pppd at Thu Oct 16 09:58:41 2008
	--> Pid of pppd: 18959
	--> Using interface ppp0
	--> local  IP address 10.182.237.127
	--> remote IP address 192.168.0.254
	--> primary   DNS address 211.137.130.3
	--> secondary DNS address 211.137.130.19


从上面的信息中可以看出我们成功的拨号上网了，不过这时候我们还需要手动把192.168.0.254添加为默认路由:

	route add default gw 192.168.0.254

ping一个google：

[ Ping结果：](#)

<
	[cocobear@cocobear ~]$ ping www.g.cn
	PING g.cn (203.208.33.100) 56(84) bytes of data.
	64 bytes from 203.208.33.100: icmp_seq=2 ttl=242 time=709 ms
	64 bytes from 203.208.33.100: icmp_seq=4 ttl=242 time=1359 ms
	64 bytes from 203.208.33.100: icmp_seq=5 ttl=242 time=1159 ms
	64 bytes from 203.208.33.100: icmp_seq=6 ttl=242 time=1021 ms
	64 bytes from 203.208.33.100: icmp_seq=7 ttl=242 time=617 ms
	^C
	--- g.cn ping statistics ---
	8 packets transmitted, 5 received, 37% packet loss, time 13411ms
	rtt min/avg/max/mdev = 617.708/973.500/1359.063/276.410 ms, pipe 2


好大的延时，还有丢包，没办法了中国移动的GPRS就这样了，EDGE在西安只有部分地区覆盖了，而且E680手机是不支持的！

终于上来了，搬到新租的房子后还没拉网线，只能先这样了。

羡慕王聪同学的3G上网啊＠_＠
## Comments

**[草儿](#4509 "2008-10-20 08:53:51"):** 想得美，还包吃包住包上网呢，我还没那个标准呢！ 我给你提供上网环境，其他自备 :-)

**[Amankwah](#4483 "2008-10-16 20:14:13"):** 不错，不错～多大流量？

**[草儿](#4479 "2008-10-16 14:51:37"):** F9不是带有移动拨号管理么，怎么还下了个包？ PS:用联通CDMA吧，哈哈。要不去我们省，大部分地区包括农村都有EDGE网的。

**[可可熊](#4480 "2008-10-16 15:40:56"):** 命令行方便，没另外下包。 去你们省给我包吃包住包上网就行！

**[luguo](#4481 "2008-10-16 16:53:18"):** 你的速度如何？ 这里的gprs暴贵！手机上网只能用3G～～

