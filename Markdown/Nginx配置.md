```
curl -o /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo
sed -i 's/SELINUX=enforcing/SELINUX=disabled/' /etc/selinux/config &> /dev/null
setenforce 0
systemctl stop firewalld
systemctl disable firewalld
iptables -F
systemctl stop NetworkManager &> /dev/null
systemctl disable NetworkManager &> /dev/null
echo 'nginx' > /etc/hostname
yum install -y gcc gcc-c++ openssl-devel zlib-devel zlib pcre-devel vim wget net-tools
groupadd -g 1001 nginx
useradd -u 900 nginx -g nginx -s /sbin/nologin
cd /usr/local/src
wget http://nginx.org/download/nginx-1.12.2.tar.gz
tar -zxvf nginx-1.12.2.tar.gz
cd nginx-1.12.2
./configure --prefix=/usr/local/nginx --with-http_dav_module --with-http_stub_status_module --with-http_addition_module --with-http_sub_module --with-http_flv_module --with-http_mp4_module --with-http_ssl_module --with-http_gzip_static_module --user=nginx --group=nginx && make && make install
ln -s /usr/local/nginx/sbin/nginx /usr/local/sbin/
nginx -t
nginx


fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;

```