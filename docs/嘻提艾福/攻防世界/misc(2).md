---
title: Misc(2)
abbrlink: ec023bbe
description: 攻防世界Misc合集
# toc: true
---

> 加塞ing

## 看雪看雪看雪

压缩包打开有五个文件, 一个图片, 三个可以组成password的文件, 和一个flag  
图片木得东西, 按顺序把password打开得到`他朝若是同淋雪`  

回到图片上, 查看属性能看到一串hex`736e6f77212121`, 解码得到`snow!!`  
用010打开flag文件发现是一串文字,但带了大量空格  
结合上面的提示猜测是snow隐写, 把flag文件里的东西粘贴到单独的文件里用password解密即可  

`SNOW.EXE -C -p 他朝若是同淋雪 1.txt`
`flag{Sn0w_M@n!!!!!!!}`

> 另: 看官方wp才知道这题还涉及到了NTFS流隐写, 比如flag文件的名字叫做`misc.jpg:flag`  
> 其他四个文件都是misc.jpg的从文件流, 如果直接解压的话是看不到文件的  
> 虽然我是直接压缩包里打开的, 所以没有这个考察点  

