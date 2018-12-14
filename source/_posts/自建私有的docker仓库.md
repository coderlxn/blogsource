---
title: 自建私有的docker仓库
date: 2018-12-12 17:49:59
tags: docker
---
* Ubuntu 16.04
* Docker version 18.09.0

Docker官方提供的docker hub功能很多，完全可以满足开发的需求，但在某些情况下需要将数据放在自己的服务器上。
所以需要自己新建一个docker仓库，官方已经提供了详细的[说明文档](https://docs.docker.com/registry/deploying/)，此处只是坐下简单的总结。
首先安装docker-ce，安装[官网提供的安装方式](https://docs.docker.com/install/linux/docker-ce/ubuntu/#upgrade-docker-ce)安装即可。

可以通过简单的命令在本地建一个仓库
```bash
docker run -d -p 5000:5000 --restart=always --name registry registry:2
```
从命令中可以看出实际是启用了一个docker container来作为服务。
如果需要外部访问功能，就需要SSL证书。
在默认情况下会新建一个volume来存储docker文件，也可以自己来指定。
```bash
docker run -d \
  --restart=always \
  --name registry \
  -v `pwd`/conf:/certs \
  -e REGISTRY_HTTP_ADDR=0.0.0.0:443 \
  -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/web.crt \
  -e REGISTRY_HTTP_TLS_KEY=/certs/web.key \
  -p 443:443 \
  registry
```