title: 折腾了一个代码高亮的插件
tags:
  - ig_syntax_hilite
id: 255
categories:
  - 编程相关
date: 2008-04-03 11:41:10
---


	awk '/.*0020*./{print}' 00 > 01
	awk ' {print $18} ' 01 > 02
	cat 02 | sort | uniq > 03
	//g' 03 > 04
	sed -e '/.*\.\.\.\.\.\./d' 04 > 05
	sed -e 's/\.\.\.//g' 05 > 06
	sed -e 's/\b\.\b/\-\-\-\-/g' 06 > 07
	rm -f 01 02 03 04 05 06 
	awk '/.*0020*./{print}' 00 > 01
	awk ' {print $18} ' 01 > 02
	cat 02 | sort | uniq > 03
	//g' 03 > 04
	sed -e '/.*\.\.\.\.\.\./d' 04 > 05
	sed -e 's/\.\.\.//g' 05 > 06
	sed -e 's/\b\.\b/\-\-\-\-/g' 06 > 07
	rm -f 01 02 03 04 05 06 


上面是效果，感觉还不错，插件叫[ig_syntax_hilite](http://blog.igeek.info/wp-plugins/igsyntax-hiliter/)，原本是是白色为基色的，因为我的博客是黑色为主，所以自己改了改，这个插件使用的是[GeSHi](http://qbnz.com/highlighter/)这个进行代码高亮的。插件里原本的GeSHi有不少时间没更新，俺就从GeSHi主页下载了最新的代码，这下可以高亮的语言增加了不少，就连bash的代码也可以，不错。

不知道为什么原来的代码行号是一行粗一点一行细一点，看着实在是不爽，找到代码改为统一的normal格式。打了个包，在下面，想用的就下载吧。

[ig_syntax_hilite黑色风格](http://cocobear.github.io/download/ig_syntax_hilite_black.tar.gz)