title: "无耐的startx[未解决]"
tags:
  - Linux
  - startx
id: 46
categories:
  - Linux
date: 2007-04-06 19:18:27
---

Wednesday, 14\. March 2007, 11:27:44


今天突然心血来潮和打开Add/Remove Software删了一下自己认为是“没用”的东西(其实确实“应该”是没用的东西，例如打印机支持)，但就是这样，重起之后X就进不去了，而且问题很奇怪，startx之后没有任何出错提示，直接是黑屏，等Ｎ长时间还是没反应，也没多想什么就重新在另一个分区装了一遍系统(fc5)，在linux text模式下安装的，选了大概是1.7G的东西，重起后又和前面一样的反应，![:zzz:](/community/graphics/smilies/zzz.gif) 
后来在王聪的建议下改了一下xorg.conf中的这一行

DefaultDepth     24

把颜色深度调低了一点，改成了8，结果startx有了反映：

	Could not init font path element unix/:7100, removing from list!
	Fatal server error:
	could not open default font 'fixed'

似乎以前就遇到过这种问题，不过把解决办法给忘了，幸好可以上网,google了下，原来是xfs服务没有启动(这时候才想起是在single用户模式下用的startx，基本上还没启动什么服务)，这下终于进去了。

不过颜色当然很难看了，本想这下应该好了，把xorg.conf文件中的DefaultDepth又改了回去，没想到重起后又是黑屏，真是搞不明白，只能继续把DefaultDepth改回去，然后在图形界面下把分辨率调了一下1024X768,重起了一下结果终于正常了，而xorg.conf中的DefaultDepth 又被改回了24,现在实在是糊涂了，真搞不明白DefaultDepth到底是怎么整的![:frown:](/community/graphics/smilies/frown.gif)

然后在grub中添加了以前系统的启动项目，按照上面的方法把DefaultDepth改为8,启动后又在图形界面中调整分辨率，终于回到了以前的桌面。

ps:新装的是kde桌面，简单的用了一下觉得似乎比gnome的快了一下，也有些地方设计的比gnome更加人性化，不过似乎习惯了gnome，也习惯了以前的这个桌面，还是用老的吧，新装的那个系统就留着吧。
