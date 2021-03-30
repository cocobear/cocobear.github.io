title: 也谈网银
tags:
  - ebank
  - network
  - security
id: 652
categories:
  - 互联网
date: 2009-05-15 13:43:24
---

先给大家看一个关于网银与USB-KEY的科普教程：

[http://apex.ncksoft.com/archives/tag/usb-key](http://apex.ncksoft.com/archives/tag/usb-key)

写得还不错，看完后你对目前网银的安全模式应该有个大概的了解。

目前公认最安全的网银模式就是使用2代U-KEY，就是在普通的USB-KEY的基础上增加一个确认按钮和LCD显示屏，这样可以极大的确保每一笔交易是经过你的确认。目前工行已经推出了二代U-KEY。

这里解释下二代KEY出现的背景，由于我们平时使用的电脑是“不可信环境”(在病毒、木马泛滥的年代，大部分人的电脑都可以被被木马随意操作)，所以我们不能相信自己的鼠标，比如我们点了下鼠标，做了一笔转帐交易，木马很可能帮你再点一次，这个时候就得在U-KEY上加一个确认按钮，防止木马偷偷做交易；我们也不能相信自己的眼睛，例如本来打算用1000块钱买个手机，结果木马把这个数字在发往U-KEY时改成了1W，而在屏幕上显示的仍然是1000，我们很无辜。这个时候你得在U-KEY上确认下我刚才是不是花了1000块去买那个手机。

U-KEY其实已经是一台完整的计算机了，根据冯·诺依曼原理，输入设备(LCD显示，按钮)、存储设备(16-32K)、运算器、控制器(智能卡CPU)，而且U-KEY还有自己的操作系统COS(Card Operating System)。由于平台的特殊性我们可以假定不会有木马进入这个U-KEY，因为所有针对U-KEY的操作都要通过COS对外提供的接口来完成，而且不同厂家的COS一般是不同的。这个时候U-KEY是一个“可信环境”，所以我们可以确保在按下“确认”按钮的时候U-KEY一定是对LCD上显示的交易进行了签名，签名算法可以确保该签名后的数据无法被修改。

最近公司在做U-KEY，不过不是传统意义上的USB-KEY，所以得和SSL,openssl，CSP，PKI，X509等这些安全、加密、证书相关的东西打交道。
## Comments

**[Kermit Mei](#5917 "2009-05-15 14:40:07"):** 最近发现在你的blog上坐沙发越来越不容易了……

**[cocobear](#5918 "2009-05-15 15:25:20"):** 很容易吧，你看现在不就是吗？ 好久没怎么写东西了。

**[可可熊](#5929 "2009-05-18 17:48:21"):** U-KEY都是基于MS的CSP架构，所以…… 这一点是由国内的银行决定的，无法改变的。

**[Silence](#5921 "2009-05-15 21:04:29"):** 很强大啊这个 blog要多更新啊

**[草儿](#5924 "2009-05-16 00:34:00"):** 看到U-KEY我总会想到Linux，然后想到Linux上好像没用过U-KEY，可能是和普及程度有关吧，至少对大众来说用的都是Windows系列。
