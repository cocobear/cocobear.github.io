title: TinyP2P
tags:
  - Python
  - TinP2P
id: 258
categories:
  - 编程相关
date: 2008-04-13 11:22:56
---

TinyP2P使用15行Python代码实现了一个P2P的工具，下面是经过[别人](http://www.exonsoft.com/~kochin/TinyP2P/tinyp2p.html)注释的代码：

	# tinyp2p.py 1.0 (documentation at http://freedom-to-tinker.com/tinyp2p.html)
	# (C) 2004, E.W. Felten
	# license: http://creativecommons.org/licenses/by-nc-sa/2.0

	# Annotated by Kochin Chang, Jan. 2005

	# Usage:
	#   Server - python tinyp2p.py password server hostname  portnum [otherurl]
	#   Client - python tinyp2p.py password client serverurl pattern
	#                   ar[0]      ar[1]    ar[2]  ar[3]     ar[4]   ar[5]

	import sys, os, SimpleXMLRPCServer, xmlrpclib, re, hmac 
	# Import libraries used in the program.
	# sys : system variables and functions.
	# os : portable OS dependent functionalities.
	# SimpleXMLRPCServer : basic XML-RPC server framework.
	# xmlrpclib : XML-RPC client support.
	# re : regular expression support.
	# hmac : RFC 2104 Keyed-Hashing Message Authentication.

	ar,pw,res = (sys.argv,lambda u:hmac.new(sys.argv[1],u).hexdigest(),re.search)
	# A multiple assignment.
	# ar < - sys.argv : the argument list.
	# pw <- lambda u:hmac.new(sys.argv[1],u).hexdigest() :
	#   a function makes an HMAC digest from a URL.
	#   INPUT: a string, u, which is a URL here.
	#   OUTPUT: a hexdecimal HMAC digest.
	#   DESCRIPTION:
	#     1\. Creates a HMAC object from the URL using network's password,
	#        sys.argv[1], as the key.
	#     2\. Returns a hexdecimal digest of the HMAC object.
	# res <- re.search : alias for the regular expression search function.

	pxy,xs = (xmlrpclib.ServerProxy,SimpleXMLRPCServer.SimpleXMLRPCServer)
	# A multiple assignment.
	# pxy <- xmlrpclib.ServerProxy : alias for the ServerProxy class.
	# xs <- SimpleXMLRPCServer.SimpleXMLRPCServer : alias for the SimpleXMLRPCServer class.

	def ls(p=""):return filter(lambda n:(p=="")or res(p,n),os.listdir(os.getcwd()))
	# a function lists directory entries.
	# INPUT: a string, p, which is a regular expression pattern.
	# OUTPUT: a list of directory entries matched the pattern.
	# DESCRIPTION:
	#   1\. Creates a function using lambda expression that takes a pathname as its
	#      parameter. The function returns true if the pattern is empty or the
	#      pathname matches the pattern.
	#   2\. Finds out what is the current working directory.
	#   3\. Retrieves a list of directory entries of current working directory.
	#   4\. Filters the list using the lambda function defined.

	if ar[2]!="client":
	# Running in server mode...

	  myU,prs,srv = ("http://"+ar[3]+":"+ar[4], ar[5:],lambda x:x.serve_forever())
	  # A multiple assignment.
	  # myU <- "http://"+ar[3]+":"+ar[4] : server's own URL.
	  # prs <- ar[5:] : URL's of other servers in the network.
	  # srv <- lambda x:x.serve_forever() :
	  #   a function to start a SimpleXMLRPCServer.
	  #   INPUT: a SimpleXMLRPCServer object, x.
	  #   OUTPUT: (none)
	  #   DESCRIPTION:
	  #     Calls the server's serve_forever() method to start handling request.

	  def pr(x=[]): return ([(y in prs) or prs.append(y) for y in x] or 1) and prs
	  # a function returns the server list.
	  # INPUT: a list, x, of servers' URLs to be added to the server list.
	  # OUTPUT: the updated server list.
	  # DESCRIPTION:
	  #   1\. For each URL in x, checks whether it's already in the server list.
	  #      If it's not in the list, appends in onto the list.
	  #   2\. Returns the updated server list.

	  def c(n): return ((lambda f: (f.read(), f.close()))(file(n)))[0]
	  # a function returns content of the specified file.
	  # INPUT: a string, n, which is a filename.
	  # OUTPUT: the content of the file in a string.
	  # DESCRIPTION:
	  #   1\. Creates a function using lambda expression that takes a file object, f,
	  #      as its parameter. The function reads the content of the file, then
	  #      closes it. The results of the read and close are put into a tuple, and
	  #      the tuple is returned.
	  #   2\. Creates a file object with the filename. Passes it to the lambda
	  #      function.
	  #   3\. Retrieves and returns the first item returned from the lambda function.

	  f=lambda p,n,a:(p==pw(myU))and(((n==0)and pr(a))or((n==1)and [ls(a)])or c(a))
	  #   a request handling function, depending on the mode, returns server list,
	  #   directory entries, or content of a file.
	  #   INPUT: a string, p, which is a hexdecimal HMAC digest.
	  #          a mode number, n.
	  #          if n is 0, a is a list of servers to be added to server list.
	  #          if n is 1, a is a pattern string.
	  #          if n is anything else, a is a filename.
	  #   OUTPUT: if n is 0, returns the server list.
	  #           if n is 1, returns directory entries match the pattern.
	  #           if n is anything else, returns content of the file.
	  #   DESCRIPTION:
	  #     1\. Verifies the password by comparing the HMAC digest received and the
	  #        one created itself. Continues only when they match.
	  #     2\. If n is 0, calls pr() to add list, a, and returns the result.
	  #        If n is 1, calls ls() to list entries match pattern a, and returns
	  #        the result enclosed in a list.
	  #        If n is any other value, retreives and return content of the file
	  #        with filename specified in a.

	  def aug(u): return ((u==myU) and pr()) or pr(pxy(u).f(pw(u),0,pr([myU])))
	  # a function augments the network.
	  # INPUT: a string, u, which is a URL.
	  # OUTPUT: a list of URL's of servers in the network.
	  # DESCRIPTION:
	  #   1\. If the URL, u, equals to my own URL, just returns the server list.
	  #   2\. Otherwise, creates a ServerProxy object for server u. Then calls its
	  #      request handling function f with a HMAC digest, mode 0, and server
	  #      list with myself added.
	  #   3\. Calls pr() with the result returned from server u to add them to my
	  #      own list.
	  #   4\. Returns the new list.

	  pr() and [aug(s) for s in aug(pr()[0])]
	  # 1\. Checks the server list is not empty.
	  # 2\. Takes the first server on the list. Asks that server to augment its
	  #    server list with my URL.
	  # 3\. For each server on the returned list, asks it to add this server to its
	  #    list.

	  (lambda sv:sv.register_function(f,"f") or srv(sv))(xs((ar[3],int(ar[4]))))
	  # Starts request processing.
	  # 1\. Defines a function with lambda expression that takes a SimpleXMLRPCServer
	  #    object, registers request handling function, f, and starts the server.
	  # 2\. Creates a SimpleXMLRPCServer object using hostname (ar[3]) and portnum
	  #    (ar[4]). Then feeds the object to the lambda function.

	# Running in client mode...
	for url in pxy(ar[3]).f(pw(ar[3]),0,[]):
	# 1\. Create a ServerProxy object using the serverurl (ar[3]).
	# 2\. Calls the remote server and retrieves a server list.
	# 3\. For each URL on the list, do the following:

	  for fn in filter(lambda n:not n in ls(), (pxy(url).f(pw(url),1,ar[4]))[0]):
	  # 1\. Create a ServerProxy object using the URL.
	  # 2\. Calls the remote server to return a list of filenames matching the
	  #    pattern (ar[4]).
	  # 3\. For each filename doesn't exist locally, do the following:

	    (lambda fi:fi.write(pxy(url).f(pw(url),2,fn)) or fi.close())(file(fn,"wc"))
	    # 1\. Define a lambda function that takes a file object, calls remote server
	    #    for the file content, then closes the file.
	    # 2\. Create a file object in write and binary mode with the filename. (I
	    #    think the mode "wc" should be "wb".)
	    # 3\. Passes the file object to the lambda function.


如果直接运行上面的代码会有问题（不知道作者那里为什么会正常运行），会有类似这样的错误提示：

	Traceback (most recent call last):
	File "tinyp2p.py", line 14, in ?
	for url in pxy(ar[3]).f(pw(ar[3]),0,[]):
	File "/usr/lib/python2.4/xmlrpclib.py", line 1096, in __call__
	return self.__send(self.__name, args)
	File "/usr/lib/python2.4/xmlrpclib.py", line 1383, in __request
	verbose=self.__verbose
	File "/usr/lib/python2.4/xmlrpclib.py", line 1147, in request
	return self._parse_response(h.getfile(), sock)
	File "/usr/lib/python2.4/xmlrpclib.py", line 1286, in _parse_response
	return u.close()
	File "/usr/lib/python2.4/xmlrpclib.py", line 744, in close
	raise Fault(**self._stack[0])
	xmlrpclib.Fault: <Fault 1: 'exceptions.TypeError:coercing to Unicode:
	need string or buffer, list found'>

经google的帮助在[这里](http://bytes.com/forum/thread719667.html)找到的解决方案：

	pr() and [aug(s) for s in aug(pr()[0])]
	to
	pr() and [aug(s) for s in aug(pr()[0])] or pr([myU])


代码中大量应用了**lambda**，还有**and**,**or**，虽然这种代码风格不值得我们学习，我们可以写出更清昕的代码，但作者是为了证明P2P应用程序是如何容易实现，作者更希望可以把代码写的更简洁，可以用在他的签名里。

还有perl写的6行代码，我看了一下grep,cat这些都出来了，是shell版吧。
## Comments

**[luguo](#3099 "2008-04-14 13:47:25"):** 那6行Perl在哪里？？ Perl自己有内置的grep，也很nb。虽然没cat，但可以定义个子函数叫cat。

**[luguo](#3100 "2008-04-14 13:51:52"):** 还有，你最后那段Python不对啊！人家是说把上面那行换成下面那行，那个“to”明显不是Python源代码啊！

**[cocobear](#3103 "2008-04-15 19:55:34"):** http://ansuz.sooke.bc.ca/software/molester/molester-min 这里有一个，貌似我上次看到的不是这个。 最后那一段只是个说明，并不是python源代码。

**[luguo](#3095 "2008-04-13 18:38:25"):** lambda，map，reduce啊，都是很nb的！ 弱弱地问一下：p2p和xmlrpc有啥关系？？

**[cocobear](#3096 "2008-04-13 23:05:15"):** XMLRPC可以实现分布式处理，这样P2P可以使用多个Server，俺这样觉得 。

