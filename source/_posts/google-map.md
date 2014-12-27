title: Google Map
tags:
  - map
  - Python
id: 723
categories:
  - Life
date: 2009-08-04 09:22:56
---

Google Map的算法分析见:[http://www.codeproject.com/KB/scrapbook/googlemap.aspx](http://www.codeproject.com/KB/scrapbook/googlemap.aspx)

Google Map使用http://mt[0-3].google.cn/mt/v=cn1.11&hl=zh-CN&x=%d&y=%d&z=%d&s=Galile这样的URL来表示一个放大等级中最小的地图分块。
mt0----mt3是四个Google Map的服务器
v=cn1.11 是当前地图的版本 一直在变化中
x,y,z分别代表当前分块所在位置和放大等级，Z=19为最大的放大等级，目前很多城市都可以查看到该等级下的地图
    在某个放大等级z下，整个地球被分成2^z分块
    x,y可以根据经纬度来计算;x的计算比较好理解(longitude->经度)：
    longitude=180+longitude                     #修正经度值到0-360 因为经度的表示是从-180--->180
    longTileSize=360.0/(pow(2,zoom))        #计算每个分块所占的角度数
    tilex =  longitude/longTileSize               #计算当前经度所在的分块位置

y的计算要牵扯到墨卡托投影这个地图算法，比较麻烦没看懂。不过并不影响使用，相应的代码上面已经给出来了，然后就可以用Python下载地图分块，使用PIL库把分块合并。

代码见[Google Code](http://code.google.com/p/pytool/source/browse/trunk/GMap2Png.py)

(这个是西安地图)：
#GMap2Png(108.80824,34.37075,109.10316,34.15366,16)
参数为你需要确定的图片左上角的经纬度和右下角的经纬度 最后一个是放大的等级
使用的时候需要注意如果你选择的经纬度范围较大，那么放大等级就不能太大，不然要生成一个巨大图片，PIL会报MemoryError的错误。

还有些问题我在[CPyUG](https://groups.google.com/group/python-cn/browse_thread/thread/15a19893a7aa503b/b9c8fa6be0056a5f#b9c8fa6be0056a5f)记录了下来，不过没使用好的解决方案。
## Comments

**[草儿](#6290 "2009-08-04 13:59:30"):** 墨卡托投影。。。。 我发现你现在就是一全才。。。

**[可可熊](#6291 "2009-08-04 15:38:05"):** 晕 没看到我说看不懂了 要站在巨人的肩膀上。

**[da壮熊](#6384 "2009-08-24 10:42:57"):** 恩，要不断学习。要不断向cocobear学习！

**[da壮熊](#6385 "2009-08-24 10:44:21"):** 呃。。。为什么IE发不了评论。而firefox却可以。。。

**[haoziyanwo](#6710 "2009-11-14 21:34:12"):** 向cocobear学习，hoho

