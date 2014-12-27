title: xylftp服务器端V1.0发布
id: 149
categories:
  - 编程相关
date: 2007-06-25 22:30:35
tags:
---

原引：http://www.xiyoulinux.cn/blog/?p=14
     
     xylftp服务器端经过了长期测试和修复，现在已经基本完善和稳定。我们宣布正式发布我们的第一个版本供大家使用！> 
     
     a) 下载：> 
     
     xylftp项目的的主页在：> 
     https://sourceforge.net/projects/xylftp> 
     
     xylftp服务器端1.0版本可以在这里下载：> 
     
     [xylftp-server-1.0.tar.gz](http://xiyoulinux.cn/down/xylftp-server-1.0.tar.gz)> 
     
     b) 安装：> 
     
     安装方法：用命令 tar -xvzf xylftp-server-1.0.tar.gz解压，然后进入文件目录> 
     xylftp/server/src/，运行”make install”命令即可完成安装。> 
     
     c) 卸载：> 
     
     卸载方法：进入文件目录 xylftp/client/src/，运行”make uninstall”命令即可完成> 
     软件卸载。> 
     
     d) 开发：> 
     
     xylftp的CVS在：> 
     http://xylftp.cvs.sourceforge.net/> 
     
     截止目前，服务器端C代码总量为2400行。参与服务器端编写的人员有：> 
     
     1.董溥：完成守护进程的建立，socket连接以及写日志，实现主程序的流程。> 
     2.郭拓：完成do_list命令。> 
     3.贾孟树：完成parse_cmd.c,telnet.c，以及do_quit命令，以及测试和维护工作。> 
     4.林峰：完成配置文件的解析，完成do_user,do_pass命令,参与do_syst,do_type,do_noop命令以及测试和维护工作。> 
     5.刘伟：完成do_pwd,do_cdup,do_cwd,do_rnfr,do_rnto,do_port命令。> 
     6.刘洋：完成do_stat，do_mode,do_fail命令,以及测试和维护工作。> 
     7.聂海海：完成do_retr,do_mkd,do_rmd,do_dele命令。> 
     8.王聪：完成do_stru命令,完成Makefile编写,并维护了王老师的三个命令以及整个流程的测试和维护工作。> 
     9.王亚刚老师：完成do_abor,do_stor,do_pasv命令。> 
     
     感谢各位参与！> 
     
     欢迎大家测试使用，并把信息及时反馈给我们！> 
     （错误报告请发送至xiyoulinux@googlegroups.com）
     
由于个人原因在编码阶段我做了很少的工作，在这里再次对所有的朋友说一声“对不起！”。一个学期了，终于有了结果，象[刘洋](http://www.amankwahly.cn)同学说的“大家通过一起做一个项目，一起风雨走来，一起体味其中的酸甜苦辣”，不管怎么样，终究是走了过来，确实没有什么槛是过不去的！
## Comments

**[Name](#736 "2007-06-25 22:54:36"):** 过程重于结果。辛苦大家了！

**[windflush](#738 "2007-06-26 08:59:45"):** 其实你编码阶段做的工作倒是不少，就是测试和维护阶段跑了而已。你看你的贡献还是蛮大的，你完成的编码量也不少哦。

**[crazyfranc](#739 "2007-06-26 11:11:44"):** 感觉你做的挺多的，不要想太多了。

