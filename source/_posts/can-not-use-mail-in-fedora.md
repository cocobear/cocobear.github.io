title: Fedora中mail命令无法发信
tags:
  - Fedora
  - Linux
  - mail
id: 286
categories:
  - Linux
date: 2008-07-04 15:46:25
---

偶尔发现我机子上的mail命令不能发信，看了下错误日志：

Jul  4 15:08:20 cocobear sendmail[4167]: m6478JxK004167: to=cocobear.cn@gmail.com, ctladdr=cocobear (500/500), delay=00:00:01, xdelay=00:00:00, mailer=relay, pri=30113, relay=[127.0.0.1] [127.0.0.1], dsn=4.0.0, stat=Deferred: Connection refused by [127.0.0.1]

发信的状态是Connection refused by [127.0.0.1]

原来是sendmail在Fedora中默认的配置有问题，修改/etc/mail/sendmail.mc文件:
	DAEMON_OPTIONS(`Port=smtp,Name=MTA')dnl
	dnl #DAEMON_OPTIONS(`Port=smtp,Addr=127.0.0.1, Name=MTA')dnl

把原来的指定Addr注释掉，原来这种配置只能向本机地址发送邮件，然后重新加载配置文件：
	make -C /etc/mail

重启服务:
	service sendmail restart

使用mail发信就正常了。

Update:080814:
今天又遇到这个问题了，CSDN那个文章没办法访问，看了看google的快照，顺便把过程写到这里。
## Comments

**[luguo](#3484 "2008-07-04 17:13:10"):** good~!

