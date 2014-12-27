title: 局域网邮件服务器配置
tags:
  - dovecot
  - sendmail
id: 566
categories:
  - Linux
date: 2009-03-16 13:02:56
---

首先安装必要的软件：sendmail,dovecot
sendmail只支持SMTP发信，dovecot可以支持POP3，IMAP等。

把自己的机器名改为一个你想使用的域名，例如coco.com（vi /etc/sysconfig/network）。配置sendmail，修改以/etc/mail/sendmail.mc下一行：

     -O DaemonPortOptions=Port=smtp,Addr=127.0.0.1 , Name=MTA> 
     +O DaemonPortOptions=Port=smtp,Addr=192.168.1.112 , Name=MTA

同时修改/etc/mail/access,添加：
Connect:192.168.1                       RELAY

编辑/etc/mail/local-host-names文件，添加你想在内部使用的一个域名，如：
coco.com

这样局域网内的用户就可以使用你的SMTP了，别人在发邮件时在SMTP服务器地址中填写你的IP地址，或者域名(需要改更hosts中coco.com的指向)。帐户就直接使用Linux系统的用户，因为在一个公司使用，所以安全问题就不需要考虑了。

安装好后先测试sendmail，可以使用 mail -s ‘your subject' somebody@coco.com命令来发邮件。也可以使用claws-mail等客户端来发邮件。

dovecot安装好后也很简单，启动后就可以使用POP3收邮件了。
## Comments

**[草儿](#5506 "2009-03-16 18:24:04"):** 你老板是不是把你当成网管用了？

**[cocobear](#5508 "2009-03-16 22:01:35"):** 能做的都做！

