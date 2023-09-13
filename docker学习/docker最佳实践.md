---
title: docker最佳实践
updated: 2023-09-04 12:13:35Z
created: 2023-09-04 12:01:06Z
latitude: 35.11449200
longitude: 109.09997500
altitude: 0.0000
---

# docker最佳实践

1， 一个容器只运行一个进程
> 将多个应用解耦到多个不同容器中，保证容器的横向扩展和复用。
> 距离：一个web应用应该包含： web应用，数据库，缓存

2，指定WORKDIR 
使用WORKDITR 来代替类似于 RUN cd ...等指令

