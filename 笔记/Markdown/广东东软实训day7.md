### 7.删除旧版本数据库

注：因为我们接下来要装新版本，如果还残留旧版本的文件，就会出现报错

[root@dongruan1 ~]# yum remove mariadb* -y

```
[root@dongruan1 ~]# which mysql
/usr/bin/which: no mysql in (/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin)

[root@dongruan1 ~]# whereis mysql
mysql: /usr/lib64/mysql
[root@dongruan1 ~]# rm -rf /usr/lib64/mysql

[root@dongruan1 ~]# find / -name "*mysql*"	   #语法：find 路径 -name “文件名”：找到指定路径下与指定名字相关的文件
/etc/selinux/targeted/active/modules/100/mysql
/root/.mysql_history
/var/lib/mysql
/var/lib/mysql/mysql
/usr/lib/firewalld/services/mysql.xml
/usr/share/vim/vim74/syntax/mysql.vim
/mnt/Packages/akonadi-mysql-1.9.2-4.el7.x86_64.rpm
/mnt/Packages/dovecot-mysql-2.2.10-8.el7.x86_64.rpm
/mnt/Packages/libdbi-dbd-mysql-0.8.3-16.el7.x86_64.rpm
/mnt/Packages/mysql-connector-odbc-5.2.5-6.el7.x86_64.rpm
/mnt/Packages/mysql-connector-java-5.1.25-3.el7.noarch.rpm
/mnt/Packages/pcp-pmda-mysql-3.11.8-7.el7.x86_64.rpm
/mnt/Packages/php-mysql-5.4.16-42.el7.x86_64.rpm
/mnt/Packages/qt5-qtbase-mysql-5.6.2-1.el7.x86_64.rpm
/mnt/Packages/qt-mysql-4.8.5-13.el7.x86_64.rpm
/mnt/Packages/rsyslog-mysql-8.24.0-12.el7.x86_64.rpm

[root@dongruan1 ~]# rm -rf /etc/selinux/targeted/active/modules/100/mysql /root/.mysql_history /var/lib/mysql /var/lib/mysql/mysql /usr/lib/firewalld/services/mysql.xml /usr/share/vim/vim74/syntax/mysql.vim

[root@dongruan1 ~]# find / -name "*mysql*"			#剩下以/mnt开头的是镜像包的安装包，不要删
/mnt/Packages/akonadi-mysql-1.9.2-4.el7.x86_64.rpm
/mnt/Packages/dovecot-mysql-2.2.10-8.el7.x86_64.rpm
/mnt/Packages/libdbi-dbd-mysql-0.8.3-16.el7.x86_64.rpm
/mnt/Packages/mysql-connector-odbc-5.2.5-6.el7.x86_64.rpm
/mnt/Packages/mysql-connector-java-5.1.25-3.el7.noarch.rpm
/mnt/Packages/pcp-pmda-mysql-3.11.8-7.el7.x86_64.rpm
/mnt/Packages/php-mysql-5.4.16-42.el7.x86_64.rpm
/mnt/Packages/qt5-qtbase-mysql-5.6.2-1.el7.x86_64.rpm
/mnt/Packages/qt-mysql-4.8.5-13.el7.x86_64.rpm
/mnt/Packages/rsyslog-mysql-8.24.0-12.el7.x86_64.rpm

```

### 8.安装新版本mysql

mysql官方网站：https://www.mysql.com/

下载yum源仓库的地址 http://repo.mysql.com/yum/mysql-5.7-community/el/7/x86_64/

```
[root@dongruan1 opt]# wget http://repo.mysql.com/yum/mysql-5.7-community/el/7/x86_64/mysql57-community-release-el7-10.noarch.rpm

[root@dongruan1 opt]# rpm -ivh mysql57-community-release-el7-10.noarch.rpm   #解压后会生成两个mysql的yum源仓库文件
警告：mysql57-community-release-el7-10.noarch.rpm: 头V3 DSA/SHA1 Signature, 密钥 ID 5072e1f5: NOKEY
准备中...                          ################################# [100%]
正在升级/安装...
   1:mysql57-community-release-el7-10 ################################# [100%]
   
有时候这个rpm解压不出文件，可以采取第二种方法：
可以先下载这个RPM包，再拉到虚拟机上
[root@dongruan1 ~]# yum install -y lrzsz		#安装此软件后，可以用鼠标把文件移动到虚拟机上


[root@dongruan1 opt]# cd /etc/yum.repos.d/
[root@dongruan1 yum.repos.d]# ll
总用量 16
drwxr-xr-x 2 root root  205 6月  22 10:11 bak
-rw-r--r-- 1 root root 2523 6月  22 10:14 CentOS-Base.repo
-rw-r--r-- 1 root root   82 6月  22 10:17 local.repo
-rw-r--r-- 1 root root 1627 4月   5 2017 mysql-community.repo
-rw-r--r-- 1 root root 1663 4月   5 2017 mysql-community-source.repo
[root@dongruan1 yum.repos.d]# yum repolist			#刷新一下缓存
[root@dongruan1 yum.repos.d]# yum install -y mysql-community-server   #默认安装最新版本
```

