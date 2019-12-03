---
title: DeskMini 310组装及黑苹果安装
tags: [deskmini, hackintosh]
toc: true
comments: true
date: 2019-12-02 14:29:33
categories:
description: 弄个迷你机箱装个黑苹果吧
---
<img src="https://asset-1258390188.cos.ap-shanghai.myqcloud.com/DeskMini310.png" style="zoom: 50%;" />
<!--more-->

## 写在开头

​    自从在办公室的电脑上装了黑苹果后用着越来越顺手，于是将`X220`笔记本也装了个黑苹果在家里用。8G的内存，2代i5的处理器，凑合能用，但是屏幕太小，分辨率太低，最近屏幕开始有横线出来，于是换个电脑的想法就有了。

​	一开始的想法自然是换个笔记本，屏幕一定要15寸以上（受够了X220的12.5寸的小屏幕），内存一定要够大，至少16G，32G最好，硬盘肯定是SATA或者NVME了，512G以上。最简单的方案就是直接买 一个新出来MacBook Pro 16寸（18999RMB），但这个价格还是劝退了，16G+512G的配置都不是很理想，而且苹果的东西都没办法升级，要是定制32G+1T价格更是离谱，苹果的东西真是消费不起。所以继续黑苹果，看中了联想Y9000，但硬盘好像黑苹果不支持，新机器拆机保修就没了，所以也不想折腾。

​	后来想了想其实我现在的电脑基本不需要移动，其实配个迷你主机就挺好的，还可以用24寸以上的4K屏。如果不想用了，还可以当`homelab`主机用。

## 装机方案选择

既然可以自己装机，那么选择余地就大了，在网上逛了几天弄出来了以下几个方案（都仅包含CPU+主板+机箱+电源），因为内存，硬盘都是通用的。

### 备选方案

1.  Intel Nuc8i5beh
2.  华擎DeskMini 310
3. 华擎Z370m-ITX/AC+立人H60机箱带电源

### 配置对比

|  | 芯片组 | CPU  | 显卡 | 价格(仅供参考) | 尺寸 | 黑苹果成功案例 |
| ---------------- | ---- | ---- | ---- | ---- | ---- | ---- |
| NUC | **Z370** | i5-8259u(不可升级) | **Iris Plus 580** | 3099 | **117 x 112 x 51 mm** | 多 |
| 310 | H310 | **i5-8500** | UHD630 | **1059+1499=2558** | 155 x 155 x 80 mm | 多 |
| H60 | **Z370** | **i5-8500** | UHD630 | 1099+240+1499=2838 | 200 x 200 x 60 mm | 少 |

### 接口对比

|      | M.2  | DP          | HDMI      | SATA | 雷电 | USB3.1 | Type-C |
| :--- | ---- | ----------- | --------- | ---- | ---- | ------ | ------ |
| NUC  | 1    | 1.2/4K@60Hz | (4K@60Hz) | 1    | 1    | 4      | 1      |
| 310  | 2    | 1.2/4K@60Hz | (4K@30Hz) | 2    | 0    | 3      | 1      |
| H60  | 2    | 1.2/4K@60Hz | (4K@30Hz) | 6    | 0    | 6      | 0      |

### 结果

这三个方案的价格差别不是很大，下来基本都是2000多。

NUC用的是低压CPU不是很理想，但是集显性能强一些，又有满血雷电3，体积最小，不过M.2接口只有一个如果用了硬盘那么无线网卡就不能用了，有人通过改造从读卡器接口弄出来一个M.2（这种方案是不考虑了，手艺不行）。

DeskMini 310的芯片组是H310比其它两个差一点，体积厚一点，其它的基本没有啥缺点。

