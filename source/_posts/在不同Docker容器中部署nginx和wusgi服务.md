---
title: 在不同Docker容器中部署nginx和wusgi服务
date: 2018-11-15 15:41:43
tags: ["docker", "nginx", "uwsgi", "python"]
---
#### 单独的uwsgi服务
首先部署并测试uwsgi服务能否正常运行。这里是build一个image，然后进行测试。
* 编写一个简单的server程序
```python
def application(env, start_response):
    start_response('200 OK', [('Content-Type','text/html')])
    return [b"Hello World"]
```
* 配置uwsgi的启动脚本，需要注意的是服务器的地址要使用0.0.0.0，否则后面的nginx访问是走不通的，因为现在还没有配置nginx，提供http服务进行测试
```ini
[uwsgi]
http = 0.0.0.0:8001
wsgi-file = server.py
master = true
processes = 4
threads = 2
stats = 127.0.0.1:9191
```
* 打包成docker image，Dockerfile内容如下
```Dockerfile
# 将官方 Python 运行时用作父镜像
FROM python

# 将工作目录设置为 /app
WORKDIR /app

# 将当前目录内容复制到位于 /app 中的容器中
ADD . /app

# 安装 requirements.txt 中指定的任何所需软件包
RUN CC=gcc pip install uwsgi

# 在容器启动时运行 app.py
CMD ["uwsgi", "server.ini"]
```
* 启动打包的image，开放8001端口
```
docker run -p 8001:8001 -d uwsgiimage
```
* 在浏览器中访问localhost:8001

#### 提供nginx代理
* 从nginx到uwsgi传输数据用的是socket连接，先修改uwsgi的访问方式，改为socket。
```ini
[uwsgi]
socket = 0.0.0.0:8001    # uwsgi的监听地址，后面配置nginx会将80端口的请求转发到这个端口
wsgi-file = server.py
master = true
processes = 4
threads = 2
stats = 127.0.0.1:9191
```
* 启动nginx
```bash
docker run -p 80:80 -d nginx
```
* 进入nginx的container中修改配置文件以提供代理功能
```
docker exec -it 7caa4a9f7204 /bin/bash
```
* 提供uwsgi服务，并将地址指向uwsgi的container
```
# /etc/nginx/conf/default.conf
server {
    listen       80;
    server_name  localhost;

    #charset koi8-r;
    #access_log  /var/log/nginx/host.access.log  main;

    location / {
        #root   /usr/share/nginx/html;
        #index  index.html index.htm;
	include uwsgi_params;
	uwsgi_pass 172.17.0.5:8001;
    }
    ...
}
```
* 重启nginx，然后在浏览器中访问localhost即可看到输出

#### 绑定服务器地址
上面的步骤中有一个比较麻烦的点，每次需要查看uwsgi的地址，然后修改nginx的配置文件。可以通过dokcer的link参数来绑定uwsgi的地址。实际就是通过一个别名来动态获取目标container的ip。
* 启动uwsgi时添加name参数
```
docker run -p 8001:8001 --name uwsgi.app -d uwsgiimage
```
* 启动nginx的container时指定link参数到uwsgi的container
```
docker run -p 80:80 --link uwsgi.app -d nginx
```
* 修改配置文件，通过name指向uwsgi
```
server {
    listen       80;
    server_name  localhost;

    #charset koi8-r;
    #access_log  /var/log/nginx/host.access.log  main;

    location / {
        #root   /usr/share/nginx/html;
        #index  index.html index.htm;
	include uwsgi_params;
	uwsgi_pass uwsgi.app:8001;
    }
    ...
}
```
* 重启nginx，在浏览器中访问localhost

#### 使用Flask框架
* 如果要使用Flask框架，则需要将uwsgi默认访问的application修改为Flask的app
```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def index():
    return "<span style='color:red'>I am app 1</span>"
```
* 在uwsgi的配置文件中增加一个`callable`参数指向app
```
[uwsgi]
socket = 0.0.0.0:8001
wsgi-file = server.py
master = true
processes = 4
callable = app
threads = 2
stats = 127.0.0.1:9191
```