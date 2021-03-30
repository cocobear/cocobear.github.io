title: 写给新手的一点学习Linux经验
tags:
  - Linux
id: 60
categories:
  - Linux
date: 2007-04-11 13:02:40
---

对于Linux我就不做过多的介绍了，你们可以使用各种各样的途径来了解Linux，当然网络是最好的途径，不过有一本比较全面一点介绍Linux书籍也是挺重要的，这里我推荐一本入门级的读物，《Linux权威指南》,具体是哪个出版社出版的不得了，我这里有电子本的，图书馆也有，如果需要电子版的可以给我发邮件。

这里顺便提一点，推荐大家使用google搜索，如果你使用baidu来搜索Linux关键词，第二、三条都是Linux培训网站（这个有时效性的！）。这一点充分说明了baidu是很垃圾的。

下面我就简单的介绍一下我学习Linux的经验，当然也可能不适合每个人，只要对大家有所帮助，大家能从中得到一些启迪就可以了。

应该是大一第二学期刚开始的时候，第一次见到Linux是在王聪的机子上，那时候觉得Linux的桌面很漂亮，那个系统应该是Turbo Linux，版本应该挺低的，应该是王聪高中买机子的时候送的光盘，所以有很长的时间了。其实当然跟王聪要的光盘装Linux完全是因为它的界面漂亮，不过装完以后就傻了，因为全是英文界面的，进去以后都不知道做什么，没几天的时间就因为重装Windows的时候把它给删了，（大家应该都有经验的，Windows隔几天重装一次系统应该是很正常的吧？）第一次接触Linux就这样结束了。

过了大概一个月左右的时间，还是在王聪的宿舍，发现他的Linux变了，是中文界面的了，于是第二次从他那里借得光盘开始装Linux，这次装的是Fedora Core 3,装完后还自己在同学的机子上把三张光盘复制了次，到现在那光盘还在。刚开始使用Fedore Core 3的时候遇到了很多问题，Windows下的文件乱码，U盘不能自动挂载，字体难看，花了好长一段时间，通过搜索，以及到一些Linux论坛找解决的办法，也就是在这一段时间内对Linux有了更深入的了解，也从论坛中得到了更多的关于Linux的信息。你们现在如果装Linux应该不会遇到这么多的问题，随着Linux的发展，较新一些的Linux发行版已经做的相当的成熟，装完系统后需要很少的改动就可以使得Linux很好的为我们服务。而且许多发行版甚至默认使用了3D桌面，比Windows确实漂亮很多。不过这样一来又产生了新的问题，你们如果使用较新的发行版也许就不能体会得到把一个十分难用，又有不少问题的系统整理为一个漂亮的，好用的系统那种感觉了。而且从中你可以学到好多的东西，比如常用的工具，shell命令，X-window原理，等等。

我刚开始接触Linux的时候，能有几个共同的爱好者在一块讨论，对自己有很大的帮助，就像现在一样，比如我这几天的时间对awk作了一些研究，觉得可以拿出来和大家分享，就可以站出来讲一下awk的使用，也许我讲的有不对的地方，大家都可以随时提出来，你这里不对，应该用双引号的。

其实交流讨论无论在什么时候对自己，对别人都有好处的，就像王老师说的一个人思想总是有限的，但是如果大家都拿出来交流时，每个人都可以获得更多的思想。

如果你觉得你已经入门了，那就可以在平常用电脑的时候尽量使用LInux，而不是Windows，不过我不建议大家这个时候就把Windows从硬盘上删除，因为可能有许多事Linux下是无法完成的，比如使用网银，Photoshop，当然，ps可以在Wine模拟，不过运行速度就相当慢了，如果你的机子不是很好，建议你不要这样做。我也曾经很早就把Windows从硬盘上删除了，不过没过多久我发现我还是离不开Windows，因为淘宝不能用了，ps也不能用了，这两个都是我那时候用的最多的东西，没办法，只能再次把Windos装上。不过现在我的硬盘里只有Linux，因为ps,淘宝现在用的不多，如果实在要用的话也可以在同学的机子上使用，这时候也就没有把Windows留在硬盘上的理由了。不过我强烈建议大家还是仔细考虑过，确认自己的确是所有的事都可以在Linux下完成的时候再动手删除Windows。

这个时候你应该确立自己的目的了，你需要做些什么，如果你是想编程，就可以开始学习一下GCC，Vi，Gdb，等一些常用的工具使用，当然这是学习C语言用到的一些工具，而如果你想学其它语言的时候，可以在google里搜索一下相关的工具，学习一下应该如何使用它们。如果你想学习Linux下的服务器相关的东西，就可以下一个apache，记得要从源码编译安装，如果rpm,deb包会隐藏好多细节，从源码编译安装中你可以学到更多的东西，更能清楚的了解apache的工作，而且也方便编译apache的时候按照你需要的去编译，把一些没用的东西去掉，增加一些必须的选项。当然你也可能有其它的目的，这都可以的，只要有一个明确的目的，切记不可以玩Linux，今天觉得KDE桌面好看，就把gnome改为KDE，明天可能又觉得debain的发行版好，于是又换一个系统，或者装几个系统，即使你的硬盘很大，也不要做这样的事情，你没必要去考虑哪个发行版更好一些，现在主流的几个发行版都有它自己的优点，我推荐使用Fedora core 5，首先我使用的是这个系统，而且这个系统比较合适初学Linux的人用，当然Ubuntu也是一个不错的选择。

**If it not broken,don't fixe it** .这是Linux里经常会提到的一句话，如果一个软件你用起一已经足够了，那为什么还要去升级呢？而且往往升级的时候会产生很多的问题，所以没有必要整天升级系统。我从大二开始一直使用的就是Fedora core 5，已经有半年多的时间没有重装过系统了，而我们宿舍使用Windows的几乎隔几天就得重装一次系统，有时候甚至得把硬盘全格了。

这里我想说一点，如果你对Linux基本熟悉，那就没有必要花很多的时间去学习Linux下各种工具，因为各种各样的工具实在是太多了，甚至上有千个，而且大部我们都不会用到的，所以我建议大家在需要的时候，再去看一下相关的工具，你可以通过man手册，或者google来学习相关工具的使用，我一般是去google的，因为man手册是一些很死的解释，也没有相关的例子，有时候会让你更迷惑的，当然man手册不是没有用的，所有的文章都不可能比man手册更全面的介绍这某个工具，有时候你还不得不去看man手册里的东西，而且不同的发行版可能同样的工具支持的选项也是不一样的。记住一点“**当你需要的时候才去学习它**”

最后我想说一下Linux思想，现在的Linux已经不仅仅是一个操作系统，它更是一种理念，一种通过现实以及网络把有着共同爱好的人联系到一块，互相帮助，学习以及交流。希望大家无论在这里还是在新闻组上都能积极讨论，交流，不要怕提出问题，有问题是很好的，而且每个人都会很乐意回答你的问题。