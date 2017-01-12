title: Smartisan OS 0.2 alpha for Nexus 4
tags:
  - android
  - nexus4
  - smartisan
id: 1189
categories:
  - Life
date: 2013-07-24 13:52:04
---

该版本是在[烟雨](http://bbs.anzhi.com/thread-7307404-1-2.html)的基础上移植了Smartisan OS 0.2 alpha版本，在速度和稳定性方面有了大幅的提升。

[![截屏20130724133050](http://7sbxmt.com1.z0.glb.clouddn.com/b8cc3b379b3872b7921ba5fb85a0020b-180x300.png)](http://c.kensou.me/blog/smartisan-os-0-2-alpha-for-nexus-4/%e6%88%aa%e5%b1%8f20130724133050/)

[![截屏20130724133111](http://7sbxmt.com1.z0.glb.clouddn.com/fd41d7152e3f1654b5c83716f3c6d614-180x300.png)](http://c.kensou.me/blog/smartisan-os-0-2-alpha-for-nexus-4/%e6%88%aa%e5%b1%8f20130724133111/)
<!-- more -->　　
[![截屏20130724133117](http://7sbxmt.com1.z0.glb.clouddn.com/654f2099e8a227475a1b2783486796c7-180x300.png)](http://c.kensou.me/blog/smartisan-os-0-2-alpha-for-nexus-4/%e6%88%aa%e5%b1%8f20130724133117/)

[![截屏20130724133135](http://7sbxmt.com1.z0.glb.clouddn.com/596e1cd7f8acb5bdef82e6edd62a324f-180x300.png)](http://c.kensou.me/blog/smartisan-os-0-2-alpha-for-nexus-4/%e6%88%aa%e5%b1%8f20130724133135/)

[![截屏20130724133145](http://7sbxmt.com1.z0.glb.clouddn.com/287b8835308fcfbd20894268f48510cb-180x300.png)](http://c.kensou.me/blog/smartisan-os-0-2-alpha-for-nexus-4/%e6%88%aa%e5%b1%8f20130724133145/)

[![截屏20130724133210](http://7sbxmt.com1.z0.glb.clouddn.com/4d44ba30af10c566589923d7199a1284-180x300.png)](http://c.kensou.me/blog/smartisan-os-0-2-alpha-for-nexus-4/%e6%88%aa%e5%b1%8f20130724133210/)

可正常使用的功能：

* 电话 
* 短信 
* 相机 
* google套件(联系人，Play) 
* WIFI 
* GPS 

已知问题：

* 如果未使用全屏方式，桌面图片会被挤压变形
* 解锁屏幕，拨号盘，由于分辨率的问题会有白边 
* Chrome浏览器闪退 （可使用其它第三方浏览器，如UC，本ROM未内置，请自行下载） 

隐藏虚拟按键，使用Pie Pro来代替虚拟按键的办法： 


首先打开 Pie Pro软件，在PIE这一屏中打开PIE PRO的开关，可以选择左侧或者右侧，任意钩选，也可同时钩选。
[![截屏20130724133240](http://7sbxmt.com1.z0.glb.clouddn.com/5bb4072f943be017f9fa87c3f4152d9d-180x300.png)](http://c.kensou.me/blog/smartisan-os-0-2-alpha-for-nexus-4/%e6%88%aa%e5%b1%8f20130724133240/)

然后在级别1中添加第一按键，点“短击”，弹出的菜单中选择“工具－返回”，然后确定，这样就添加了一个抽屉式的菜单,
[![截屏20130724134012](http://7sbxmt.com1.z0.glb.clouddn.com/1f506bf971c7fdb2073bca1c9fc6dd45-180x300.png)](http://c.kensou.me/blog/smartisan-os-0-2-alpha-for-nexus-4/%e6%88%aa%e5%b1%8f20130724134012/)

使用时手指从屏幕左侧或右侧向屏幕内滑动，就会弹出你设定好的菜单，同样的方法，你可以最多添加5个按键。
[![截屏20130724133229](http://7sbxmt.com1.z0.glb.clouddn.com/c93425ecf7f4fe93760418a6d08f1e91-180x300.png)](http://c.kensou.me/blog/smartisan-os-0-2-alpha-for-nexus-4/%e6%88%aa%e5%b1%8f20130724133229/)

添加好后可以关闭系统的虚拟按键：使用自带的文件管理器，选择设置：

[![截屏20130724133015](http://7sbxmt.com1.z0.glb.clouddn.com/376981f83907b43ff9fbb48b7eb48f93-180x300.png)](http://c.kensou.me/blog/smartisan-os-0-2-alpha-for-nexus-4/%e6%88%aa%e5%b1%8f20130724133015/)

打开ROOT权限
[![截屏20130724133025](http://7sbxmt.com1.z0.glb.clouddn.com/4b2f58c53a6f454290a4f663de25fff7-180x300.png)](http://c.kensou.me/blog/smartisan-os-0-2-alpha-for-nexus-4/%e6%88%aa%e5%b1%8f20130724133025/)

[![截屏20130724133033](http://7sbxmt.com1.z0.glb.clouddn.com/40f8acd4f870eec26164dbda16a4f393-180x300.png)](http://c.kensou.me/blog/smartisan-os-0-2-alpha-for-nexus-4/%e6%88%aa%e5%b1%8f20130724133033/)

选择/system/build.prop文件
[![截屏20130724132956](http://7sbxmt.com1.z0.glb.clouddn.com/f103d82a76720d4aa902ffbcb9909b46-180x300.png)](http://c.kensou.me/blog/smartisan-os-0-2-alpha-for-nexus-4/%e6%88%aa%e5%b1%8f20130724132956/)

注释掉这行：> #qemu.hw.mainkeys=0
[![截屏20130724132932](http://7sbxmt.com1.z0.glb.clouddn.com/ca27a1957683efea9c254960f7d81ece-180x300.png)](http://c.kensou.me/blog/smartisan-os-0-2-alpha-for-nexus-4/%e6%88%aa%e5%b1%8f20130724132932/)


[下载地址（百度网盘）](http://pan.baidu.com/share/link?shareid=3790763553&uk=1896185499 "下载地址（百度网盘）")

我用了几天的时间，日常使用不会有大的问题，不过毕竟只是一个alpha版，太挑剔者请慎重，这里只是为了第一时间体验。

有任何问题可以在这里交流，不过不一定可以解决。

[围脖交流：http://weibo.com/u/1924225341](http://weibo.com/u/1924225341 "微博 ")
## Comments

**[可可熊](#25381 "2013-07-25 09:17:27"):** 拨号盘右侧有白条，不影响使用。

