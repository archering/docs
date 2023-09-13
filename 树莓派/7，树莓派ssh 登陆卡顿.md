---
title: 7，树莓派ssh 登陆卡顿
updated: 2022-02-20 11:50:45Z
created: 2022-02-19 07:20:30Z
latitude: 34.26350000
longitude: 108.92460000
altitude: 0.0000
---

# 7，树莓派ssh 登陆卡顿

输入命令有明显的滞后现象

1， 修改location信息 为china

2，如果你使用了铝合金外壳，WIFI信号必然受影响。最简单的检查办法就是ping raspberrypi.local看看延时


3， 解决办法

（1）sudo nano /etc/ssh/sshd_config	末尾添加一行:

	IPQoS cs0 cs0

	ctrl+x, 按y，回车即保存退出。
	然后

         service sshd restart

   重启sshd


遇到以下情况

==== AUTHENTICATING FOR org.freedesktop.systemd1.manage-units ===
Authentication is required to restart 'ssh.service'.
Authenticating as: ,,, (pi)
Password: 

8g  那个输入  zhangdong@Rasp
2g  那个输入  zhangdongberry




IPQoS的解释
对应的服务 IPv4优先级/ EXP / 802.1P DSCP(二进制) DSCP(十进制) TOS(十六进制) 应用
BE 0 0 0 0 Internet
AF1 Green 1 1010 10 28 Leased Line
AF2 Green 2 10010 18 48 IPTV VOD
AF3 Green 3 11010 26 68 IPTV Broadcast
AF4 Green 4 100010 34 88 NGN/3G Singaling
EF 5 101110 46 B8 NGN/3G voice
CS6 6 110000 48 C0 Protocol
CS7 7 111000 56 E0 Protocol


（2）关闭Wifi的PowerSaving: 
     创建一个新文件 /etc/modprobe.d/8192cu.conf      sudo nano /etc/modprobe.d/8192cu.conf,添加如下内容

# Disable power saving
options 8192cu rtw_power_mgnt=0 rtw_enusbss=1 rtw_ips_mode=1

然后输入sudo reboot重启

（3）编辑 /etc/ssh/sshd_config关闭dns 

	#添加一行
	useDNS no
	然后service sshd restart重启sshd



