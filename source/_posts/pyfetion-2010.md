title: PyFetion 2010
tags:
  - PyFetion
  - Python
id: 857
categories:
  - Life
date: 2010-09-21 15:14:13
---

年初就看到飞信更新新的版本，但是原来的版本还是照常能用，就一直没理它，最近2008版本的协议据说很多地方用不了，抽空更新了一下PyFetion。移动官方于2010年9月15号发布了飞信2010正式版，所以新的PyFetion也将基于这个版本实现。

网上飞信的实现很多，不过很多要不是不开源，要不是实现过于麻烦；用 Python写这个飞信，的目的是为了方便一些用户订制，把飞信的功能折腾进自己的程序里、部署在服务器上做一些好玩的东西。

很多网上的飞信实现了http://xxx.xx.xx/api?p=136xxxxxxxx&pw=xxxx&t=136xxx&sms=hello这样的WEB接口， 为了安全起见大家在使用这样的接口的时候还是要多加留心。其实在dreamhost之类支持Python的主机上部署一个Python的应用还是很方便的，GAE上使用HTTP方式也是可以的。

svn更新记录：
http://code.google.com/p/pytool/source/detail?r=83
打包下载：
http://code.google.com/p/pytool/downloads/list

大概试了一下登录，发送消息，收消息都可以。还有不太完善的地方我会慢慢修正。
## Comments