华擎Z370m-ITX/AC+立人H60机箱的方案就是完全自己装机了，接口丰富，这个方案来源在[这里](https://nodeedge.com/itx-build-note.html)。自带的WIFI是Intel芯片黑苹果无法使用。我想买的时候发现主板没货，再加上博主说自己遇到了静电的问题，所以就放弃了。

最后就选择了今天的主角华擎DeskMini 310。



## 配置

|                | 型号                          | 价格 |
| -------------- | ----------------------------- | ---- |
| CPU            | I5-8500                       |      |
| 主板/机箱/电源 | DeskMini 310                  |      |
| 内存           | 笔记本 DDR4 2666 16G x2       |      |
| SSD            | 三星 860 evo 512G             |      |
| 无线/蓝牙      | DW1820A(BCM94356) 096JNT      |      |
| 天线           | NGFF M2无线网卡转接线天线     |      |
| USB排线        | 双口USB线9孔杜邦USB带耳朵挡板 |      |



## 机器组装

DeskMini 310是一个准系统，安装起来挺方便，主要就是硬盘，网卡，CPU的安装，主要参考了这几个地址：

[Deskmini-310-黑苹果折腾记](https://w2x.me/2019/06/13/Deskmini-310-黑苹果折腾记/)

[组一台迷你主机 DeskMini 310](https://blog.fooleap.org/deskmini-310.html)

[为了4K显示器，我重新配了一台迷你主机！Deskmini 310晒单](https://post.smzdm.com/p/aek8k2lq)

安装过程比较简单，不过还是耗费了2个多小时，其中无线网卡最难装，一个是天线，需要从机箱走天线出来（用内置天线的话需要离你的无线设备很近）;另一个是我用的1820A(096) 需要屏蔽前3后2。盗用黑果小兵的一张图，这是两张网卡，一正一反放在一起的照片，需要屏蔽的就是图中框出来的5个金手指，看这个图觉得挺容易，拿到网卡发现太难了，这网卡太小！

![](http://7.daliansky.net/DW1820A/DW1820A_Cover_pins.jpg)

## 系统安装

### 制作macOS Mojave安装盘

建议使用[`Etcher`](https://www.balena.io/etcher/)制作安装盘，镜像文件可以从黑果小兵那里[下载](https://blog.daliansky.net/macOS-Mojave-10.14.4-18E226-official-version-with-Clover-4903-original-image.html)，我安装的版本是`Mojave 10.14.4`，然后替换掉里面的`CLOVER`。我的CLOVER来源主要是[这个](https://github.com/yuqi/Deskmini-310-Hackintosh
)。

`CLOVER`里最主要的就是`kext`驱动文件，[这里](https://www.kancloud.cn/chandler/mac_os/480623)有个对`kext`的介绍比较详细，可以了解一下。



### BIOS设置

我的BIOS版本是P4.2

> Load UEFI Defaults
>   * Advanced
>     - CPU Configuration
>       - CPU C States Support : Enable
>         - CFG LOCK : Disabled
>     * Chipset Configuration
>       * VT-d: Disabled
>       * Onboard HD Audio: Enabled
>     * USB Configuration
>       * XHCI Hand-off: Enabled
>     * Super IO Configuration
>       * Serial Port: Disabled
>   * Security
>     
>     * Secure Boot: Disabled(by default)
>   - Boot
>     * CSM disable
>



### 安装macOS

​	选择U盘引导，中间要重起两次，注意每次选择的启动项。安装成功后就基本可以用了。需要额外处理的有WIFI，蓝牙，HDMI声音输出。

### WIFI蓝牙(`DW1820A`)

​    建议参考黑苹果小兵的文章[DW1820A/BCM94350ZAE/BCM94356ZEPA50DX插入的正确姿势](https://blog.daliansky.net/DW1820A_BCM94350ZAE-driver-inserts-the-correct-posture.html)。

我使用的WIFI蓝牙二合一网卡型号是`DW180A`，M.2接口，芯片是`BCM94356`，这个型号的网卡有几种类型，我用到的是`096JNT`，也有别的也可以用，但是可能会有些差异，购买的时候需要特别注意。

<img src="https://asset-1258390188.cos.ap-shanghai.myqcloud.com/dw1820a.jpg" style="zoom: 25%;" />

我装的时候一开始没有做屏蔽，用了上面文章里的配置结果就无法启动了，会卡在：

> AirPort——Brcm43XX::stat: Failed 'startGated()'

后面在小兵的帮助下修改了`config.plist`文件，然后重起结果是蓝牙可以使用，WIFI不正常，最后我才做了前3后2的屏蔽，重起后一切都正常。

### HDMI声音

现在很多显示器都有内置音箱，用起来很方便。我开始使用的`CLOVER`是无法使用的显示器的音箱，在解决网卡蓝牙问题的时候顺便这个问题也解决了，有点意外的惊喜。



### 工作正常

- [x] Ethernet (Intel I219V7)
- [x] Audio (Realtek ALC233)
- [x] WIFI/Bluetooth (DW1820A)
- [x] Graphics (Intel UHD 630)
- [x] Dual Monitors (DELL U2715H x 2) \*
- [x] Shutdown/Restart