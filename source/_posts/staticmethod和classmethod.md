---
title: staticmethod和classmethod
date: 2018-09-30 15:48:10
tags: Python
---
Python对象的方法定义可以有三种形式： @classmethod 装饰的类方法、@staticmethod 装饰的静态方法以及不带装饰器的实例方法。本文列出了它们的区别。
先看一个示例
```python
class A(object):
    def m1(self, n):
        print("self:", self)

    @classmethod
    def m2(cls, n):
        print("cls:", cls)

    @staticmethod
    def m3(n):
        pass

a = A()
a.m1(1) # self: <__main__.A object at 0x000001E596E41A90>
A.m2(1) # cls: <class '__main__.A'>
A.m3(1)
```
* m1 是实例方法，第一个参数是self
调用m1使用的a是实例对象，调用时会自动把对象传递给self进行绑定
* m2 是类方法，第一个参数是cls
调用时会自动讲class作为参数传递给cls进行绑定
* m3 是静态方法，参数可有可无
适用于包含在类内但与类元素无关的方法

