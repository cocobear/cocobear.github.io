title: PyFetion更新
tags:
  - PyFetion
  - Python
id: 523
categories:
  - 编程相关
date: 2009-02-20 14:40:37
---

详细的更新请到[google code ](http://code.google.com/p/pytool/source/browse/trunk/PyFetion/PyFetion.py)查看。

1.  get_info()函数参数处理who遗失的情况；
2.  向服务器以HTTP方式提交密码时使用urllib.quote()编码；
3.  修改send_msg()函数；
4.  清理代码中直接退出的exit函数；
5.  重写__tcp_recv()函数，以前这里有可能会丢失数据，比如好友列表过长；
6.  增加隐身登录功能，login(True)表示隐身登录。

以前还有人提到增加隐身登录，我看了下08 PC版的飞信，只能是先登录，然后再设置隐身模式，这样的话增加设置隐身模式就没必要了。

至于登出飞信更是没必要了。

09.02.23 Update:	

1.  上次的代码get_uri被我改出错了，修正了一下；
2.  修改了使用手机号直接发送的方式，因为目前可以使用tel: 13888888888的方式发送短信，不用从联系人中一个一个找了；不过以前似乎这种方式不行；
3.  修正了获取联系人信息的XML数据，可以完整的获取联系人；
## Comments

**[hawkli](#5750 "2009-04-20 17:48:58"):** nav.fetion.com.cn不能访问了，唉是不是被封了啊，还有什么办法么？

**[cocobear](#5606 "2009-03-27 15:00:25"):** 呵呵,希望能有更多利用飞信的好东东.

**[jifenghy](#5604 "2009-03-27 10:32:15"):** 非常谢可可熊，用了你的pyfetion，写了一个简单的应用，每天早上7点天气预报短信到我的手机上，部署到了GAE上。 代码放到了http://code.google.com/p/weathermobile/ 谢谢你！

**[可可熊](#5308 "2009-02-27 10:00:52"):** To:xinqianli 2.6版本的我没有试过，但是2.6如果用这种方式很容易卡在那里。 没有仔细去研究到底是什么原因，先记一下，有机会再瞧瞧。 多谢了。

**[可可熊](#5311 "2009-02-27 14:10:58"):** 我已经算过长度了，在recv里面使用了size，这个参数可以不加，我似乎做得有些画蛇添足。 这一块似乎不太稳定，因为飞信服务器有时候返回好友列表的时候中间会隔不短的一段时间，弄的我很郁闷。在Windows下你可以先把这个参数去掉。

**[K.T.S](#5312 "2009-02-27 14:39:24"):** windows下 _tcp_recv方法里如果socket超时，raise PyFetionSocketError(e.read())报错，socket.timeout似乎没有read方法

**[可可熊](#5259 "2009-02-23 16:01:18"):** To:volans http://code.google.com/p/pytool/source/detail?r=22 已经更新。 请查看398+ 多谢反馈。

**[Kermit Mei](#5245 "2009-02-23 00:32:48"):** $ ./PyFetion.py 12345 corrent your mobile NO. and password 上面的提示中corrent是什么意思？这个怎么用？给个范例萨。 BTW:我没有用过飞信，这个发短信要钱不？

**[small](#5244 "2009-02-22 21:49:44"):** 不错，鼓励可可熊，坚持更新

**[volans](#5228 "2009-02-20 15:24:53"):** 已经移植了你的代码到GAE上，目前只实现了发短信。 http://gaefetion.appspot.com/

**[volans](#5255 "2009-02-23 14:39:37"):** 有的人没有使用飞信业务，但他同意你加他为你的飞信好友了，这些人在你的列表里只有手机号，而没有飞信号码。用官方的飞信软件的好友类表里是有这些人的，图标是绿色（而申请了飞信业务，有飞信号码的好友是灰色） 不知道这样描述你是否明白，或者你看看你的好友列表。

**[Jove](#5239 "2009-02-22 15:35:44"):** hehe, 好像只要在实例化PyFetion时,选HTTP登录方式就可以在GAE上用了. 这下我可以给朋友发短信提醒了

**[Jove](#5238 "2009-02-22 14:22:57"):** volans, 能共享一下你在GAE上的移植么. 好像GAE里不能使用socket包,所以错误信息如下 File "/base/data/home/apps/aa/1.331593899342273539/main.py", line 41, in get phone.login() File "/base/data/home/apps/aa/1.331593899342273539/PyFetion.py", line 88, in login self.__register(self.__ssic,self.__domain) File "/base/data/home/apps/aa/1.331593899342273539/PyFetion.py", line 204, in __register response = self.__SIPC.send() File "/base/data/home/apps/aa/1.331593899342273539/PyFetion.py", line 315, in send self.__tcp_init() File "/base/data/home/apps/aa/1.331593899342273539/PyFetion.py", line 424, in __tcp_init self.__sock = socket.socket(socket.AF_INET,socket.SOCK_STREAM) AttributeError: 'module' object has no attribute 'socket'

**[可可熊](#5254 "2009-02-23 13:53:28"):** 没有飞信号码的好友？ 我不太理解，飞信好友中还有没有飞信号码的好友吗？

**[可可熊](#5251 "2009-02-23 09:03:58"):** To:Jove GAE上应该不能直接使用底层的socket，所以只能使用HTTP的方式；HTTP方式速度有点慢，传输的数据多一些，如果你有DH主机的话直接在上面用Python很方便。

**[草儿](#5229 "2009-02-20 17:37:26"):** 没想到你还在继续更新着，我还以为你都停下了呢。

**[nemo](#5230 "2009-02-20 17:56:01"):** 挺不错呢~

**[iefan](#5231 "2009-02-20 18:33:23"):** 只是如何保持一直在线状态呢？总不能每次发短信都得登录一下吧

**[volans](#5252 "2009-02-23 09:06:42"):** Jove，如你所说，改成http方式就可以了，没做过多别的修改。

**[volans](#5253 "2009-02-23 09:07:38"):** 可可熊：为什么获得好友列表只能获得飞信用户呢？没有飞信号码的那些好友将不能返回……

**[可可熊](#5249 "2009-02-23 08:59:14"):** To:iefan 一直保持在线我没考虑，因为没打算发大量的短信，如果要发很多短信，建议使用定时短信。 至于实现保持在线，只是定时发一发keepalive消息。

**[可可熊](#5250 "2009-02-23 09:00:08"):** To: Mei 我不理解你为什么要在命令行后加参数，我给出的例子里面就不是那样写的啊。

**[xinqianli](#5305 "2009-02-27 01:41:24"):** 在windows下面，用Python 2.6， 原代码中 data = self.sock.recv(size,socket.MSG_WAITALL) 这一行应该用下列代码替换，否则会报告no attribute 'MSG_WAITALL'错误。 buffer="" while len(buffer)<size: data = self.sock.recv(size-len(buffer)) if not data: break buffer+=data

**[xinqianli](#5310 "2009-02-27 12:43:04"):** To 可可熊 我对python不熟悉，只是对fetion有点兴趣，学习了一下你的代码 ^_^ 可能你主要在unix-like平台测试的，windows下面socket不支持MSG_WAITALL选项，似乎python库也没有考虑提供封装，因此要根据消息长度自己算len吧

**[帆船书会](#5758 "2009-04-24 17:50:37"):** 恩，对，ptfetion似乎不能用了啊。

**[damon](#5761 "2009-04-25 10:04:23"):** 昨天晚上成功测试发送！ 马上就停电了 郁闷

**[xiangking](#5457 "2009-03-12 19:36:47"):** 学习！ 什么时候把这个修改成能在智能手机上使用就好了

**[damon](#5762 "2009-04-25 11:46:56"):** 125.69.45.214 - - [24/Apr/2009:20:38:44 -0700] "GET / HTTP/1.1" 500 0 - "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; .NET CLR 2.0.50727; TheWorld),gzip(gfe)"E 04-24 08:38PM 44.702 : Traceback (most recent call last): File "/base/data/home/apps/djybbs/1.333029036534336496/PyFetion.py", line 642, in main() File "/base/data/home/apps/djybbs/1.333029036534336496/PyFetion.py", line 639, in main time.sleep(Timeout2);

**[Eric](#5993 "2009-06-01 16:18:54"):** 你好，我在linux下使用PyFetion有问题，短信发送不出去，在windows下能发送出去，然后我看到代码上有这个配置： 是不是platform需要修改为linux下的，还是其他的原因呢。谢谢！

**[Eric](#5994 "2009-06-01 16:24:07"):** platform="W5.1" 这个是否说明运行平台是在windows下，如果我是在linux下又该如何修改呢？

**[可可熊](#5995 "2009-06-01 16:31:00"):** Linux下使用没有问题，代码是跨平台的。

**[Eric](#5996 "2009-06-01 17:30:17"):** 不需要修改什么吗，我现在就是遇到这个问题了，windows下能发送，linux就不能发送了。你的意思代码完全不需要修改吗？

**[cocobear](#5997 "2009-06-01 18:26:44"):** 你遇到问题，你要说明是什么样的，给出具体的描述，不然别人没办法了解你的情况。

**[Eric](#6013 "2009-06-03 10:07:36"):** 谢谢你的提醒，我现在测试又通过了，问题不在代码上。再请教一个问题呢，那些关于飞信的XML，有参考资料吗。

**[可可熊](#6021 "2009-06-03 15:00:01"):** XML相关的资料很多的，这是标准格式，你看相关的文档就行了。

**[Leo Jay](#6106 "2009-06-10 16:32:20"):** 刚才下载了你最新的r41的代码，好像里面有tab和space混用的情况。

**[Eric](#6151 "2009-06-18 11:43:31"):** 你好，我又来了，我想问个问题就是我现在有个应用，就是想通过指定端口来发送短信，不知道这个是不是直接在电话号码后面跟上端口号就行了呢？

**[Terry](#6504 "2009-09-21 12:57:00"):** 我也在GAE上搭建了一个https://fetionlib.appspot.com/ 可以群发、定时发送、加好友和程序调用等。

