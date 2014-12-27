title: Html中的表格
id: 53
categories:
  - 编程相关
date: 2007-04-07 01:54:48
tags:
---

表格确实是刚学Html时最难的地方，觉得这篇文章确实不错，这里推荐给Html新手。

记个东西：如果在Html中要显示<table>，需要写为：

&lt;table&gt;

<可以用&lt;代替;

&lt可以使用&amp;lt;代替;

&amp;lt;可以使用&amp;amp;lt;代替

如此继续……，今天突然不知道如何在Html中显示<，然后知道如何显示<的时候又想知道如何显示&lt;，然后通过万能的Google得到以上结果。

以下是正文：

任何表格都包含三个最基本的要素：表格，表格行和格子。他们分别用
	<table>、<tr>、<td>
表示，其中<td>是直接写数据的区域是所谓的Table Data区域，借此你可以记住格子的表示。

  源代码：

<table border>

  <tr>

    <td>格子1</td>

    <td>格子2</td>

  </tr>

  <tr>

    <td>格子3</td>

    <td>格子4</td>

  </tr>

</table>




格子1	格子2
格子3	格子4
  记住：<td>永远要以<tr>结束正如<tr>永远要以<table>

 其他：

1、<th> - 定义表头，与<td>同级，使<th>包含下的内容自动加重显示

2、<caption align=#>xxx</caption> #=left, center, right 定义表格的标题

3、表格边框尺寸设置<table border=#>，缺省值2

4、表格尺寸设置<table border width=# height=#>

5、格子间隙设置：<table border cellspacing=#>

6、格子内部空白大小设置 <table border cellpadding=#>

7、<td colspan=2>跨两列的格子</td> 列间隔等于2

8、<td rowspan=2>跨两行的格子</td> 行间隔等于2

9、表格内文字的水平对齐 <tr align=#>、<th align=#>、<td align=#> #=left, center, right

10、表格内文字的垂直布局<tr valign=#>、<th valign=#>、<td valign=#> #=top,middle,bottom,baseline

11、表格在页面中的对齐/布局(Floating Table)：<table align=#> #=left,right,center

12、单元格的背景色彩和背景图象 <td bgcolor=#> 、<td background=”URL”>

13、表格边框的色彩 <table bordercolor=#>

14、表格边框色彩的亮度控制

<table bordercolorlight=#>亮边框色彩

<table bordercolordark=#> 暗边框色彩

转载自[HERE](http://zmb.hbsz.cn/rjjc/ShowArticle.asp?ArticleID=410)
## Comments

**[cocobear](#1692 "2007-09-16 00:43:51"):** 楼上的怎么不给个你的链接呢?我也好去看看啊。

**[沸点](#1690 "2007-09-15 17:26:42"):** 谢谢哦！ 用在我的博客上了！

