

### 7.通过ip，端口，域名使用nginx服务

了解服务运行的本质，要去了解配置文件

/usr/local/nginx/conf/nginx.conf是主配置文件

在linux里面，服务只要修改完配置文件，一定要重新加载配置文件或重启服务

默认的配置文件如下

```
worker_processes  1;
events {
    worker_connections  1024;
}
http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;

    server {
        listen       80;								#监听的端口号
        server_name  localhost;					#服务名：可以用IP或域名代替
        location / {
            root   html;
            index  index.html index.htm;
        }
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }


}

```

重新加载配置文件

nginx -s reload



修改IP，配置如下：

```
server {
        listen       80;								#监听的端口号
        server_name  192.168.245.135;					#服务名：可以用IP或域名代替
        location / {
            root   html;								#内容家目录
            index  index.html index.htm;				#识别的首页文件格式
        }
```

修改端口，配置如下：

```
server {
        listen       8000;								#监听的端口号
        server_name  192.168.245.135;					#服务名：可以用IP或域名代替
        location / {
            root   html;
            index  index.html index.htm;
        }
```

修改域名，配置如下：

```
server {
        listen       80;								#监听的端口号
        server_name  class.qingzhi.com;					#服务名：可以用IP或域名代替
        location / {
            root   html;
            index  index.html index.htm;
        }
```

当修改域名时，还需要修改宿主机的本地dns文件

C:\Windows\System32\drivers\etc\hosts文件是windows操作系统本地的dns文件

```
打开并添加一项：
192.168.245.135 class.qingzhi.com		#当去访问class.qingzhi.com这一个地址时，会去找到192.168.245.135这一台服务器
```



### 8.使用nginx搭建ucenter项目

需要用到 lnmp 架构 = liunx+nginx+mysql+php 

代码需要在这种架构上运行

1.安装php

```
[root@dongruan1 conf]# yum install -y php php-fpm
[root@dongruan1 conf]# systemctl start php-fpm    #启运php解释器
[root@dongruan1 conf]# systemctl status php-fpm
```

修改nginx.conf，完整配置文件如下图：

```
worker_processes  1;
events {
    worker_connections  1024;
}
http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;

server {
        listen 80;
        server_name 192.168.245.135;
        location ~* \.php$ {
                root /usr/local/nginx/html;					#内容家目录
                fastcgi_pass 127.0.0.1:9000;
                fastcgi_index index.php;
                include fastcgi.conf;
        
       }
                
                
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }   
```



出现下图证明nginx与php已经能正常联系

