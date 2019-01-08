---
title: 使用nginx对kibana进行地址转发
date: 2018-12-19 17:45:43
tags: ["nginx", "kibana"]
---
现在有这么一个情况，在服务器上部署了elk的模块，但服务器只对外开放了80端口，想要访问kibana的dashboard时，需要使用nginx进行请求的转发。
目标是可以通过`http://server.com/elk`来访问kibana的dashboard。
首先是修改kibana的配置文件，增加如下内容
```
# kibana.yml
server.basePath: "/elk"
server.rewriteBasePath: false
```
然后是修改nginx的配置文件
```
# default.conf

upstream elk {
    server localhost:5601;
}

server {
    listen       80;
    server_name  localhost;

    location /elk/ {
        proxy_pass  http://kibana:5601/;
        rewrite ^/elk(.*)$ $1 break;
    }
}
```
这里是通过docker swarm来部署的服务，可以直接使用kibana作为host来访问服务，如果是分开部署的，需要确保转发地址的正确性。
现在启动这两个服务，即可通过`http://server.com/elk`来访问kibana的dashboard