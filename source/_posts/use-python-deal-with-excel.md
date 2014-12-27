title: 使用Python处理Excel表格
tags:
  - excel
  - Python
id: 476
categories:
  - 编程相关
date: 2009-01-16 13:26:47
---

给俺的[boss](http://hao150.cn/)写的一个小工具，使用Python对Excel进行统计，然后把结束生成一个新的Excel表格，使用到了[xlrd](http://www.lexicon.net/sjmachin/xlrd.htm)和[pyExcelerator](http://sourceforge.net/projects/pyexcelerator)两个库。

**简单的介绍一下这两个库，先说xlrd，这个库读Excel比较方便，各种方法使用起来也挺方便：**

bk = xlrd.open_workbook('your.xls')
sh = bk.sheets()[-1]
上面两句就可以打开Excel表格中的一个sheet，sheets得到的是一个list，存放所有的sheet。
sh.nrows是该sheet中的行数，知道这个后就可以使用for循环来读所有的单元格了：
sh.row(i)[3]这个就代表第i行的第4列。

**再看看pyExcelerator，这个用起来有点别扭：**

sheets = parse_xls('result.xls')
先打开一个表格，sheets是一个list，包含了所有表格的内容，每一项就是一个sheet，而每个sheet是二元tuple，第一个是该sheet的名字，第二个是一个dict，dict的key是一个二元组，表示单元格的坐标，如(0,0)，第一行第一列。
从上面的分析中可以得出要访问Excel中第一个sheet的第一行第一列元素需要：
sheets[0][1][(0,0)]
pyExcelerator也不能得到行列数。

写文件也比较简单：
wb = Workbook()
ws = wb.add_sheet('result')
ws.write(0,0,“hello”)
wb.save('result.xls')
就不解释了:-)

写文件时需要注意直接写Unicode内容进去，而不要写编码过的内容。

给boss的代码

```
#!/usr/bin/env python
# -*- coding=utf-8 -*-
#Using GPL v2
#Author: cocobear.cn@gmail.com

import xlrd
from pyExcelerator import *

city = [(u'山城','[2,3]d+'),(u'水国','4d+'),(u'火县','5d+'),
       (u'土城','6d+'),(u'土国','7d+'),(u'火乡','8[1-5]d+'),
       (u'水乡','8[067]d+'),]

fname = '0107CRM.xls'
bk = xlrd.open_workbook(fname)
sh = bk.sheets()[-1]
nrows =  sh.nrows
#result中按顺序存放各city中各套餐的数量
#顺序为XTa+、XTb、XTb+
result = []
for i in range(len(city)):
    result.append([0,0,0])

for r in range(1,nrows):
    num = str(sh.row(r)[3])[7:]
    flag = False
    for i in range(len(city)):
        if re.match(city[i][1],num):
            flag = True
            if sh.row(r)[2].value == 3001.0:
                break
            name = sh.row(r)[0].value.encode('utf8')
            if 'XTa＋' in name:
                result[i][0]+=1
            if 'XTb'  in name and 'XTb＋' not in name:
                result[i][1]+=1
            if 'XTb＋' in name:
                result[i][2]+=1
    if not flag:
        print "NO:"+num

print result

titles = [u'局向',u'数',u'M录入数',u'X数',…………]
wb = Workbook()
ws = wb.add_sheet('result')
for i in range(len(titles)):
    ws.write(0,i,titles[i])

for i in range(len(city)):
    ws.write(i+1,0,city[i][0])
    ws.write(i+1,1,result[i][0])
    ws.write(i+1,4,result[i][1])
    ws.write(i+1,7,result[i][2])
    ws.write(i+1,10,result[i][0]+result[i][1]+result[i][2])
ws.write(i+2,1,"=SUM(B2:B8)")
wb.save('result.xls')
```
## Comments

**[草儿](#4908 "2009-01-23 13:25:34"):** 靠，干嘛把地址写那么清楚啊~

**[刘鑫](#4877 "2009-01-17 12:10:16"):** 你比我早,嘿嘿,我正要拿Python处理Excel呢,前几天刚研究了下Excel的COM接口

**[Kermit.Mei](#4989 "2009-02-03 08:31:24"):** 这个很有用，我要好好学习一下，呵呵。

**[Amankwah](#4873 "2009-01-16 22:17:05"):** Excel格式是公开的？

**[wind](#4874 "2009-01-17 00:01:34"):** 看不明白，等我能看明白了再来瞅瞅

**[vvoody](#4875 "2009-01-17 00:20:56"):** 只用过 pyExcelerator 写过一个转课表的，还好用。 嗯，编码问题当时确实我好搞了一番。

**[小慧](#6869 "2009-12-15 19:37:15"):** 这个文章不错！我刚好需要了解python操作Excel的知识。 希望可以发更多关于python的帖子出来。

**[可可熊](#6874 "2009-12-15 22:31:12"):** To:小慧 可以看看PyFetion

**[9命怪猫](#8217 "2010-06-18 11:15:41"):** 其实，类似功能，用Excel内置的宏或者函数库就可以搞定了。 Excel比想象中强大得多，只是大家不愿意去发掘！

**[zhan](#15183 "2012-09-25 21:05:43"):** Excel内置函數是可以做到大部份的事，但置入一堆函數後，復雜度絕對超過比你自已寫出來的程式累

