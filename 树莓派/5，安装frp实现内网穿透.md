---
title: 5，安装frp实现内网穿透
updated: 2022-02-20 09:03:18Z
created: 2022-02-19 04:10:40Z
latitude: 34.26350000
longitude: 108.92460000
altitude: 0.0000
---

# 5，安装frp实现内网穿透

> 为了实现内网穿透，需要一台公网的机器

我这里用的是腾讯云的轻量应用服务器

### 需要服务端部署frp的服务端程序

腾讯云服务器安装frp
https://cloud.tencent.com/developer/article/1481212

```
1. wget --no-check-certificate https://raw.githubusercontent.com/clangcn/onekey-install-shell/master/frps/install-frps.sh -O ./install-frps.sh
2. chmod 700 ./install-frps.sh
3. ./install-frps.sh install


============== Check your input ==============
You Server IP      : 162.14.71.36
Bind port          : 5443
kcp support        : true
vhost http port    : 80
vhost https port   : 443
Dashboard port     : 6443
Dashboard user     : admin
Dashboard password : FPXVYc7D
token              : B5D1jIvGYCVmjILa    // 客户端需要用到
tcp_mux            : true
Max Pool count     : 100
Log level          : info
Log max days       : 3
Log file           : enable

```

全过程自动化

```
Congratulations, frps install completed!
==============================================
You Server IP      : 162.14.71.36
Bind port          : 5443
KCP support        : true
vhost http port    : 7080
vhost https port   : 1443
Dashboard port     : 6443
token              : H6nhY14wRyQcyfqu
tcp_mux            : true
Max Pool count     : 50
Log level          : info
Log max days       : 3
Log file           : enable
==============================================
frps Dashboard     : http://162.14.71.36:6443/
Dashboard user     : admin
Dashboard password : TLfjWVds
==============================================
```

//使用以下命令进行管理，

```
frps status manage : frps {start|stop|restart|status|config|version}
Example:
  start: frps start
   stop: frps stop
restart: frps restart
```

### 安装frp客户端

#### 下载amd64架构的frp文件包：

```
// https://github.com/fatedier/frp/releases   手动下载

//看下你自己是什么系统

uname -a

//如下 则使用linux  arm 64的版本
//Linux raspberrypi 5.10.92-v8+ #1514 SMP PREEMPT Mon Jan 17 17:39:38 GMT 2022 aarch64 GNU/Linux


// 直接下载
sudo wget https://github.com/fatedier/frp/releases/download/v0.39.1/frp_0.39.1_linux_arm64.tar.gz
```

#### 先download先下载下来,然后用 SFTP上传大树莓派

然后用tar命令解压下载的压缩包：

```
sudo tar -zxvf frp_0.35.1_linux_arm64.tar.gz
```

编辑解压之后文件夹里面的  客户端 frpc.ini文件：

```
sudo nano frpc.ini
```

frpc.ini文件内容如下：

```
[common]
[common]
server_addr = 162.14.71.36
server_port = 5443
token = H6nhY14wRyQcyfqu


[ssh]
type = tcp
local_ip = 192.168.31.100
local_port = 22
remote_port = 2222
use_encryption = true

[pointainer]
type = tcp
local_ip = 192.168.31.100
local_port = 9000
remote_port = 9000
type = https
custom_domains = hellome.icu
use_encryption = true
use_gzip = true
```

设置frpc 客户端每次树莓派开机自启动

（1）创建系统服务

```
创建文件 /lib/systemd/system/frpc.service 
```

（2） 文件内容如下

```
frpc.service 
```

\[Unit\]
Description=frpc
After=network.target

\[Service\]
TimeoutStartSec=10
RestartSec=30s
Restart=on-failure
ExecStart=/usr/local/frp/frpc -c /usr/local/frp/frpc.ini
ExecReload=/usr/local/frp/frpc reload -c /usr/local/frp/frpc.ini
ExecStop=/bin/kill $MAINPID

\[Install\]
WantedBy=multi-user.target

保存后退出

—————————————
更新服务文件 sudo systemctl daemon-reload
开启 sudo systemctl start frpc
关闭 sudo systemctl stop frpc
重启 sudo systemctl restart frpc
查看状态 sudo systemctl status frpc
设置开机启动 sudo systemctl enable frpc
设置开机不启动 sudo systemctl disable frpc