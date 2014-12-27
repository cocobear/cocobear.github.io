title: Python学习笔记七
tags:
  - Python
id: 225
categories:
  - 编程相关
date: 2007-12-21 17:02:06
---

**import module与from module import funtion区别：**
import module导入模块后你需要使用module.function()来调用一个函数，而from module import function导入一个function后你可以直接使用它。

请在你经常要使用这个function或者你确认你的代码中不会与导入的function冲突时使用from module import function，不然请使用import module。