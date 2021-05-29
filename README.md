# rxctf-2021-writeup

## 0x00

在 `word` 中全选，把所有字底色改为红色或其他非白色颜色

![](https://raw.githubusercontent.com/RJXH/pictures/main/rxctf2021/0x00.00.png)

答案为 `rxctf{rjxh2021}`

## 0x01

思科交换机使用的是 `type7` 加密，我们可以在网上找到 [解密工具](http://www.atoolbox.net/Tool.php?Id=992) 得到 
`jingwenlovesensen`

# 0x02

凯撒加密想做偏移两位即可得到答案
`Welcometothectf`

## 0x03

密码串用 `Base64` 解码失败，如图：

![](https://raw.githubusercontent.com/RJXH/pictures/main/rxctf2021/0x03.00.png)

但是，还是看到了 `ey` ，题目说是异常数据，可猜测里面某些字母应该是 小写，才能保证解码出来是正确的

尝试将第一个用小写 `aGV5IULSB3ZLVSE=`

解码结果 
![](https://raw.githubusercontent.com/RJXH/pictures/main/rxctf2021/0x03.01.png)

果然猜测是对的

那么就可以通过逐位变换小写来解码 4 个字符为 单位观察解码后是否为可见字符。 

用 `Pyhton` 来解析：

```
import base64
import binascii


b64str = 'AGV5IULSB3ZLVSE='
f = open('flag.txt', 'w')


def base64code(s, d):
    if d == len(b64str):
        f.write(binascii.b2a_qp(base64.b64decode(s)).decode('UTF-8') + '\n')
    else:
        base64code(s + b64str[d], d + 1)
        if b64str[d].isalpha():
            base64code(s + b64str[d].lower(), d + 1)


base64code('', 0)
f.close()
f = open('flag.txt', 'r')

for i in f.readlines():
    if '=' not in i:
        print(i)


```

如果不会写 `Python` 也可以通过 [在线 Base64](https://base64.us/) 来手动解码

## 0x04

根据`六十甲子`顺序表可得 `28` `30` `23` `8` `17` `10` `16` `30`

一甲子是六十年，加起来就是 `88` `90` `83` `68` `77` `70` `76` `90`

根据 `ASCII` 码对照表得 `XZSDMFLZ` 使用栅栏密码 `2` 栏处理后得

`XMZFSLDZ` 

凯撒密码向左位移 `5` 位得 `SHUANGYU`

## 0x05

通过 `bing` 搜索引擎搜索相关关键词，查找得到一个文本 [加密网站](https://www.qqxiuzi.cn/bianma/wenbenjiami.php?s=yinyue) 可将
文本加密为音乐符号

在网站中输入

`§♩♫‖§♯‖♭♩♩♯♭§♫♪♯‖♯♩♩‖‖♭♭♪‖‖♭‖‖‖‖♭♭♬§♭∮♫‖§♩♯‖§♪==`

进行解密，得到
`软件协会 CTF 竞赛`

## 0x06

尝试以 `txt` 方式打开，并在底部发现了这样

![](https://raw.githubusercontent.com/RJXH/pictures/main/rxctf2021/0x06.00.png)

底部发现了 `code.txt`

很明显是拼接了文件，查看其 `hex` 

![](https://raw.githubusercontent.com/RJXH/pictures/main/rxctf2021/0x06.01.png)

JPG 文件尾[[1]](#file)后面还有文件

但其实凭感觉可能可以通过解压方式打开，更改后缀名为 `zip` 后打开

![](https://raw.githubusercontent.com/RJXH/pictures/main/rxctf2021/0x06.02.png)

果然出现 `code.txt` 文件

打开 `code.txt` 得到答案
`BgRCEuSbgook2uur`

## 0x07

使用 `PhotoShop` 等修图软件打开，关闭绿色通道并放大后

![](https://raw.githubusercontent.com/RJXH/pictures/main/rxctf2021/0x07.00.png)

发现答案 
`BgRCEuSbgook2uur`

## 0x08

通过 `NotePad++` 加插件 `HEX-Editor` 可打开查看 `hex`

```
[0][3]
[24][3]
[7][3]
[19][3]
```
看起来像是数组，按照两位来定位数组那么 `[0][3]` 会超出长度，那么应该是按照四位

![](https://raw.githubusercontent.com/RJXH/pictures/main/rxctf2021/0x08.00.png)

得到答案 `828c`

## 0x09

此段数据出现明显异常，怀疑后面加入了二进制

![](https://raw.githubusercontent.com/RJXH/pictures/main/rxctf2021/0x09.01.png)

按行转 `10` 进制得 `48` `81` `54` `97` 最后通过 `ASCII` 表得出答案
`NQ6a`

#### <span id="file">附：常见的文件头和尾</span>

| 拓展名 | 文件头 | 文件尾 |
| ---- | ---- | ---- |
| JPEG（jpg)  | FF D8 FF | FF D9 |
| PNG（png)  | 89 50 4E 47 | AE 42 60 82 |
| JPEG（jpg)  | 47 49 46 38	 | 00 3B |
| ZIP Archive(zip)  | 50 4B 03 04 | 50 4B |