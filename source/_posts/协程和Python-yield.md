---
title: 协程和Python yield
date: 2018-12-10 10:14:12
tags: Python
---
#### 简介
协程是在一个线程中并发执行多个函数的调用，同一个时刻只有一个方法在运行。
协程没有线程切换的开销，访问文件等资源时不需要加锁，执行效率比线程高，尤其是需要很多个线程的情况。

#### 示例
```python
def consumer():
    r = ''
    while True:
        n = yield r  # 实际执行时有三个步骤：1. return 一个值，2. pause，3. 赋值给n  通过send方式调用，赋值给n的时候实际是将send的参数赋值给了n
        if not n:
            return
        print('[CONSUMER] Consuming %s...' % n)
        r = '200 OK'

def produce(c):
    c.send(None)
    n = 0
    while n < 5:
        n = n + 1
        print('[PRODUCER] Producing %s...' % n)
        r = c.send(n)
        print('[PRODUCER] Consumer return: %s' % r)
    c.close()

c = consumer()
produce(c)
```