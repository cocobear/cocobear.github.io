title: xmms乱码解决的误区以及播放音乐问题（FC系列）
tags:
  - Fedora
  - Linux
  - xmms
id: 15
categories:
  - Linux
date: 2007-04-06 19:17:10
---

Thursday, 30\. November 2006, 13:07:48

大多人的xmms出现乱码在网上找到的解决办法基本上是：

第一步：禁用ID3V2标签
首选项=< 音频输入输出插件 选中 MPEG Layer 1/2/3 播放器 然后再点下面的 “配置 ” 切换到标题后选择“禁用ID3V2标签” =< “确定”
第二步：选择字体
我的是
引用:
（点击首选项的字体标签，直接copy下面的就可以了）
播放清单：
-sony-*-*-*-*-*-16-*-*-*-*-*-iso8859-1,-*-*-*-*-*-*-16-*-*-*-*-*-gbk-0
把前面那两个勾都打上。还有其它的字体设置如：
主窗口：
-sony-*-*-*-*-*-16-*-*-*-*-*-iso8859-1,-*-*-*-*-*-*-16-*-*-*-*-*-gbk-0
第三步：修改标题显示：
很多贴子里面都没提到这一步！
在标题格式里只填上 %f ， 默认的好象是 %p-%t ，不要默认的

在一般情况下这种办法是可行的（我在FC5和FC1都是这样解决的），但是在FC6中这样做是没有用的。

真正的解决办法：

首先用locale命令查看下你的语言设置：
# locale
LANG=zh_CN.GB18030
LC_CTYPE=zh_CN.GB18030
LC_NUMERIC="zh_CN.GB18030"
LC_TIME="zh_CN.GB18030"
LC_COLLATE="zh_CN.GB18030"
LC_MONETARY="zh_CN.GB18030"
LC_MESSAGES="zh_CN.GB18030"
LC_PAPER="zh_CN.GB18030"
LC_NAME="zh_CN.GB18030"
LC_ADDRESS="zh_CN.GB18030"
LC_TELEPHONE="zh_CN.GB18030"
LC_MEASUREMENT="zh_CN.GB18030"
LC_IDENTIFICATION="zh_CN.GB18030"
LC_ALL=

我的locale命令输出是这样的，也就是说在我当前的用户下面语言编码的设置为zh_CN.GB18030（fc6选择中文语言安装输出应该不是这样的，因为我选择的是英文语言安装界面，在安装包里选择了中文支持），重要的就在这里，如果你在命令行下运行xmms会得到与下面相似的输出：

bash-2.05b$ xmms
The font "-misc-simsun-medium-r-normal--14-*-*-*-*-*-gb2312.1980-0,
-misc-simsun-medium-r-normal--14-*-*-*-*-*-gb2312.1980-0" does not support all the required character sets for the current locale "zh_CN.GB18030"
(Missing character set "ISO10646-1")
The font "-misc-simsun-medium-r-normal-*-*-120-*-*-c-*-gbk-0" does not support all the required character sets for the current locale "zh_CN.GB18030"
(Missing character set "ISO10646-1")

产生这样输出的原因是你使用的字体与当前的locale语言编码设置不相同，现在我们需要改当前用户的locale，编辑当前用户下的.bashrc文件，添加类似这样的语句：

export LANG="zh_CN.utf8"
export LC_CTYPE="zh_CN.utf8"

其实这里就是根据的你喜好你也可以选择gb2310编码，重要的是编码与字体的编码一致就OK了。

重启X，问题就会解决。

额外的内容：

许多人装好fc6(其它的fc系列也一样）后，直接

yum install xmms

结果不能播放mp3，其实这与fc系列自带播放器不能播放mp3的原因是一样的，因为版权问题（FC自带的yum源是官方的，害怕版权问题）。

解决办法：
yum install xmms-mp3

前提是你要添加freshrpms的yum源（你也可以直接添加freshrpms源，使用yum install xmms，这样直接就可以播放mp3）

建议：安装好fc6后直接添加freshrpms与linva源

rpm -ivh [http://ftp.freshrpms.net/pub/freshrpms/fedora/linux/5/freshrpms-release/freshrpms-release-1.1-1.fc.noarch.rpm](http://ftp.freshrpms.net/pub/freshrpms/fedora/linux/5/freshrpms-release/freshrpms-release-1.1-1.fc.noarch.rpm)

rpm -ivh [http://rpm.livna.org/livna-release-5.rpm](http://rpm.livna.org/livna-release-5.rpm)

播放wma格式解决办法：

yum install xmms-wma