![](https://note-1308251438.cos.ap-guangzhou.myqcloud.com/typora/202207051830192.png)





安装ucenter

上传UCenter_1.5.2_SC_UTF8.zip到/opt

```
[root@dongruan1 opt]# yum install -y unzip      #安装解压工具
[root@dongruan1 opt]# mkdir ucenter
[root@dongruan1 opt]# unzip UCenter_1.5.2_SC_UTF8.zip -d /opt/ucenter
[root@dongruan1 opt]# cd /opt/ucenter
[root@dongruan1 ucenter]# mv upload/* /usr/local/nginx/html/ucenter/

```

到浏览器输入：http://服务器IP/ucenter/install/index.php，如下图：

![image-20220623160429088](https://note-1308251438.cos.ap-guangzhou.myqcloud.com/typora/202207051830566.png)

按要求修改配置：

```
vim /etc/php.ini
修改：211 short_open_tag = Off
为：short_open_tag = On

修改后需要重启下php,nginx
[root@dongruan1 install]# nginx -s stop				#关闭	nginx	
[root@dongruan1 install]# nginx						#启动 nginx
[root@dongruan1 install]# systemctl restart php-fpm		#重启php
```

需要修改权限与依赖

![image-20220623160947933](https://note-1308251438.cos.ap-guangzhou.myqcloud.com/typora/202207051830709.png)

解决方案：

```
1.修改权限
[root@dongruan1 ucenter]# cd  /usr/local/nginx/html/ucenter
[root@dongruan1 ucenter]# chmod a+w -R ./data			#-R：为递归，a+w：所有人都添加可写权限

2.添加mysql模块联接
[root@dongruan1 ucenter]# yum install -y php-mysql		#安装php与mysql的依赖包
[root@dongruan1 ucenter]# systemctl restart php-fpm		#需重启php
```

操作完成后如下图：

![image-20220623161743945](https://note-1308251438.cos.ap-guangzhou.myqcloud.com/typora/202207051830647.png)

下一步：把ucenter的库创建在数据库上

数据库密码与管理员密自填：

![image-20220623161926779](https://note-1308251438.cos.ap-guangzhou.myqcloud.com/typora/202207051830731.png)



报错：

![image-20220623162116731](https://note-1308251438.cos.ap-guangzhou.myqcloud.com/typora/202207051830763.png)

主要原因：数据库不允许root远程登录

```
mysql> select user,host from mysql.user;
+---------------+-----------+
| user          | host      |
+---------------+-----------+
| mysql.session | localhost |
| mysql.sys     | localhost |
| root          | localhost |					#root只允许本地登录
+---------------+-----------+
3 rows in set (0.00 sec)

mysql> update mysql.user set host='%' where user='root';
Query OK, 1 row affected (1.61 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> select user,host from mysql.user;
+---------------+-----------+
| user          | host      |
+---------------+-----------+
| root          | %         |					#此时root可以任意网段登录
| mysql.session | localhost |
| mysql.sys     | localhost |
+---------------+-----------+
3 rows in set (0.01 sec)

mysql> flush privileges;
Query OK, 0 rows affected (0.01 sec)
```

此时，返回界面，点击上一步，再重新执行下一步



最后一步，以创始人身份登录ucenter

注：如果验证码不清楚，请刷新一下网页

![](https://note-1308251438.cos.ap-guangzhou.myqcloud.com/typora/202207051830822.png)

主界面如下：

![image-20220623162723262](https://note-1308251438.cos.ap-guangzhou.myqcloud.com/typora/202207051830399.png)

此时创建一个用户，为接下来安装ucenter-home做准备

![image-20220623164343585](https://note-1308251438.cos.ap-guangzhou.myqcloud.com/typora/202207051830384.png)



### 9.安装ucenter-home

上传UCenter_Home_2.0_SC_UTF8.zip到/opt

```
[root@dongruan1 opt]# mkdir ucenter-home
[root@dongruan1 opt]# unzip UCenter_1.5.2_SC_UTF8.zip -d /opt/ucenter-home
[root@dongruan1 opt]# cd /opt/ucenter-home
[root@dongruan1 ucenter-home]# mkdir /usr/local/nginx/html/ucenter-home
[root@dongruan1 ucenter-home]# mv upload/* /usr/local/nginx/html/ucenter-home/

```

到浏览器输入：http://服务器IP/ucenter-home/install/index.php，如下图：

![image-20220623163215553](https://note-1308251438.cos.ap-guangzhou.myqcloud.com/typora/202207051830376.png)

解决方法：

```
[root@dongruan1 ucenter-home]# cd /usr/local/nginx/html/ucenter-home
[root@dongruan1 ucenter-home]# cp config.new.php config.php
```

![image-20220623163438159](https://note-1308251438.cos.ap-guangzhou.myqcloud.com/typora/202207051830318.png)

解决方法：

```
[root@dongruan1 ucenter-home]# chmod a+w config.php
```

![image-20220623163558268](https://note-1308251438.cos.ap-guangzhou.myqcloud.com/typora/202207051830591.png)

解决方法：

```
[root@dongruan1 ucenter-home]# chmod a+w -R uc_client data attachment
```

![image-20220623163753730](https://note-1308251438.cos.ap-guangzhou.myqcloud.com/typora/202207051830528.png)

![image-20220623163909741](https://note-1308251438.cos.ap-guangzhou.myqcloud.com/typora/202207051831209.png)

![](https://note-1308251438.cos.ap-guangzhou.myqcloud.com/typora/202207051831213.png)

![image-20220623164025351](https://note-1308251438.cos.ap-guangzhou.myqcloud.com/typora/202207051831291.png)

此时，需要填在ucenter创建的用户名与密码，并开通管理员空间

![image-20220623164113336](https://note-1308251438.cos.ap-guangzhou.myqcloud.com/typora/202207051831271.png)



![image-20220623164503245](https://note-1308251438.cos.ap-guangzhou.myqcloud.com/typora/202207051831473.png)

小试一下，进入我的空间，发布日志：

![image-20220623164854936](https://note-1308251438.cos.ap-guangzhou.myqcloud.com/typora/202207051831471.png)





10.作业：

完成ucenter与ucenter-home 的搭建，并截图：要求地址栏http://域名（需要是你的名字拼音全写）举例如下

![image-20220623165152501](https://note-1308251438.cos.ap-guangzhou.myqcloud.com/typora/202207051831636.png)
