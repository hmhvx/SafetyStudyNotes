### 1.今日目标

安装之前请初始化环境，不然可能出现各种错

mysql数据库

1.安装数据库5.5并初始化

2.建库建表建数据并导出

3.体验升级mysql5.7版本

4.再体验数据迁移









### 2.数据库分哪两种类型？

关系型与非关系型

但关系型代表是谁？mysql（被甲骨文公司收购了）和oracle(甲骨文公司的王牌软件，大公司，商业版的)

非关系型是：mongodb（json格式的文档型数据库）和redis（key=value   必须会的，缓存型数据库，典型应用场景：微博）

新型分布式数据库：性能更高，高可用，高可靠，天生支持分布式 ，TIDB，POLARDB



centos7以前的版本，是有mysql的安装，但到centos7变成了mariadb



### 3.安装本地数据库

[root@dongruan1 yum.repos.d]# yum install -y mariadb-server mariadb

查看版本：

mysql -V

启动数据库：

[root@dongruan1 yum.repos.d]# systemctl start mariadb
[root@dongruan1 yum.repos.d]# systemctl status mariadb

[root@dongruan1 yum.repos.d]# mysql_secure_installation    #安全配置初始化

```
NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user.  If you've just installed MariaDB, and
you haven't set the root password yet, the password will be blank,
so you should just press enter here.

Enter current password for root (enter for none):        #请输入root密码，因为我们之前没有密码，所以这里直接回车

Setting the root password ensures that nobody can log into the MariaDB
root user without the proper authorisation.

Set root password? [Y/n]					#是否要设置root密码
New password: 								#输入两次密码
Re-enter new password: 
Password updated successfully!
Reloading privilege tables..
 ... Success!


By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.

Remove anonymous users? [Y/n]  					#是否移除匿名用户，为了数据库安全起见，选择y

Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? [Y/n]				#是否允许root远程登录,要选择y，不然不能通过第三方软件进行登录

By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? [Y/n]			#是否要移除测试库，为了安全起见，选择y
- Dropping test database...
 ... Success!
 - Removing privileges on test database...
 ... Success!

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? [Y/n]					#是否要更新权限表，一定要选择更新 y
... Success!

Cleaning up...

All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.

Thanks for using MariaDB!

```



### 4.数据库登录

方法1：

[root@dongruan1 yum.repos.d]# mysql -uroot -p123456     # -u 后接用户   -p后接密码

方法2：

[root@dongruan1 ~]# mysql -uroot -p
Enter password:

数据库创建数据流程：

先建库--->建表--->插入数据

```
MariaDB [(none)]> create database dongruan1 ;
Query OK, 1 row affected (0.00 sec)

MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| dongruan1            |
+--------------------+
4 rows in set (0.00 sec)

MariaDB [(none)]> use dongruan1
Database changed
MariaDB [dongruan1]> show tables;
Empty set (0.00 sec)

MariaDB [dongruan1]> create table person(				#写法1
    -> id int,
    -> name varchar(15),
    -> math char(10),
    -> chinese char(10),
    -> english char(10),
    -> );

MariaDB [dongruan1]> create table person( id int, name varchar(15), math char(10), chinese char(10), english char(10) );										#写法2
Query OK, 0 rows affected (0.00 sec)

MariaDB [dongruan1]> show tables;
+-------------------+
| Tables_in_dongruan1 |
+-------------------+
| person            |
+-------------------+
1 row in set (0.00 sec)

MariaDB [dongruan1]> select * from person;
Empty set (0.00 sec)

MariaDB [dongruan1]> insert into person value(1,"lu",75,85,95);
Query OK, 1 row affected (0.01 sec)

MariaDB [dongruan1]> select * from person;
+------+------+------+---------+---------+
| id   | name | math | chinese | english |
+------+------+------+---------+---------+
|    1 | lu   | 75   | 85      | 95      |
+------+------+------+---------+---------+
1 row in set (0.00 sec)

```

### 5.使用第三方软件远程连接

![image-20220622105414160](https://note-1308251438.cos.ap-guangzhou.myqcloud.com/typora/202206221054222.png)

主要原因就是权限不够

```
MariaDB [dongruan1]> select user,host from mysql.user				#mysql.user是数据库的权限表
    -> ;
+------+-----------+
| user | host      |
+------+-----------+
| root | 127.0.0.1 |
| root | ::1       |
| root | localhost |
+------+-----------+
3 rows in set (0.00 sec)

MariaDB [dongruan1]> update mysql.user set host='%' where host='localhost';		
Query OK, 1 row affected (0.00 sec)								#更新root登录权限，%代表所有网段都可以登录
Rows matched: 1  Changed: 1  Warnings: 0

MariaDB [dongruan1]> select user,host from mysql.user;
+------+-----------+
| user | host      |
+------+-----------+
| root | %         |
| root | 127.0.0.1 |
| root | ::1       |
+------+-----------+
3 rows in set (0.00 sec)

MariaDB [dongruan1]> flush privileges;							#在数据库里修改了权限后，需要刷新一下权限
Query OK, 0 rows affected (0.00 sec)
```

![image-20220622105914832](https://note-1308251438.cos.ap-guangzhou.myqcloud.com/typora/202206221059876.png)





### 6.导出数据

```
[root@dongruan1 ~]# mysqldump -uroot -p -B dongruan1 >/opt/dongruan1.sql
Enter password: 
[root@dongruan1 ~]# ll /opt
总用量 4
-rw-r--r-- 1 root root 2115 6月  22 11:06 dongruan1.sql
```

