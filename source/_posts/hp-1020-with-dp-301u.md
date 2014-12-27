title: HP 1020 Plus打印机与DP-301U使用问题
tags:
  - Life
id: 431
categories:
  - Life
date: 2008-12-19 15:33:30
---

中午本来在写一段代码，头过来说让俺看下那个网络打印机，结果折腾了几个小时。

问题：无法打印
打印机：HP LasterJet 1020 Plus
打印服务器：D-Link DP-301U 10/100 Ethernet USB Print Server

本来应该是很简单的办公配置，没想到HP 1020与DP-301U在一起使用时确不行，网上有[提到](http://itbbs.pconline.com.cn/network/8926100.html)需要运行一个PS Monitor的东西，结果找了半天也没找到这个软件，最后发现从[d-link官网](http://www.dlink.com.cn/webapp/dlinkweb/product_model/productViewSupport.action?productModel.modelId=1000003162&data.subGroupId=0&data.groupId=0)下载的一个东西比较可疑，**PS Utility.rar**，上面写着是RAR下载下来是exe文件，刚开始的时候没注意，后来解压来，结果那PS Monitor的安装文件PSMSetup.exe果然在这里面，按照[这篇文章](http://gzsean.blog.51cto.com/194266/89553)来配置打印机，测试一下，没问题，终于搞定了。

按照网上的说法，这个PS Monitor的东西必须一直在后台运行，但是我把我的电脑关了以后别的同事照样可以打印，晕死了，到底是怎么回事！！

在Linux使用HP 1020可以使用[这里](http://foo2zjs.rkkda.com/)的驱动，Fedora10里面没有1020的驱动。
## Comments

**[草儿](#4703 "2008-12-19 21:49:04"):** 怎么你也用的那个打印机……

**[luguo](#4705 "2008-12-20 10:48:46"):** 汗，我在公司时也是用的HP LasterJet的打印机。。。。。

**[luguo](#4706 "2008-12-20 10:50:01"):** p.s linux下直接修改一下配置文件就可以啊～～我是直接复制的同事的～～

**[Amankwah](#4711 "2008-12-21 23:35:08"):** 不懂，从来不打印～

