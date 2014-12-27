title: SDL绘图库
id: 209
categories:
  - 编程相关
date: 2007-10-20 23:59:46
tags:
---

SDL本身并没有提供绘图的函数，不过已经有第三方的实现，比如SDL_draw。我看了它的一些代码，感觉很乱，里面宏的应用应该说是到了“滥用”的地步，（也许作者为了显示自己高超的架御宏的能力吧）。

只好自己去实现这些基本的绘图函数，目前已经可以实现点，线段，矩形，圆，椭圆的函数。这些基本上已经够用了，不过里面有个斜线的实现使用了别人的代码，没看懂他的实现方法，有时间再想想吧。

给个截图：
[![screenshot.png](http://7sbxmt.com1.z0.glb.clouddn.com/screenshot.thumbnail.png)](http://7sbxmt.com1.z0.glb.clouddn.com10/screenshot.png "screenshot.png")

BTW：SDL还是算比较底层的吧，这些很基本的东西也得自己写函数实现:-)

代码可以在[这里](http://cocobear.github.io/code/tar/draw_demo.tar.gz)下载。
## Comments

**[honkin](#2058 "2007-10-21 14:44:37"):** 你好，在校内网上小昕的页面看到你，感觉你的博客很好，能不能做个连接。 http://www.honkin.info/ HonKin's Blog 我已经做好，如果愿意请留言告诉我

**[Amankwah](#2051 "2007-10-21 09:08:32"):** 好，有前途！

**[luguo](#2052 "2007-10-21 11:25:17"):** 沙发先～

**[cocobear](#2059 "2007-10-21 15:29:54"):** Amankwah被spam掉了！

