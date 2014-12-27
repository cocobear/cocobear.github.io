title: 关于Stardict的字典
tags:
  - pystardict
  - sdcv
  - stardict
id: 433
categories:
  - 编程相关
date: 2008-12-31 13:37:24
---

前几天看了下sdcv(Console version of Stardict)，感觉很不爽，C++的代码，花了好长时间才理清楚那些结构，我觉得使用C语言来写的话可以让代码更清晰、简洁，或者C++也可以写得更好一些。因为本来就是个很简单的东西，从一个文件中读取单词索引，然后与输入比较，最后在另外一个文件里面查到解释，加上一些模糊匹配，如果需要再加上对国际化的支持，就这些东西。

因为打算在我做的东西里面使用字典，想用stardict的字典，找到了一个[PyStardict](http://github.com/lig/pystardict/tree/master)，不过速度太慢了，加载一个11M的字典需要10s的时间，原因是字典的idx文件中存放的索引使用Python需要一个字节一个字节的去判断字符串长短，而sdcv在实现的时候使用的是C++，它里面读idx文件的时候使用到了指针来确实一个字符串的大小，而Python没办法做到，只能一个一个读然后判断是否字符串结束。
sdcv-0.4.2/lib/lib.cpp:629:

        for (guint32 i=0; i<wc; i++) {
            index_size=strlen(p1) +1 + 2*sizeof(guint32);

            if (i % ENTR_PER_PAGE==0) {
                wordoffset[j]=p1-idxdatabuffer;
                ++j;
            }
            p1 += index_size;
        }


谁有什么好的办法吗？
## Comments

**[HH](#5184 "2009-02-16 21:43:09"):** 那哥们在大学编写的，现在Redhat上班呢 http://www.huzheng.org/

**[可可熊](#5209 "2009-02-18 21:21:54"):** 这位兄弟是？ 你说的我知道。

