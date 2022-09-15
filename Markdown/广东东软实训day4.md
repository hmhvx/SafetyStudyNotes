Linux服务前期环境准备

### 1.1.1  清空关闭防火墙

**由于前期尚未学习“防火墙”为了不受到防火墙影响实验的顺利进行，因此清空并关闭防火墙。**

**[root@base ~]# iptables -F**			**#清空防火墙规则。**

**[root@base ~]# systemctl stop firewalld**			**#关闭防火墙。**

**[root@base ~]# systemctl disable firewalld**		**#设置开机不启动防火墙。**

**[root@study opt]# systemctl status firewalld** **#查看防火墙状态**

 

### 1.1.2  关闭SElinux

**[root@base ~]# getenforce**			**#查看SElinux状态。**

**Disabled**

 

**1.** 临时关闭（机器重启则会失效）：

[root@base ~]# setenforce 0		#临时关闭SElinux，0表示关闭。

 

**2.** 永久关闭SElinux：

[root@base ~]# vim /etc/selinux/config				#修改SElinux配置文件，修改SELINUX=disabled，如图 1-1 所示，修改完成后并保存退出，修改文件后需要重启主机则会生效。

![img](https://note-1308251438.cos.ap-guangzhou.myqcloud.com/typora/202206291032568.jpg) 

图 1-1 SElinux配置文件

**注意：永久关闭SElinux需要重启主机则生效，如果当前主机是生产环境不能重启主机的条件下，但又要实现永久关闭SElinux的情况下，需要先做临时关闭，再修改其配置文件实现永久关闭，但不需要重启主机。**

### 1.1.3  配置静态IP

[root@base ~]# vim /etc/sysconfig/network-scripts/ifcfg-ens33

TYPE="Ethernet"

BOOTPROTO="static"			#模式为静态分配

IPADDR=xxx			#IP地址

GATEWAY=xxx		#网关

NAME="ens33"

DEVICE="ens33"

ONBOOT="yes"		#是否开启网络

Liunx里面所有服务，只要修改配置文件，必须重启服务

 

关闭NetworkManager 服务：

[root@base ~]# systemctl stop NetworkManager		#关闭 NetworkManager 。

[root@base ~]# systemctl disable NetworkManager	#设置开启不启动。

 

 

 

### 1.1.5  修改主机名

[root@base ~]# vim /etc/hostname 		#修改主机名配置文件（永久生效，需要重启）。

base

[root@base ~]# hostname mysql 	#临时设置，立即生效（需要重启当前终端）。

base

 

### 1.1.6  配置Yum源

**1．**配置本地Yum源：

[root@base ~]# mount /dev/sr0  /mnt/		#挂载光驱。

[root@base ~]# echo "/dev/sr0 /mnt iso9660 defaults 0 0" >> /etc/fstab		#设置开机自动挂载光驱。

[root@base ~]# mv /etc/yum.repos.d/\* 	/etc/yum.repos.d/bak	

#备份/etc/yum.repos.d/目录的所有文件。

 

[root@base ~]# vim /etc/yum.repos.d/local.repo		#使用vim创建一个新的文件，并写入如下内容

[local]

name= this is a local.repo

baseurl=file:///mnt

enabled=1

gpgcheck=0

 

**2．**配置网络yum源：

阿里云镜像源站点（[http://mirrors.aliyun.com/](http://mirrors.aliyun.com/)）。

CentOS镜像参考：[http://mirrors.aliyun.com/help/centos](http://mirrors.aliyun.com/help/centos)

 

下载Yum源：

[root@base ~]# curl -o /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo

 

 

生成Yum缓存：

[root@base ~]# yum list

 

 

 

重启主机，使以上设置生效:

[root@base ~] reboot

 

 

编写脚本

**1.** 必须创建脚本文件是以.sh结尾

**2.** 文件内容一定要以#！/bin/bash开头

**3.** chmod +x  xxx.sh给文件执行权限

**4.** 用 ./xxx.sh 或者 sh xxx.sh方式执行

**5.** cat > a <<eof 或者 cat >> a <<eof (一个>表示覆盖，两个>表示追加)



 

 

 