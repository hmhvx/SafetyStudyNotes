### 今日目标：

1.使用nginx实现负载均衡

2.了解公司架构



上传安装包到/opt

```
[root@dongruan1 opt]# unzip Discuz-X3.4-SC-UTF8-v20210816.zip -d discuz
[root@dongruan1 opt]# cd discuz/
[root@dongruan1 opt]# mkdir /usr/local/nginx/html/discuz
[root@dongruan1 discuz]# mv upload/* /usr/local/nginx/html/discuz/
```

打开浏览器输入地址：

![image-20220707192827482](https://note-1308251438.cos.ap-guangzhou.myqcloud.com/typora/202207071928575.png)

[root@dongruan1 install]# chmod a+w -R data uc_client uc_server config



安装成功后

[root@dongruan1 install]# rm -rf index.php





















### 1.负载均衡

如果公司业务只集中一台机器上面，会容易造成卡顿，web服务器也是有承载上限的，所以我们能不能把压力均分



准备两台机器

注：需要环境初始化，一定要关闭防火墙，关闭seliunx

1.两台机都安装apache服务

```
[root@qingzhi1 conf]# yum install -y httpd
[root@qingzhi1 conf]# vim /etc/httpd/conf/httpd.conf
修改：95 #ServerName www.example.com:80
为：ServerName 本机IP:80								#更快的启动apache服务
[root@qingzhi1 conf]# cd /var/www/html/
[root@qingzhi1 html]# ll
总用量 0
[root@qingzhi1 html]# echo this is a good day >index.html			#编辑首页内容
[root@qingzhi1 html]# systemctl start httpd				
```

在浏览器输入http://IP地址，以验证是否成功运行

![image-20220624165521784](https://note-1308251438.cos.ap-guangzhou.myqcloud.com/typora/202207051831962.png)

![image-20220624165536414](https://note-1308251438.cos.ap-guangzhou.myqcloud.com/typora/202207051831967.png)

2.修改nginx配置文件

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
upstream htmlserver{					#负载均衡组
server 192.168.245.135:80;
server 192.168.245.136:80;
}
    server {
        listen       8000;						#修改端口为8000
        server_name  localhost;
        location / {
            root   html;
            index  index.html index.htm;
 	location / {
            root   html;
            index  index.html index.htm;       
        if ($request_uri ~* \.html$){				#当访问网址是以html结尾的就会跳转到下述条件
	 	proxy_pass http://htmlserver;				#重定向到上述的负载均衡组
	}   	 
     }
}
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
}

```

重新加载配置文件：

```
[root@qingzhi1 conf]# nginx -s reload
```

在浏览器输入http://ip地址:8000/index.html，可以发现实现了访问同一个网址时，会跳转到两台机子上

![image-20220624165952710](https://note-1308251438.cos.ap-guangzhou.myqcloud.com/typora/202207051831057.png)

![image-20220624170003251](https://note-1308251438.cos.ap-guangzhou.myqcloud.com/typora/202207051831049.png)

