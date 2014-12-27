title: 如何使用自动代理脚本
tags:
  - script
id: 6
categories:
  - 互联网
date: 2007-04-06 19:16:45
---

当你在教育网的时候如果想访问外网，唯一的办法就是用代理，但是如果直接在浏览器里设置了代理，那么所有的访问就都要通过代理，就连教育网的一些站点也要通过代理了，这样做对网络资料是一种浪费，自动代理脚本就是实现了访问外网的时候自动使用代理，而访问教育网的时候就直接连接。这样有效的提高了访问的效率。

当你在教育网的时候如果想访问外网，唯一的办法就是用代理，但是如果直接在浏览器里设置了代理，那么所有的访问就都要通过代理，就连教育网的一些站点也要通过代理了，这样做对网络资料是一种浪费，自动代理脚本就是实现了访问外网的时候自动使用代理，而访问教育网的时候就直接连接。这样有效的提高了访问的效率。

下面是代理脚本，你可以复制在文本编辑器里，然后保存为proxy.pac，在浏览器里设置使用代理-->使用自动代理。

DIRECT为不需要使用代理直接连接的网址.

	/* ================================================

	注意:
	1.在使用前必须正确设置代理地址xxx,xxx,xxx,xxx:xxx
	2.可针对自身作出修改,如直通网址,IP段等
	================================================ */
	function FindProxyForURL(url,host)
	{
	if(isPlainHostName(host)) return "DIRECT";

	//设置不经过代理直通网站网址,可自己按格式添加
	else if(
	(shExpMatch(host,"localhost"))
	|| (shExpMatch(host,"*.edu.cn"))
	) return "DIRECT";

	//CERNET分担费用政策中定义的国内流量地址列表-主表[20061015]
	else if(isInNet(host,"58.17.0.0","255.255.0.0")) return "DIRECT";
	else if(isInNet(host,"58.24.0.0","255.254.0.0")) return "DIRECT";
	else if(isInNet(host,"58.154.0.0","255.254.0.0")) return "DIRECT";
	else if(isInNet(host,"58.192.0.0","255.240.0.0")) return "DIRECT";
	else if(isInNet(host,"58.240.0.0","255.254.0.0")) return "DIRECT";
	else if(isInNet(host,"59.32.0.0","255.240.0.0")) return "DIRECT";
	else if(isInNet(host,"59.64.0.0","255.240.0.0")) return "DIRECT";
	else if(isInNet(host,"60.0.0.0","255.224.0.0")) return "DIRECT";
	else if(isInNet(host,"60.63.0.0","255.255.0.0")) return "DIRECT";
	else if(isInNet(host,"61.28.0.0","255.255.240.0")) return "DIRECT";
	else if(isInNet(host,"61.48.0.0","255.248.0.0")) return "DIRECT";
	else if(isInNet(host,"61.128.0.0","255.192.0.0")) return "DIRECT";
	else if(isInNet(host,"61.232.0.0","255.252.0.0")) return "DIRECT";
	else if(isInNet(host,"61.236.0.0","255.254.0.0")) return "DIRECT";
	else if(isInNet(host,"61.240.0.0","255.252.0.0")) return "DIRECT";
	else if(isInNet(host,"121.48.0.0","255.254.0.0")) return "DIRECT";
	else if(isInNet(host,"121.192.0.0","255.252.0.0")) return "DIRECT";
	else if(isInNet(host,"121.248.0.0","255.252.0.0")) return "DIRECT";
	else if(isInNet(host,"125.216.0.0","255.248.0.0")) return "DIRECT";
	else if(isInNet(host,"137.189.0.0","255.255.0.0")) return "DIRECT";
	else if(isInNet(host,"140.113.0.0","255.255.0.0")) return "DIRECT";
	else if(isInNet(host,"143.89.0.0","255.255.0.0")) return "DIRECT";
	else if(isInNet(host,"144.214.0.0","255.255.0.0")) return "DIRECT";
	else if(isInNet(host,"147.8.0.0","255.255.0.0")) return "DIRECT";
	else if(isInNet(host,"152.101.0.0","255.255.0.0")) return "DIRECT";
	else if(isInNet(host,"152.104.0.0","255.255.0.0")) return "DIRECT";
	else if(isInNet(host,"158.132.0.0","255.255.0.0")) return "DIRECT";
	else if(isInNet(host,"158.182.0.0","255.255.0.0")) return "DIRECT";
	else if(isInNet(host,"159.226.0.0","255.255.0.0")) return "DIRECT";
	else if(isInNet(host,"161.207.0.0","255.255.0.0")) return "DIRECT";
	else if(isInNet(host,"162.105.0.0","255.255.0.0")) return "DIRECT";
	else if(isInNet(host,"166.111.0.0","255.255.0.0")) return "DIRECT";
	else if(isInNet(host,"167.139.0.0","255.255.0.0")) return "DIRECT";
	else if(isInNet(host,"168.160.0.0","255.255.0.0")) return "DIRECT";
	else if(isInNet(host,"192.86.104.0","255.255.255.0")) return "DIRECT";
	else if(isInNet(host,"202.4.128.0","255.255.224.0")) return "DIRECT";
	else if(isInNet(host,"202.38.0.0","255.255.0.0")) return "DIRECT";
	else if(isInNet(host,"202.40.192.0","255.255.224.0")) return "DIRECT";
	else if(isInNet(host,"202.45.32.0","255.255.224.0")) return "DIRECT";
	else if(isInNet(host,"202.75.64.0","255.255.224.0")) return "DIRECT";
	else if(isInNet(host,"202.84.16.0","255.255.254.0")) return "DIRECT";
	else if(isInNet(host,"202.95.0.0","255.255.224.0")) return "DIRECT";
	else if(isInNet(host,"202.96.0.0","255.240.0.0")) return "DIRECT";
	else if(isInNet(host,"202.112.0.0","255.248.0.0")) return "DIRECT";
	else if(isInNet(host,"202.120.0.0","255.254.0.0")) return "DIRECT";
	else if(isInNet(host,"202.122.32.0","255.255.240.0")) return "DIRECT";
	else if(isInNet(host,"202.127.0.0","255.255.192.0")) return "DIRECT";
	else if(isInNet(host,"202.127.128.0","255.255.128.0")) return "DIRECT";
	else if(isInNet(host,"202.130.0.0","255.255.224.0")) return "DIRECT";
	else if(isInNet(host,"202.130.224.0","255.255.224.0")) return "DIRECT";
	else if(isInNet(host,"202.131.208.0","255.255.240.0")) return "DIRECT";
	else if(isInNet(host,"202.189.96.0","255.255.224.0")) return "DIRECT";
	else if(isInNet(host,"202.192.0.0","255.240.0.0")) return "DIRECT";
	else if(isInNet(host,"203.81.16.0","255.255.240.0")) return "DIRECT";
	else if(isInNet(host,"203.87.224.0","255.255.224.0")) return "DIRECT";
	else if(isInNet(host,"203.93.0.0","255.255.0.0")) return "DIRECT";
	else if(isInNet(host,"203.128.128.0","255.255.224.0")) return "DIRECT";
	else if(isInNet(host,"203.192.0.0","255.255.224.0")) return "DIRECT";
	else if(isInNet(host,"203.207.64.0","255.255.192.0")) return "DIRECT";
	else if(isInNet(host,"203.207.128.0","255.255.128.0")) return "DIRECT";
	else if(isInNet(host,"203.208.0.0","255.255.224.0")) return "DIRECT";
	else if(isInNet(host,"203.212.0.0","255.255.240.0")) return "DIRECT";
	else if(isInNet(host,"210.5.0.0","255.255.224.0")) return "DIRECT";
	else if(isInNet(host,"210.12.0.0","255.254.0.0")) return "DIRECT";
	else if(isInNet(host,"210.14.160.0","255.255.224.0")) return "DIRECT";
	else if(isInNet(host,"210.14.192.0","255.255.192.0")) return "DIRECT";
	else if(isInNet(host,"210.15.0.0","255.255.128.0")) return "DIRECT";
	else if(isInNet(host,"210.15.128.0","255.255.192.0")) return "DIRECT";
	else if(isInNet(host,"210.21.0.0","255.255.0.0")) return "DIRECT";
	else if(isInNet(host,"210.22.0.0","255.255.0.0")) return "DIRECT";
	else if(isInNet(host,"210.25.0.0","255.255.128.0")) return "DIRECT";
	else if(isInNet(host,"210.25.128.0","255.255.192.0")) return "DIRECT";
	else if(isInNet(host,"210.26.0.0","255.254.0.0")) return "DIRECT";
	else if(isInNet(host,"210.28.0.0","255.252.0.0")) return "DIRECT";
	else if(isInNet(host,"210.32.0.0","255.240.0.0")) return "DIRECT";
	else if(isInNet(host,"210.51.0.0","255.255.0.0")) return "DIRECT";
	else if(isInNet(host,"210.52.0.0","255.254.0.0")) return "DIRECT";
	else if(isInNet(host,"210.72.0.0","255.252.0.0")) return "DIRECT";
	else if(isInNet(host,"210.76.0.0","255.254.0.0")) return "DIRECT";
	else if(isInNet(host,"210.78.0.0","255.255.0.0")) return "DIRECT";
	else if(isInNet(host,"210.79.224.0","255.255.224.0")) return "DIRECT";
	else if(isInNet(host,"210.82.0.0","255.254.0.0")) return "DIRECT";
	else if(isInNet(host,"210.192.96.0","255.255.224.0")) return "DIRECT";
	else if(isInNet(host,"211.64.0.0","255.248.0.0")) return "DIRECT";
	else if(isInNet(host,"211.80.0.0","255.240.0.0")) return "DIRECT";
	else if(isInNet(host,"211.96.0.0","255.248.0.0")) return "DIRECT";
	else if(isInNet(host,"211.136.0.0","255.248.0.0")) return "DIRECT";
	else if(isInNet(host,"211.144.0.0","255.240.0.0")) return "DIRECT";
	else if(isInNet(host,"211.160.0.0","255.248.0.0")) return "DIRECT";
	else if(isInNet(host,"218.0.0.0","255.224.0.0")) return "DIRECT";
	else if(isInNet(host,"218.56.0.0","255.248.0.0")) return "DIRECT";
	else if(isInNet(host,"218.64.0.0","255.224.0.0")) return "DIRECT";
	else if(isInNet(host,"218.96.0.0","255.252.0.0")) return "DIRECT";
	else if(isInNet(host,"218.104.0.0","255.252.0.0")) return "DIRECT";
	else if(isInNet(host,"218.108.0.0","255.255.0.0")) return "DIRECT";
	else if(isInNet(host,"218.192.0.0","255.240.0.0")) return "DIRECT";
	else if(isInNet(host,"218.240.0.0","255.248.0.0")) return "DIRECT";
	else if(isInNet(host,"219.72.0.0","255.255.0.0")) return "DIRECT";
	else if(isInNet(host,"219.128.0.0","255.224.0.0")) return "DIRECT";
	else if(isInNet(host,"219.216.0.0","255.248.0.0")) return "DIRECT";
	else if(isInNet(host,"219.224.0.0","255.240.0.0")) return "DIRECT";
	else if(isInNet(host,"219.242.0.0","255.254.0.0")) return "DIRECT";
	else if(isInNet(host,"219.244.0.0","255.252.0.0")) return "DIRECT";
	else if(isInNet(host,"220.160.0.0","255.224.0.0")) return "DIRECT";
	else if(isInNet(host,"220.192.0.0","255.240.0.0")) return "DIRECT";
	else if(isInNet(host,"220.234.0.0","255.255.0.0")) return "DIRECT";
	else if(isInNet(host,"220.248.0.0","255.252.0.0")) return "DIRECT";
	else if(isInNet(host,"220.252.0.0","255.255.0.0")) return "DIRECT";
	else if(isInNet(host,"221.0.0.0","255.240.0.0")) return "DIRECT";
	else if(isInNet(host,"221.130.0.0","255.254.0.0")) return "DIRECT";
	else if(isInNet(host,"221.137.0.0","255.255.0.0")) return "DIRECT";
	else if(isInNet(host,"221.172.0.0","255.252.0.0")) return "DIRECT";
	else if(isInNet(host,"221.192.0.0","255.248.0.0")) return "DIRECT";
	else if(isInNet(host,"221.200.0.0","255.252.0.0")) return "DIRECT";
	else if(isInNet(host,"221.204.0.0","255.254.0.0")) return "DIRECT";
	else if(isInNet(host,"221.208.0.0","255.252.0.0")) return "DIRECT";
	else if(isInNet(host,"221.212.0.0","255.255.0.0")) return "DIRECT";
	else if(isInNet(host,"221.214.0.0","255.254.0.0")) return "DIRECT";
	else if(isInNet(host,"221.216.0.0","255.248.0.0")) return "DIRECT";
	else if(isInNet(host,"221.224.0.0","255.240.0.0")) return "DIRECT";
	else if(isInNet(host,"222.16.0.0","255.240.0.0")) return "DIRECT";
	else if(isInNet(host,"222.32.0.0","255.224.0.0")) return "DIRECT";
	else if(isInNet(host,"222.64.0.0","255.224.0.0")) return "DIRECT";
	else if(isInNet(host,"222.132.0.0","255.252.0.0")) return "DIRECT";
	else if(isInNet(host,"222.136.0.0","255.248.0.0")) return "DIRECT";
	else if(isInNet(host,"222.160.0.0","255.252.0.0")) return "DIRECT";
	else if(isInNet(host,"222.168.0.0","255.248.0.0")) return "DIRECT";
	else if(isInNet(host,"222.176.0.0","255.240.0.0")) return "DIRECT";
	else if(isInNet(host,"222.192.0.0","255.240.0.0")) return "DIRECT";
	else if(isInNet(host,"222.208.0.0","255.248.0.0")) return "DIRECT";
	else if(isInNet(host,"222.216.0.0","255.254.0.0")) return "DIRECT";
	else if(isInNet(host,"222.218.0.0","255.255.0.0")) return "DIRECT";
	else if(isInNet(host,"222.222.0.0","255.254.0.0")) return "DIRECT";
	else if(isInNet(host,"222.240.0.0","255.248.0.0")) return "DIRECT";

	//不经过代理Google附表
	else if(isInNet(host,"64.233.160.0","255.255.224.0")) return "DIRECT";
	else if(isInNet(host,"66.102.0.0","255.255.240.0")) return "DIRECT";
	else if(isInNet(host,"66.249.64.0","255.255.224.0")) return "DIRECT";
	else if(isInNet(host,"72.14.192.0","255.255.192.0")) return "DIRECT";
	else if(isInNet(host,"209.85.128.0","255.255.128.0")) return "DIRECT";
	else if(isInNet(host,"216.239.32.0","255.255.224.0")) return "DIRECT";

	//设置代理地址"xxx.xxx.xxx.xxx:xxx"
	else return "PROXY 222.24.19.25:80";
	}

上面这个代理是邮电学院的，需要是本校的IP才能使用的。