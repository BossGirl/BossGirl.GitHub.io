---
layout: post
title: "ISCC_2017部分题目WriteUp"
date: 2017-5-25 14:58:44
description: "ISCC2017部分题目WP"
tag: CTF
---


# Basic #
### Wheel Cipher ###
题目描述：身为二战时期的密码专家，你截获了通信员身上的一段密文、密钥序列和加密列表。你能看懂吗？

附件：	
    
    加密表：
    1:	< ZWAXJGDLUBVIQHKYPNTCRMOSFE <
    2:	< KPBELNACZDTRXMJQOYHGVSFUWI <
    3:	< BDMAIZVRNSJUWFHTEQGYXPLOCK <
    4:	< RPLNDVHGFCUKTEBSXQYIZMJWAO <
    5:	< IHFRLABEUOTSGJVDKCPMNZQWXY <
    6:	< AMKGHIWPNYCJBFZDRUSLOQXVET <
    7:	< GWTHSPYBXIZULVKMRAFDCEONJQ <
    8:	< NOZUTWDCVRJLXKISEFAPMYGHBQ <
    9:	< XPLTDSRFHENYVUBMCQWAOIKZGJ <
    10:	< UDNAJFBOWTGVRSCZQKELMXYIHP <
    11：	< MNBVCXZQWERTPOIUYALSKDJFHG <
    12：	< LVNCMXZPQOWEIURYTASBKJDFHG <
    13：	< JZQAWSXCDERFVBGTYHNUMKILOP <
    
    密钥为：2，3，7，5，13,12,9，1，8，10，4，11，6
    密文为：NFQKSEVOQOFNP
    
此题提示在题目上wheel cipher即杰弗逊转轮加密器（Jefferson wheel cipher，详情可见百度。
> 其使用方法：
> 进行秘密通信的双方必须各自拥有完全一样转轮加密器。当一方要把一段文字（不超过 36字）秘密通知身处异地的对方时，只需转动加密器上的各片转轮，使这段文字正好出现在同一行上，这时转轮上排列的其他25 行都是无意义的乱码；再把其中任意一行的乱码抄下来交给信使。信使并不知道这段乱码文字的意义，只负责把它送交对方。对方收到乱码信后，只需拿出自己保存的同样的装置，转动上面各片转轮，让其中一行的排列和这段乱码同处在一行上，然后再查看其他25 行上的内容，其中必然有一行显示出加密者要传达的信息，而其他行显示的都是乱码

所以我们得到如下字符：

    NACZDTRXMJQOYHGVSFUWIKPBEL
    FHTEQGYXPLOCKBDMAIZVRNSJUW
    QGWTHSPYBXIZULVKMRAFDCEONJ
    KCPMNZQWXYIHFRLABEUOTSGJVD
    SXCDERFVBGTYHNUMKILOPJZQAW
    EIURYTASBKJDFHGLVNCMXZPQOW
    VUBMCQWAOIKZGJXPLTDSRFHENY
    OSFEZWAXJGDLUBVIQHKYPNTCRM
    QNOZUTWDCVRJLXKISEFAPMYGHB
    OWTGVRSCZQKELMXYIHPUDNAJFB
    FCUKTEBSXQYIZMJWAORPLNDVHG
    NBVCXZQWERTPOIUYALSKDJFHGM
    PNYCJBFZDRUSLOQXVETAMKGHIW
观察可发现第19列字符为一句话：	

    flag：FIREINTHEHOLE


###公邮密码_100###
> 题目描述：小明的U盘中毒了。病毒把小明的文档压缩并加密。现在小明忘了公邮密码，然而记录密码的文档却被病毒压缩并加密，你能帮助小明解开吗？

