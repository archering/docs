---
title: 安装docker
updated: 2022-01-24 13:21:06Z
created: 2022-01-24 06:36:52Z
latitude: 34.26350000
longitude: 108.92460000
altitude: 0.0000
---

# 安装docker
> linxu 环境安装 docker 

## 第一步 准备

```
sudo apt-get update

sudo apt-get install  apt-transport-https ca-certificates  curl gnupg2 lsb-release software-properties-common

```

## 第二步 安装

>docker带ce和不带ce的区别

>CE( Community Edition)是社区版，简单理解是免费使用，提供小企业与小的IT团队使用,希望从Docker开始，并尝试基于容器的应用程序部署。

>EE(Docker Enterprise Edition)是企业版，收费。提供功能更强。适合大企业与打的IT团队。为企业开发和IT团队设计，他们在生产中构建、交付和运行业务关键应用程序


```
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun

```

## 测试

```
sudo docker run hello-world
```
看到如下表示成功
```
root@VM-0-13-debian:~# sudo docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
2db29710123e: Pull complete 
Digest: sha256:975f4b14f326b05db86e16de00144f9c12257553bba9484fed41f9b6f2257800
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/

```

## 设置docker随系统自启动

```
sudo systemctl enable docker
```


## 创建一个 Volume

>考虑数据持久化的问题，创建一个 Volume

```
# 创建名为 joplin 的 volume
docker volume create joplin
```

## 查看是否创建成功

```
docker volume inspect joplin
```

看到如下表示成功

```
root@VM-0-13-debian:/home/app/joplin# docker volume inspect joplin
[
    {
        "CreatedAt": "2022-01-24T14:57:45+08:00",
        "Driver": "local",
        "Labels": {},
        "Mountpoint": "/var/lib/docker/volumes/joplin/_data",
        "Name": "joplin",
        "Options": {},
        "Scope": "local"
    }
]
```


## 安装 PostgreSQL

```
docker pull postgres:latest
```

## 建立容器并后台运行

>  说明： 下面参数的意义
>  joplin_postgres为你的容器名字

> pwd39zd改为你要设置的密码。

> dv_pgdata为docker volume的名称，/var/lib/docker/volumes/joplin/_data对应docker volume的实际路径，这两个不存在时会自动创建。

> 这里 /var/lib/docker/volumes/joplin/_data 对应docker volume inspect joplin 这一步里面的Mountpoint

```
docker run  --restart=always --name joplin_postgres -v dv_pgdata:/var/lib/docker/volumes/joplin/_data -e POSTGRES_PASSWORD=pwd39zd -p 5432:5432 -d postgres:latest
```

## 进入容器

```
docker exec -it joplin_postgres bash
```

## 建数据库，建表

```
psql -U postgres
```

```
CREATE USER root WITH PASSWORD 'pwd39zd';
CREATE DATABASE joplin OWNER root;
GRANT ALL PRIVILEGES ON DATABASE joplin to root
```

输入命令

`\q`   退出

退到服务器的ssh

ctrl + d


## 下载joplin服务端镜像并运行

#### 拉取安装joplin 服务器程序
```
docker pull joplin/server:latest
```

#### 设置配置文件

```
	nano /home/app/joplin/.env
```
内容如下
```
APP_BASE_URL=https://162.14.71.36
APP_PORT=22300
//这个APP_BASE_URL比较重要，要配置成未来用以访问的url，否则会报错误
//我这里写的是我自己的公网服务器地址
```


#### 建立容器并后台运行

```
docker run -d --name joplin_server -v joplin:/home/joplin --env-file /home/app/joplin/.env -p 22300:22300 joplin/server:latest

```

## 查看运行状态

`docker ps -a`

## 重启容器

```
docker container restart joplin_server
```

## 查看那个在用

```
#查看nginx的进程
ps -ef|grep nginx

#查看端口号
lsof -i:80

```

## 配置nginx 反向代理

检查 nginx配置文件写的对不对 

```
 nginx -t
```

重新加载nginx 配置文件

```
nginx -s reload
```