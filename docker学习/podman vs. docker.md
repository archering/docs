---
title: podman vs. docker
updated: 2023-09-04 11:36:27Z
created: 2023-09-04 11:32:55Z
latitude: 35.11449200
longitude: 109.09997500
altitude: 0.0000
---

# podman vs. docker

docker是一个client-server架构，所以dokcer容器的运行需要有一个运行中的进程来支撑，这个进程由deamon守护。

podman是一个deamon-less设计，这并不是说他的运行不需要服务支撑，所以podman需要借助其他工具来管理他的服务以支持运行态的容器。比如systemd

