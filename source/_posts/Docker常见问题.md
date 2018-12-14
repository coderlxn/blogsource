---
title: Docker常见问题
date: 2018-11-02 10:39:37
tags: docker
---
#### docker save 和 docker load 丢失tag信息
如果docker save时使用的是hash字段，导出的文件中不会包含tag信息，再次load后会导致tag丢失。
可以使用tag名称来导出数据
```bash
docker save xmppclient:6 -o xmppclient.tar
# 导入时会恢复tag信息
docker load -i xmppclient.tar
Loaded image: xmppclient:6
```
#### docker container变更开放端口
通过`docker run`命令创建一个container时，可以指定要放开的端口。在container创建完成后如果要修改开发的端口，则不能直接把端口开放出来。
一个可行的方案是通过`docker commit`将当前的container保存为image，然后在此基础上创建新的container