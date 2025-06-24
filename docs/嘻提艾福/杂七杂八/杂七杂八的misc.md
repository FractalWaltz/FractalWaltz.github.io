---
title: 杂七杂八的misc
date: 2025-05-07 16:16:58
description: 一些简单misc复现
---



# [CISCN 2022 东北]pixel

[[CISCN 2022 东北]pixel](https://www.nssctf.cn/problem/2399)

## 参考文章

[CISCN2022东北赛区复赛Writeup-MapleLeaves](https://blog.csdn.net/Do1phln/article/details/125356501)  
虽然解出来不是一个顺序

## 思路

主要是看群里师傅在聊到~~秒掉~~这题,就找到跟着做了  
下载附件`pixel.rar`,文件夹里一个加密压缩包`flag.rar`和一个`pass.txt`   
不知道能不能爆破这种套文件夹的,ARCHPR是没成    

打开`pass.txt`,根据提示把棋谱中的数字当做密码对压缩包解压,出来个`flag.dat`  
没有打开方式,扔到010editor里看一看感觉什么都不是  
用B神的PuzzleSolverのfile-format得到  
`类型: ZIP/APK/DOCX/XLSX/PPTX, 异或文件头: 50 4B 03 04, 异或: 0x9e, 文件头相似度: 1.0`  

也就是个zip,在010editor里tools->hex operations->binary xor->选择unsigned byte,填入9e,hex即可   
改成zip,发现需要解压密码,回到刚刚异或后的结果里,发现最底下有一串01二进制  
解密出来`You_Cannot_find_it`,总之扔进去试试能不能解密  
进去之后是flag.png,继续使用密码解密  
打开图片什么都没有,扔到010editor中可以看到相同的特征(即IDAT都是200ch)  
(一般第一次用010打开png会自动下载对应的脚本,没有去官网找下好了)  

同样的,扔到Stegslove里也能在b0和g0看到隐写特征(有黑点应该算是?)
按照旨意前往[pixeljihad](https://sekao.net/pixeljihad/)把图片扔进去,password就接着用之前得到的`You_Cannot_find_it`,得到flag  

flag `flag{3dba7e5c09b0da5182a373e3cbb99670}`

