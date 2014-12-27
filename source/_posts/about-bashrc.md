title: "关于.bashrc[管理主机时遇到的问题]"
tags:
  - Linux
  - Shell
id: 61
categories:
  - Linux
date: 2007-04-11 14:05:28
---

习惯了Fedora core 下在.bashrc这个文件中设置一些常用的环境变量，以及alias，这几天在服务器上边通过修改.bashrc文件在里面加入一些alias，但是每次登录进来并不能生效，必须使用source命令才行，给提供商发一份邮件，没想到没分钟就收到了，这会儿也不知道是美国时间的什么时候，真是感叹他们的办事效率，原来是和bash_profile这个文件有关的，其实从shell登录以后最开始是寻找/etc/profile，然后搜索~/.bash_profile, ~/.bash_login, 和 ~/.profile，而Fedora core 中的.basr_profile文件里面是：

	`# Get the aliases and functions
	if [ -f ~/.bashrc ]; then
	        . ~/.bashrc
	fi

	# User specific environment and startup programs

	PATH=$PATH:$HOME/bin

	export PATH
	unset USERNAME`

这样就清楚了，.bashrc文件是通过.bash_profile来生效的，好了，在服务器上把这段代码加入，以后登录服务器就可以使用自己shell下常用的一些设置了，方便的很，呵呵。

因为服务器上没有lftp，觉得下东西的时候不太方便，试了一下可不可以自己装一下，于是在自己的目录下创建opt目录，编译安装lftp的时候使用选项：

--prefix=/home/cocobear/opt/lftp

这样，就可以在服务器上使用好用的lftp了，呵呵，Linux主机太强大了！！