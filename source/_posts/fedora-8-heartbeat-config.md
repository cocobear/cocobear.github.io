title: Fedora 8配置Heartbeat
tags:
  - Fedora
  - heartbeat
  - Linux
id: 301
categories:
  - Linux
date: 2008-08-07 17:28:24
---

Heartbeat是http://linux-ha.org中HA项目的核心，HA是(High Availability)的缩写；简单来说就是为了提供高可靠的应用服务，例如拿两来机子来做HA，提供WEB服务，一台机子为主服务器平时提供WEB服务，HA就是保证在主服务器出现故障(例如掉电)的时候另一台机子可以立刻接手继续提供WEB服务，从而使用户觉得服务未曾中断；实际应用中可能使用更多的机子组成集群。

Heartbeat可以使用串口或者以太网来实现对主机的监测，这里使用的是以太网，在两台机器上分别配双网卡，用一根双绞线把两台机器连接在一起，另外两个网卡连到路由；配置是：

f801(主机名；使用这台作为主服务器)：
eth0: 192.168.1.110
eth1: 10.0.0.1

f802:
eth1: 192.168.1.111
eth2: 10.0.0.2

网络配置好后测试无误后再检查两台机器上的WEB服务是否可以正常使用；一切都正常后使用yum安装Heartbeat：

yum install heartbeat

安装好heartbeat后开始配置，三个主要配置文件都在： /usr/share/doc/heartbeat-2.1.3/下面，需要我们手工修改后拷贝到/etc/ha.d/中(两台服务器使用的脚本基本一样)。

编辑authkeys文件，下面的配置使用了sha1作为认证方式(这里需要注意的是该文件的权限必须被设置为600)：

auth 2
#1 crc
2 sha1 HI!
#3 md5 Hello!

编辑ha.cf，注释下掉以下内容：

keepalive 1               ##设定心跳(监测)时间时间为1秒
warntime 10               ##设定警告时间
deadtime 30              ##设定确定主机宕机时间
initdead 120              ##第一宕机时间
ucast eth1 10.0.0.2     ##使用eht1做心跳监测 也就是连接两PC的网卡
udpport 694              ##使用udp端口694 进行心跳监测
node f801                 ##节点1，必须要与 uname -n得到的结果一致。
node f802                 ##节点2
第二台服务器由于网卡是eth(192.*)1,eth2(10.*)，所以相应的配置文件中ucast 需要使用eth2。

编辑haresources，添加以下内容：

f801                    192.168.1.118 httpd
表示主服务器使用192.168.1.118作为WEB服务的IP，f801为主服务器。

配置好后在主服务器上启动Heartbeat：
service heartbeat start
出错的话一般是配置文件的问题，根据提示修改就可以了；Heartbeat会自动根据haresource配置文件启动相应的服务程序。
然后在另一个台器上启动Heartbeat；

完成后可以使用tcpdump来测试两台机器间的心跳：

[root@f801 ~]# tcpdump -i eth0 -p udp port 694
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on eth0, link-type EN10MB (Ethernet), capture size 96 bytes
11:43:46.431448 IP ha01.filenet-nch > ha02.ha-cluster: UDP, length 219
11:43:46.433968 IP ha01.filenet-nch > ha02.ha-cluster: UDP, length 216
11:43:47.431456 IP ha01.filenet-nch > ha02.ha-cluster: UDP, length 216
11:43:48.432516 IP ha01.filenet-nch > ha02.ha-cluster: UDP, length 216

如果这里出问题的话很有可能是防火墙的问题，或者是配置文件里ucast里的设置，需要仔细检查。
最后就可以做测试，关闭主服务器，根据配置文件里的响应时间()，服务器f802会接管主服务器的任务。

参考文章：
http://www.bitscn.com/linux/network_manage/200805/140501_3.html
http://www.xxlinux.com/linux/article/network/app/20070329/8009.html
## Comments

**[luguo](#4027 "2008-08-10 20:15:59"):** DP走网管路线了？

**[Amankwah](#4012 "2008-08-07 20:42:32"):** 好东西，有机会试试看~

**[cocobear](#4028 "2008-08-11 14:13:14"):** 没有啊；只是工作中要用到；

