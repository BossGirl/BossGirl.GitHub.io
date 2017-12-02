---
layout: post
title: "Misc-数据包相关"
date: 2017-09-14 14:58:44
description: "CTF"
tag: Misc-Data packet
---


## SCTF Misc300
[http://www.shiyanbar.com/ctf/720](http://www.shiyanbar.com/ctf/720)
在分组19中找到：

	LMHASH：9e94258a03356914b15929fa1d2e290fab9c8f9f01999448
	NTHASH：013f3cb06ba848f98a6ae6cb4a76477c5ba4e45cda73b475

## 0x01 DNS数据包

第一步，把所有的DNS流过滤出来，然后按显示顺序逐个取数值，数值在每个IP前面有显示（以十六进制进制显示，然而我之前也找到过这个数值，但是没有发现什么特征就放过了，后来才知道我发现的是第一串是纯数字，所以给放过了），把找到的十六进制字符串按顺序拼接起来。
第二步，HEX->ASCII->base64
![](http://ou0111n4v.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE20170801212315.png)
![](http://ou0111n4v.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE20170801212351.png)


## 0x02 HTTP数据包
这道题感觉有些坑...但是仔细一想倒也没毛病。关键是把数据隐藏到某个http流后面，在查看的时候发现了好多图片，但是没头绪，继续找，看到了
	
	GET /askldj3lkj234.php HTTP/1.1
有个php文件，打开在最下面看到了flag;真·坑

![](http://ou0111n4v.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE20170801210956.png)

## 0x01 A记录
嗯，评论说很好的一道题，对于我来说，emmm....

这是一个数据包分析题，用到了aircrack-ng，用这个工具做的题之前在iscc上遇到过，但是那个题因为工具有点问题没做出来。这次再来。

	1.用aircrack-ng检查cap包（命令行）：
	..>aircrack-ng.exe shipin.cap
![](http://ou0111n4v.bkt.clouddn.com/cap%E5%88%86%E6%9E%90.png)

	2.使用弱口令字典破解wpa加密,wordlist填入字典（GUI）：

![](http://ou0111n4v.bkt.clouddn.com/cap1.png)
![](http://ou0111n4v.bkt.clouddn.com/cap%E5%88%86%E6%9E%902.png)


	3.用密码解密cap,分离得到一个cap（命令行）：
	..>aircrack-ng.exe shipin.cap -e 0719 -p 88888888
![](http://ou0111n4v.bkt.clouddn.com/cap3.png)
	
	4.联系题目，A记录的第一条

![](http://ou0111n4v.bkt.clouddn.com/cap4.png)

	5.结合题目，网址就是flag。
getflag
	
## 0x02 抓到你了
这个题主要用到了wireshark来分析数据包。首先把下载得到的文件加后缀为zhua.cap,然后用wireshark打开，查找`icmp`

	Data: 2122232425262728292a2b2c2d2e2f30
提交getflag

>ICMP是（Internet Control Message Protocol）Internet控制报文协议。它是TCP/IP协议族的一个子协议，用于在IP主机、路由器之间传递控制消息。控制消息是指网络通不通、主机是否可达、路由是否可用等网络本身的消息。这些控制消息虽然并不传输用户数据，但是对于用户数据的传递起着重要的作用。

## NSCTF misc250
tag:pcapng数据包分析+爆破rar密码

首先用wireshark打开文件，导出文件，提取出一个txt和一个rar。
![](http://ou0111n4v.bkt.clouddn.com/%E6%95%B0%E6%8D%AE%E5%8C%85.png)

txt内容如下：

	<html>
	<head><tittle>KEY</tittle></head>
	<body>
	<p>密码是nsfocus+5位数字</p>
	<a href="./key.rar">key</a>
	</body>
	</html>
而rar文件名为key.rar，所以文本里面指的密码`focus+5位数字`是这个压缩包的。接下来破解压缩包。一开始没想到用字典，直接打开ARCHPR去破密码，然而密码有一部分已知字母一部分未知数字，不好设置范围；后来想到用字典，不得不说黑刀炒鸡好用好么！太让人感动了~它有个“在每个密码前插入字符串”功能，不要太适合~！用了25mins终于爆出密码~开森~


![](http://ou0111n4v.bkt.clouddn.com/rar%E5%AF%86%E7%A0%81.png)

ok，getflag
