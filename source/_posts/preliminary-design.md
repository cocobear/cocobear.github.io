title: 项目文档–概要设计说明书 V0.1
id: 50
categories:
  - 编程相关
date: 2007-04-06 19:18:34
tags:
---

一	引言

1.1	编写目的

		确定整个程序设计框架，以及需要实现的功能，对程序接口进行定义，定义运行状态以及出错处理。

1.2	背景

		a.项目名称：xylFTP(待定)
		b.
		任务提出者：王老师
		开发者：见软件需求说明书1.2.b
		用户：服务端主要面向Linux用户，客户端面向所有可以使用JVM的用户。
		中心：运行FTP服务器的机器

1.3	定义

		JVM：java virtual machine
		FTP：File Transfer Protocol

1.4	参考资料

		a.软件需求说明书[url]http://wangcong.org/blog/?p=119[/url]
		b.见软件需求说明书1.4

[B]二		总体设计[/B]

2.1	需求规定

		a.输入：服务端需要配置文件xylftp.con，不接受其它参数输入。客户端接受从终端输入的命令，以及启动客户端所带参数。
		-u	$user 使用$user连接，当不使用-u时使用shell用户名连接。
		-h	[$command]显示客户端所允许的命令，$command指定的情况下显示$command相关使用方法。
		-v	显示版本信息
		-d	显示更多额外信息，供查错使用。
		-a	自动登录，使用.xylftpauto.conf文件中包含的内容自动登录

		b.输出：服务端在运行过程中产生文件xylftp.log，收集一些客户端连接信息，包括登录时间，IP，下载、上传文件、数据量，以及断线时间。客户端响应终端输入显示帮助，以及服务端返回的信息，同时保存登录时用户信息在xylftpauto.conf文件中，供自动登录使用。

2.2	运行环境

		服务端支持i368，x86_64，客户端适用于所有可使用JVM的机器上。

2.3	基本设计概念和处理流程

		服务端：
		目的是设计一个简洁，高效的FTP服务器，并不涉及到较为复杂的安全策略(有关FTP的明文传输)，以较快的速度完成整体框架的设计，然后对具体模块进行改进，优化。
		服务端程序配套的带有一个xylftptool的工具，可以用它来生成配置文件(当配置文件被改乱或者丢失的时候使用)，以及检查配置文件的错误，同时可以生成用户数据，采用md5加密存储。服务端启动的时候先读取xylftp.conf文件，将文件中的参数传递给主程序，然后通过参数解析函数完成参数的处理，把相应的值赋给全局变量以及函数。主程序调用ftp_linsten()函数进行监听,如果相应端口(未必是21,可根据xylftp.conf文件更改)有连接请求，则调用ftp_server()处理，此时应该考虑服务端使用的是passive还是port模式，分别采取不同的处理方式，如果是passive模式，则应开启相应的data port等待客户端的数据连接，如果是port模式，则使用端口20数据传输的初始化。随后该函数使用fork()建立新的进程处理用户请求。主程序继续处于ftp_listen()状态，直到进程被杀死。

		客户端;
		设计一个简单，实用的FTP客户端，支持常用的FTP命令，使用java标准库开发，并且预留GUI接口。
		客户端首先接收命令行的输入，使用命令解释程序来解析命令，如果本地可以完全的命令，直接调用相应的函数完成，如果需要传送到远程FTP主机，则传输相应的命令到远程FTP。同时客户端不断接收服务端的处理结果，并显示。

2.4	结构

		服务端：

2.5	功能需求与程序的关系

		服务端提供FTP服务，客户端连接服务器使用其提供的功能。

2.6	人工处理过程

		无人工处理过程

2.7	尚未解决的问题

		服务端运行模式。

[B]三	接口设计	[/B]

3.1	用户接口

		服务端不向用户提供任何命令，用户可以通过修改配置文件xylftp.conf文件来实现对服务端启动方式的修改。

		客户端提供命令如help，quit,bye等，详见需求规定(暂时未完成)

3.2	外部接口

		服务端需要在运行有Linux系统主机的shell下启动，无交互模式。客户端需要JVM的支持。

3.3	内部接口

		见2.4结构

[B]四	运行设计[/B]

4.1	运行模块组合

		(vsftpd有两种启动模式：stand alone以及super daemon,不知道我们需要这样做吗?具体两种方式见附文一。
		服务端支持被动模式吗?如果支持被动模式难度似乎会增加很多，见附文二)

4.2	运行控制

		由4.1决定

4.3	运行时间

		由4.1决定

[B]五	系统数据结构设计[/B]

5.1	逻辑结构设计要点

		应该要用到排序算法可使用哈唏排序

5.2	物理结构设计要点

		直接使用系统调用存储文件，不考虑存取的物理关系。

5.3	数据结构与程序的关系

		没必要

[B]六	系统出错处理设计[/B]

6.1	出错信息

		服务端：
		error01-读取xylftp.conf文件错误，请确认xylftp.conf存在，并且有读取权限。
		error02-xylftp.conf文件格式错误，请根据说明更改xylftp.conf文件内容。
		error03-xylftp已经运行，结束xylftp进行，重新启动。
		error04-其它FTP服务器(也可能是其它应用程序)占用21端口，结束相关占用21端口程序的进程。
		error05-用户名不存在，使用其它用户名登录，或者联系FTP管理员。
		error06-密码错误，尝试其它密码，或者联系FTP管理员。
		error07-[大家添加一些]
		客户端：
		error01-无法连接远程主机，请确认远程主机是否活动，使用ping命令，请确认远程主机是否使用默认端口21
		error02-无法上传文件出错，请确认使用的用户是否具有相应目录的写权限。
		error03-无法下载文件出错，请确认使用的用户是否具有相应的读取权限，以及本地目录是否有写入权限。
		error04-下载过程中出错，请确认网络处于连接状态，并重新下载。
		error05-远程主机关闭，请与远程FTP管理员联系。
		error06-[大家添加一些]

6.2	补救措施

		下载过程中出错(包括文件传输中断，以及文件校验不符)，则重新下载，重试次数3次。

6.3	系统维护设计

	服务端运行时产生详细的运行记录供维护使用，客户端使用-d参数显示详细的服务端返回信息。

附文一：

vsftpd 啟動的模式
vsftpd 可以擁有兩種啟動的方式，分別是一直在監聽的 stand alone ，一種則是透過 xinetd 這個 super daemon 來管理的方式，兩種方式所使用的啟動程序不太相同，而我們的 CentOS 則預設是以 stand alone 來啟動的。 那什麼時候應該選擇 stand alone 或者是 super daemon 呢？如果你的 ftp 伺服器是提供給整個網際網路來進行大量下載的任務，例如各大專院校的 FTP 伺服器，那建議你使用 stand alone 的方式， 服務的速度上會比較好。如果僅是提供給內部人員使用的 FTP 伺服器，那使用 super daemon 來管理即可啊。

附文二：
Tinyftp is a small ftp server. It implements a set of all
the neccessary  commands for performing the main actions,
suppported by the protocol. The server is based on rfc959.
This server does not support "passive" mode, because it needs
a simultaneous communication on a custom free port for
additional data transfers. For this we will need an IPC, which
hardens the implementation a lot.
The only valid user, that can log in is "anonymous", because of
the different authentication mechanism on different platforms.
All server options are set from the command line parameters.