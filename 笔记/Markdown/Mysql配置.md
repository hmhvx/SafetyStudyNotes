```
curl -o /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo
sed -i 's/SELINUX=enforcing/SELINUX=disabled/' /etc/selinux/config &> /dev/null
setenforce 0
systemctl stop firewalld
systemctl disable firewalld
iptables -F
systemctl stop NetworkManager &> /dev/null
systemctl disable NetworkManager &> /dev/null
echo 'mysql' > /etc/hostname
yum remove -y mariadb-libs

tar -zxvf mysql-5.7.25-linux-glibc2.12-x86_64.tar.gz
mv mysql-5.7.25-linux-glibc2.12-x86_64 /usr/local/mysql
cd /usr/local/mysql/
echo "export PATH=\$PATH:/usr/local/mysql/bin" >> /etc/profile
source /etc/profile
mkdir data
mkdir log
groupadd mysql && useradd -r -g mysql -s /bin/false mysql
[root@mysql mysql]# vim /etc/my.cnf
[root@mysql mysql]# cat /etc/my.cnf
[client]
socket=/usr/local/mysql/mysql.sock
[mysqld]
basedir=/usr/local/mysql
datadir=/usr/local/mysql/data
pid-file=/usr/local/mysql/data/mysqld.pid
socket=/usr/local/mysql/mysql.sock
log_error=/usr/local/mysql/log/mysql.err

chmod 750 data/ && chown -R mysql . && chgrp -R mysql . && bin/mysqld --initialize --user=mysql
cp /usr/local/mysql/support-files/mysql.server /etc/init.d/mysqld && service mysqld start
cat /usr/local/mysql/log/mysql.err | grep password
mysql -uroot -p"=dm(zM-_q1R4"
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'jaking';
Query OK, 0 rows affected (0.00 sec)
mysql> flush privileges;
Query OK, 0 rows affected (0.00 sec)
mysql> exit
Bye


```