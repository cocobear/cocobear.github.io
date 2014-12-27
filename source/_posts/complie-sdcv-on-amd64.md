title: sdcv在amd64上编译问题
tags:
  - sdcv
id: 424
categories:
  - 编程相关
date: 2008-12-18 13:13:27
---

sdcv是stardict的命令行替代，打算用stardict的字典，所以看看如何从字典中取词，sdcv比较小巧，拿来看挺合适，不过在我机子上编译出现了点问题：

	> lib.cpp:516: error: no matching function for call to ‘min(long unsigned int, guint32&)’

解决办法：

     -fread(wordentry_buf, std::min(sizeof(wordentry_buf), page_size), 1, idxfile); > 
         +fread(wordentry_buf,std::min(sizeof(wordentry_buf),(size_t)page_size),1,idxfile);

--------------------------------------------split---------------------------------------
听说WordPress2.7在后台的UI上变化不小，今天试着升级上来，发现果然是完全改变了以前的UI，大量使用了JS，感觉还不错:-)

好久没有Opera了，因为又出现了无法输入的现象，只好去用了Firefox一段时间，今天换了下Ibus输入法，发现可以在Opera里面使用，赶快换回Opera，firefox的速度还是与Opera无法相比的。对了Ibus这个输入法也不错。
## Comments

**[草儿](#4691 "2008-12-18 19:02:34"):** 哈哈，现在的后台是不是比以前好用的多~ ibus默认应该不能在opera里使用吧，我当初可是修改了参数才能用的。

**[可可熊](#4699 "2008-12-19 09:29:15"):** 2.7后台做得确实好啊，很从人性化的设计，很方便，赞一个。

**[luguo](#4696 "2008-12-18 23:43:18"):** 看来我又该升级wordpress了～～哈哈～

