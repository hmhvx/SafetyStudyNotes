```
curl -o /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo
sed -i 's/SELINUX=enforcing/SELINUX=disabled/' /etc/selinux/config &> /dev/null
setenforce 0
systemctl stop firewalld
systemctl disable firewalld
iptables -F
systemctl stop NetworkManager &> /dev/null
systemctl disable NetworkManager &> /dev/null
echo 'php' > /etc/hostname
yum install -y gcc gcc-c++ libxml2-devel libcurl-devel openssl-devel bzip2-devel vim wget net-tools
wget ftp://mcrypt.hellug.gr/pub/crypto/mcrypt/libmcrypt/libmcrypt-2.5.7.tar.gz
tar -zxvf libmcrypt-2.5.7.tar.gz
cd libmcrypt-2.5.7/
./configure --prefix=/usr/local/libmcrypt && make && make install
cd ..
wget http://cn2.php.net/distributions/php-5.6.27.tar.gz
tar -zxvf php-5.6.27.tar.gz
cd php-5.6.27/
./configure --prefix=/usr/local/php5.6 --with-mysql=mysqlnd --with-pdo-mysql=mysqlnd --with-mysqli=mysqlnd --with-openssl --enable-fpm --enable-sockets --enable-sysvshm --enable-mbstring --with-freetype-dir --with-jpeg-dir --with-png-dir --with-zlib --with-libxml-dir=/usr --enable-xml --with-mhash --with-mcrypt=/usr/local/libmcrypt --with-config-file-path=/etc --with-config-file-scan-dir=/etc/php.d --with-bz2 --enable-maintainer-zts && make && make install
groupadd -g 1001 nginx
useradd -u 900 nginx -g nginx -s /sbin/nologin
cp php.ini-production /etc/php.ini
cp sapi/fpm/init.d.php-fpm /etc/init.d/php-fpm
chmod +x /etc/init.d/php-fpm
chkconfig --add php-fpm
chkconfig php-fpm on
cp /usr/local/php5.6/etc/php-fpm.conf.default /usr/local/php5.6/etc/php-fpm.conf
修改内容如下：

25 pid = run/php-fpm.pid
149 user = nginx
150 group = nginx
164 listen = 192.168.10.12:9000  #PHP主机的IP地址
235 pm.max_children = 50
240 pm.start_servers = 5
245 pm.min_spare_servers = 5
250 pm.max_spare_servers = 35

service php-fpm start




```
