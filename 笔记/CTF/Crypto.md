# 编码

## ASCII编码
![Pasted image 20220718191928.jpg](https://img-blog.csdnimg.cn/d32c158f07d24fc0abb80936d54d31ed.png)
**例子**
```
明文：hello,world.
十六进制：0x680x650x6c0x6c0x6f0xff0c0x770x6f0x720x6c0x640x2e
十进制：1041011081081112551211911111410810046
二进制：011010000110010101101100011011000110111100101100011101110110111101110010011011000110010000101110
```
[解码网站](https://coding.tools/cn/ascii-to-hex)

## Base编码

### Base16编码
>Base16编码使用16个ASCII可打印字符（数字0-9和字母A-F）对任意字节数据进行编码。Base16先获取输入字符串每个字节的二进制值（不足8比特在高位补0），然后将其串联进来，再按照4比特一组进行切分，将每组二进制数分别转换成十进制，在下述表格中找到对应的编码串接起来就是Base16编码。可以看到8比特数据按照4比特切分刚好是两组，所以Base16不可能用到填充符号“=”。

**例子**
```
明文：hello，world.123456
base16: 68656C6C6F2C776F726C642E313233343635
特征：大写字母(A-Z)和数字(0-9)，不用‘=’补齐。
```
[解码网站-1](https://www.qqxiuzi.cn/bianma/base.php?type=16)
[解码网站-2](https://stool.chinaz.com/base16)

### Base32编码
>Base32编码是使用32个可打印字符（字母A-Z和数字2-7）对任意字节数据进行编码的方案，编码后的字符串不用区分大小写并排除了容易混淆的字符，可以方便地由人类使用并由计算机处理。Base32将任意字符串按照字节进行切分，并将每个字节对应的二进制值（不足8比特高位补0）串联起来，按照5比特一组进行切分，并将每组二进制值转换成十进制来对应32个可打印字符中的一个。由于数据的二进制传输是按照8比特一组进行（即一个字节），因此Base32按5比特切分的二进制数据必须是40比特的倍数（5和8的最小公倍数）。例如输入单字节字符“%”，它对应的二进制值是“100101”，前面补两个0变成“00100101”（二进制值不足8比特的都要在高位加0直到8比特），从左侧开始按照5比特切分成两组：“00100”和“101”，后一组不足5比特，则在末尾填充0直到5比特，变成“00100”和“10100”，这两组二进制数分别转换成十进制数，通过上述表格即可找到其对应的可打印字符“E”和“U”，但是这里只用到两组共10比特，还差30比特达到40比特，按照5比特一组还需6组，则在末尾填充6个“=”。填充“=”符号的作用是方便一些程序的标准化运行，大多数情况下不添加也无关紧要，而且，在URL中使用时必须去掉“=”符号。

**例子**
```
明文：hello，world.123456
base32: NBSWY3DPFR3W64TMMQXDCMRTGQ3DK===
特征：大写字母(A-Z)和数字(2-7)，不满5的倍数，用‘=’补齐。
```
[解码网站-1](https://www.qqxiuzi.cn/bianma/base.php)
[解码网站-2](https://stool.chinaz.com/base32)
### Base64编码
>Base64编码是使用64个可打印ASCII字符（A-Z、a-z、0-9、+、/）将任意字节序列数据编码成ASCII字符串，另有“=”符号用作后缀用途。Base64将输入字符串按字节切分，取得每个字节对应的二进制值（若不足8比特则高位补0），然后将这些二进制数值串联起来，再按照6比特一组进行切分（因为2^6=64），最后一组若不足6比特则末尾补0。将每组二进制值转换成十进制，然后在上述表格中找到对应的符号并串联起来就是Base64编码结果。由于二进制数据是按照8比特一组进行传输，因此Base64按照6比特一组切分的二进制数据必须是24比特的倍数（6和8的最小公倍数）。24比特就是3个字节，若原字节序列数据长度不是3的倍数时且剩下1个输入数据，则在编码结果后加2个=；若剩下2个输入数据，则在编码结果后加1个=。

**例子**
```
明文：hello，world.123456
base64: aGVsbG8sd29ybGQuMTIzNDY1
特征：大小写字母（A-Z，a-z）和数字（0-9）以及特殊字符‘+’，‘/’，不满3的倍数，用‘=’补齐。
```
[解码网站-1](https://www.qqxiuzi.cn/bianma/base64.htm)
[解码网站-2](https://base64.us/)

### Base58编码

>**Base58**将字节转换为可打印字符，与Base64不同，去掉易于输入混淆的字符，不使用数字 “0“，字母大写“O“，字母大写 “I“，和字母小写 “l“，以及 “+“ 和 “/“ 符号，常作为cdkey，虚拟钱包(BTC）账号编码（base58check)

**例子**
```
明文：hello，world.123456
base58: 2smDFYXWKE8vc8XA8dadEYcSqcQb
特征：相比Base64，Base58不使用数字"0"，字母大写"O"，字母大写"I"，和字母小写"l"，以及"+"和"/"符号，最主要的是后面不会出现'='。
```
[解码网站-1](https://www.ruovea.com/base58.html)
[解码网站-2](http://www.atoolbox.net/Tool.php?Id=932)

### Base85编码

>Base85是一种类似于Base64的二进制文本编码形式，通过使用五个ASCII字符来表示四个字节的二进制数据。例如，它用于将图像嵌入到Adobe PDF文件中。Base85也称为Ascii85，是Paul E. Rutter为btoa实用程序开发的一种二进制文本编码形式。通过使用五个ASCII字符来表示四个字节的二进制数据（使编码量1 / 4比原来大，假设每ASCII字符8个比特），它比更有效UUENCODE或Base64的，它使用四个字符来表示三个字节的数据（1 / 3的增加，假设每ASCII字符8个比特）。用途是Adobe的PostScript和Portable Document Format文件格式，以及Git使用的二进制文件的补丁编码。与Base64一样，Base85编码的目标是对二进制数据可打印的ASCII字符进行编码。但是它使用了更大的字符集，因此效率更高一些。具体来说，它可以用5个字符编码4个字节（32位）。

**例子**
```
明文：hello，world.123456
base85: BOu!rDst>tGAhM<A1fSl1GgsI
特征：特点是奇怪的字符比较多，但是很难出现等号
```
[解码网站-1](http://www.atoolbox.net/Tool.php?Id=934)
[解码网站-2](http://www.hiencode.com/base85.html)

### Base91编码

>Bas符e91需要91个字来表示ASCII编码的二进制数据。Base91编码是从94个可打印ASCII字符（0x21-0x7E）中，以下三个字符被省略以构建Base91编码表：-（破折号，0x2D）\（反斜杠，0x5C）'（撇号，0x27）

**例子**
```
明文：hello，world.123456
明文：hello,world.123456
base91: TPwJh>go2Tv!_,aRA2IbLmA
特征：由91个字符（0-9，a-z，A-Z,!#$%&()*+,./:;<=>?@[]^_`{|}~”）组成不支持中文。
```
[解码网站-1](http://www.atoolbox.net/Tool.php?Id=935)
[解码网站-2](http://www.hiencode.com/base91.html)

### Base100编码

>Base100编码/解码工具（又名：Emoji表情符号编码/解码），可将文本内容编码为Emoji表情符号；同时也可以将编码后的Emoji表情符号内容解码为文本。

**例子**
```
明文：hello，world.123456
base100: 👟👜👣👣👦📦💳💃👮👦👩👣👛🐥🐨🐩🐪🐫🐬🐭
特征：就是一堆Emoji表情
```
[解码网站-1](http://www.atoolbox.net/Tool.php?Id=936)
[解码网站-2](https://ctf.bugku.com/tool/base100)

## Unicode编码

>众所周知ASCII码用一个字节表示一个字符，但是表示不了中文（和除了英语的其他语言）于是提出Unicode，用两个字节表示一个字符。原本的英文字符（ASCII码）就在高位加0  
包含UTF-8（以8位为一个编码单位的可变长编码），UTF-16（16位），UTF-32等 

**例子**
```
明文：hello，world.
&#x [hex]：&#x0068;&#x0065;&#x006C;&#x006C;&#x006F;&#xFF0C;&#x0077;&#x006F;&#x0072;&#x006C;&#x0064;&#x002E;

&# [hex]：&#00104;&#00101;&#00108;&#00108;&#00111;&#65292;&#00119;&#00111;&#00114;&#00108;&#00100;&#00046;

\u [hex]：\U0068\U0065\U006C\U006C\U006F\U002C\U0077\U006F\U0072\U006C\U0064\U002E

\u+ [hex]：\U+0068\U+0065\U+006C\U+006C\U+006F\U+FF0C\U+0077\U+006F\U+0072\U+006C\U+0064\U+002E
```
[解码网站-1](http://www.mxcz.net/tools/Unicode.aspx)
[解码网站-2](http://www.msxindl.com/tools/unicode16.asp)
[解码网站-3](https://www.sojson.com/unicode.html)
[解码网站-4](http://tool.chinaz.com/tools/unicode.aspx)

## HTML编码

>MDN：[HTML 实体](https://links.jianshu.com/go?to=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FGlossary%2FEntity)是一段以连字号（`&`）开头、以分号（`;`）结尾的文本（字符串）。实体常常用于显示保留字符（这些字符会被解析为 HTML 代码）和不可见的字符（如“不换行空格”）。你也可以用实体来代替其他难以用标准键盘键入的字符。

**例子**
```
明文：hello，world.
十进制：&#104;&#101;&#108;&#108;&#111;&#65292;&#119;&#111;&#114;&#108;&#100;&#46;
十六进制：&#x68;&#x65;&#x6C;&#x6C;&#x6F;&#xFF0C;&#x77;&#x6F;&#x72;&#x6C;&#x64;&#x2E;
```
[解码网站-1](http://www.toolzl.com/tools/htmlende.html)
[解码网站-2](https://www.qqxiuzi.cn/bianma/zifushiti.php)


## Escape/Unescape编码（%u）

>**Escape/Unescape加密解码/编码解码**,又叫%u编码，其实就是字符对应UTF-16 16进制表示方式前面加%u。**Unescape解码/解密**，就是去掉”%u”后，将16进制字符还原后，由utf-16转码到自己目标字符。如：字符“中”，UTF-16BE是：“6d93”，因此Escape是“%u6d93”，反之也一样！

**例子**
```
明文：hello，world.
密文：%u0068%u0065%u006c%u006c%u006f%uff0c%u0077%u006f%u0072%u006c%u0064%u002e
```
[解码网站-1](http://web.chacuo.net/charsetescape/)
[解码网站-2](http://www.hiencode.com/escape.html)

## URL编码

>url编码又叫百分号编码，是统一资源定位(URL)编码方式。URL地址（常说网址）规定了常用地数字，字母可以直接使用，另外一批作为特殊用户字符也可以直接用（/,:@等），剩下的其它所有字符必须通过%xx编码处理。 现在已经成为一种规范了，基本所有程序语言都有这种编码，如js：有encodeURI、encodeURIComponent，PHP有 urlencode、urldecode等。编码方法很简单，在该字节ascii码的的16进制字符前面加%. 如 空格字符，ascii码是32，对应16进制是’20’，那么urlencode编码结果是:%20。

**例子**
```
明文：Hello World
密文：Hello%20World

```
[解码网站-1](http://web.chacuo.net/charseturlencode)
[解码网站-2](http://www.jsons.cn/urlencode)

## Hex编码

> Hex 全称 是Intel HEX。Hex文件是由一行行符合Intel HEX文件格式的文本所构成的ASCII文本文件。在Intel HEX文件中，每一行包含一个HEX记录。这些记录由对应机器语言码和/或常量数据的**十六进制编码数字**组成。

**例子**
```
明文：hello，world.
密文（带%）：%68%65%6c%6c%6f%ef%bc%8c%77%6f%72%6c%64%2e
密文（不带%）：68656C6C6FEFBC8C776F726C642E
```
[解码网站-1](https://www.107000.com/T-Hex)
[解码网站-2](http://stool.chinaz.com/hex)


# 加密

## ADFGX密码

>电文中的所有单词都由A、D、F、G、X五个字母拼成，因此被称为ADFGX密码。ADFGX密码本质是字符替换，然后进行移位

[在线网站](http://www.atoolbox.net/Tool.php?Id=918)
[在线网站](http://www.hiencode.com/adfgx.html)

## ADFGVX密码

>1918年3月Fritz Nebel上校发明了这种密码，并提倡使用。它结合了改良过的Polybius方格替代密码与单行换位密码。这个密码以使用于密文当中六个字母 A, D, F, G, V, X命名

[在线网站](http://www.atoolbox.net/Tool.php?Id=917)
[在线网站](http://www.hiencode.com/adfgvx.html)

## 仿射密码

>仿射密码（Affine cipher）是一种表单替换密码，通过对字母数值进行简单的乘法和加法方程运算，而得到另一个与其对应的字母，从而进行加密； 仿射加密函数：F(x) = (ax + b) (mod m)，其中a和b互质，m是字母的数量； 仿射解密函数：F(x) = a-1(x - b) (mod m)，其中a-1是a在Zm群的乘法逆元，m是字母的数量。

[在线网站](http://www.atoolbox.net/Tool.php?Id=911)
[在线网站](http://www.metools.info/code/affinecipher183.html)

## 博福特密码

>**博福特密码**，是一种类似于[维吉尼亚密码](https://baike.baidu.com/item/%E7%BB%B4%E5%90%89%E5%B0%BC%E4%BA%9A%E5%AF%86%E7%A0%81)的[替代密码](https://baike.baidu.com/item/%E6%9B%BF%E4%BB%A3%E5%AF%86%E7%A0%81)，由弗朗西斯·蒲福（Francis Beaufort）发明。它最知名的应用是哈格林M-209密码机。 [1]  博福特密码属于[对等加密](https://baike.baidu.com/item/%E5%AF%B9%E7%AD%89%E5%8A%A0%E5%AF%86)，即加密算法与解密算法相同。

```
例如，明文的第一个字母为D，则先在表格中找到第D列。由于密钥的第一个字母为F，于是D列从上往下找到F。这一F对应的行号为C，因而C便是密文的第一个字母。以此类推可以得到密文。以下便是一个密钥为FORTIFICATION时的例子：
明文：DEFENDTHEEASTWALLOFTHECASTLE
密钥：FORTIFICATIONFORTIFICATIONFO
密文：CKMPVCPVWPIWUJOGIUAPVWRIWUUK
```

[在线网站](http://www.hiencode.com/beaufort.html)
[在线网站](https://wtool.com.cn/beaufort.html)

## 凯撒密码

>凯撒密码是一种替换加密技术，明文中的所有字母都在字母表上向后（或向前）按照一个固定数目进行偏移后被替换成密文。例如，当偏移量是3的时候，所有的字母A将被替换成D，B变成E，以此类推。这个加密方法是以罗马共和时期凯撒的名字命名的，据称当年凯撒曾用此方法与其将军们进行联系。

```
明文:  defend the east wall of the castle
密文: efgfoe uif fbtu xbmm pg uif dbtumf
```

[在线网站](https://www.qqxiuzi.cn/bianma/kaisamima.php)
[在线网站](https://zh.planetcalc.com/1434/)

## 四方密码

>四方密码是一种对称式加密法，由法国人Felix Delastelle（1840年–1902年）发明。
>这种方法将字母两个一组，然后采用多字母替换密码。
四方密码用4个5×5的矩阵来加密。每个矩阵都有25个字母（通常会取消Q或将I,J视作同一样，或改进为6×6的矩阵，加入10个数字）。

```
明文：i love Four-Square cipher
Key1：ZBNFCTLOPVAGMIKUYDHSWXERQ
key2：KREBDCOPGIHSATZXWLVYQMFNU
密文：TTANZIKVDWSXBXNDVTVEDM

```

[在线网站](http://www.metools.info/code/four-square244.html)
[在线网站](https://wtool.com.cn/four.html)

## 栅栏密码

>所谓栅栏密码，就是将要加密的明文分为N个一组，再从每组的选出一个字母连起来，形成一段无规律的密文。栅栏密码并非一种强的加密法，其加密原理限制了密钥的最高数量不可能超过明文字母数，而实际加密时密钥数目更少，因此有些密码分析员甚至能用手直接解出明文。

```
明文：hello world
number：2
密文：hlowrdel ol
```

[在线网站](https://www.qqxiuzi.cn/bianma/zhalanmima.php)
[在线网站](http://www.metools.info/code/fence154.html)

## Rot13密码

>套用ROT13到一段文字上仅仅只需要检查字符字母顺序并取代它在13位之后的对应字母，有需要超过时则重新绕回26英文字母开头即可。 A换成N、B换成O、依此类推到M换成Z，然后序列反转：N换成A、O换成B、最后Z换成M。只有这些出现在英文字母里头的字符受影响；数字、符号、空白字符以及所有其他字符都不变。因为只有在英文字母表里头只有26个，并且26 = 2 × 13，ROT13函数是它自己的逆反

[在线网站](https://www.jisuan.mobi/puzzm6z1B1HH6yXW.html)
[在线网站](http://www.mxcz.net/tools/rot13.aspx)

## 维吉尼亚密码

>维吉尼亚密码是在凯撒密码基础上产生的一种加密方法，它将凯撒密码的全部25种位移排序为一张表，与原字母序列共同组成26行及26列的字母表。另外，维吉尼亚密码必须有一个密钥，这个密钥由字母组成，最少一个，最多可与明文字母数量相等。

```
明文：I've got it.
密钥：ok
密文：W'fs qcd wd.
```

[在线网站](http://www.atoolbox.net/Tool.php?Id=856)
[在线网站](https://www.guballa.de/vigenere-solver)

## RSA密码

>RSA算法的具体描述如下：
（1）任意选取两个不同的大素数p和q计算乘积  ；
（2）任意选取一个大整数e，满足 ，整数e用做加密钥（注意：e的选取是很容易的，例如，所有大于p和q的素数都可用）；
（3）确定的解密钥d，满足 ，即 是一个任意的整数；所以，若知道e和，则很容易计算出d  ；
（4）公开整数n和e，秘密保存d  ；
（5）将明文m（m<n是一个整数）加密成密文c，加密算法为 
（6）将密文c解密为明文m，解密算法为 
然而只根据n和e（注意：不是p和q）要计算出d是不可能的。因此，任何人都可对明文进行加密，但只有授权用户（知道d）才可对密文解密  。


[在线网站](https://www.bejson.com/enc/rsa/)
[在线网站](http://www.metools.info/code/c81.html)