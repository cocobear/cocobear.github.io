title: vim中使用sdcv(stardict CLI版)
tags:
  - Linux
  - sdcv
  - Vim
id: 280
categories:
  - Linux
date: 2008-05-19 15:31:36
---

[这里](http://my.opera.com/yunt/blog/show.dml/304842)介绍的vim中使用辞典查词的方法不错，利用了vim的一些特性，这样平时使用vim的时候就可以手不离键盘要完成查词了。

sdvc是命令行版的Stardict,可以在这里[下载](http://sourceforge.net/projects/sdcv)，目前最新的版本是0.4.2，不过在ADM64平台下编译会出错，我在网上找到了一个[补丁](http://patches.ubuntu.com/by-release/debian/s/sdcv/sdcv_0.4.2-2.2.patch)，不过这个补丁是针对debain用户的，有些用不上，那些找不到文件的patch就可以直接忽略掉。

如果在gvim中使用则在~/.gvimrc中添加：
```
function  Mybln() 
     let  expl=system('sdcv  -n  '  . 
           \  v:beval_text  . 
           \  '|fmt  -cstw  40') 
     return  expl 
   endfunction 

   set  bexpr=Mybln() 
   set  beval
```
如果直接在vim中使用，则在~/.vimrc中添加：
```
function!  Mydict() 
   let  expl=system('sdcv  -n  '  . 
         \   expand("<cword>")) 
   windo  if 
         \  expand("%")=="diCt-tmp"  | 
         \  q!|endif 
   25vsp  diCt-tmp 
   setlocal  buftype=nofile  bufhidden=hide  noswapfile 
   1s/^/\=expl/ 
   1 
endfunction 
nmap  F  :call  Mydict()<cr> 
```

gvim中鼠标移动到单词上就可以得到翻译，在vim中光标移动到单词上，使用shift＋f可以新分割出来一个窗口显示单词的翻译。

原文请使用梯子访问。
