---
title: 进入docker容器
updated: 2023-09-04 11:54:15Z
created: 2023-09-04 11:52:12Z
latitude: 35.11449200
longitude: 109.09997500
altitude: 0.0000
---

# 进入docker容器

```
docker exec -it <container id> /bin/bash

# -it参数：容器的 Shell 映射到当前的 Shell，然后你在本机窗口输入的命令，就会传入容器。

# /bin/bash：容器启动以后，内部第一个执行的命令。这里是启动 Bash，保证用户可以使用 Shell。
```