title: Python与Lua中的尾部调用优化
tags:
  - Lua
  - Python
id: 595
categories:
  - Life
date: 2009-03-31 14:23:12
---


	>     function foo (n)
	>>       if n > 0 then return foo(n - 1) end
	>>     end
	> foo(100)
	> foo(1000)
	> foo(10000)
	> foo(100000)
	> foo(1000000)


如果函数最后一句是return g(...)这样的形式，Lua将会把这句解释为goto g(x)，因为这里除了对g函数调用，再没有别的事做，也不需要保存堆栈里调用函数的信息。因此上面即使调用很多次也没有出现堆栈溢出的问题，把上面的代码转换为Python:

	>>> def foo(n):
	...     if n>0: return foo(n-1)
	... 
	>>> foo(100)
	>>> foo(1000)
	...
	  File "<stdin>", line 2, in foo
	  File "</stdin><stdin>", line 2, in foo
	  File "</stdin><stdin>", line 2, in foo
	RuntimeError: maximum recursion depth exceeded

很快出就现了堆栈溢出的问题，在这里表现为达到了递归调用的最大限制。Python自身没有实现尾部调用优化，不过也可以通过实现一个decorator办法来实现:

	#!/usr/bin/env python2.4
	# This program shows off a python decorator(
	# which implements tail call optimization. It
	# does this by throwing an exception if it is 
	# it's own grandparent, and catching such 
	# exceptions to recall the stack.

	import sys

	class TailRecurseException:
	  def __init__(self, args, kwargs):
	    self.args = args
	    self.kwargs = kwargs

	def tail_call_optimized(g):
	  """
	  This function decorates a function with tail call
	  optimization. It does this by throwing an exception
	  if it is it's own grandparent, and catching such
	  exceptions to fake the tail call optimization.

	  This function fails if the decorated
	  function recurses in a non-tail context.
	  """
	  def func(*args, **kwargs):
	    f = sys._getframe()
	    if f.f_back and f.f_back.f_back 
	        and f.f_back.f_back.f_code == f.f_code:
	      raise TailRecurseException(args, kwargs)
	    else:
	      while 1:
	        try:
	          return g(*args, **kwargs)
	        except TailRecurseException, e:
	          args = e.args
	          kwargs = e.kwargs
	  func.__doc__ = g.__doc__
	  return func

	@tail_call_optimized
	def factorial(n, acc=1):
	  "calculate a factorial"
	  if n == 0:
	    return acc
	  return factorial(n-1, n*acc)

	print factorial(10000)
	# prints a big, big number,
	# but doesn't hit the recursion limit.

	@tail_call_optimized
	def fib(i, current = 0, next = 1):
	  if i == 0:
	    return current
	  else:
	    return fib(i - 1, next, current + next)

	print fib(10000)
	# also prints a big number,
	# but doesn't hit the recursion limit.

代码来自[http://code.activestate.com/recipes/474088/](http://code.activestate.com/recipes/474088/)

在这段代码里tail_call_optimized是一个decorator，在执行factorial函数前，这个decorator先执行，tail_call_optimized中通过sys._getframe()这个方法会返回一个frame对象，包含了堆栈顶部的信息，当发现当前调用是一个递归调用:
	  if f.f_back and f.f_back.f_back 
        and f.f_back.f_back.f_code == f.f_code:

则抛出一个异常，下面的代码则截获异常，继续执行，这样就避免了堆栈的使用，很巧妙的一种方式。

在python-dev的邮件列表里，有人曾经做过一个Python的尾调用优化的[补丁](http://mail.python.org/pipermail/python-dev/2004-July/046150.html)，不过Guido拒绝了这个补丁:
     I'm not interested in adding this to the official Python release.> 
     
     One reason is that if an exception happens in such a tail-recursive> 
     call, the stack trace will be confusing.> 
     
     Another reason is that I don't think it's a good idea to try to> 
     encourage a Scheme-ish "solve everything with recursion" programming> 
     style in Python.> 
     
     But feel free to maintain this as an independent modification, a la> 
     Stackless -- I'm sure there are people who would like to try this> 
     out.> 
     
     --Guido van Rossum (home page: http://www.python.org/~guido/)

</stdin>
## Comments

**[shellex](#5834 "2009-05-04 20:39:43"):** 唉。真是的。加入尾递归支持又不意味着大家都会do every thing with recursion... 多个选择多好...

**[shellex](#5835 "2009-05-04 20:40:03"):** 非得hack下。真是的。

**[xiongharry](#6624 "2009-10-21 09:25:07"):** 黑色主题，配深色字体，看起来真累。

**[Louis Vuitton Outlet San Diego California](#24622 "2013-05-17 19:36:03"):** Louis Authentic LV Outlet Handbags

