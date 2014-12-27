title: 更新两个小脚本
tags:
  - pysdcv
  - Python
  - sdcv
id: 583
categories:
  - Life
date: 2009-03-20 17:32:12
---

一个是以前写的导出抓虾收藏的工具，增加了保存抓虾上文章的功能，因为只导出链接有些文章地址可能已经失效。

另一个就是pysdcv，前面文章介绍过了，整理了一下，测试了一下，然后放在了google code上面，直接使用星际译王的词典，查一个单词大概0.15秒的时间：

     cocobear@0-0 /home/cocobear/Work/pytool/pysdcv $ make > 
     gcc lookup.c -g -I/usr/include/python2.5 -lz -shared -fPIC -o lookup.so> 
     cocobear@0-0 /home/cocobear/Work/pytool/pysdcv $ time ./pysdcv.py test> 
     *[test]> 
     n. 测试, 试验, 化验, 检验, 考验, 甲壳> 
     vt. 测试, 试验, 化验> 
     vi. 接受测验, 进行测试> 
     【医】 试验, 测验> 
     【经】 检验, 试验, 测试> 
     相关词组:> 
       put sth to the test> 
       stand the test> 
       give a test> 
       take a test> 
       test sb's ability> 
     
     real	0m0.164s> 
     user	0m0.143s> 
     sys	0m0.010s

如果是未使用gzip压缩过的dict文件，则0.05秒左右。
## Comments

**[可可熊](#5577 "2009-03-24 10:32:30"):** 几行代码的一个玩具，呆呵。

**[Kermit](#5551 "2009-03-21 21:39:15"):** 确实比较慢，不过对用户影响不大，呵呵。

**[可可熊](#5550 "2009-03-21 20:18:29"):** 一年之计在于春嘛

**[草儿](#5543 "2009-03-20 22:32:15"):** 这几天你勤快的很嘛

