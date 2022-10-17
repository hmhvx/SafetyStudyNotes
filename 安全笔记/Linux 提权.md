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






















































