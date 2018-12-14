---
title: nginx转发http和https
date: 2018-12-14 17:26:06
tags: nginx
---
现在有一个网站在4567端口提供http服务。为了提高安全性，转换为https的服务。
现在采用的方式是nginx作为代理服务，nginx自然不必多说，简单，强大。
将安全证书下载到cert文件夹下，分别是`nginx.crt`和`nignx.key`，新建配置文件`default.conf`内容如下
```
# 提供https服务，将ssl验证后的结果转发到实际的服务上
server { 
    listen 443 ssl;
    server_name www.example.com;    

    ### SSL cert files ### 
    ssl on;
    ssl_certificate /usr/cert/nginx.crt;
    ssl_certificate_key /usr/cert/nginx.key;
    ssl_session_timeout 5m;

    location / {
        proxy_pass http://127.0.0.1:4567;
    }
}

# 将普通的http服务转发到443端口进行安全校验
server {
    listen 80 default_server;
    listen [::]:80 default_server;
    server_name _;
    return 301 https://$host$request_uri;
}
```
现在使用的是docker来创建nginx服务，省去的编译nginx的麻烦，而且基于alpine的nginx镜像只有不到18M。
创建服务的命令如下
```
docker run -d -p 443:443 -p 80:80 -v `pwd`/cert:/usr/cert -v `pwd`/conf/default.conf:/etc/nginx/conf.d/default.conf --name nginx --rm nginx:alpine
```
主要是开发443和80两个端口，然后将安全证书和配置文件挂载到container上