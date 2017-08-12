---
layout: post
title: "利用Burpsuite修改http头"
date: 2017-08-11 14:58:44
description: "CTF"
tag: CTF
---

## 头有点大[实验吧]
[http://www.shiyanbar.com/ctf/29](http://www.shiyanbar.com/ctf/29 "头有点大")

题目如下：
![](http://ou0111n4v.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE20170811160055.png)
题目列了三个要求：
```
1.用.net framework 9.9框架；
2.地处英国；
3.使用IE浏览器
```
结合题目所知，本题考察对http(s)头的理解。
作以下修改：
![](http://ou0111n4v.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE20170811160804.png)
getflag~
> 知识点：.net framework 9.9 在请求头中表示—— .NET CLR 9.9