###说我作弊，需要证据_100###
> 题目描述：X老师怀疑一些调皮的学生在一次自动化计算机测试中作
> 弊，他使用抓包工具捕获到了Alice和Bob的通信流量。狡猾的Alice
> 和Bob同学好像使用某些加密方式隐藏通信内容，使得X老师无法破解
> 它，也许你有办法帮助X老师。X老师知道Alice的RSA密钥为
> (n, e) = (0x53a121a11e36d7a84dde3f5d73cf, 0x10001) 
> (192.168.0.13)?,Bob的RSA密钥为
> (n, e) =(0x99122e61dc7bede74711185598c7, 0x10001) 
> (192.168.0.37)

解题链接：[https://www.honoki.net/2015/10/hack-lu-2015-creative-cheating/#more-482](https://www.honoki.net/2015/10/hack-lu-2015-creative-cheating/#more-482)

###你猜猜。。_100###

> 描述：我们刚刚拦截了，敌军的文件传输获取一份机密文件，请君速速破解。


    附件：504B03040A0001080000626D0A49F4B5091F1E0000001200000008000000666C61672E7478746C9F170D35D0A45826A03E161FB96870EDDFC7C89A11862F9199B4CD78E7504B01023F000A0001080000626D0A49F4B5091F1E00000012000000080024000000000000002000000000000000666C61672E7478740A0020000000000001001800AF150210CAF2D1015CAEAA05CAF2D1015CAEAA05CAF2D101504B050600000000010001005A000000440000000000

解题步骤：hex转ascii发现里面存有压缩文件flag.txt，十六进制保存文件.zip得到加密的文件，爆破密码得txt，再用base64解密得

	flag：daczcasdqwdcsdzasd
###神秘图片_100###


> 题目描述：小明最近参加一个叫共济会的社团，社长一天神秘失踪，在社长电脑桌面上同学们发现一张奇怪的照片，为找到社长，社员们正在努力解密这张照片，可是一直找不到答案，你们发现神秘蛛丝马迹吗?

解题思路：kali下binwalk分离文件，得到一张图片，猪圈密码呗

###告诉你个秘密_100###
> 题目描述：简单加密
    附件：
    636A56355279427363446C4A49454A7154534230526D6843
    56445A31614342354E326C4B4946467A5769426961453067
	
	解题步骤：
    636A56355279427363446C4A49454A7154534230526D6843
    Y2pWNVJ5QnNjRGxKSUVKcVRTQjBSbWhD  //hex转base64
    r5yG lp9I BjM tFhB                //base64*2
    t	 o    n   g					  //低头看键盘
    
    56445A31614342354E326C4B4946467A5769426961453067
    VDZ1aCB5N2lKIFFzWiBiaE0g   //hex转base64
    T6uh y7iJ QsZ bhM          //base64*2
    y    u    a   n            //低头看键盘
    
    flag：tongyuan
###PHP_encrypt_1_150###
> 题目描述：大黑阔在某数据库中提取到了管理员的密码，但是密码是加密的，本要放弃的黑阔突然发现加密竟然是可逆的，网页上的脚本被黑阔提取出来了，你能够帮助黑阔解密吗？黑阔感激不尽。
> 加密数据：fR4aHWwuFCYYVydFRxMqHhhCKBseH1dbFygrRxIWJ1UYFhotFjA=

> 附件：Basic-09

###二维码_150###
> 题目描述：这是一个二维码

> 附件：Basic-07
#Misc#
###眼见非实_100###
>题目描述：眼见非实

>附件：Misc-02
###就在其中_150###
> 题目描述：啊。我好像捕获到了什么不得了的东西。
> 
> 附件：Misc-03
###很普通的Disco_250###
> 题目描述：普通的DISCO我们普通的摇~~~~

> 附件:Misc-04

###很普通的数独_300###
> 没那么简单~就能做出~数独的答案~尤其是在~看过了那么多的题目~ 

>附件:Misc-05

###再见李华_200###
> 假如你是李华（LiHua)，收到乔帮主一封密信，没有任何特殊字符，请输入密码，不少于1000个字。同学，记得署名哦～

> 附件：Misc-06

###寻"文"启事_400###
> 众里寻他千百度，蓦然回首，那人却在，灯火阑珊处。

> 附件：Misc-07