**[feiyezi](#9069 "2010-12-10 15:26:03"):** 我有两个号码，怎么一个号码可以登陆成功，另外一个号码却登陆失败，提示位 register failed ,这不是在登陆的时候需要验证码，而是在注册的时候那段code 跑到step 2的时候失败，郁闷啊！

**[磁县论坛](#8670 "2010-10-03 16:19:38"):** 用的不错

**[helen](#9032 "2010-12-03 16:42:31"):** 你好，我用你这个PyFetion，发送给我自己没问题，但是发送给别人，别人就接收不到，是不是那些人不是我的飞信好友就无法接收？还是那些人也要开通飞信？

**[cocobear](#8624 "2010-09-26 11:13:50"):** 移动的飞信

**[小康](#8649 "2010-09-29 23:14:42"):** 这个也不是怎么好用

**[七月和五月](#9031 "2010-12-03 15:25:31"):** 接收电脑来的信息时反应慢为什么呀，有没有解决办法？ 官方的版本，收到信息时只有一个包过来，直接就是M开头的，就是信息了，为什么pyhton版的要收两个发两下还不行呢，M开头包是过来的晚还是，过来了，处理的慢？

**[小康](#8620 "2010-09-25 22:23:58"):** 这是什么东西？

**[cocobear](#8618 "2010-09-25 13:38:56"):** To: Sutra 上传了加密的C代码，你可以自己编译一下。我 这里没有MAC的环境。

**[yjmade](#8983 "2010-11-23 17:55:18"):** r91 GAE遇到同上错误 : ApplicationError: 5 Traceback (most recent call last): File "/base/data/home/apps/yjfetion/1.346417405917402291/try.py", line 16, in main() File "/base/data/home/apps/yjfetion/1.346417405917402291/try.py", line 7, in main if fe.login(FetionOnline): File "/base/data/home/apps/yjfetion/1.346417405917402291/PyFetion.py", line 840, in login if not self.__get_uri(): File "/base/data/home/apps/yjfetion/1.346417405917402291/PyFetion.py", line 1336, in __get_uri ret = http_send(url,login=True) File "/base/data/home/apps/yjfetion/1.346417405917402291/PyFetion.py", line 654, in http_send conn = urllib2.urlopen(request) File "/base/python_runtime/python_dist/lib/python2.5/urllib2.py", line 124, in urlopen return _opener.open(url, data) File "/base/python_runtime/python_dist/lib/python2.5/urllib2.py", line 381, in open response = self._open(req, data) File "/base/python_runtime/python_dist/lib/python2.5/urllib2.py", line 399, in _open '_open', req) File "/base/python_runtime/python_dist/lib/python2.5/urllib2.py", line 360, in _call_chain result = func(*args) File "/base/python_runtime/python_dist/lib/python2.5/urllib2.py", line 1115, in https_open return self.do_open(httplib.HTTPSConnection, req) File "/base/python_runtime/python_dist/lib/python2.5/urllib2.py", line 1080, in do_open r = h.getresponse() File "/base/python_runtime/python_dist/lib/python2.5/httplib.py", line 197, in getresponse self._allow_truncated, self._follow_redirects) File "/base/python_runtime/python_lib/versions/1/google/appengine/api/urlfetch.py", line 241, in fetch return rpc.get_result() File "/base/python_runtime/python_lib/versions/1/google/appengine/api/apiproxy_stub_map.py", line 527, in get_result return self.__get_result_hook(self) File "/base/python_runtime/python_lib/versions/1/google/appengine/api/urlfetch.py", line 331, in _get_fetch_result raise DownloadError(str(err))

**[Sutra](#8601 "2010-09-22 14:24:49"):** 用了个 so 呀，Mac OS X上失败了： OSError: dlopen(./RSA_Encrypt.so, 6): no suitable image found. Did find: ./RSA_Encrypt.so: unknown file type, first eight bytes: 0x7F 0x45 0x4C 0x46 0x01 0x01 0x01 0x00

**[不做愤青](#8605 "2010-09-23 20:15:14"):** 这里有没有大神 在gae上使用过这个库？到底能不能用啊？我在gae上 用pyfetion0.3，本地测试都好用的，一部署到gae上就不行了。。。高人啊。。。给个demo吧。。。

**[草儿](#8598 "2010-09-22 09:00:27"):** coco最近一直在忙这个？都不见你上线了

**[YY](#9246 "2010-12-17 15:05:09"):** 不能增加和删除好友，每次做这个操作就出现提示如下，log文件我发到你邮箱了 PyFetion:del 15874141747 删除15874141747失败 PyFetion:add 13574162826 添加13574162826失败

**[liuyp](#8779 "2010-10-12 21:14:48"):** 刚刚试过还是不能登录，“登录失败“，如果密码不对则”手机号密码错误“，用svn checkout 的代码也不行。

**[可可熊](#8592 "2010-09-21 16:16:21"):** 现在可以登录了吗？是没有输入验证码的问题吧？

**[可可熊](#9089 "2010-12-13 12:21:49"):** 需要是好友才可以发

**[ybyygu](#8591 "2010-09-21 16:06:56"):** 前边没输上 "飞信发现您本次变更了登录地点。为保证您的帐号安全，需要您输入验证码，这可以防止恶意程序的自动登录。"

**[ybyygu](#8590 "2010-09-21 16:06:26"):** 还是登录不了: Line:822 Fun:login Register Failed!

**[simon](#8854 "2010-10-26 22:35:45"):** Debian 4.3.2-1.1 File "fetion.py", line 450, in sys.exit(main()) File "fetion.py", line 406, in main ret = phone.login(FetionOnline) File "/home/nako521/PyFetion/PyFetion.py", line 820, in login response = self.__register(self._ssic,self._domain) File "/home/nako521/PyFetion/PyFetion.py", line 1269, in __register self.get("REG",step,response) File "/home/nako521/PyFetion/PyFetion.py", line 222, in get response = self.__RSA_Encrypt(plain,len(plain),a2b_hex(key[:-6]),a2b_hex(key[-6:])) File "/home/nako521/PyFetion/PyFetion.py", line 608, in __RSA_Encrypt crypto_handler = ctypes.cdll.LoadLibrary(lib) File "/usr/lib/python2.5/ctypes/__init__.py", line 431, in LoadLibrary return self._dlltype(name) File "/usr/lib/python2.5/ctypes/__init__.py", line 348, in __init__ self._handle = _dlopen(self._name, mode) OSError: ./RSA_Encrypt.so: wrong ELF class: ELFCLASS32 Mac OS: Traceback (most recent call last): File "fetion.py", line 450, in sys.exit(main()) File "fetion.py", line 406, in main ret = phone.login(FetionOnline) File "/Users/Simon/PyFetion/PyFetion.py", line 820, in login response = self.__register(self._ssic,self._domain) File "/Users/Simon/PyFetion/PyFetion.py", line 1269, in __register self.get("REG",step,response) File "/Users/Simon/PyFetion/PyFetion.py", line 222, in get response = self.__RSA_Encrypt(plain,len(plain),a2b_hex(key[:-6]),a2b_hex(key[-6:])) File "/Users/Simon/PyFetion/PyFetion.py", line 608, in __RSA_Encrypt crypto_handler = ctypes.cdll.LoadLibrary(lib) File "/System/Library/Frameworks/Python.framework/Versions/2.6/lib/python2.6/ctypes/__init__.py", line 423, in LoadLibrary return self._dlltype(name) File "/System/Library/Frameworks/Python.framework/Versions/2.6/lib/python2.6/ctypes/__init__.py", line 345, in __init__ self._handle = _dlopen(self._name, mode) OSError: dlopen(./RSA_Encrypt.so, 6): no suitable image found. Did find: ./RSA_Encrypt.so: unknown file type, first eight bytes: 0x7F 0x45 0x4C 0x46 0x01 0x01 0x01 0x00

**[cocobear](#8975 "2010-11-22 15:27:29"):** 最新的R90版本 使用Python的RSA实现。 GAE上请使用HTTP方式登录。GAE我也不太熟 这个错误我没看明白是怎么回事。

**[杯具阿](#8976 "2010-11-22 16:24:13"):** 大哥，有错： Traceback (most recent call last): File "w.py", line 38, in fetion.logout() File "/dev/shell/python/PyFetion.py", line 864, in logout self.send() File "/dev/shell/python/PyFetion.py", line 407, in send raise PyFetionSocketError(405,'http stoped') PyFetion.PyFetionSocketError: (405, 'http stoped')

**[ihipop](#8906 "2010-11-10 11:35:54"):** 请问如何让消息可以换行呢？ 我使用pyfetion0.3 + nagios实现短信预警，可是发现不支持\n换行符，这样发来的短信很难看

**[the729](#8904 "2010-11-09 17:13:51"):** 不能登录啊。 =====>=====登录失败 就是这样。我自己在mac上编译了那个so，用_DEBUG也试过，不是so的问题应该。 我还移植了前面说的http://stuvel.eu/rsa的库，如果你感兴趣给我邮件，不过问题是我现在登录不了啊。。。。

**[wbchn](#8970 "2010-11-21 21:46:34"):** 发现SVN中新的版本 r89 缺少了下面的语句： from uuid import uuid1 在GAE上还是不能正常使用： ret = phone.login(FetionHidden) File "/base/data/home/apps/wb****354066065519429/PyFetion.py", line 838, in login if not self.__get_uri(): File "/base/data/home/apps/wbch＊＊＊＊＊nweb/1-50.346354066065519429/PyFetion.py", line 1334, in __get_uri ret = http_send(url,login=True) File "/base/data/home/apps/wbc＊＊＊＊.3463065519＊＊＊＊＊429/PyFetion.py", line 652, in http_send conn = urllib2.urlopen(request) File "/base/python_runtime/python_dist/lib/python2.5/urllib2.py", line 124, in urlopen return _opener.open(url, data) File "/base/python_runtime/python_dist/lib/python2.5/urllib2.py", line 381, in open response = self._open(req, data) File "/base/python_runtime/python_dist/lib/python2.5/urllib2.py", line 399, in _open '_open', req) File "/base/python_runtime/python_dist/lib/python2.5/urllib2.py", line 360, in _call_chain result = func(*args) File "/base/python_runtime/python_dist/lib/python2.5/urllib2.py", line 1115, in https_open return self.do_open(httplib.HTTPSConnection, req) File "/base/python_runtime/python_dist/lib/python2.5/urllib2.py", line 1080, in do_open r = h.getresponse() File "/base/python_runtime/python_dist/lib/python2.5/httplib.py", line 197, in getresponse self._allow_truncated, self._follow_redirects) File "/base/python_runtime/python_lib/versions/1/google/appengine/api/urlfetch.py", line 241, in fetch return rpc.get_result() File "/base/python_runtime/python_lib/versions/1/google/appengine/api/apiproxy_stub_map.py", line 527, in get_result return self.__get_result_hook(self) File "/base/python_runtime/python_lib/versions/1/google/appengine/api/urlfetch.py", line 331, in _get_fetch_result raise DownloadError(str(err)) 不知道您的邮件地址，只能 在这儿给您留言了。

**[abalest](#8732 "2010-10-05 13:37:11"):** 一直用可可熊的pyfetion，以前做了个gae应用。不过已经好久不能在gae上登录飞信了，这次的这个2010可以部署在gae上么？ 看到有个dll文件，这个gae上有作用么？

**[gbate](#8960 "2010-11-18 22:34:34"):** 放到GAE上出错，不懂！ 11-18 06:30AM 32.554 /_ah/xmpp/message/chat/ 500 4600ms 120cpu_ms 2kb 0.1.0.10 - - [18/Nov/2010:06:30:37 -0800] "POST /_ah/xmpp/message/chat/ HTTP/1.1" 500 2326 - - "sms2laopo.appspot.com" ms=4600 cpu_ms=120 api_cpu_ms=0 cpm_usd=0.003732 loading_request=1 E 11-18 06:30AM 37.118 ApplicationError: 5 Traceback (most recent call last): File "/base/python_runtime/python_lib/versions/1/google/appengine/ext/webapp/__init__.py", line 513, in __call__ handler.post(*groups) File "/base/data/home/apps/sms2laopo/1.346306055953622693/gtalk.py", line 19, in post fetion.login(FetionOnline) File "/base/data/home/apps/sms2laopo/1.346306055953622693/PyFetion.py", line 792, in login if not self.__get_uri(): File "/base/data/home/apps/sms2laopo/1.346306055953622693/PyFetion.py", line 1306, in __get_uri ret = http_send(url,login=True) File "/base/data/home/apps/sms2laopo/1.346306055953622693/PyFetion.py", line 634, in http_send conn = urllib2.urlopen(request) File "/base/python_runtime/python_dist/lib/python2.5/urllib2.py", line 124, in urlopen return _opener.open(url, data) File "/base/python_runtime/python_dist/lib/python2.5/urllib2.py", line 381, in open response = self._open(req, data) File "/base/python_runtime/python_dist/lib/python2.5/urllib2.py", line 399, in _open '_open', req) File "/base/python_runtime/python_dist/lib/python2.5/urllib2.py", line 360, in _call_chain result = func(*args) File "/base/python_runtime/python_dist/lib/python2.5/urllib2.py", line 1115, in https_open return self.do_open(httplib.HTTPSConnection, req) File "/base/python_runtime/python_dist/lib/python2.5/urllib2.py", line 1080, in do_open r = h.getresponse() File "/base/python_runtime/python_dist/lib/python2.5/httplib.py", line 197, in getresponse self._allow_truncated, self._follow_redirects) File "/base/python_runtime/python_lib/versions/1/google/appengine/api/urlfetch.py", line 241, in fetch return rpc.get_result() File "/base/python_runtime/python_lib/versions/1/google/appengine/api/apiproxy_stub_map.py", line 527, in get_result return self.__get_result_hook(self) File "/base/python_runtime/python_lib/versions/1/google/appengine/api/urlfetch.py", line 331, in _get_fetch_result raise DownloadError(str(err)) DownloadError: ApplicationError: 5 I 11-18 06:30AM 37.153 This request caused a new process to be started for your application, and thus caused your application code to be loaded for the first time. This request may thus take longer and use more CPU than a typical request for your application.

**[wjy5200](#8959 "2010-11-18 20:52:17"):** 刚刚试了一下，一直提示“登录失败“，

**[邯郸征婚](#8811 "2010-10-20 09:33:48"):** 再来看看 ~~

**[邢台征婚](#8957 "2010-11-18 09:02:54"):** 丫真懒，多久也不来一次

**[flyingzl](#8940 "2010-11-16 23:35:11"):** 恩，提示登入错误……

**[shector](#8947 "2010-11-17 13:03:59"):** 从另一个python实现加上PKCS1填充改出来的，patch见http://code.google.com/p/pytool/issues/detail?id=26#c8 不过GAE上还是没搞定……

**[不做愤青](#8771 "2010-10-06 15:58:42"):** 这个貌似也不能gae上用。。。

**[可可熊](#9726 "2011-03-15 11:46:55"):** 获取隐身的这个功能估计是被封掉了，协议更新了

**[NikeAirMax](#8932 "2010-11-16 15:01:30"):** 都没有用过飞信，我是不是OUT了啊

**[ayanamist](#8884 "2010-11-04 18:32:30"):** 不能部署到GAE上的同学，可以看看这个：http://stuvel.eu/rsa，接口和楼主的是一样的，自己做一下代码迁移吧。虽然说纯Python的代码效率不高，不过总比没有强

**[vinsonlv](#8928 "2010-11-15 18:05:15"):** http://stuvel.eu/rsa 这个没有做PKCS1填充，要自己改

**[邢台征婚](#8927 "2010-11-15 17:11:25"):** 很久没来了

**[邯郸征婚](#8774 "2010-10-08 17:24:16"):** 还是老板的好用，我到现在也没换。

**[YY](#9262 "2010-12-18 23:50:33"):** 今天又测试了下，由官方飞信加一个手机为好友，fetion.py运行时没有任何提示的。

**[YY](#9263 "2010-12-19 00:18:27"):** 还发现一个问题，当我开fetion.py，然后本机开一个官方飞信，之间互为好友，然后用fetion.py向官方飞信发送消息这个很快很正常，可是如果我用官方飞信向fetion.py发送消息时，fetion.py要显示收到的消息总是有一个很长的延时。如果两边都用官方的飞信发送是没有这种问题的，这个我猜跟socket的线程处理有关，麻烦你看看。

**[pt](#9364 "2010-12-29 20:51:25"):** 请问怎么使用.. 我装好python后，打开feiton.py，输入帐号密码，读取了一下很快就不见了.还没看清是什么

**[可可熊](#10173 "2011-05-22 15:00:42"):** 没有测试邮箱申请的号。本来就只是为向手机发短信用的，所以就没考虑邮箱。

**[ihipop](#9306 "2010-12-23 15:42:00"):** http://code.google.com/p/pytool/issues/detail?id=46

**[寒塘雁迹](#9932 "2011-05-02 13:44:21"):** 测试发现如下问题 用两个飞信号，一个是用移动的手机号注册的飞信，另一个是用邮箱注册的飞信，pyfetion登陆的是手机注册的那个，然后互相发送信息。 用pyfetion向官方客户端发送信息可以正常的发送 但是用官方的向pyfetion发送信息就接收不到，不知道这个是什么原因，难道用邮箱注册的飞信不好用么？

**[爱好者](#9644 "2011-02-22 16:14:10"):** 获取隐身信息出错,这个功能不好用，怎么回事啊？

**[珠](#9678 "2011-03-02 15:37:39"):** 。。。。。

**[cocobear](#9755 "2011-03-21 09:05:08"):** 在命令行下执行 不要直接双击 有可能是有出错提示

**[vccv](#9744 "2011-03-18 14:58:22"):** 请问这个如何使用，为什么闪一下就什么都没有了，连账号和密码都不能输入

**[jack](#13492 "2012-03-28 23:44:00"):** linux 下如何输入验证码？

**[cocobear](#13494 "2012-03-29 00:04:35"):** 只能找个查看图片的来看一下图片。

**[LLco](#18051 "2012-11-28 12:14:29"):** coco熊啊，飞信2012年11月28日又来了一个升级（http://feixin.10086.cn/bulletin/2856/），pyfetion报错： Traceback (most recent call last): File "fetion.py", line 438, in sys.exit(main()) File "fetion.py", line 394, in main ret = phone.login(FetionOnline) File "D:\Program Files\Python26\PyFetion\PyFetion.py", line 811, in login response = self.__register(self._ssic,self._domain) File "D:\Program Files\Python26\PyFetion\PyFetion.py", line 1268, in __registe r self.get("REG",2,response) File "D:\Program Files\Python26\PyFetion\PyFetion.py", line 212, in get nonce = re.search('nonce="(.+?)"',extra[0]).group(1) AttributeError: 'NoneType' object has no attribute 'group' 我看了一下，返回的error信息： Line:441 Fun:send response:SIP-C/4.0 410 Gone F: 964421729 I: 1 Q: 1 R 是410Gone，是不是他们改服务器地址了？

**[cocobear](#18055 "2012-11-28 15:49:15"):** 新版本已经更新，就是服务器的地址变了。