查看版本：

[root@dongruan1 yum.repos.d]# mysql -V
mysql  Ver 14.14 Distrib 5.7.38, for Linux (x86_64) using  EditLine wrapper

启动服务：

```shell
[root@dongruan1 yum.repos.d]# systemctl status mysqld
● mysqld.service - MySQL Server
   Loaded: loaded (/usr/lib/systemd/system/mysqld.service; enabled; vendor preset: disabled)
   Active: inactive (dead)
     Docs: man:mysqld(8)
           http://dev.mysql.com/doc/refman/en/using-systemd.html

[root@dongruan1 yum.repos.d]# systemctl start mysqld

[root@dongruan1 yum.repos.d]# systemctl status mysqld
● mysqld.service - MySQL Server
   Loaded: loaded (/usr/lib/systemd/system/mysqld.service; enabled; vendor preset: disabled)
   Active: active (running) since 三 2022-06-22 14:09:34 CST; 3s ago
     Docs: man:mysqld(8)
           http://dev.mysql.com/doc/refman/en/using-systemd.html
  Process: 18848 ExecStart=/usr/sbin/mysqld --daemonize --pid-file=/var/run/mysqld/mysqld.pid $MYSQLD_OPTS (code=exited, status=0/SUCCESS)
  Process: 18799 ExecStartPre=/usr/bin/mysqld_pre_systemd (code=exited, status=0/SUCCESS)
 Main PID: 18851 (mysqld)
   CGroup: /system.slice/mysqld.service
           └─18851 /usr/sbin/mysqld --daemonize --pid-file=/var/run/mysqld/mysqld.pid

6月 22 14:09:31 dongruan1 systemd[1]: Starting MySQL Server...
6月 22 14:09:34 dongruan1 systemd[1]: Started MySQL Server.

```

报错：

```
[root@dongruan1 yum.repos.d]# mysql -uroot -p
Enter password: 
ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: YES)			#密码不对
```

在5.7的版本里，安装完数据库后，需要初始化root密码，可以通过查看/var/log/mysqld.log数据库日志获取临时密码

注：如果旧版本删除不干净，会残留/var/lib/mysql这一文件，会影响临时密码生成

解决方案：可以删除/var/lib/mysql这一文件，再重启数据库服务，就可以有临时密码生成

临时密码获取：

方法1

```
[root@dongruan1 yum.repos.d]# cat /var/log/mysqld.log |grep password
2022-06-22T06:09:32.575517Z 1 [Note] A temporary password is generated for root@localhost: ZskGiUh3d#:s
```

方法2：

```
[root@dongruan1 yum.repos.d]# grep password /var/log/mysqld.log
2022-06-22T06:09:32.575517Z 1 [Note] A temporary password is generated for root@localhost: ZskGiUh3d#:s
```

通过临时密码进入到数据库后，需要立即修改数据库密码

由于5.7版本数据库密码设置非常严格，要求是大小写+数字+特殊符号+标点符号，所以我们要修改的密码限制

```
mysql> set global validate_password_policy=0;			#定义密码复杂度：0是只检查长度
Query OK, 0 rows affected (0.00 sec)

mysql> set global validate_password_length=1;			#定义密码长度，默认是>=8位数，设置为1后，长度>=4位数
Query OK, 0 rows affected (0.00 sec)

举例：
mysql> alter user 'root'@localhost identified by '123';
ERROR 1819 (HY000): Your password does not satisfy the current policy requirements

mysql> alter user 'root'@localhost identified by '123456';
Query OK, 0 rows affected (0.00 sec)

```

此时，数据库已正常可以使用

### 9.数据迁移

方法1：

