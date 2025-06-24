---
title: Misc(1)
abbrlink: ec023bbe
description: 攻防世界Misc合集
# toc: true
---

> 这里是攻防世界Misc合集,记录自己的结题思路和总结  


## Misc文件类型

下载下来里面是一大堆数字和字母,猜测是hex编码,丢进厨子里解出来一个`46ESAB_UEsDBBQAAAAIAAldC...`  
开头是Base64的反写,于是把后面一大串再丢进b64解码  

到这里厨子已经有魔法棒了,总之是PK开头的特征,直接unzip或者再转成16进制手动解压即可

`flag{0bec0ad3da2113c70e50fd5617b8e7f9}`

## Banmabanma

[在线扫描](https://online-barcode-reader.inliteresearch.com/)一维码

`TENSHINE`

## 适合作为桌面

下载下来是一个图片,拉进stegsolve中可以看到在Red plane 1里隐藏了个二维码  
在线扫描后可以得到一串数字,这里卡主了去看了wp  

总之观察开头`03 F3 0D 0A`猜测是pyc的16进制,丢进010里另存为就好了(010复制16进制需要ctrl+shift+v...)  

这里需要用pycdc来反编译,uncompyle6出来的结果貌似是错的  

```python
def flag():
    str = [102, 108, 97, 103, 123, 51, 56, 97, 53, 55,
           48, 51, 50, 48, 56, 53, 52, 52, 49, 101, 55, 125]
    flag = ''
    for i in str:
        flag += chr(i)

print flag
```
修改下py的格式运行即可  
`flag{38a57032085441e7}`

## 心仪的公司

流量分析题,~~但先去看wp了~~  

一种解法是直接Strings搜`{}`或`f14g`之类的  

另一种是根据题目的文件名`webshell.pcapng`,尝试搜索webshell字段`tcp/http contains "webshell"`  
这样就只出来几个流了, ~~可以直接在其中一个里找到flag,~~ 有一个post流可疑,追踪一下拉到最底下可以看到flag  

`fl4g:{ftop_Is_waiting_4y}`


## pure_color

stegsolve拉到Blue plane 0即可看到flag  

`flag{true_steganographers_doesnt_need_any_tools}`

> 但没看懂这个flag,"真正的隐写玩家不需要任何工具"  
> 看了下wp,用画图打开切换一下黑白就好了,是这个意思吗  
> 还是得多学隐写 

## 2017_Dating_in_Singapore

脑洞题,等我CCBC大成归来再解 

找了个[wp](https://github.com/cmacckk/CTFWriteUp/blob/main/xctf/misc/2017_Dating_in_Singapore.md),总之是按`-`分割开来(正好是12个)  
然后再两两一组作为日期,每一排作为月份,以此类推  
最后再把每个月里的点按顺序连起来正好是flag的样式  

`HITB{CTFFUN}`

## simple_transfer

流量分析(?)  
总之是练手,还是看了wp  

依据提示在wireshark里使用分组字节流尝试搜索flag,我一开始是瞪眼法找了个可疑的流  
然后翻一下对应的流里,会发现一个file.pdf的字段,猜测流量里隐写了一个pdf  

也可以直接用binwalk,就可以发现有隐藏pdf,导出后可以在一个文件里找到flag信息,但需要手动填补一下     
或者用[foremost](https://github.com/raddyfiy/foremost)导出pdf  
但导出的pdf在Chrome里直接打开是全黑的,在vscode里打开又能直接看到flag,很迷  

有空再钻研具体原理了  

`HITB{b3d0e380e9c39352c667307d010775ca}`


## Training-Stegano-1

010打开能看到最后有个`Look what the hex-edit revealed: passwd:steganoI`  
一开始还以为是加密,最后发现直接把password当flag就好了  

`steganoI`

## can_has_stdio?

一个奇怪的文件,010打开是个五角星(笑死)  
总之是BF代码,找个在线的丢进去就好了  

`flag{esolangs_for_fun_and_profit}`

## Erik-Baleog-and-Olaf

打开是个奇怪文件,拖到010里发现是个png,拉到最底下有一个指向图床的link  
但下载下来之后发现图片差不多,也没有其他东西了 ~~,于是去看了wp~~   

把题目附件放到stegsolve里点几下可以看到中心有个二维码,可以手动截出来修补下扫出flag  

也可以用图床的原图片与附件进行比较,保留不同的部分即可得到二维码  
用stegsolve自带的compare要选择对应的plane才能得到二维码  
或者自己写个像素对比的脚本  

```python
if img1.getpixel((i, j)) == img2.getpixel((i, j)):
  pass
else:
  new1.putpixel((i, j), img1.getpixel((i, j)))
```

~~但我去了都扫不出来怎么回事呢~~  

`flag{`#`justdiffit}`  



> 搜了下wp说是用https://www.imagemagick.org/script/compare.php  
> 等哪天自己想整合进小工具再试试  

## János-the-Ripper

看名字可知这是要用到[John the ripper](https://www.openwall.com/john/)的  
总之打开压缩包,里面是另一个现在可以直接当压缩包打开的文件,再里面就是需要密码的flag.txt  
随便找个爆破都行了,密码是fish

`flag{ev3n::y0u::bru7us?!}`

## Test-flag-please-ignore

直接解压,010打开文件,是16进制转字符串

`flag{hello_world}`

## hong

音频题(吗)  
直接打不开,010也看不出啥  
最后用foremost提取了下出来了jpg,用binwalk也行  

`BCTF{cute&fat_cats_does_not_like_drinking}`

## misc_pic_again

附件是个图片,010打开没啥东西,stegsolve看gray bits发现有隐写特征  
然后就是看lsb隐写,选RGB的0位和lsb模式后可以看到是个pk开头的zip,另存为打开  

里面是一个文件,接着010打开瞪眼法找到flag

`hctf{scxdc3tok3yb0ard4g41n~~~}`

> windows不会zsteg喵

## hit_the_core

core是linux的文件,所以windows看不了.jpg  

用strings或者010瞪眼法可以看到一串可疑字符串  
`cvqAeqacLtqazEigwiXobxrCrtuiTzahfFreqc{bnjrKwgk83kgd43j85ePgb_e_rwqr7fvbmHjklo3tews_hmkogooyf0vbnk0ii87Drfgh_n kiwutfb0ghk9ro987k5tfb_hjiouo087ptfcv}`  

~~才发现strings已经集成在git里了,干脆顺手把foremost丢进去~~  
卡壳了,看wp说是每隔4位提取出来就是flag  
不过解出来有标准的头,靶场没给提示,倒是也能看出来头全是大写  

`ALEXCTF{K33P_7H3_g00D_w0rk_up}`  

## glance-50

下载下来是个gif,中间有飞快的黑点,猜测是每一帧拼接起来是flag  
顺手搜个脚本正好是这题的,于是自己抄了个  

```python 
im = Image.open(filepath)
try:
    while (1):
        new_im.paste(im, (length, 0, length+im.width, height))
        im.seek(im.tell()+1)  
        length += im.width

except EOFError:
    pass
```

`TWCTF{Bliss by Charles O'Rear}`

> wp提到有个在线网址(https://tu.sioe.cn/gj/fenjie/)

## normal_png

010/foremost/stegsolve无果后遂摆爛  
最后是宽高爆破,找个[脚本](https://github.com/AabyssZG/Deformed-Image-Restorer)就好了  


`flag{B8B68DD7007B1E406F3DF624440D31E0}`


## Aesop_secret

下载下来是个gif,先010一把梭到最后发现有串字符,尝试无果后结合题目名猜测是AES加密  
回来看gif,有闪动的痕迹,把每一帧拼接覆盖后能得到一张写了ISCC的图片,猜测是秘钥  

[在线解密](https://tools.wujingquan.com/aesencrypt/)两次即可  

`flag{DugUpADiamondADeepDarkMine}`


> 很迷,尝试用ISCC作为秘钥在厨子里加密了,结果永远不一样   
> 稍微查了下,赛博厨子的AES相关是用`node forge`处理的  
> 而本题wp提到的加密网站都是用的`cryptojs`  
> 之后再写一写问题  
> [脚本之家](https://tools.jb51.net/static/devtools/js/crypto-js/aes.js)  
> [sojson](https://scdn.sojson.com/ui/js/common/CryptoJS/rollups/aes.js)  
> 站长工具和随波逐流也是用的cryptojs v3.0.2  
> https://github.com/digitalbazaar/forge/issues/134


## a_good_idea

打开压缩包是张图片,010打开瞪眼法发现里面有PK塞了一个zip   
当然做的时候直接双击打开zip了,现在的压缩软件已经很高级了  

里面是两张图,和前面的alof一样是对比,直接用前面的脚本得到一个二维码,扫描得到flag

`NCTF{m1sc_1s_very_funny!!!}`


## Ditf

附件是一张图片,下载下来010一把梭发现塞了个rar,分离出来是个流量文件但是要密码  
爆破了一会无果后回去看图片,crc梭一下宽高得到密码  
打开流量文件,分析下发现有个get png的流量,追踪发现里面存在一个html文件,塞了个看起来是加密了的数据  
丢进厨子里b64一下得到flag

~~看了一堆wp也没懂怎么灵光一闪的~~

`flag{Oz_4nd_Hir0_lov3_For3ver}`

## miss_01

> 已过时,因为新佛曰寄了
> 所以出题人少从旮旯角落找只能在线解密的吧()  


下载下来是一个压缩包,里面两个文件`omisc.docx`和`fun.zip`   
fun.zip里是一个要密码的音频,于是先尝试打开docx发现提示头部密码不对  

猜测是伪加密,去010里吧omisc对应的frflag和deflag都改成00,然后就能解压了    
打开是一串字符串,~~然后没头猪了,转身向wp走去~~    

参考[Lunatic佬的文章](https://goodlunatic.github.io/posts/1ad9200/)可知,这一串字符串是Rabbit加密(开头`U2FsdGVkX1`)  
但是需要秘钥,在设置里把隐藏字符打开可以看到多出来了一行字符和一个网址,同上可知这是希尔密码  
不过直接复制不太行,改个字体就好了  

在线解密后得到秘钥,再把Rabbit给解了,得到音频包的密码  

> 希尔加密也是一搜一个奇怪的,有只支持四位的,有支持整个字母表的  
> 感觉有时间自己手搓一个会好点  

这题用到的解密网站有`https://zh.planetcalc.com/3327/#` `https://demo2.yunser.com/crypto/hill/`  

然后rabbit解密也是用赛博厨子解不了,得找个用CryptoJS的解  

> 参考文章https://blog.csdn.net/m0_64444909/article/details/123728264  
> 总之这个开头的是Crypto.js专属  


得到一串字符串,丢进厨子里提示base32,得到unicode编码转中文

然后就是新佛曰,网站下架了直接找密码`Live beautifully, dream passionately, love completely.`   
最后在Audacity里切换到频谱图即可看到flag  
不过aegisub打开也能看到,什么烤肉man的羁绊   

`flag{m1sc_1s_funny2333}`


## m0_01

流量分析题,打开压缩包是个键盘流量分析  
用免费版的ctf-neta可得`884080810882108108821042084010421`  

查了下这是云影密码,写个对应的脚本解密即可  

`flag{THISISFLAG}`

## 津门杯2021-m1

下载是个bmp,先试了010和foremost都没东西,最后看了下gray bits貌似有东西,lsb拿到一串字符  
丢进厨子里得到flag

`flag{l5DGqF1pPzOb2LU919LMaBYS5B1G01FD}`

## running

给了一个exe,但是拉到桌面显示是文档图标,猜测是隐写了个啥,foremost一下  

出来一个zip,里面是另一个exe和一个写了error的word,于是执行下exe  
出来一个tif文件,搜了下是要用ps打开的,电脑上没有  

去搜了下说是ps打开可以看到两个图层,取消一个能得到解密flag用的脚本  
010拉到tif文件最后可以看到`run-> njCp1HJBPLVTxcMhUHDPwE7mPW`,套一下即可  

`flag{mkBq0IICOMUUwdLiTICQvF6nOX}`

> 另: 看了下wp说这是一个自解压文件,用其他压缩软件打开能看到代码注释  
> 但博主用的是nanazip,[显示注释的issue](https://github.com/M2Team/NanaZip/issues/148)貌似一直挂着  

## pcap1

神秘题  
流量分析拉下来先协议分析或者瞪眼法,总之发现tcp流量最大,`tcp contains "flag"`一下  

出来一个流量,追踪下发现是段python代码和一个加密后的字符串(超级长)  

总之分析一下核心代码即可  
```python
enc_ciphers = ['rot13', 'b64e', 'caesar']

def encode(pt, cnt=50):
    tmp = '2{}'.format(b64encode(pt))
    for cnt in range(cnt):
        c = random.choice(enc_ciphers)
        i = enc_ciphers.index(c) + 1
        _tmp = globals()[c](tmp)
        tmp = '{}{}'.format(i, _tmp)

    return tmp
```
逻辑是把输入的字符随机进行三种加密里的一种,对原串加密后把对应加密的下标+1然后放在开头作为新串,重复cnt次  

解密的话就先取出密文的首位,然后按对应的加密方法解密,重复即可  
当然cnt未知,但字符串也给的不是很多,全遍历一次都行  

加密方法就3种,rot13给了转换表,凯撒移3位转回去就行,b64直接用decode就行

```python
def decode(pt):
    for i in range(len(pt)):
        c = pt[0]
        ## print("c = ", c)
        if c == '1':
            pt = rot13(pt[1:])
        if c == '2':
            pt = b64e(pt[1:])
        if c == '3':
            pt = caesar(pt[1:])
        ## print("pt = ", pt[0])
    print(pt)
    return pt
```

需要注意原题是py2的环境,现在是py3需要手动改一下,maketrans和translate函数都在str库下了,b64得到的是bytes需要手动转一下str  

`flag{li0ns_and_tig3rs_4nd_b34rs_0h_mi}`

## misc2-1

下载的图片打不开,分离了一下木得东西,010看一下  
发现开头是`E1 FF D8 FF`,结尾是`FF D7 BD EE D9`,是个4位反写的jpg文件(中间瞪眼法也可以发现反写了)  
写个脚本即可,出来的图片即为flag  

```python
with open('png/task_flag.jpg', 'rb') as tmp:
    with open('png/flag.jpg', 'ab') as fg:
        f = tmp.read()
        i = 0
        while i < len(f):
            fg.write(f[i:i+4][::-1])
            i += 4
```


`flag{F098996689560BBB1B566EBC10D5E564}`

## Let_god_knows

瞎眼题,建议直接写flag

010和foremost无果后再stegsolve里看,最后能在red plane 0 的道字上面隐约看到个二维码  
想办法提取出来扫描即可,但这题除了让你瞎眼之外学不到啥啊也   

`flag{Ok@y!G0d_know5_n0w}`

> 不过出题人的出法可以看看就是了,也是lsb  
> 但解题思路还是硬看....瞎眼  

## 碎纸机11

拼图题  
按道理是要用gaps,不过这题按原顺序直接拼接就行  

出来二维码,扫一下即可  

`flag{You Can Repair A Picture From Splices Baesd On Entropy}`

## Encode

丢厨子里手动rot13，然后一路魔法棒即可(rot13-hex-b32-b64-b85)  

`flag{W0w_y0u_c4n_rea11y_enc0d1ng!}`  

确实是套娃


## Check

下载图片,foremost和010无结果,丢进StegSolve里lsb提取可以看到一串`&#x7b`之类的,是html编码,丢进厨子即可  

`flag{h0w_4bouT_enc0de_4nd_pnG}`

## fakezip

下载是需要密码的压缩包,结合题目猜测是伪加密,把de和frflag都改了即可  
得到图片ocr一下  

`flag{39281de6-fe64-11ea-adc1-0242ac120002}`