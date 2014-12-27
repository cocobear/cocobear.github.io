title: C语言中宏的技巧
id: 201
categories:
  - 编程相关
date: 2007-10-09 22:17:05
tags:
---

	#include <stdio .h>
	#include <limits .h>

	#define STR(s) #s
	#define CONS(a,b) (int)(a##e##b)

	int main(void)
	{
	        printf("int max %s\n",STR(INT_MAX));
	        printf("%d\n",CONS(2,3));
	        return 0;
	}

使用单个#号的时候，参数会变作为字符串被替换。
使用两个##号时，参数会连接在一起。

BTW：网上能找到的一篇被大量转载的文章（C语言宏定义技巧）中的代码有问题，上面的代码可以完全正常运行。</limits></stdio>
## Comments

**[luguo](#1944 "2007-10-10 14:08:48"):** ##不宜过分使用，你昨天看的代码可以说就属于滥用了～！ 另外，建议你看看我写的这篇论文： http://wangcong.org/down/lkllp-wangcong.pdf 里面有很多C语言方面的技巧～！

