title: Kindle Touch入手记
tags:
  - kindle
id: 940
categories:
  - Life
date: 2012-03-11 17:51:20
---

上个月买了个Kindle Touch，直接从Amazon购买，然后走的友家快递转运到国内。本来2.9日就在Amazon下单了，2.11友家通过USC发往中国上海，可是直到3.4号上海海关才处理了单子，3.6号EMS寄到我的手里，国内的海关效率实在是不敢恭维。还好的是完整无缺的送到了我手上，我买的是带有Special Offers的，价格为99$，连运费、保险下来一共是656块钱，还算挺便宜的，国内的汉王都比这贵。

拿到手以后看到显示效果确实不错，手感也不错，不过屏幕就是有一些小：只有6寸，而DXG版本又太大（价格也贵），所以买了这个Touch。刚拿到后下了个中文的书进去，果然是有字体不一样粗细的问题，看起来很不舒服，果断开始越狱。Kindle越狱和别的不太一样，目前网上的方法一般都是通过给Kindle写一个开发者的KEY进去，不会对系统有任何影响，所以可以放心大胆的越狱。

越狱后更换了字体，立刻舒服很多了，网上目前有两款横屏插件（亚马逊自己的系统竟然没有横屏功能），有一个插件是通过菜单手动做横屏，不过问题是横屏后屏幕的右边被切掉了那么一两行字，很不舒服，所以安装了另外一个可以自动横屏的（Kindle Touch有重力感应的硬件），亚马逊这Kindle Touch发布的也太仓促了，基本硬件功能都没做好。

一直很期待Kindle的推送功能，所以装完这些后，就在几个网站上注册了Kindle的推送，试着发了一些到Kindle上，结果Amazon的账户里是都有了，就是没到我的Kindle上，我在Kindle上可以正常访问网站，账户也登录进去了。于是我开始自己折腾，恢复初厂设置、卸载插件弄了好久仍然不行，最后还联系了亚马逊的客服，按照他们的要求做reset、Unregister，结果Unregister的时候一直无法进行，试了两次后，客户让我把Kindle寄回去，他们现在会先寄一个新的Kindle过来。虽然我在网上了解到这种情况下我寄过去的邮费亚马逊会出，但是还是觉得太麻烦，先没让客服给我做replacement order.

今天想着是不是可以看一下Kindle的出错日志呢，于是装了一个usbnet的插件，ssh过去看了下Sync时候的错误：

     120311:062741 todo[846]: E todo_get_document:CURL_EASY_PERFORM:res=28,url=https://todo-g7g.amazon.com/FionaTodoListProxy/getItems?count=20&device_lto=0&reason=Customer&sof tware_rev=1496040004&patch_rev=0&currentTransportM ethod=wifi:> 
     120311:062741 todo[846]: E todo:ProcessingToDoFailed:reason=getDocumentFailed NullDocument,errno=0:Null ToDo document returned

Kindle已经访问服务器去取内容了，但是得到是是Null，再加上之前我这里有时候登录不上Amazon，所以中午特意跑永合豆浆那里吃饭，顺便用那里的WIFI试一下。吃饭的时候连上去点了“Sync and Check for Items“，仍然没有反映，还以为不是网络的问题，没想到回到酒店以后发现Archived Items里面多了东西，原来那个同步不是立刻就完成的，我吃饭的时候他自己下载了。现在可以在Kindle上看到我推送过来的标题，但无法下载，说明酒店这里的网络有问题。

还好没急着去换货，不然又不知道得折腾多久。

网上都说Kindle的这种屏幕比较脆弱，是不是应该去买个套子+屏保呢？

顺便说一下，以前时候博客一直无法访问，只有在国外才能访问，还以为是被墙了呢，后来发现是这个域名的解析服务器被墙了，换了一个后现在正常了。国内的网络真让人烦，一会儿封这一会儿封那儿，被折腾死了！！

附1：Kindle Touch 5.0.4 越狱包 http://db.tt/hmGoRogn 

Tips: 可以通过https://www.dropbox.com/enable_shmodel来打开Dropbox的分享功能，这样不必把要分享的文件放入Public文件夹了
## Comments

**[Wendy](#13334 "2012-03-15 11:05:47"):** 拿到手还折腾了这么久呢！这个好玩意就得你用才能用出价值！我还在犹豫中.......

**[Wendy](#13335 "2012-03-15 11:06:13"):** 对了，注意到你换博客风格了!非常不错！

**[cocobear](#13338 "2012-03-15 11:59:08"):** 呵呵，ipad3出来了，才499$。

**[小卒](#13354 "2012-03-16 23:58:21"):** 看来大家兴趣很相似啊，之前看python 飞信库找到的你。去年我也海淘了touch，amazon.com在国内有时会被间歇性墙，有可能你是这个问题。

**[Wendy](#13404 "2012-03-21 14:09:26"):** 太贵了！

**[cocobear](#13405 "2012-03-21 14:12:10"):** 现在一手机都卖3000多了！

**[草儿](#13294 "2012-03-12 09:30:05"):** 你终于还是入手了。。。 PS：为什么不把我的域名解析也重做一下呢。。。

**[tgyhg](#19468 "2013-01-01 16:24:04"):** Device Information: Name: "Chinese-SI iPhone" UDID: a84807391cc91db03fa736ad28c856e50f52a2b7 Model: iPhone Localized Model: iPhone Firmware Version: 6.0.2 Multitasking Supported: Yes Capacity (Disk Space): 13.47 GB Available (Free Space): 7.39 GB System Name: iPhone OS Date: 2013年1月1日