```
[root@dongruan1 opt]# mysql -uroot -p  </opt/dongruan1.sql				#在命令行导入数据库数据
Enter password: 
[root@dongruan1 opt]# mysql -uroot -p						#登录数据库验证
Enter password: 
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| dongruan1            |
| sys                |
+--------------------+
5 rows in set (0.00 sec)

mysql> use dongruan1
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> show tables;
+-------------------+
| Tables_in_dongruan1 |
+-------------------+
| person            |
+-------------------+
1 row in set (0.00 sec)

mysql> select * from person;
+------+------+------+---------+---------+
| id   | name | math | chinese | english |
+------+------+------+---------+---------+
|    1 | lu   | 85   | 99      | 95      |
+------+------+------+---------+---------+
1 row in set (0.00 sec)
```

方法2：

```
[root@dongruan1 opt]# mysql -uroot -p						#登录数据库
Enter password:
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.00 sec)

mysql> source /opt/dongruan1.sql							#进入数据库里面导入
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| dongruan1            |
| sys                |
+--------------------+
5 rows in set (0.00 sec)

```



作业：

要求1：

截图：在虚拟机里执行mysql -V，一个是5.5版本，一个是5.7版 本的，主机名是你们名字拼音，不能缩写

要求2：

在数据库里执行show databases:的结果要包含 dongruan1的库，还需要select * from 表的结果





### 1.今日目标

1.使用编译安装nginx

2.如何通过ip，端口，域名使用nginx

3.使用nginx搭建ucenter项目





### 2.编译安装

在linux里面，一般是服务的话，都会分成三部份，一个是配置文件，一个是日志文件，一个是主程序部份（包括源码）

到目前我们都是用yum源安装

这种安装方式非常方便 ，但有一个缺点，只要是一个服务，就是安装的配置文件必在/etc目录下，日志文件必在/var/log目录下面

因为软件已经指定了安装路径

只要不涉及服务，基本上是用编译安装，相当于自定义安装



### 3.编译安装三步骤：

1.configure ：预编译，这一步骤主要是为了检测环境与依赖是否符合软件安装与使用

2.make：编译，就是把一些源码编译成可以在liunx上运行的程序

3.make install ：安装



### 4.了解nginx

目前市场上nginx是排第一的web服务器，apache是排第二的web服务器，可查百度百科





### 5.编译安装nginx

