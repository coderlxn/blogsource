---
title: Python Locust并发压力测试
date: 2018-12-10 10:05:59
tags: ["Python", "Locust"]
---
#### Locust简介
Locust是一个Python语言开发的测试工具，主要用于web网页或后台接口的测试。更具用户编写的测试方法，创建大量的测试协程来发送网络请求，测试服务器对于并发请求的响应能力。
#### 安装
从github上clone源码 https://github.com/locustio/locust ，进到项目目录使用命令`python3 setup.py install`来安装依赖的库和当前项目。
#### 功能说明
使用python编写测试模板，启动测试程序后指定并发的请求的数量，然后开始测试
#### 示例
```python
from locust import HttpLocust, TaskSet, task


def index(l):
    l.client.get("/")

def stats(l):
    l.client.get("/info")

class UserTasks(TaskSet):
    # one can specify tasks like this
    tasks = [index, stats]
    
    # but it might be convenient to use the @task decorator
    @task
    def page404(self):
        self.client.get("/hello")
    
class WebsiteUser(HttpLocust):
    """
    Locust user class that does requests to the locust web server running on localhost
    """
    host = "http://127.0.0.1:5000"
    min_wait = 2000
    max_wait = 5000
    task_set = UserTasks
```
使用命令`locust -f locustfile.py --host=http://www.126.com`
