---
title: openssl制作自签名证书
updated: 2023-09-08 02:27:23Z
created: 2023-09-07 12:27:14Z
latitude: 34.34157500
longitude: 108.93977000
altitude: 0.0000
---

# openssl制作自签名证书
> https://blog.csdn.net/m0_61747530/article/details/131054552

# 1、生成CA证书

### 1.1 生成CA机构的私钥
```
openssl genrsa -out ca.key 2048
```
### 1.2 生成CA机构自己的证书申请文件
```
openssl req -new -key ca.key -out ca.csr
```
### 1.3 生成自签名证书

#CA机构用自己的私钥和证书申请文件生成自己签名的证书，俗称自签名证书，这里可以理解为【根证书】。

> 生成CA机构自签名根证书；  基于ca.key 私钥和ca.csr请求文件， 生成ca.crt根证书  
```
openssl x509 -req -in ca.csr -signkey ca.key -out ca.crt -days 365
```

# 3、生成服务端的证书

### 3.1 生成服务器私钥key
```
openssl genrsa -out server.key 2048
```
### 3.2 根据服务器私钥文件生成证书请求文件csr。
这个文件中会包含申请人的一些信息，所以执行下面这行命令过程中需要用户在命令行输入一些用户信息，随便填写，一路回车即可。 
```
openssl req -new -key server.key -out server.csr
```
&nbsp;

注意： 给网站用的证书需要注意 Common Name  和域名一致

```
Country Name (2 letter code) \[AU\]:CN
State or Province Name (full name) \[Some-State\]:
Locality Name (eg, city) \[\]:
Organization Name (eg, company) \[Internet Widgits Pty Ltd\]:NGOO
rganizational Unit Name (eg, section) \[\]:
Common Name (e.g. server FQDN or YOUR name) \[\]:helloe.siteEmail Address \[\]:ericeverme@outlook.com
```

### 3.3 CA机构给服务端证书签名

#根据CA机构的自签名证书ca.crt或者叫根证书生、CA机构的私钥ca.key、服务器的证书申请文件server.csr生成服务端证书。

>基于 (1) CA机构的私钥，(2) CA机构的根证书， 和(3)服务端的请求文件server.csr  生成服务端证书 server.crt  
```
openssl x509 -req -CA ca.crt -CAkey ca.key -CAcreateserial -in server.csr -out server.crt -days 365
```
# 5、生成客户端证书

### 5.1
```
openssl genrsa -out client.key 2048  
```
### 5.2
```
openssl req -new -key client.key -out client.csr
```
### 5.3 CA机构给客户端证书签名
```
openssl x509 -req -CA ca.crt -CAkey ca.key -CAcreateserial -in client.csr -out client.crt -days 365
```

# 7、查看证书的有效期

openssl x509 -in server.crt -noout -dates

# 8、如何检查openssl生成的pem的证书与私钥是否匹配

### 方式一

# openssl x509 -noout -modulus -in server.crt | openssl md5

# openssl rsa -noout -modulus -in server.key | openssl md5

### 方法二

# openssl pkey -in server.key -pubout -outform pem | sha256sum

# openssl x509 -in server.crt -pubkey -noout -outform pem | sha256sum


总结：首先需要有一个机构CA, 这个机构要有自己的私钥ca.key , 然后这个机构需要被认证有自己的公章 ca.cert（根证书）


生成服务端证书， 假设你在一个叫 hello.com的公司上班， 你需要生成自己的证书

那么你的服务端首先需要有自己的server.key 私钥

然后你需要写一个请求文件 server.csr 

最后需要通过CA机构给你盖个章