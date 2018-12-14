---
title: 《流畅的Python》笔记
date: 2018-10-16 17:40:54
tags: Python
---
#### 通过实现特殊方法__foo__来实现pythonic的方法调用
```python
from math import hypot
class Vector:
	def __init__(self, x=0, y=0):
		self.x = x
		self.y = y
	def __repr__(self):
		return 'Vector(%r, %r)' % (self.x, self.y)
	def __abs__(self):
		return hypot(self.x, self.y)
	def __bool__(self):
		return bool(abs(self))
	def __add__(self, other):
		x = self.x + other.x
		y = self.y + other.y
		return Vector(x, y)
	def __mul__(self, scalar):
		return Vector(self.x * scalar, self.y * scalar)
```
#### Python会忽略代码里 []、{} 和 () 中的换行
#### 生成器表达式
生成器表达式会逐个产出元素，而不是先建立一个完整的列表。
```python
symbols = '$¢£¥€¤'
tuple(ord(symbol) for symbol in symbols)
# (36, 162, 163, 165, 8364, 164)
import array
array.array("I", (ord(symbol) for symbol in symbols))
array('I', [36, 162, 163, 165, 8364, 164])
```

#### 具名元组
1. 创建一个具名元组需要两个参数，一个是类名，另一个是类的各个字段的名字。后者
可以是由数个字符串组成的可迭代对象，或者是由空格分隔开的字段名组成的字符串。
2. 存放在对应字段里的数据要以一串参数的形式传入到构造函数中
3. 可以通过字段名或者位置来获取一个字段的信息。
```python
from collections import namedtuple
City = namedtuple("City", "name country population coordinates")
tokyo = City("Tokyo", "Jp", 36.933, (35.689, 139.69))
```

#### 对象切片
* 在切片和区间操作里不包含区间范围的最后一个元素是 Python 的风格，这个习惯符合
Python、C 和其他语言里以 0 作为起始下标的传统。
* 可以用 s[a:b:c] 的形式对 s 在 a 和 b 之间以 c 为间隔
取值。c 的值还可以为负，负值意味着反向取值
* 如果赋值的对象是一个切片，那么赋值语句的右侧必须是个可迭代对象。即便只有单
独一个值，也要把它转换成可迭代的序列。
```python
l = list(range(10))
l[2:5] = [20, 30]
l[2:5] = 100 # error
l[2:5] = [100]
```

#### 对序列使用+和*
* Python的序列默认支持+和\*操作，这两个操作都不会修改原有的操作对象，而是构建一个新的序列
* 不要对可变对象使用\*操作来赋值，可能会产生不确定的结果

#### 如果一个函数或方法对对象进行的是就地改动，那它就应该返回None

#### 泛映射类型
非抽象映射类型一般不继承这些抽象基类，会直接对dict或collections.User.Dict进行扩展

#### 使用setdefault可以快速的完成对dict的查询和插入
```
index.setdefault(word, []).append(location)
```

#### 如果某个键在映射里不存在，而仍想得到一个默认值，有两个途径实现
1. 通过defaultdict而不是默认的dict
2. 自定义一个dict的子类，在子类中实现__missing__方法

#### 字典变种
* collections.OrderedDict   这个类型在添加键的时候会保持顺序，因此键的迭代次序总是一致的
* collections.ChainMap 	该类型可以容纳数个不同的映射对象，然后在进行键查找操作的时候，这些对象会被当作一个整体被逐个查找，直到键被找到为止。
* collections.Counter 	这个映射类型会给键准备一个整数计数器。每次更新一个键的时候都会增加这个计数器

#### 不可变映射类型
types 模块中引入了一个封装类名叫 MappingProxyType。如果给这个类一个映射，它会返回一个只读的映射视图。

#### 内置规约函数
* sum 和 reduce 的通用思想是把某个操作连续应用到序列的元素上
* all(iterable)  如果 iterable 的每个元素都是真值，返回 True；all([]) 返回 True。
* any(iterable)  只要 iterable 中有元素是真值，就返回 True；any([]) 返回 False。

####  可调用对象
调用运算符（()）可以应用到其他对象上，可以使用内置callable()来判断对象是否能调用
* 用户定义的函数
* 内置函数   使用 C 语言（CPython）实现的函数，如 len 或 time.strftime。  单独的函数
* 内置方法   使用 C 语言实现的方法，如 dict.get。   类中的函数
* 方法  		在类的定义体中定义的函数。
* 类			调用类时会运行类的 __new__ 方法创建一个实例，然后运行 __init__ 方法，初始化实例，最后把实例返回给调用方。
* 类的实例	如果类定义了 __call__ 方法，那么它的实例可以作为函数调用。
* 生成器函数	使用 yield 关键字的函数或方法。调用生成器函数返回的是生成器对象。

#### 函数装饰器在导入模块时立即执行，而被装饰的函数只有在明确调用时运行

#### 带参数的装饰器实际是一个工厂函数，运行时返回一个装饰器方法
```python
registry = set()

# 实际是调用一个工厂函数来返回一个装饰器
def register(active=True):
    def decorate(func):
        print("running register(active=%s)->decorate(%s)" % (active, func))
        if active:
            registry.add(func)
        else:
            registry.discard(func)

        return func
    return decorate

@register(active=False)
def f1():
    print("running f1()")

@register()
def f2():
    print("running f2()")

def f3():
    print("running f3()")

f1()
f2()
f3()
```

#### Python的变量赋值是贴便签而不是装盒子

#### 使用引用作为参数时，要提防引用的复制和指向，尽可能的创建副本
