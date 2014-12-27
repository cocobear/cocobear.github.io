title: 关于串口互联的问题
tags:
  - Python
  - serial
id: 310
categories:
  - 编程相关
date: 2008-08-29 14:53:55
---

先来段插曲：

Python中有pyserial这个模块可以进行串口操作，今天试一下但发现写个最简单的脚本也出错：

	import serial

	serial = serial.Serial()
	serial.port = 1
	serial.timeout = 1

	serial.read(1)

运行的结果是:
serial = serial.Serial()
AttributeError: 'module' object has no attribute 'Serial'
仔细看了看serial库中确实有Serial这个类， 但为什么会出现这样的问题，想了半天没明白，最后google得到结论，问题是“我的文件名是serial.py”，贴出老外的解释：
     Because  "import  serial"  imports  whatever  thing  it  finds  named  serial,   > 
     and  it's  not  finding  PySerial  because  you  have  a  module  named   > 
     serial.py.  > 
      -bob[来自](http://mail.python.org/pipermail/pythonmac-sig/2005-September/015000.html)

这次记下了，再不会犯这样的错误了。

搜pyserial相关资料的时候发现英文的很少，搜到不少cz（捷克）域名的文章，还有些de（德国）的，看不懂啊，现在发现英文资料真好，看着真舒服。（可惜那个Nginx是俄国人写的，又是一堆俄文，唉……）

好了，插曲结束，说下正题，本来是想实现个工具可以把机器上两个串口联起来，可以省去用线把两个串口联起来，但经过试验，又仔细想了想，发现这个想法是不可能的，因为程序对串口进行操作的话必须进行打开操作，而应用程序会先打开串口，所以就无法得到串口发出的数据，即使通过钩子技术获得数据，但也不能向这个串口发送数据，所以原理上应该是无法实现的。
## Comments

**[dream](#4130 "2008-08-31 15:52:53"):** 哈哈，谢谢，谢谢，咱们俩的问题如出一辄，我的问题是文件的名字取成了unittest.py

**[Amankwah](#4120 "2008-08-29 20:48:06"):** 以后技术资料只写中文的，让外国人也郁闷~ 现在工控机通信几乎以串口为主，我整爽了~

**[luguo](#4118 "2008-08-29 16:33:22"):** Python还能进行这么底层的操作～！强悍～～

