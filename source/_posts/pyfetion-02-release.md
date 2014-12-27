title: PyFetion 0.2 发布
tags:
  - PyFetion
  - Python
id: 743
categories:
  - Life
date: 2009-12-12 20:14:05
---

PyFetion 0.2版本发布，协议根据移动09.11.04的飞信版本：Fetion2008 3.5.2(安全加强版) 

[http://code.google.com/p/pytool/](http://code.google.com/p/pytool/)
更新内容包含：

1.  增加查看飞信好友是否隐身功能
2.  增加登录时状态的选择[隐身 在线 忙碌 离开]
3.  日志改用Python的logging模块
4.  增加对好友状态改变的处理( 如上线等)
5.  重写TCP方式中的底层通信函数
6.  使用对列保存接收到的多余消息( 例如发短信时本来应该返回200 OK 却先来了个BN通知消息，以前这样会出错，现在底层会把BN消息放在队列中，返回200 OK)
7.  修改了一些异常处理方式
8.  增加登出，删除好友函数
9.  改写get_contactlist函数，使用一个dict保存当前的好友列表
10.  增加一个receive函数 做客户端的时候可以在一个线程中主调用该函数，所有的消息都会yield出来(请参考fetion.py)
11.  修正向PC发送消息的命令，飞信新增加了一个CatMsg的命令
12.  增加接收从最新版本PC端发送的消息功能;这个比较麻烦新版本飞信对每一个新会话使用fork出一个线程的方式;

        John先给服务器说我要开个新会话，服务应答一个消息说，你去这个IP吧，带着这个密钥
        于是John就连到了这个IP，并使用这个密钥登录，接着发一个包含Bob的uri的邀请命令;
        服务器把这个通知(包含IP 密钥和邀请者John的uri)给Bob，Bob收到服务器的通知后也用同样的密钥登录该IP
        这时John收到Bob进入会话的消息，他就开始正式发消息了13.  许多清理了修正
14.  调整类的结构
15.  改用MIT License
16.  增加了一个CLI的飞信客户端 跨平台支持
17.  Fedora8 Python2.5.1测试;Windowx XP Python2.6.4测试;Win7 Python2.6.2测试;Mac 10.5.7 Python2.5.1
18.  我忘记在这里列出来的

./fetion.py
------------------------基于PyFetion的一个CLI飞信客户端-------------------------

        命令不区分大小写中括号里为命令的缩写

        help[?]            显示本帮助信息
        ls[l]                列出好友列表
        status[st]        改变飞信状态 参数[0隐身 1离开 2忙碌 3在线]
                             参数为空显示自己的状态
        msg[m]           发送消息 参数为序号或手机号 使用quit退出
        sms[s]            发送短信 参数为序号或手机号 使用quit退出
                             参数为空给自己发短信
        find[f]            查看好友是否隐身 参数为序号或手机号
        add[a]           添加好友 参数为手机号或飞信号
        del[d]            删除好友 参数为手机号或飞信号
        cls[c]            清屏
        quit[q]           退出对话状态
        exit[x]           退出飞信

fetion.py特色:

1.  多线程支持，同时收发消息
2.  添加，删除，好友，判断好友是否隐身功能
3.  占用资源少，我正写这博客的时候官方的飞信占我96.8M的内存
4.  跨平台支持
5.  扩展性好，加两行代码就可以实现从手机发命令关机等功能
6.  其它我没发现的
## Comments

**[taotao](#6853 "2009-12-14 13:21:07"):** 我错了，uuid是对的，但是在登录的第二步会返回500错误。我不知道这是是不是GAE对urllib重载导致的。出错的地方就在5次retry那，因为不是405错误，所以直接raise跳出了。

**[cocobear](#6854 "2009-12-14 15:39:55"):** 500错误是飞信服务器那边的问题，我试过了，可以登录，发短信 不过也遇到过500错误，飞信服务器不太稳定，前几天有一段时间就干脆登录不上去了。

**[yorks](#6855 "2009-12-14 16:36:11"):** 能改成手机终端登录吗？

**[可可熊](#6856 "2009-12-14 18:40:33"):** To:yorks 首先你手机得有一个shell,然后得支持Python。 我的手机太老了，不行。

**[非金属](#6895 "2009-12-19 12:33:06"):** 这么多人关注...

**[yorks](#6896 "2009-12-19 13:43:59"):** """协议应该都是一样的吧。我不太清楚手机上协议和PC上的区别。 但是你用这样的协议在手机上实现了，也应该可以使用飞信的，除非你走的是移动的网络，然后移动直接判断你手机号那种方式。""" 移动有根据移动的网络（IP？？）来判断登录方式？但是从您给出的协议上看，看不出移动是如何判断的？所以我想手机版的协议和PC版的协议应该不同，这仅仅猜测，无法验证呢。。。

**[柠檬园主](#7132 "2010-01-13 01:12:30"):** 用get_contactlist方法得到的好友列表里是这样的 {'sip:888888888@fetion.com.cn;port=8888':['nickname','13888888888','0','B'],'sip.....} 这样的格式，然后我在LOGIN的时候将这个LIST转换出另一个辞典 {'13888888888','sip:88888888@fetion.com.cn;port=8888'} 再判断要发送的手机号，如果不是本机（登录手机号）并且号码在这个DICT里，就取出SIP来。然后在send的时候把原来和to由tel:13888888888改成了sip:888888888,但后怎么再login取出来的contactlist辞典里，value的list里的第二项总是空，也就没办法测试是否能成功了。郁闷。

**[柠檬园主](#7130 "2010-01-12 23:22:57"):** 原来是因为飞信改协议了。 不过按下面这个博客，他已经解决了。 {“2009年12月9日中国移动飞信服务器升级，变更了登录地址和部分协议。升级后的协议无法直接给接收方手机号(tel)发送短信，只能给飞信号(sip uri)、自己的手机号发送短信。本人通过重新抓包，对飞信协议进行分析，修改了sms.api.bz接口代码，通过将接收方手机号(tel)转换为 user-id，再通过user-id转换为飞信号(sip uri)，进行短信发送，一切OK。”} 地址：http://blog.s135.com/fetion_api/

**[cocobear](#7140 "2010-01-13 16:58:31"):** 你可以参考fetion.py 从手机号获取对应的uri 使用Uri发送短信就可以了。

**[cocobear](#7124 "2010-01-12 11:29:13"):** 给好友发短信，使用sip:xxx 这种方式，而不是直接使用手机号，移动目前不支持手机号那种方式。

**[柠檬园主](#7133 "2010-01-13 01:36:22"):** 搞定了，就是按上面的办法。 在login里增加下面for k,v开始的部分 #self.get_personal_info() if not self.get_contactlist(): d_print("get contactlist error") return False for k,v in self.contactlist.items(): print v print k[:13] if k[:4]=='sip:' and len(v[1])==11 and v[1].isdigit(): self.num2sip[v[1]] = k[:13] send_msg下的这个判断 elif flag != "CatMsg" and len(to) == 11 and to.isdigit(): 下面改成： if to != self.mobile_no and to in self.num2sip.keys(): to = self.num2sip[to] else: to = "tel:"+to 当然num2sip = {}要在class前面先定义一下。

**[辉](#7041 "2009-12-31 10:33:41"):** 是JDK吗 JDK是1.6 用的JyDT支持Jython2.1

**[cocobear](#7042 "2009-12-31 11:00:07"):** Python版本低了，要2.5以上。

**[辉](#7043 "2009-12-31 11:00:08"):** 我下了个Jython2.5 运行fetion.py又出现下面的错误啊Traceback (most recent call last): File "fetion.py", line 450, in sys.exit(main()) File "fetion.py", line 399, in main mobile_no = raw_input(toEcho(“13810389680”)) File "fetion.py", line 347, in toEcho return str.decode('utf-8').encode((os.name == 'posix' and 'utf-8' """or 'cp936'""")) TypeError: expected a str

**[cocobear](#6852 "2009-12-14 13:19:21"):** To: 剑23 刚修正了这个问题，感谢反馈。

**[匆匆](#6858 "2009-12-15 00:48:45"):** 真不错。上一个星期还看PyFetion呢，这周就更新了:) BTW:好像用退格键删中文会有问题，好像只删了半个中文。。。 Exception in thread Thread-4: Traceback (most recent call last): File "/usr/lib64/python2.6/threading.py", line 522, in __bootstrap_inner self.run() File "./fetion.py", line 90, in run self.cmd(raw_input(self.hint)) File "./fetion.py", line 107, in cmd self.phone.send_sms(toUTF8(arg)) File "./fetion.py", line 344, in toUTF8 return str.decode((os.name == 'posix' and 'utf-8' or 'cp936')).encode(') File "/usr/lib64/python2.6/encodings/utf_8.py", line 16, in decode return codecs.utf_8_decode(input, errors, True) UnicodeDecodeError: 'utf8' codec can't decode byte 0xe8 in position 3: uned end of data

**[可可熊](#6857 "2009-12-14 18:57:59"):** To:BackFire 你可以看下PyFetion.log 卡在哪儿了，估计是你网络的问题。

**[BackFire](#6850 "2009-12-14 12:38:54"):** 我这里python fetion.py输入手机号和密码之后，就一直显示滚动的“--->” 为啥？

**[剑23](#6851 "2009-12-14 12:42:30"):** 好友列表序号对应有问题，显示的序号对应有些正确，但有些对不上

**[可可熊](#6842 "2009-12-14 00:18:34"):** select只有使用TCP方式才需要用到，uuid以前似乎正常。或者自己写一个这样的函数。

**[yorks](#6892 "2009-12-18 21:07:42"):** """To:yorks 首先你手机得有一个shell,然后得支持Python。 我的手机太老了，不行。""" 其实我是想了解一下fetion手机版的协议？不知道如何下手！

**[柠檬园主](#7117 "2010-01-11 16:05:11"):** 可可熊，我是从SVN上当的PYF下来， 目前还在本地开发测试，但为什么只能发送给自己？给飞信好友发都收不到，并且也不返回错误，这个是飞信服务器的问题吗？代码如下： phone = PyFetion.PyFetion("13XXXXXXXX","XXXXXXXX","HTTP") #try: phone.login() #只能发送给飞信好友（或自己） phone.send_sms(msg) #发给自己 phone.send_sms(msg,"15XXXXXXX20") #发给好友 phone.logout()

**[可可熊](#6870 "2009-12-15 19:45:17"):** To:匆匆 删除中文有没问题啊，这个是raw_input这个函数，你自己手动试下这个函数，你看出错吗？

**[CODY](#6871 "2009-12-15 22:20:24"):** 用CLS客户端，接收信息的速度特别慢，有时候还收不到，这是什么原因？

**[可可熊](#6872 "2009-12-15 22:27:24"):** To:CODY 接收从PC来的信息会慢一点，有5秒的延时; 收不到是收不到哪儿的信息呢？有没有具体的日志

**[CODY](#6881 "2009-12-16 14:34:47"):** 原因已经查明，你在self._sock.recv中没有设置超时，导致卡死在那里。 加了setTimeOut后就不会卡死了。。。

**[zengleo](#6889 "2009-12-17 13:28:04"):** 这个真棒，一直关注可可熊博客~ 呵呵！

**[cocobear](#7052 "2010-01-01 21:43:06"):** 如果你是从浏览器里面复制的代码，可能会出错的，直接下载代码，或者使用SVN.

**[taotao](#6840 "2009-12-13 23:19:10"):** select不要就行了吧，但是好像uudi系列函数工作不太正常，所以没搞定。。。

**[草儿](#6820 "2009-12-12 23:12:47"):** 希望pyfetion越来越强大，虽然现在不用飞信了，还是顶一个。

**[taotao](#6821 "2009-12-12 23:32:48"):** 可可熊真勤劳^^

**[taotao](#6822 "2009-12-12 23:35:37"):** 谢谢之余想问下这个支不支持群发短信的呢？

**[a](#6819 "2009-12-12 22:06:43"):** 感谢cocobear的辛勤劳作，向你表示由衷的敬意！ 你的PyFetion真的很棒！

**[可可熊](#6836 "2009-12-13 21:55:13"):** To:taotao GAE上使用HTTP方式，我测试过HTTP方式，发送短信 消息是没问题的，不过我没有在GAE上测试。 你可以测试一下。 HTTP方式的变化不大

**[阿神](#6837 "2009-12-13 22:37:58"):** nice work. keep up!

**[Homer](#6835 "2009-12-13 21:17:03"):** 支持一下作者的辛勤工作～ 辛苦了～

**[小卒](#6830 "2009-12-13 13:54:47"):** 太好了，先收下了。

**[可可熊](#6893 "2009-12-18 23:49:18"):** 协议应该都是一样的吧。我不太清楚手机上协议和PC上的区别。 但是你用这样的协议在手机上实现了，也应该可以使用飞信的，除非你走的是移动的网络，然后移动直接判断你手机号那种方式。

**[taotao](#6833 "2009-12-13 16:12:22"):** To:可可熊 En, it's ok, thank you!

**[taotao](#6834 "2009-12-13 17:52:47"):** 请问可可熊，现在2009的协议是不是GAE就真不能用了呢？

**[1](#6827 "2009-12-13 10:49:27"):** 在python2.6测试成功，期待可以移植到python3.1 我自己试过用2to3来转换,转换后由于python3默认定义所有字符串就是unicode，而且严格区分bytes与str对象，后来实在搞不惦哈

**[可可熊](#6828 "2009-12-13 11:40:36"):** To:taotao 群发短信就是一个一个的发短信，你可以写个循环 for i in []: send_sms('hi',i) To:1 python 3.1改动还比较大，我没有环境测试。等这个版本的一些问题解决了我会折腾下3.1

**[辉](#7031 "2009-12-30 11:11:41"):** 运行Fetion.py时出现错误Traceback (innermost last): File "fetion.py", line 7, in ? File "F:\bea\workplace\Test\\.cachedir\packages\PyFetion\PyFetion.py", line 97 class SIPC(): ^ SyntaxError: invalid syntax 是怎么回事啊，我在eclipse上装的Jython 不懂Python 帮下忙谢谢

**[cocobear](#7033 "2009-12-30 18:46:43"):** 你的版本是多少，好像不支持这class。

**[nsnake](#7601 "2010-03-12 12:28:31"):** 不知道他的手机号到url是怎么转换的

**[tina](#8170 "2010-06-03 15:02:45"):** 你好，试了下查看好友隐身的功能。一输入就卡住了 只能重启pyfetion 试了很多人 都不行 请问这是为什么呢

**[可可熊](#8172 "2010-06-04 09:13:24"):** 可能对方的版本的原因，我是在旧的版本上测试的。

**[韩世界15680180146](#8223 "2010-06-19 21:30:30"):** 我下了你的新版本，就2个文件。这个应该是LINUX下用的吧。有WINDOWS下的版本吗？？？

**[天文也](#8343 "2010-07-25 21:00:35"):** https://uid.fetion.com.cn/ssiportal/SSIAppSignIn.aspx 不可用了？请楼主确认！

**[smilepig](#8346 "2010-07-26 08:27:56"):** @天文也 飞信换协议了又～～～等可可熊更新吧～～

**[frank](#8377 "2010-08-02 15:03:02"):** 最近开始不能用了，是不是uri变了还是什么，麻烦更新下，谢谢。

