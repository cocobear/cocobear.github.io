title: Android4.2.2源码编译并修改系统App
tags:
  - android
  - app
id: 1003
categories:
  - Life
date: 2013-06-18 09:59:17
---

[![android-logo](http://7sbxmt.com1.z0.glb.clouddn.com/android-logo.jpg)](http://c.kensou.me/blog/compile-android4-2-2/android-logo/)

Android4.2.2源码编译

这不是一份详细的Android源码编译指南，这里只是除官方指南外的记录，请先阅读[官方编译指南](http://source.android.com/source/initializing.html "官方编译指南")

官方的编译指南挺详细的，最好看仔细一点，比如ccache这个东西，如果你老在不同的分枝切换编译这个东西能节省不少的时间，官方推荐的操作系统是Ubuntu 10.04　64位,　不过我这里原来这里有一个下载好的12.04，看到官方说明也也支持所以就在12.04上面进行编译了。

花了几天的时间下载源码，因为老是卡死在一个地方，本来把机器开着一个晚上下载，希望第二天能下载完，结果不知道啥时候已经卡死了，所以断断续续的下载了几天，天朝的环境真是不管做什么都“**多难兴邦**”。

下载完所有的源码后，最好能打包一下放在别的地方，省得弄乱了需要重新下载：

     tar -jcvf android.tar.bz2 android/

编译的时候如果你不做别的事情，最好根据你的CPU指定多线程编译:

     make -j8

我在编译的时候由于使用虚拟机，而且同时又在做别的事情，经常会造成虚拟机死掉，有时使用Ctrl+c中断了编译过程，所以出现了下面的错误：

     make: *** [out/target/product/generic/obj/SHARED_LIBRARIES/libwebcore_intermediates/LINKED/libwebcore.so] Error 1

在Google Group里找到了原因：
[https://groups.google.com/forum/?fromgroups#!topic/android-porting/LVpm39BAqRw
](https://groups.google.com/forum/?fromgroups#!topic/android-porting/LVpm39BAqRw "Solution")

解决办法：

     make clean-libwebcore > 
     make -j4 libwebcore> 
     make -j4

修改并编译系统App方法一:

如果你需要单独编译某个App，会生成apk文件和odex文件，如果你只想生成apk文件，修改在Android.mk文件

	cd packages/apps/Stk/> 
	vi Android.mk

	LOCAL_PACKAGE_NAME := Stk> 
	LOCAL_CERTIFICATE := platform> 
	#这行是新添加的:> 
	WITH_DEXPREOPT := false
修改完后使用命令：mm

编译完会在out目录下会生成Stk.apk文件，把该文件push到手机上，就可以替换原有的系统程序了：

     adb push Stk.apk /system/app/> 
     1236 KB/s (39262 bytes in 0.031s)> 
     adb shell chmod 644 /system/app/Stk.apk> 
     adb reboot

折腾了几天自己编译的系统程序（system app)总是不能在手机上运行，而从别的地方下载的app就可以用上面的命令运行，早上突然意识到是不是TARGET不对，于是试了一下：

     You're building on Linux> 
     
     Lunch menu... pick a combo:> 
          1\. full-eng> 
          2\. full_x86-eng> 
          3\. vbox_x86-eng> 
          4\. full_mips-eng> 
          5\. full_grouper-userdebug> 
          6\. full_tilapia-userdebug> 
          7\. mini_armv7a_neon-userdebug> 
          8\. mini_armv7a-userdebug> 
          9\. mini_mips-userdebug> 
          10\. mini_x86-userdebug> 
          11\. full_mako-userdebug> 
          12\. full_maguro-userdebug> 
          13\. full_manta-userdebug> 
          14\. full_toroplus-userdebug> 
          15\. full_toro-userdebug> 
          16\. full_panda-userdebug> 
     
     Which would you like? [full-eng] 8> 
     or:> 
     lunch mini_armv7a-userdebug

再次重新运行mm命令，push到手机上，终于可以运行了，网上看到的文章一般都没有提到lunch的作用，默认的情况下lunch会被设定为：full-eng，而这个选项是用给虚拟机的，所以生成的apk在手机上无法运行，所以要选择编译选项为arm。full_mako-userdebug指的是Nexus 4，因为我这里编译的只是上层应用所以用这个选项也可以。

我这里用的测试手机是MOTO ME525+ 已刷机 CM10.1（4.2.2） 已ROOT，如果你也有类似的需要请注意保持手机系统的版本和你编译的源码一致，并且你的手机已经ROOT。


修改并编译系统App方法二:

首先下载App源代码:

     git clone https://android.googlesource.com/platform/packages/apps/Launcher -b android-4.2.2_r1

然后把下载的代码通过在Eclipse的菜单<android Project from Existing Code>新建一个工程，在工程目录下新建文件夹libs, 用来存放out/target/common/obj/JAVA_LIBRARIES/framework_intermediates/classes.jar
文件，这个文件包含了一些隐藏的系统API，我们在Eclipse里编译系统app的时候需要调用这个库。

在工程文件的属性中按以下步骤添加系统库：

     Java Build Path－> Libraries - > Add Library -> User Library -> User Libraries -> New -> input User library name(like framework_android) and checkbox System library on -> Add JARs -> Select ./libs/classes.jar file> 
     
     Select: Order and Export  -> make framework_android on the top and checked

我这里使用自己编译生成的framework_intermediates/classes.jar文件总是无法成功调用到隐藏的系统API,但是通过查看jar包,可以看到com.android.internal这样的系统API. 使用别人在网上发出来的classes.jar文件可以找到系统API,不过因为不是4.2的,所以我这里编译的时候出现了其它引用错误. 

**参考文章:**

http://www.iteye.com/topic/937547

http://www.mkyong.com/android/attach-android-source-code-to-eclipse-ide
