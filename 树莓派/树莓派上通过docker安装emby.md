---
title: 树莓派上通过docker安装emby
updated: 2022-02-25 13:39:38Z
created: 2022-02-25 13:33:42Z
latitude: 34.26350000
longitude: 108.92460000
altitude: 0.0000
---

树莓派上通过docker安装emby

```
docker run -d --name emby-server --restart unless-stopped -p 8096:8096 -p 8920:8920 -p 1900:1900/udp emby/embyserver_arm64v8:latest
```