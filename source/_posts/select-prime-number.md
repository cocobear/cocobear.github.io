title: 筛选法求质数
tags:
  - C
  - Python
id: 358
categories:
  - 编程相关
date: 2008-11-18 16:59:49
---

在[这里](http://www.leninlee.cn/?p=774)看到用Lua和Python写的使用筛选法求质数的代码，俺自己也写了写，Python版用到了上面链接中一位兄弟的tips :-)

先来C语言版的: 

	#include <stdio .h>
	#include <math .h>

	#define NUM 2000000

	int main(void)
	{
	    int primes[NUM];
	    int i,j;
	    for (i=0;i<num ;i++) {
	        primes[i] = 1;

	    }
	    primes[0] = 0;
	    primes[1] = 0;
	    for (i=1;i<(long)sqrt(NUM)+1;i++) {
	        if (primes[i]) {
	            for (j=pow(i,2);j<NUM;j+=i) {
	                primes[j] = 0;
	            }
	        }
	    }

	    long sum = 0;
	    for (i=0;i<NUM;i++) {
	        if (primes[i]) sum+=i;
	    }
	    printf("%ld\n",sum);

	    return 0;
	}


计算两百万以内质数和大约0.1秒左右：
	[cocobear@cocobear wxpython]$ time ./a.out 
	142913828922

	real	0m0.100s
	user	0m0.088s
	sys	0m0.012s


 接着来可爱的Python版: 

	from math import sqrt
	NUM = 2000000
	prime_num = [i for i in xrange(NUM+1)]
	for i in xrange(2,int(sqrt(NUM))+1):
	    if prime_num[i]:
	        start = i**2
	        step  = i 
	        prime_num[start::step] = ( (NUM - start)/step + 1)*[0]
	print sum(prime_num)-1


执行时间1秒多点：

	[cocobear@cocobear wxpython]$ time python prime.py 
	142913828922

	real	0m1.204s
	user	0m1.051s
	sys	0m0.093s


Python不到10行的代码也有不错的效率:-)
以上测试平台为:

Fedora 9 AMD64 4600+ 4G

计算5的阶乘



	reduce(lambda x,y:x*y,range(1,5+1))


## Comments

**[vvoody](#4685 "2008-12-15 16:24:08"):** 学习了。 PS：目前配色对代码阅读不太好，python 那段方括号、圆括号都看不见了，全选才能看见。 我一开始没反应过来，想python语法什么时候变成这样了 ;-)

**[wind](#4621 "2008-11-20 16:26:45"):** 话说俺的数学还是很烂，知道这个算法应该可行，但是我没法子用数学来证明…… 还有，这个背景太黑，然后代码也不亮，看得我好累啊。

**[可可熊](#4620 "2008-11-19 10:14:32"):** 别人博客上看到的，就没事写着玩了。 老大俺还是很生气，送俺一个apple的本就不生气了。

**[Amankwah](#4618 "2008-11-18 23:35:09"):** 筛法，呵呵～

**[Amankwah](#4619 "2008-11-18 23:36:31"):** 我修改你的链接了，不要生气啊～

**[luguo](#4617 "2008-11-18 23:22:55"):** 呵呵，怎么想起写这个程序了？

**[可可熊](#4742 "2008-12-31 10:51:42"):** 配色改了，呵呵。

**[Tanky Woo](#8259 "2010-06-29 14:30:46"):** 写的很好。学习了。

