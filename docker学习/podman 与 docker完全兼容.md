---
title: podman 与 docker完全兼容
updated: 2023-09-07 03:39:48Z
created: 2023-09-07 03:38:17Z
latitude: 34.34157500
longitude: 108.93977000
altitude: 0.0000
---

# podman 与 docker完全兼容

实际使用中只需要把docker替换为podman即可

```
#  docker run -d 8080:80 nginx:alpine
   podman run -d 8080:80 nginx:alpine
  
```