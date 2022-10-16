## Dokcer 安装

### 自动安装

> curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun

### 手动安装

># 先对旧版本进行卸载
>sudo apt-get remove docker docker-engine docker.io containerd runc
>#进行系统或者工具的更新
>apt-get update && apt-get upgrade -y && apt-get dist-upgrade -y
>#清理更新缓存
>apt-get clean
>#进行apt安装Docker
>apt-get install docker.io -y

## Docker 加速

[加速获取地址](https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors)

>sudo mkdir -p /etc/docker 
>sudo tee /etc/docker/daemon.json <<-'EOF' 
>{ 
>	"registry-mirrors": ["https://o3qxg9m2.mirror.aliyuncs.com"] 
>} 
>EOF 
>sudo systemctl daemon-reload 
>sudo systemctl restart docker

## Docker 使用

>#系统命令
>systemctl start docker				# 启动docker 
>systemctl stop docker				# 停止docker 
>systemctl restart docker			# 重启docker 
>systemctl enable docker				# 设置docker开机自启
>#基本命令
>docker version					# 查看docker版本 
>docker info							# 查看docker详细信息 
>docker --help						# 查看docker命令
>#镜像命令
>docker images						# 查看docker镜像列表 
>docker images -a					# 列出本地所有镜像 
>docker images --digests				# 显示镜像的摘要信息 
>docker search redis					# 从DockerHub上查找redis镜像
>docker pull redis					# 从Docker Hub上下载redis镜像
>docker rmi 373f0984b070				# 删除IMAGE ID为373f0984b070的镜像
>#运行命令
># -p 6379:6379	端口映射：前表示主机部分,后表示容器部分
># -d	在后台运行容器（不进入终端）并打印容器ID/容器名
># --name myredis表示自定义容器名为myredis
>docker run -d -p 6379:6379 --name myredis redis:latest# 根据镜像创建并运行容器
># 容器命令
>docker container ls 或 docker ps				# 查看正在运行的容器 
>docker container ls -a 或 docker ps -a			# 列出所有容器 
>docker container start 容器ID 或 容器名称		# 启动容器 
>docker start 容器ID 或 容器名称					# 启动容器 
>docker container stop 容器ID 或 容器名称			# 停止容器 
>docker stop 容器ID 或 容器名称					# 停止容器 
>docker container rm 容器ID 或 容器名称			# 删除容器 
>docker rm 容器ID 或 容器名称						# 删除容器 
>docker container logs -f 容器ID 或 容器名称		# 查看容器日志 
>docker exec -it name /bin/bash 					# 进入name（容器名/id）中开启交互式的终端，exit退出

