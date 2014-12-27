title: SDL使用点阵字库
id: 226
categories:
  - 编程相关
date: 2007-11-27 15:39:03
tags:
---

以前的文章中已经提到SDL是相对比较底层的一个开发库，因此需要自己手动写一些比较常用的功能库，前段时间完成了绘图库，这两天写程序时突然发现我还需要一个显示字符的函数，在SDL邮件列表里问了一下，大家的回答都是使用一个bmp的图像文件，把ASCII码的可见字符存在这幅图片中，然后读取图片中的信息，但这样不能灵活的控制字符的大小、颜色，那应该怎么办呢？点阵字库，就是这个东西，以前做DOS下的游戏时也接触到过比较熟悉，只是在寻找字库时花了好大功夫，最后还是从DOS98系统中找到了asc16这个ASCII的点阵字库文件。这个字库的大小是16x8，还有些点阵字库的生成程序但都需要注册不然只能生成最大16x16的字库，郁闷，有时间我自己研究下这个字库生成，写个Linux下的。

当然SDL还有相应的SDL_ttf可以使用，不过如果你对画面中字体要求不是很高的话没必要使用的。

我简单的说一下ASC16这个字库的使用:


     unsigned char match[16];> 
             FILE *ASC;> 
             unsigned char temp = 0x80;> 
             unsigned int addr  = 0; /*offset of a character*/> 
             int x1 = x;> 
             int y1 = y;> 
     
             if (size == 16) {> 
                     ;> 
             }> 
             if ((ASC = fopen("ASC16","rb")) == NULL) {> 
                     fprintf(stderr,"Open ASC16 error!\n");> 
                     return -1;> 
             }> 
             addr = c < < 4;/*设置读取偏移量，offset = 字符的ascii值x16 + 字库首地址（一般为0）；*/> 
             fseek(ASC,addr,SEEK_SET);> 
             fread(match,16,1,ASC);/*每次读一个16bytes大小的块，ASC16中每个字符所使用的空间大小是16bytes*/> 
             for(int i = 0;i&lt;16;i++)> 
             {> 
                     if (i > 0){> 
                             x = x1;> 
                             y++;> 
                     }> 
     
                     temp = 0x80;/*二进制的10000000*/> 
                     for(int k = 0; k < 8; k++)> 
                     {> 
                             if (match[i] & temp) {> 
                                     Draw_Pixel(surface,x,y,color);/*当这一位为1时在此处显示一点*/> 
                                     x++;> 
                             }> 
                             else {> 
                                     x++;> 
                             }> 
                             temp = temp>>1;/*右移，比较下一位*/> 
                     }> 
             }


## Comments

**[crazyfranc](#2515 "2007-11-30 21:11:17"):** 感觉挺好的啊。

