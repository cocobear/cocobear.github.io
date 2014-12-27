title: OpenCV试用
tags:
  - OpenCV
id: 926
categories:
  - Life
date: 2011-12-10 23:53:14
---

**Windows下使用Python开发环境配置：
**
* 下载OpenCV-2.3.1-win-superpack.exe
* 解压后复制opencv\build\python\2.6\cv2.pyd文件到C:\Python26\Lib\site-packages下
* Python文件中使用：import cv2.cv as cv

**关于CaptureFromFile函数支持的文件格式：
**
官方的文档中没有详细的说明所支持的格式，我只知道AVI应该可以，但是我的原始视频是MOV格式（很多数码相机选择小格式拍出来的视频都是MOV），刚开始下了一个MOV to AVI MPEG WMV Converter软件，但是不是无法转换，就是转换出来的AVI格式该函数无法正确加载，网上几乎都找不到关于使用opencv应使用何种AVI编码的文件，最后终于发现使用QuickTime专业版转换出来的AVI文件可以使用该函数打开。
    看了一下QuickTime转换出来的AVI文件，发现压缩使用的是“Cinepank Codec”，而MOV to AVI MPEG WMV Converter软件也有一个压缩选项是“Cinepak Codec by Radius”，但是该压缩选项只能转换出来100多K的大小，不明白是怎么回事。

    求“关于视频中运动物体的轨迹跟踪，路径分析”相关的代码、文章、软件。