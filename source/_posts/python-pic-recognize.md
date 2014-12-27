title: 使用Python进行验证码识别
tags:
  - orc
  - Python
id: 298
categories:
  - 编程相关
date: 2008-08-04 16:59:16
---

以前写过一个刷校内网的人气的工具，Java的(以后再也不行Java程序了)，里面用到了验证码识别，那段代码不是我自己写的:-) 校内的验证是完全单色没有任何干挠的验证码，识别起来比较容易，不过从那段代码中可以看到基本的验证码识别方式。这几天在写一个程序的时候需要识别验证码，因为程序是Python写的自然打算用Python进行验证码的识别。

以前没用Python处理过图像，不太了解PIL(Python Image Library)的用法，这几天看了看PIL，发现它太强大了，简直和ImageMagic，PS可以相比了。([这里](http://www.pythonware.com/library/pil/handbook/index.htm)有PIL不错的文档)
由于上面的验证码是24位的jpeg图像，并且包含了噪点，所以我们要做的就是去噪和去色，我拿PS找了张验证码试了试，使用PS滤镜中的去噪效果还行，但是没有在PIL找到去噪的函数，后来发现中值过滤后可以去掉大部分的噪点，而且PIL里有现成的函数，接下来我试着直接把图像转换为单色，结果发现还是会有不过的噪点留了下来，因为中值过滤时把不少噪点淡化了，但转换为音色时这些噪点又被强化显示了，于是在中值过滤后对图像亮度进行加强处理，然后再转换为单色，这样验证码图片就变得比较容易识别了:

![pic](http://7sbxmt.com1.z0.glb.clouddn.com/22.jpeg)
![pic1](http://7sbxmt.com1.z0.glb.clouddn.com/tmp.jpeg)

上面这些处理使用Python才几行：

    im = Image.open(image_name)
    im = im.filter(ImageFilter.MedianFilter())
    enhancer = ImageEnhance.Contrast(im)
    im = enhancer.enhance(2)
    im = im.convert('1')
    im.show()


接下来就是提取这些数字的字模，使用shell脚本下载100幅图片，抽出三张图片获取字模：

	#!/usr/bin/env python
	#encoding=utf-8

	import Image,ImageEnhance,ImageFilter
	import sys

	image_name = "./images/81.jpeg"
	im = Image.open(image_name)
	im = im.filter(ImageFilter.MedianFilter())
	enhancer = ImageEnhance.Contrast(im)
	im = enhancer.enhance(2)
	im = im.convert('1')
	#im.show()
	                #all by pixel
	s = 12          #start postion of first number
	w = 10          #width of each number
	h = 15          #end postion from top
	t = 2           #start postion of top

	im_new = []
	#split four numbers in the picture
	for i in range(4):
	    im1 = im.crop((s+w*i+i*2,t,s+w*(i+1)+i*2,h))
	    im_new.append(im1)

	f = file("data.txt","a")
	for k in range(4):
	    l = []
	    #im_new[k].show()
	    for i in range(13):
	        for j in range(10):
	            if (im_new[k].getpixel((j,i)) == 255):
	                l.append(0)
	            else:
	                l.append(1)

	    f.write("l=[")

	    n = 0
	    for i in l:
	        if (n%10==0):
	            f.write("\n")
	        f.write(str(i)+",")
	        n+=1
	    f.write("]\n")

把字模保存为list，用于接下来的匹配；

提取完字模后剩下来的就是对需要处理的图片进行与数据库中的字模进行匹配了，基本的思路就是看相应点的重合率，但是由于噪点的影响在对(6,8)(8,3)(5,9)的匹配时容易出错，俺自己针对已有的100幅图片数据采集进行分析，采用了双向匹配(图片与字模分别作为基点)，做了半天的测试终于可以实现100%的识别率。

	#!/usr/bin/env python
	#encoding=utf-8

	import Image,ImageEnhance,ImageFilter
	import Data

	DEBUG = False

	def d_print(*msg):
	    global DEBUG
	    if DEBUG:
	        for i in msg:
	            print i,
	        print
	    else:
	        pass

	def Get_Num(l=[]):
	    min1 = []
	    min2 = []
	    for n in Data.N:
	        count1=count2=count3=count4=0
	        if (len(l) != len(n)):
	            print "Wrong pic"
	            exit()
	        for i in range(len(l)):
	            if (l[i] == 1):
	                count1+=1
	                if (n[i] == 1):
	                    count2+=1
	        for i in range(len(l)):
	            if (n[i] == 1):
	                count3+=1
	                if (l[i] == 1):
	                    count4+=1
	        d_print(count1,count2,count3,count4)

	        min1.append(count1-count2)
	        min2.append(count3-count4)
	    d_print(min1,"\n",min2)
	    for i in range(10):
	        if (min1[i] < = 2 or min2[i] <= 2):
	            if ((abs(min1[i] - min2[i])) < 10):
	                return i
	    for i in range(10):            
	        if (min1[i] <= 4 or min2[i] <= 4):
	            if (abs(min1[i] - min2[i]) <= 2):
	                return i

	    for i in range(10):
	        flag = False
	        if (min1[i] <= 3 or min2[i] <= 3):
	            for j in range(10):
	                if (j != i and (min1[j] < 5 or min2[j] &lt;5)):
	                    flag = True
	                else:
	                    pass
	            if (not flag):
	                return i
	    for i in range(10):            
	        if (min1[i] <= 5 or min2[i] <= 5):
	            if (abs(min1[i] - min2[i]) <= 10):
	                return i
	    for i in range(10):
	        if (min1[i] <= 10 or min2[i] <= 10):
	            if (abs(min1[i] - min2[i]) <= 3):
	                return i

	#end of function Get_Num

	def Pic_Reg(image_name=None):
	    im = Image.open(image_name)
	    im = im.filter(ImageFilter.MedianFilter())
	    enhancer = ImageEnhance.Contrast(im)
	    im = enhancer.enhance(2)
	    im = im.convert('1')
	    im.show()
	                    #all by pixel
	    s = 12          #start postion of first number
	    w = 10          #width of each number
	    h = 15          #end postion from top
	    t = 2           #start postion of top
	    im_new = []
	    #split four numbers in the picture
	    for i in range(4):
	        im1 = im.crop((s+w*i+i*2,t,s+w*(i+1)+i*2,h))
	        im_new.append(im1)

	    s = ""
	    for k in range(4):
	        l = []
	        #im_new[k].show()
	        for i in range(13):
	            for j in range(10):
	                if (im_new[k].getpixel((j,i)) == 255):
	                    l.append(0)
	                else:
	                    l.append(1)

	        s+=str(Get_Num(l))
	    return s
	print Pic_Reg("./images/22.jpeg")


这里再提一下验证码识别的基本方法：截图，二值化、中值滤波去噪、分割、紧缩重排（让高矮统一）、字库特征匹配识别。
这里只是针对一般的验证码，高级验证码的识别<a href="http://huaidan.org/archives/2085.html">这里有篇不错的文章，太复杂的话涉及的东西就多了，那俺就没兴趣了，人工智能(好恐怖)，俺只喜欢简单的东西。
## Comments

**[crazyfranc](#4005 "2008-08-06 23:49:32"):** 很强大嘛！

**[Amankwah](#3996 "2008-08-04 20:57:07"):** 很好很强大

**[luguo](#3997 "2008-08-05 17:21:03"):** 恩，强悍啊～～有时间看看～最近比较忙。。。

**[cocobear](#4178 "2008-09-09 13:35:57"):** 在另外一篇文章中有下载的。 http://cocobear.github.io/2008/08/11/tool-of-python-sn/

**[可可熊](#5491 "2009-03-15 20:44:52"):** 现在可以在上面那个地址找到了。

**[mage](#5476 "2009-03-14 10:26:53"):** import Data 这个模块是你自己写的吧 能否提供查看的地方？多谢 另： http://cocobear.github.io/2008/08/11/tool-of-python-sn/ 是什么地址，打开“404”

**[lucky](#4177 "2008-09-09 11:38:15"):** 老大啊，我试了代码，但是import Data不能通过，Data是哪个模块啊

**[AFITY开源社区](#8228 "2010-06-22 13:03:30"):** 实用~

**[newcode](#10486 "2011-07-06 11:30:11"):** Data模块哪里下载？ 下面这个地址似乎找不到此模块： http://cocobear.github.io/2008/08/11/tool-of-python-sn/

**[xhashao](#9393 "2011-01-05 18:43:35"):** 能不能发我一份代码，谢谢了！好像蛮久啦

**[可可熊](#9730 "2011-03-15 11:51:15"):** 上面代码都贴出来了

**[luren](#20088 "2013-01-18 17:11:46"):** http://cocobear.github.io/2008/08/04/python-pic-recognize/ 老大啊，我试了代码，但是import Data不能通过，Data是哪个模块啊

