## 1.编写shell脚本

```copy
[root@localhost 720]# vim goip.sh
[root@localhost 720]# cat goip.sh
#! /bin/bash

# 获取ip
ip add |awk -F '[ /]+' '/ens33$/{print "ip:"$3}'
# 获取网关
ip route|awk 'NR==1 {print "geteway:"$3}'
# 获取DNS
awk 'NR!=1 {print "DNS:" $2}' /etc/resolv.conf

[root@localhost 720]# bash goip.sh
ip:192.168.10.129
geteway:192.168.10.2
DNS:114.114.114.114

```

## 2.授予脚本可执行权限

```copy
[root@localhost 720]# chmod +x goip.sh
[root@localhost 720]# ls -l goip.sh
-rwxr-xr-x 1 root root 193 7月  21 10:48 goip.sh
[root@localhost 720]# ./goip.sh
ip:192.168.10.129
geteway:192.168.10.2
DNS:114.114.114.114
```

## 3.复制脚本添加到PATH变量中

```copy
[root@localhost 720]# echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin
[root@localhost 720]# cp goip.sh  /usr/bin
```

## 4.定义命令别名(临时)

```copy
[root@localhost 720]# alias goip='goip.sh'
[root@localhost ~]# goip
ip:192.168.10.129
geteway:192.168.10.2
DNS:114.114.114.114
```