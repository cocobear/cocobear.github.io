title: F7中启动Mysql出错
tags:
  - Fedora
  - MySQL
id: 164
categories:
  - Linux
date: 2007-07-25 09:33:27
---

F7在默认安装Mysql后启动时会出错：

使用mysqld_safe启动：
     [cocobear@cocobear mysql]$ sudo mysqld_safe> 
     nohup: ignoring input and redirecting stderr to stdout> 
     Starting mysqld daemon with databases from /var/lib/mysql> 
     STOPPING server from pid file /var/lib/mysql/cocobear.pid> 
     070725 09:27:34  mysqld ended

使用service启动：

     [cocobear@cocobear mysql]$ sudo service mysqld start> 
     Timeout error occurred trying to start MySQL Daemon.> 
     启动 MySQL：                                               [失败]
这种情况下mysql.log出错提示为：
     070725 09:28:59  mysqld started> 
     070725  9:28:59  InnoDB: Operating system error number 13 in a file operation.> 
     InnoDB: The error means mysqld does not have the access rights to> 
     InnoDB: the directory.> 
     InnoDB: File name ./ibdata1> 
     InnoDB: File operation call: 'open'.> 
     InnoDB: Cannot continue operation.> 
     070725 09:28:59  mysqld ended

根据日志文件的出错信息大致可以认定为权限问题，修改/var/lib/mysql/下的这几个文件权限：
ibdata1  ib_logfile0  ib_logfile1
     [cocobear@cocobear mysql]$ sudo chmod 766 ib*
然后重新启动：
     [cocobear@cocobear mysql]$ sudo service mysqld start> 
     启动 MySQL：                                               [确定]

很奇怪为什么会产生这样的问题，纳闷～
## Comments

**[crazyfranc](#1337 "2007-07-31 11:26:54"):** P哥.我网站被封了咋办?--总不能凉拌吧!没备案,还有补救方法没.

**[windflush](#1379 "2007-08-04 19:28:50"):** 这么惨？为什么会被封啊？？？ 不会哪天又轮到我们之中的谁了吧。

**[cocobear](#1358 "2007-08-01 19:49:35"):** 已经帮你更换了IP 不过DH说这是最后一次允许我换IP了:-( 不知道这个IP为什么会被封，郁闷。

**[Name](#1247 "2007-07-26 00:33:49"):** 路过～～

**[cocobear](#1294 "2007-07-29 00:23:30"):** http://iamin.blogdriver.com/iamin/1239091.html http://www.research.att.com/~bala/papers/h0vh1.html

**[br](#1250 "2007-07-26 08:46:15"):** 没有遇到过，找到原因了赶紧贴出来哦

