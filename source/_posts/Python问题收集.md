---
title: Python问题收集
date: 2018-09-26 09:39:49
tags: Python
---
#### python引入模块时import与from ... import的区别
* import datetime是引入整个datetime包，如果使用datetime包中的datetime类,需要加上模块名的限定。
```python
import datetime
print datetime.datetime.now()
```
* from datetime import datetime是只引入datetime包里的datetime类,在使用时无需添加模块名的限定。
```python
from datetime import datetime
print datetime.now()
```
#### python主线程退出时，工作线程不会退出，而是等待工作完成后退出

#### 函数内部修改全局变量
* 变量名在等号左边，且在变量域内首次出现，就认为是在定义局部变量
```python
x = 1
def test():
    x = 2
    print(x)

test()    #2
print(x)    #1
```
* 加上global x，则函数内部引用的x就是全局x了
```python
x = 1
def test():
    global x
    x = 2
    print(x)

test()    #2
print(x)    #2
```
* 在局部变量赋值前使用变量会报错，即使外层有同名变量
```python
x = 1
def test():
    print(x)    #Error
    x = 2
    print(x)

test()
print(x)
```
#### binary和string转换
通过decode和encode来实现
```python
>>> b'a string'.decode('ascii')
'a string'

>>> 'a string'.encode('ascii')
b'a string'
```

#### 判断是否存在并调用类的方法
```python
class Foo:
	def do_foo(self):
		pass
	def do_bar(self):
		pass
		
foo = Foo()
hasattr(foo, "do_foo")
f = getattr(foo, "do_foo")
f()
```

#### 向MySQL数据库添加时间数据
可以通过指定格式的字符串来添加时间
```python
def insertIntoChannel(self, user):
        conn = JDBCUtils.getConnection()
        cursor = conn.cursor()
        dt=datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
        cursor.execute("insert into user(id,age,name,create_time,update_time) \
                      values('%d','%d','%s','%s','%s')" % \
                       (user.getId(),user.getAge(), user.getName(),dt,dt))
        cursor.close()
        conn.commit()
        conn.close()
```

#### str的format方法中数字会当做字面量进行处理
```
'{}://{}{}'.format('34343', 343433, 'gggg')
'34343://343433gggg'
```

#### importlib进行相对路径导入的时候，需要指定包的的相对关系
    a
    |
    + - __init__.py
      - b
        |
        + - __init__.py
          - c.py
```python
# For relative imports you have to a) use relative name b) provide anchor explicitly:
importlib.import_module('.c', 'a.b')

#Of course, you could also just do absolute import instead:
importlib.import_module('a.b.c')
```

#### list中的append和extend的区别
  * append，appends ojbect at the end
  * extend, extends list by appending elements from the iterable
```python
x = [1, 2, 3]
x.append([4, 5])
print (x)   # [1, 2, 3, [4, 5]]

x = [1, 2, 3]
x.extend([4, 5])
print (x)   # [1, 2, 3, 4, 5]
```

#### yield
yield 是一个类似 return 的关键字，只是这个函数返回的是个生成器。
```python
>>> def createGenerator() :
...    mylist = range(3)
...    for i in mylist :
...        yield i*i
...
>>> mygenerator = createGenerator() # create a generator
>>> print(mygenerator) # mygenerator is an object!
<generator object createGenerator at 0xb7555c34>
>>> for i in mygenerator:
...     print(i)
0
1
4
```
当你调用这个函数的时候，函数内部的代码并不立马执行 ，这个函数只是返回一个生成器对象

#### Process的start会启动线程，但线程中的内容不会执行，需要join之后才会执行。必须要start之后才能调用join

#### Python不支持对于metaclass的多重继承
如果存在多重继承的情况，会抛出异常
TypeError: Error when calling the metaclass bases metaclass conflict: the metaclass of a derived class must be a (non-strict) subclass of the metaclasses of all its bases