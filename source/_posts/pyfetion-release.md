title: PyFetion发布
tags:
  - fetion
  - PyFetion
  - Python
id: 434
categories:
  - Life
date: 2008-12-31 11:09:50
---

本来想着继续完善后再放出来，不过看到[别人已经有做出来的](http://blog.donews.com/gradetwo/archive/2008/12/26/1412882.aspx?Pending=true)，俺就不重复劳动了，[前篇文章](http://c.kensou.me/blog/2008/12/03/write-fetion-with-python-pyfetion/)已经提到了基本的功能，这里也就不重复了。

我需要的功能差不多了，最近事多也就不改了，各位需要使用飞信聊天的的话建议用官方的，或者期待我第一个链接提到的。做开发的话俺还是用自己的比较清楚，当然别人的如果开放的话也可以参考一下。

[ 这里写个简单的文档吧：](#)


	def main(argv=None):
	    try:
	        phone = PyFetion("138888888","888888","TCP")
	        #创建一个PyFetion类的对象，参数是手机号，密码，和登录的方式，TCP 或者 HTTP 
	        #登录时也可以选择06或者08协议，在代码的最上面自己修改
	    except PyFetionInfoError,e:
	        print "corrent your mobile NO. and password"
	        return -1
	        #处理获取配置信息错误，只有在手机号状态异常或者密码错误时会出现

	    phone.login()
	    #向服务器做登录请求

	    #phone.get_offline_msg()
	    #获取离线消息，没实现:-)

	    #phone.add("13888888888")
	    #添加好友 参数为手机号 如果对方没有注册飞信则会添加该手机为好友
	    #phone.get_info()
	    #获取自己飞信号的基本信息

	    #phone.get_contact_list()
	    #获取联系人列表 返回的是一个XML串 我没做处理

	    #phone.send_sms("Hello, ",long=True)
	    #最关键的一个功能 发送短信  该函数的原型为send_sms(self,msg,to=None,long=False):
	    #分别为消息内容，发送对象，是否使用长短信格式(支持180字)

	    #phone.send_msg("hello","13888888888")
	    #发送IM消息 发送对客户端上

	s = "2008-12-31 02:39:00."
	    for i in range(100,500):
	        time = s + str(i)
	        phone.send_schedule_sms("请注意，这个是定时短信",time)
	        #这是一个发送定时短信的函数，参数分别为发送内容，发送时间，发送对象的手机号（如果该值为空则发送给自己） 
	        #这里需要注意一点飞信发送定时短信使用的是标准的时间，中国的时间是+8的，所以你在发送的时候要在自己电脑时间上减个8
	        #还有一点发送的时间要至少比当前时间多5分钟以上
	        #这里我测试了一下和秒钟可以发多少短信 试着发了400 成功了100 不过最后到时间只到手机上两条 呵呵 也许是延时吧，我上次至少发了十来条
	    #time_format = "%Y-%m-%d %H:%M:%S"
	    #time.strftime(time_format,time.gmtime())
	    #获取当前标准时间的函数，我为大家想得真周到啊:-) 对了还得提醒大家先import time




[代码下载](http://cocobear.github.io/code/PyFetion.py)
## Comments

**[可可熊](#5788 "2009-04-28 23:52:26"):** To:nile9997 该列表不包括手机号，要获得手机号需要使用get_info。

**[nile997](#5804 "2009-05-01 15:15:57"):** 现在如果要给一个好友发短信，PyFetion是不是不能判断对方是不是在PC端？ 我现在处理的方法是先判断一下他是不是在PC端，然后再选择send_msg或者send_sms...

**[草儿](#5746 "2009-04-19 13:17:08"):** 看到了http://www.cnbeta.com/articles/82231.htm这篇文章，有人发上去了。

**[holisp](#5741 "2009-04-18 13:08:22"):** 我在本地的gap上测试是可以给自己的手机通过http方式发送的

**[zay](#5742 "2009-04-18 19:54:56"):** bear同学我成功的移植到了GAE 谢谢你提供的代码，pyfetion我自己还修改了些小地方，不过这代码还不是很“透明”，等完善几个地方后，发给你过目下……不知道你有没有放在google code上，这样可以方便共同开发 很感谢你 http://zayfetion.appspot.com/ 用的是cocobear分享的pyfetion

**[可可熊](#4810 "2009-01-12 10:00:49"):** 隐身登录暂时没加，有时间俺会弄弄。现在不记得在哪儿加。

**[nonight](#4792 "2009-01-07 17:46:02"):** 怎么隐身登陆？ 是不是在url中要多加个登陆状态的参数呢？“%s?mobileno=%s&pwd=%s”

**[liuxin](#4779 "2009-01-05 10:53:29"):** 莫非前一段python-cn上传的热热闹闹的 python飞信库就是你？ 嘿嘿 赞个

**[Zoom.Quiet](#4777 "2009-01-03 16:11:06"):** 整个 code.google 的空间发布吧，有什么问题，也好统一管理不是？

**[volans](#4788 "2009-01-06 10:44:21"):** AttributeError: 'URLError' object has no attribute 'code', 那个错误可以把，这行代码： except urllib2.URLError, e: 替换成： except urllib2.HTTPError, e: 其实问题的根源不在这里，应该是一个timeout的错误。反正我在有http代理的情况下没成功过，次次都是timeout。

**[可可熊](#5778 "2009-04-27 21:35:19"):** To:nile997 可以获得好友的手机号，前提是该好友对你设置了显示手机号的权限。

**[可可熊](#4782 "2009-01-05 12:03:09"):** 也没多热闹吧:-) 正打算弄个google code呢。

**[la.onger](#4746 "2008-12-31 12:49:42"):** 真是太感谢了。我找到了一些py的fetion库，不过功能上不如您的，您这个放出来了，对我真是太有用了

**[anoum](#4749 "2008-12-31 13:51:51"):** windows xp 下出错了 Traceback (most recent call last): File "test_pyFetion.py", line 13, in phone.login() File "D:\\...\\...\pyfetion\PyFetion.py", line 79, in login (self.__ssic,self.__domain) = self.__get_uri() File "D:\\...\\...\pyfetion\PyFetion.py", line 225, in __get_uri ret = self.__http_send(url,login=True) File "D:\\...\\...\pyfetion\PyFetion.py", line 192, in __http_send code = e.code AttributeError: 'URLError' object has no attribute 'code' shell returned 1

**[可可熊](#4750 "2008-12-31 14:32:01"):** 我在XP下试过没问题； 把188行的 request 打印出来瞧瞧。

**[www](#4751 "2008-12-31 15:36:05"):** ok,very good.

**[anoum](#4752 "2008-12-31 16:17:12"):** 我用的IE代理上网的...可能和这个有关系吧 回家再试.

**[可可熊](#4753 "2008-12-31 16:19:48"):** 不支持代理的。

**[菜哥](#4756 "2008-12-31 17:00:10"):** 试试设置环境变量 set HTTP_PROXY=http://xxxxx:8080 urllib库是支持http代理的；

**[可可熊](#4757 "2008-12-31 17:15:10"):** 这个就只能使用HTTP方式通信了，这种使用代理很方便的，库里也有使用代理用到的函数。

**[nile997](#5777 "2009-04-27 20:46:48"):** send_sms 既可以发给手机号码，也可以发给飞信号码。呵呵，看见了。

**[nile997](#5776 "2009-04-27 20:37:03"):** get_contact_list() 获得了好友列表。 但是这个好友列表并不包括好友的手机号码。 那么就不能根据这个好友列表通过PyFetion发送短信吗？

**[spark](#5389 "2009-03-06 15:06:16"):** armiuswu写于09年01月31日 好像目前无法移植到gae上，gae不支持 User-Agent 的header, 好像也不真正支持https协议...... 现在gae支持urllib这些库了 。

**[nile997](#5826 "2009-05-04 12:30:06"):** get_info获取的是自己的信息。 我的意思是怎么样获取所有好友的手机号码？

**[可可熊](#5827 "2009-05-04 13:04:03"):** ret = self.get_info(uri) no = re.findall('mobile-no="(.+?)" ',ret) 看这段代码。

**[nile997](#5837 "2009-05-04 21:17:28"):** 嗯，现在先获得contact_list,然后通过get_info获取每个人的手机号码。 谢谢可可熊的耐心！

**[nile997](#5843 "2009-05-05 18:36:07"):** 不过这样的效率实在不高…

**[可可熊](#5845 "2009-05-05 21:04:33"):** 你想实现什么样的功能？

**[ttytty](#4930 "2009-01-28 15:07:46"):** 这个确实很好的一个东西，给自己发短信，但是我们普通人没有自己的在线主机，不知道楼主或者楼里哪位神仙可以把这个移植到GAE上去，提供个url接口（手机号，密码，发送目标，发送内容），然后很多有飞信人就可以用这个来做些事情了

**[spark](#5150 "2009-02-14 22:54:50"):** 一楼的anoum,这个脚本执行时候如果路径中有中文就会出现你的错误，把这个脚本文件放到全英文的路径下运行即可。 另外，怎么收消息呢？能不能把这个功能补上，这样的话，让飞信机器人一直在线，我在外边就可以用手机短信控制电脑了。

**[Jay](#5175 "2009-02-15 22:29:09"):** Yangmin写于09年01月15日 在我的linux服务器上试了一下，发现能够登录，添加好友，但是就是发不了信息（发到PC客户端和手机终端都不行）。我把相关信息贴在这里，请帮忙看下，谢谢。 python version:2.5 date : Thu Jan 15 16:21:11 CST 2009 类似这种格式（不知道有没有影响） 测试代码： from PyFetion import * phone = PyFetion("13466660571","superlee","TCP") phone.login() phone.send_sms("Hello, ","1348888888",long=True) phone.send_msg("hello","134888888") 执行以上代码后，错误信息如下： [PyFetion]:code 200 [PyFetion]:code 200 [PyFetion]:content M fetion.com.cn SIP-C/2.0 F: 586767865 I: 1 Q: 1 M T: hello C: text/plain K: SaveHistory L: 11 13426310813 [PyFetion]:response SIP-C/2.0 500 Server Internal Error T: 13426310813 I: 1 Q: 1 M [PyFetion]:code 500 [PyFetion]:code 500 我的错误和他的一样，该怎么修正呢？

**[phomeray](#4994 "2009-02-03 16:51:57"):** 测试了下脚本，发送对象 to 要是飞信用户才能收到

**[可可熊](#4970 "2009-02-01 10:58:23"):** GAE不太了解:-(

**[可可熊](#5165 "2009-02-15 10:07:55"):** 你不可以用自己的手机回复到自己的飞信上边去。

**[armiuswu](#4962 "2009-01-31 21:27:37"):** 好像目前无法移植到gae上，gae不支持 User-Agent 的header, 好像也不真正支持https协议......

**[Solrex Yang](#4863 "2009-01-16 00:14:38"):** Great work!

**[可可熊](#4864 "2009-01-16 09:46:48"):** To:Yangmin 那个send_msg函数有点问题,我改后面代码的时候影响到这个函数了.我会尽快修改的.

**[可可熊](#4865 "2009-01-16 09:47:21"):** 不过发短信那个函数是没有问题的.

**[可可熊](#5208 "2009-02-18 21:20:17"):** 问题和他一样，我前面也回答过了，我的代码有点问题。

**[Yangmin](#4858 "2009-01-15 16:22:51"):** 在我的linux服务器上试了一下，发现能够登录，添加好友，但是就是发不了信息（发到PC客户端和手机终端都不行）。我把相关信息贴在这里，请帮忙看下，谢谢。 python version:2.5 date : Thu Jan 15 16:21:11 CST 2009 类似这种格式（不知道有没有影响） 测试代码： from PyFetion import * phone = PyFetion("13466660571","superlee","TCP") phone.login() phone.send_sms("Hello, ","1348888888",long=True) phone.send_msg("hello","134888888") 执行以上代码后，错误信息如下： [PyFetion]:code 200 [PyFetion]:code 200 [PyFetion]:content M fetion.com.cn SIP-C/2.0 F: 586767865 I: 1 Q: 1 M T: hello C: text/plain K: SaveHistory L: 11 13426310813 [PyFetion]:response SIP-C/2.0 500 Server Internal Error T: 13426310813 I: 1 Q: 1 M [PyFetion]:code 500 [PyFetion]:code 500

**[nonight](#4800 "2009-01-10 17:01:09"):** 你好， 怎么隐身登陆呢？

**[andy](#6207 "2009-07-05 08:28:22"):** 您好，由于我是一名java开发人员，项目上又有此方面的需求，不知朋友能否写一份java版的另：我看到有个jython项目，尝试着将它放在了eclipse中进行调试，但报如下的错 File "D:\jython2.5.0\Lib\socket.py", line 1408, in make_ssl_socket ssl_socket.startHandshake() at com.sun.net.ssl.internal.ssl.Alerts.getSSLException(Unknown Source) at com.sun.net.ssl.internal.ssl.SSLSocketImpl.fatal(Unknown Source) at com.sun.net.ssl.internal.ssl.Handshaker.fatalSE(Unknown Source) at com.sun.net.ssl.internal.ssl.Handshaker.fatalSE(Unknown Source) at com.sun.net.ssl.internal.ssl.ClientHandshaker.serverCertificate(Unknown Source) at com.sun.net.ssl.internal.ssl.ClientHandshaker.processMessage(Unknown Source) at com.sun.net.ssl.internal.ssl.Handshaker.processLoop(Unknown Source) at com.sun.net.ssl.internal.ssl.Handshaker.process_record(Unknown Source) at com.sun.net.ssl.internal.ssl.SSLSocketImpl.readRecord(Unknown Source) at com.sun.net.ssl.internal.ssl.SSLSocketImpl.performInitialHandshake(Unknown Source) at com.sun.net.ssl.internal.ssl.SSLSocketImpl.startHandshake(Unknown Source) at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method) at sun.reflect.NativeMethodAccessorImpl.invoke(Unknown Source) at sun.reflect.DelegatingMethodAccessorImpl.invoke(Unknown Source) at java.lang.reflect.Method.invoke(Unknown Source) javax.net.ssl.SSLHandshakeException: javax.net.ssl.SSLHandshakeException: sun.security.validator.ValidatorException: PKIX path validation failed: java.security.cert.CertPathValidatorException: timestamp check failed 是否是因为这个jython中socket.py的问题？

**[仰望星空](#6181 "2009-06-27 13:12:08"):** 正是我要找的，谢谢!

**[可可熊](#6203 "2009-07-03 13:49:36"):** 你要使用utf-8编码你需要发送的内容。 pyfetion代码中有示例的。看下你文件的编码和python代码上面的coding是否相符。

**[三无浪子](#6201 "2009-07-02 18:39:55"):** 你好！请问一下，我用你的程序发短信的时候，若短信内容为英文时没问题，但是为中文时就是乱码，我也找了好多关于Python字符集的资料看了，也照着改了，但是没有一个办法好使的。所以还是向前辈请教一下。

**[三无浪子](#6209 "2009-07-05 21:43:46"):** 你好！请问一下，针对Python Fetion目前的版本，如果对方没有注册飞信，我只是加了他为手机好友，在这种情况下，我给他是发不了短信的吧？

