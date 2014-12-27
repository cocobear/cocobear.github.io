title: "随手笔记[Linux技巧]"
tags:
  - Linux
  - tips
id: 38
categories:
  - Linux
date: 2007-04-06 19:18:05
---

Monday, 1\. January 2007, 13:58:44


提取rpm包中的文件：

使用工具rpm2cpio和cpio 
rpm2cpio xxx.rpm | cpio -vid
rpm2cpio xxx.rpm | cpio -idmv 
rpm2cpio xxx.rpm | cpio --extract --make-directories 
参数i和extract相同，表示提取文件。v表示指示执行进程 
d和make-directory相同，表示根据包中文件原来的路径建立目录 
m表示保持文件的更新时间。

./configure的时候发现某个lib已经安装，但仍然提示未安装如下面的情况：

	checking for SDL - version < = 1.2.4... yes
	checking for Mix_OpenAudio in -lSDL_mixer... no
	configure: error: SDL_mixer library required
	[cocobear@cocobear supertux-0.1.3]$ rpm -q SDL_mixer
	SDL_mixer-1.2.6-7.fc5
	[cocobear@cocobear supertux-0.1.3]$


这种情况，你很可能需要安装:_SDL_mixer-**devel**_

I met that problem,and find [some information](http://lists.centos.org/pipermail/centos/2004-June/042714.html)

VIM乱码解决：

把/etc/vimrc中的 set fileencodings=utf-8,latin1 改为 set fileencodings=gb2312,gb18030,utf-8,latin1 

sudo vi 编辑文档时没有高亮，原因是在fc5中普通用户下的vi已经被默认改为vim，如下：

	[cocobear@cocobear php]$ alias
	alias bye='halt -p'
	alias du='du -h'
	alias l.='ls -d .* --color=tty'
	alias ll='ls -l --color=tty'
	alias ls='ls --color=tty'
	alias vi='vim'
	alias which='alias | /usr/bin/which --tty-only --read-alias --show-dot --show-tilde'

知道原因后，只要使用sudo vim就可以了