```
[root@dongruan1 opt]# wget http://nginx.org/download/nginx-1.20.2.tar.gz			

[root@dongruan1 opt]# tar -zxvf nginx-1.20.2.tar.gz
[root@dongruan1 opt]# cd nginx-1.20.2
注： 在liunx里面一般都有个命令 -h   ；命令 --help 作为帮忙手册
[root@dongruan1 nginx-1.20.2]# ./configure --help    #查看预编译的帮忙手册，了解有哪一些选项或功能可以添加
[root@dongruan1 nginx-1.20.2]# ./configure \
--prefix=/usr/local/nginx \							#指定nginx安装的目录地址
--user=nginx \										#指定nginx的用户
--group=nginx 	\									#指定nginx的用户组
--sbin-path=/usr/local/nginx/nginx \				#指定nginx的启动文件地址
--conf-path=/usr/local/nginx/conf/nginx.conf \		#指定nginx的配置文件地址
--error-log-path=/var/log/nginx/nginx.log \			#指定nginx的错误日志文件地址
--http-log-path=/var/log/nginx/access.log \			#指定nginx的访问日志文件地址
--modules-path=/usr/local/nginx/modules \			#指定nginx的模块（包括官方与第三方软件支持的模块）存放目录
--with-select_module --with-poll_module --with-threads  --with-http_ssl_module --with-http_v2_module  --with-http_realip_module --with-http_image_filter_module --with-http_sub_module --with-http_flv_module --with-http_gunzip_module --with-http_gzip_static_module  --with-http_stub_status_module --with-stream
#带with其它项为nginx开启的功能选项

./configure: error: C compiler cc is not found

[root@dongruan1 nginx-1.20.2]# yum install -y gcc gcc-c++

./configure: error: the HTTP rewrite module requires the PCRE library.
You can either disable the module by using --without-http_rewrite_module
option, or install the PCRE library into the system, or build the PCRE library
statically from the source with nginx by using --with-pcre=<path> option.

[root@dongruan1 nginx-1.20.2]# yum install -y pcre pcre-devel

./configure: error: SSL modules require the OpenSSL library.
You can either do not enable the modules, or install the OpenSSL library
into the system, or build the OpenSSL library statically from the source
with nginx by using --with-openssl=<path> option.

[root@dongruan1 nginx-1.20.2]# yum install -y openssl openssl-devel

./configure: error: the HTTP image filter module requires the GD library.
You can either do not enable the module or install the libraries.

[root@dongruan1 nginx-1.20.2]# yum install -y gd go-devel

creating objs/Makefile						#出现这个标识，证明预编译已经成功通过

Configuration summary
  + using threads
  + using system PCRE library
  + using system OpenSSL library
  + using system zlib library

  nginx path prefix: "/usr/local/nginx"
  nginx binary file: "/usr/local/nginx/nginx"
  nginx modules path: "/usr/local/nginx/modules"
  nginx configuration prefix: "/usr/local/nginx/conf"
  nginx configuration file: "/usr/local/nginx/conf/nginx.conf"
  nginx pid file: "/usr/local/nginx/logs/nginx.pid"
  nginx error log file: "/var/log/nginx/nginx.log"
  nginx http access log file: "/var/log/nginx/access.log"
  nginx http client request body temporary files: "client_body_temp"
  nginx http proxy temporary files: "proxy_temp"
  nginx http fastcgi temporary files: "fastcgi_temp"
  nginx http uwsgi temporary files: "uwsgi_temp"
  nginx http scgi temporary files: "scgi_temp"

[root@dongruan1 nginx-1.20.2]# make 	#编译
[root@dongruan1 nginx-1.20.2]# make install 		#安装
[root@dongruan1 nginx-1.20.2]# cd /usr/local/nginx/
[root@dongruan1 nginx]# ll
总用量 8032
drwxr-xr-x 2 root root     333 6月  23 10:17 conf
drwxr-xr-x 2 root root      40 6月  23 10:17 html
drwxr-xr-x 2 root root       6 6月  23 10:17 logs
-rwxr-xr-x 1 root root 8222280 6月  23 10:17 nginx				#nginx主程序

[root@dongruan1 nginx]# ln -s /usr/local/nginx/nginx /usr/sbin/nginx		#生成软链接，相当windows的快捷方式，让我们																		  可以在命令行直接敲nginx以启动程序
[root@dongruan1 nginx]# ll /usr/sbin/nginx 
lrwxrwxrwx 1 root root 22 6月  23 10:26 /usr/sbin/nginx -> /usr/local/nginx/nginx
[root@dongruan1 nginx]# which nginx
/usr/sbin/nginx
[root@dongruan1 nginx]# nginx					#出现此错，主要是因为没有nginx的用户
nginx: [emerg] getpwnam("nginx") failed
[root@dongruan1 nginx]# id nginx
id: nginx: no such user
[root@dongruan1 nginx]# useradd nginx 
[root@dongruan1 nginx]# nginx				#启动nginx

```

验证是否成功启动：

方法1：

```
[root@dongruan1 nginx]# ps -ef |grep nginx				#查看进程命令，出现如下列显示，证明nginx已正常运行
root       8560      1  0 10:27 ?        00:00:00 nginx: master process nginx
nginx      8561   8560  0 10:27 ?        00:00:00 nginx: worker process
root       8563    840  0 10:31 pts/0    00:00:00 grep --color=auto nginx
```

方法2：

在浏览器上输入 http://服务器IP，出现如下界面则证明nginx成功运行

![image-20220623103427147](https://note-1308251438.cos.ap-guangzhou.myqcloud.com/typora/202207051830043.png)



如果出现如下界面，优先检查防火墙与seliunx是否关闭

![image-20220623103834621](https://note-1308251438.cos.ap-guangzhou.myqcloud.com/typora/202207051830993.png)

nginx家目录 为主目录下面的html目录、

```
[root@dongruan1 html]# cd /usr/local/nginx/html
[root@dongruan1 html]# >index.html 					#清空文件内容
[root@dongruan1 html]# cat index.html 
[root@dongruan1 html]# echo this is a good day >index.html
```

在浏览器刷新一下：

![image-20220623104734587](https://note-1308251438.cos.ap-guangzhou.myqcloud.com/typora/202207051830060.png)



### 6.上传源码到html目录

举例：上传坦克大战

![image-20220623110048447](https://note-1308251438.cos.ap-guangzhou.myqcloud.com/typora/202207051830009.png)

浏览器刷新一下：

![image-20220623110102325](https://note-1308251438.cos.ap-guangzhou.myqcloud.com/typora/202207051830081.png)



