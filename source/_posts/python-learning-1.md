title: Python学习笔记一
id: 183
categories:
  - 编程相关
date: 2007-09-20 22:23:10
tags:
---

**数据类型：**

* Python字符串很灵活，不像C语言有很多限制，做个简单的字符串函数也需要很多代码，举个简单的例子：

  ```python
  >>>hacker = "hacker"	
  >>>"hack" in myjob	
  True
  
  ```





很简单就可以测试某个字符串中是否包含另一个字符串。

*   而分隔字符串就更简单了：(图片是分隔的字符串的解释)

```python
> >>>s = "sliceofspam"	> 
> >>> s[-4:]	> 
> 'spam'	> 
> >>> s[7:]	> 
> 'spam'	> 
> >>> s[7:11]	> 
> 'spam'
```


![python_string](https://asset-1258390188.cos.ap-shanghai.myqcloud.com/python_string1.bmp)


* 字符串是不可变的数据类型，不可以使用下面的赋值：

  > >>> s = "cocobear"	> 
  > >>> s[0] = s*   字符串与列表的相互连接：
## Comments

**[dream](#1786 "2007-09-24 14:45:54"):** 我有个Python short reference card, 要不要？ :)

**[Amankwah](#1787 "2007-09-24 18:35:14"):** 这个好，娃好好学python，到时候交我，哈哈

**[cocobear](#1792 "2007-09-24 19:06:31"):** TO:dream 是电子书吗？如果不太大的话给我发一份，呵呵。

**[luguo](#1763 "2007-09-21 13:38:49"):** Python的list是很有用的～～ p.s. 你那是用的Learning Python里的图片？

**[cocobear](#1766 "2007-09-21 22:49:36"):** 嗯，就是那里面的图片，被我加了个淡蓝色的框:-)

**[kongove](#3442 "2008-06-23 18:13:41"):** ”hack” in myjob 应该为 ”hack” in hacker

