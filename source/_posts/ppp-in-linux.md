title: Linux下的ppp拨号
tags:
  - Linux
  - pppoe
id: 104
categories:
  - Linux
date: 2007-05-18 13:39:17
---

前段时间写那个adsl拨号脚本的时候修改了/etc/sysconfig/network-scripts/ifcfg-cnc，结果就出现问题了，具体可查看[这篇文章](http://cocobear.github.io/?p=68)。当时按照前面文章的修改后已经有好长一段时间拨号都正常，但是今天早上拨号的时候突然发现又出现问题了，而且情况似乎与前一次差不多，以下是拨号的日志：

     May 18 09:26:04 cocobear pppd[1630]: pppd 2.4.3 started by root, uid 0
     May 18 09:26:04 cocobear pppd[1630]: Using interface ppp0
     May 18 09:26:04 cocobear pppd[1630]: Connect: ppp0 < --> /dev/pts/0
     May 18 09:26:04 cocobear pppoe[1642]: PADS: Service-Name: ''
     May 18 09:26:04 cocobear pppoe[1642]: PPP session is 9283
     May 18 09:26:05 cocobear pppd[1630]: CHAP authentication succeeded: Welcome to .May 18 09:26:05 cocobear pppd[1630]: local  IP address 124.89.60.31
     May 18 09:26:05 cocobear pppd[1630]: remote IP address 221.11.2.1
     May 18 09:26:05 cocobear pppd[1630]: primary   DNS address 221.11.1.67
     May 18 09:26:05 cocobear pppd[1630]: secondary DNS address 221.11.1.68
     May 18 09:36:10 cocobear pppd[1630]: No response to 3 echo-requests
     May 18 09:36:10 cocobear pppd[1630]: Serial link appears to be disconnected.
     May 18 09:36:10 cocobear pppd[1630]: Connect time 10.1 minutes.
     May 18 09:36:10 cocobear pppd[1630]: Sent 490999 bytes, received 5168828 bytes.
     May 18 09:36:16 cocobear pppd[1630]: Connection terminated.
     May 18 09:36:16 cocobear pppd[1630]: Modem hangup
     May 18 09:36:21 cocobear pppd[1630]: Exit.
     May 18 09:36:21 cocobear pppoe[1642]: read (asyncReadFromPPP): Session 9283: Input/output error
     May 18 09:36:21 cocobear pppoe[1642]: Sent PADT
     May 18 09:36:21 cocobear adsl-connect: ADSL connection lost; attempting re-conne

刚拨号成功连线10分钟就断开，通过[Google](http://www.google.com)搜索“No response to 3 echo-requests“，也没有什么比较有用的介绍，只有一篇文章中提到要修改/etc/ppp/options中的lcp-echo-failure值，把它改的大一点，但我的Fedora core 5的这个文件没有那一选项，不过我倒是在同一目录中的pppoe-server-options文件中找到那个选项，可惜的是修改后问题仍然存在。

     May 18 09:36:27 cocobear pppd[2038]: pppd 2.4.3 started by root, uid 0
     May 18 09:36:27 cocobear pppd[2038]: Using interface ppp0
     May 18 09:36:27 cocobear pppd[2038]: Connect: ppp0 < --> /dev/pts/0
     May 18 09:36:58 cocobear pppd[2038]: LCP: timeout sending Config-Requests
     May 18 09:36:58 cocobear pppd[2038]: Connection terminated.
     May 18 09:36:58 cocobear pppd[2038]: Using interface ppp0
     May 18 09:36:58 cocobear pppd[2038]: Connect: ppp0 < --> /dev/pts/4
     May 18 09:37:02 cocobear pppoe[2039]: Timeout waiting for PADO packets
     May 18 09:37:03 cocobear pppd[2038]: tcflush failed: Bad file descriptor
     May 18 09:37:03 cocobear pppd[2038]: Exit.

后来重拨时一直是上面的错误， “LCP: timeout sending Config-Requests”，在[Google](http://www.google.com)搜索了很多关于这个错误的页面，但也没有找到比较有用的信息。

     May 18 10:30:52 cocobear pppd[2674]: pppd 2.4.3 started by root, uid 0
     May 18 10:30:52 cocobear pppd[2674]: Using interface ppp0
     May 18 10:30:52 cocobear pppd[2674]: Connect: ppp0 < --> /dev/pts/8
     May 18 10:31:06 cocobear pppoe[2675]: PADS: Service-Name: ''
     May 18 10:31:06 cocobear pppoe[2675]: PPP session is 11436
     May 18 10:31:06 cocobear pppd[2674]: CHAP authentication succeeded: Welcome to .May 18 10:31:06 cocobear pppd[2674]: local  IP address 124.89.79.5
     May 18 10:31:06 cocobear pppd[2674]: remote IP address 221.11.2.1
     May 18 10:31:06 cocobear pppd[2674]: primary   DNS address 221.11.1.67
     May 18 10:31:06 cocobear pppd[2674]: secondary DNS address 221.11.1.68
     May 18 10:32:26 cocobear pppd[2674]: No response to 3 echo-requests
     May 18 10:32:26 cocobear pppd[2674]: Serial link appears to be disconnected.
     May 18 10:32:26 cocobear pppd[2674]: Connect time 1.4 minutes.
     May 18 10:32:26 cocobear pppd[2674]: Sent 1946 bytes, received 1038 bytes.
     May 18 10:32:26 cocobear pppoe[2648]: Timeout waiting for PADO packets
     May 18 10:32:32 cocobear pppd[2674]: Connection terminated.
     May 18 10:32:32 cocobear pppd[2674]: Modem hangup
     May 18 10:32:37 cocobear pppd[2674]: Exit.
     May 18 10:32:37 cocobear pppoe[2675]: read (asyncReadFromPPP): Session 11436: Input/output error
     May 18 10:32:37 cocobear pppoe[2675]: Sent PADT
     May 18 10:32:37 cocobear adsl-connect: ADSL connection lost; attempting re-connection.
这个是重试了N次后终于连线上去，但只有1.4分钟的时间，又是”No response to 3 echo-requests“这个错误。

最后在[一篇文章](http://www.linuxquestions.org/questions/showthread.php?t=40369)中看到重新使用adsl-setup创建一个ppp拨号。照着做了后结果竟然可以连接上去，而且好长时间也再没有断开！

     USERCTL=yes
     BOOTPROTO=dialup
     NAME=DSLppp0
     DEVICE=ppp0
     TYPE=xDSL
     ONBOOT=no
     PIDFILE=/var/run/pppoe-adsl.pid
     FIREWALL=NONE
     PING=.
     PPPOE_TIMEOUT=80
     LCP_FAILURE=3
     LCP_INTERVAL=20
     CLAMPMSS=1412
     CONNECT_POLL=6
     CONNECT_TIMEOUT=60
     DEFROUTE=yes
     SYNCHRONOUS=no
     ETH=eth0
     PROVIDER=cnc
     USER=11100022395
     PEERDNS=no
     DEMAND=no
     IPV6INIT=no
     PERSIST=no
这是新产生的ifcfg-ppp0的文件内容。

直到现在还是没有弄明白这到底是怎么一回事，写在这里只做个记录吧，如果有人知道原因告诉我一声就更好了。
似乎还有人提到这可能和ISP有关，这个原理我倒是觉得很有可能，我们学校Windows下有时候拨号也是老拨不上去，但具体的错误和这个是否一样，我就不得而知了。
## Comments

**[cocobear](#183 "2007-05-18 13:59:39"):** **TO:wangcong** 这个和咱们那个脚本没有关系啊，那个脚本没把改变/etc/sysconfig/network-scripts/ifcfg-XXX里的选项，只是改变用户名的 有两个USER应该是运行脚本非正常退出产生的吧？

**[wangcong](#184 "2007-05-18 13:51:02"):** 我早就遇到这个问题了。貌似是咱那脚本有问题，在我这里运行完，那个配置文件中有两个USER=XXXXX。 而且，我发现，那两个timeout也不能随便改。

