title: DR.COM再谈
tags:
  - drcom
id: 248
categories:
  - 互联网
date: 2008-03-19 19:58:49
---

看到[草儿](http://hao150.cn)同学从别人那里转载的DR.COM共享上网方式太麻烦了，虽然有效但并不是一种很好的解决方案，下面说下我的解决方案:

一:
先使用MAC扫描器扫描你使用的一段IP，如192.168.11.*(在未上网的情况下，别人的机子也是未上网情况下)，然后使用一个叫做MACBIND的工具，它会把扫描出来的MAC与对应的IP绑定.也就是下面的命令:

arp -s 192.168.11.111 00-00-00-00-00-00

你也可以写一个批处理让它一直运行(我没试过)

上面解决了局域网问题，接下来使用Squid这个代理软件(完全不必隐藏进程，不需要担心DR.COM会发现)代理，它可以作为一个服务启动.简单的几下配置就可以了。

二:
一种更巧妙的方法，开机时别让DR.COM那个插件运行，手动运行它:点右键->运行方式，把"保护我的计算机和数据不受未授权程序的活动影响"这一项选上，然后确定就可以了。(前提是你的arp缓存表正常，如果不正常使用arp -d清空一下缓存表就行了)
接下来仍然是使用Squid代理软件来共享上网。

简单的说一下原理，DR.COM限制共享的方法就是使用插件与服务器保持心跳(前面的文章分析过)，保证该插件不会被关闭，然后篡改ARP缓存表，使得局域网无法正常使用，同时检测是否有代理软件的进程。针对这些特点我们就可以得出以上的方法。

还可以模拟DR.COM插件向服务器发送数据，上学期分析了一段时间协议，这学期没办号，没有条件弄了，所以在Linux下只能wine模拟了。

不想说什么了，中国的情况就是这样……
## Comments

**[luguo](#3029 "2008-03-22 15:48:52"):** dp同学终于现身了～～

**[cocobear](#3020 "2008-03-20 10:37:46"):** 很明显 Linux下DR.COM插件处于一个被隔离的范围中，它的作用只是与服务器保持“心跳”，对代理进程的检测与篡改ARP缓存表是无法做到了。 顺便说明一下，第一种方法如果在上网的情况下玩局域网游戏的话会有些卡（我试着打了CS，很卡的说），因为DR.COM与MACBIND一直在不停得修改ARP缓存，不过代理上网不会有问题的。 第二种方法我也是根据Linux下DR.COM处于隔离状态不能非法篡改ARP缓存表而想到的。

**[sk](#3037 "2008-03-26 18:18:30"):** 写的很好！

**[Amankwah](#3013 "2008-03-19 22:34:36"):** 赞一下DP！ 不过在Linux下模拟完全没有这个问题，然后用iptables的nat转发一下就能共享了~

**[草儿](#3012 "2008-03-19 22:13:40"):** 写的好，写的妙，写的我上网哈哈笑。

