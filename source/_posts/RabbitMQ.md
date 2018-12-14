---
title: RabbitMQ
date: 2018-10-19 19:02:20
tags: RabbitMQ
---
#### RabbitMQ web控制台无法登录
http://localhost:15672/api/whoami 提示400错误。原因是浏览器缓存问题
后台错误提示`Malformed header. Please consult the relevant specification.`

#### 安装Docker版
```
docker run -d --name rabbitmq --publish 5671:5671  --publish 5672:5672 --publish 4369:4369 --publish 25672:25672 --publish 15671:15671 --publish 15672:15672 rabbitmq:management
```
web控制台在端口`15672`