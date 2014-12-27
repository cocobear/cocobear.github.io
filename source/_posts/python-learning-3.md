title: Python学习笔记三
id: 191
categories:
  - 编程相关
date: 2007-09-27 20:20:44
tags:
---

**Python中完整的if语句是**：
if condition:
&nbsp;&nbsp;&nbsp;&nbsp;XX
elif:
&nbsp;&nbsp;&nbsp;&nbsp;YY
else:
&nbsp;&nbsp;&nbsp;&nbsp;ZZ

**Python中没有switch语句，不过可以使用dictionary,或者if-elif-else来实现**。

**关于缩进：Python是以缩进来识别语句块的，例如：**

x = 1
if x:
    &nbsp;&nbsp;&nbsp;&nbsp;y = 0
    &nbsp;&nbsp;&nbsp;&nbsp;if y:
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print 'block2'
    &nbsp;&nbsp;&nbsp;&nbsp;print 'block1'
print 'block0'

上面这段程序的结果是：
block1
block0
但如果缩进改为这样：

x = 1
if x:
    &nbsp;&nbsp;&nbsp;&nbsp;y = 0
    &nbsp;&nbsp;&nbsp;&nbsp;if y:
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print 'block2'
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print 'block1'
print 'block0'
结果就变为了：
block0
**长字符串**
    print "a very loooooooooooooooooooooooong statement "\
        "test"
两行都使用双引号，第一行末使用反斜杠。

**关于while语句：**
while <test>:             # Loop test
    <statements1>         # Loop body
else:                     # Optional else
    <statements2>         # Run if didn't exit loop with break
else是可选的，而且只有在while中的循环没有进行时才执行。

</statements2></statements1></test>
## Comments

**[wind](#1825 "2007-09-28 09:23:07"):** 好玩，用缩进来标志程序块 这样程序员就得注意排版了，难怪python写出来的代码漂亮。

**[cocobear](#1826 "2007-09-28 10:19:26"):** 好玩 不好玩 看看你几年没写博客了

**[luguo](#1828 "2007-09-28 12:31:33"):** list/dictionory再加上lambda东东就无敌了啊～～

**[crazyfranc](#1831 "2007-09-28 19:34:18"):** 没switch？有case没，感觉和shell有很多相通之处。缩进识别语句块很好吗？

