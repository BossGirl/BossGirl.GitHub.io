---
layout: post
title: "day3 ouc_oj"
date:  2020-05-19 14:58:44
description: "day3 wp"
tag: 总结
---

## Web-Part

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
