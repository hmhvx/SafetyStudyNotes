*语法*

>dirb [参数]  [服务]  [域名]

---

```
-a 设置user-agent
-p <proxy[:port]>设置代理
-c 设置cookie
-z 添加毫秒延迟，避免洪水攻击
-o 输出结果
-X 在每个字典的后面添加一个后缀
-H 添加请求头
-i 不区分大小写搜索
```
---

使用/usr/share/wordlists/dirb/big.txt 字典来扫描Web服务
`dirb http://192.168.1.116 /usr/share/wordlists/dirb/big.txt 
`
*User Agent中文名为用户代理，简称 UA，它是一个特殊字符串头，使得服务器能够识别客户使用的操作系统及版本、CPU 类型、浏览器及版本、浏览器渲染引擎、浏览器语言、浏览器插件*
设置UA和cookie
`dirb http://192.168.1.116 -a "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:75.0) Gecko/20100101 Firefox/75.0" -c "BAIDUID=D5C6351DAC89EF8811A51DF3A9A9C0C4:FG=1; HMACCOUNT=2906306413846532; BIDUPSID=D5C6351DAC89EF8811A51DF3A9A9C0C4; PSTM=1585744543; BDORZ=FFFB88E999055A3F8A630C64834BD6D0; H_PS_PSSID=30974_1438_31124_21098; HMVT=6bcd52f51e9b3dce32bec4a3997715ac|1587436663|; delPer=0; PSINO=6; BDRCVFR[gltLrB7qNCt]=mk3SLVN4HKm"`

对每个字典添加.dist后缀并延迟100毫米
`dirb http://192.168.1.116  -X .dist -z 100 -o test.txt`

使用代理
`dirb http://192.168.1.116  -p 46.17.45.194:5210`