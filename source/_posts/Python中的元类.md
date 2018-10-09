---
title: Python中的元类
date: 2018-09-30 20:38:33
tags: ["Python","metaclass"]
---
#### 类也是对象
Python中类的概念借鉴于Smalltalk，在大多数编程语言中，类就是一组用来描述如何生成一个对象的代码段。在Python中这一点仍然成立。但是，Python中的类还远不止如此。类同样也是一种对象。可以执行下面操作：
* 将它赋值给一个变量
* 拷贝它
* 为它增加属性
* 作为函数参数进行传递
```python
>>> print ObjectCreator     # 你可以打印一个类，因为它其实也是一个对象
<class '__main__.ObjectCreator'>
>>> def echo(o):
…       print o
…
>>> echo(ObjectCreator)                 # 你可以将类做为参数传给函数
<class '__main__.ObjectCreator'>
>>> print hasattr(ObjectCreator, 'new_attribute')
Fasle
>>> ObjectCreator.new_attribute = 'foo' #  你可以为类增加属性
>>> print hasattr(ObjectCreator, 'new_attribute')
True
>>> print ObjectCreator.new_attribute
foo
>>> ObjectCreatorMirror = ObjectCreator # 你可以将类赋值给一个变量
>>> print ObjectCreatorMirror()
<__main__.ObjectCreator object at 0x8997b4c>
```
#### 通过type来创建和扩展class
```python
>>> Foo = type('Foo', (), {'bar':True})
>>> FooChild = type('FooChild', (Foo,),{})
>>> print FooChild
<class '__main__.FooChild'>
>>> print FooChild.bar   # bar属性是由Foo继承而来
True
>>> def echo_bar(self):
…       print self.bar
…
>>> FooChild = type('FooChild', (Foo,), {'echo_bar': echo_bar})
>>> hasattr(Foo, 'echo_bar')
False
>>> hasattr(FooChild, 'echo_bar')
True
>>> my_foo = FooChild()
>>> my_foo.echo_bar()
True
```
#### type是所有对象的最原始的类
type就是创建类对象的类。你可以通过检查__class__属性来看到这一点。Python中所有的东西，注意，我是指所有的东西——都是对象。这包括整数、字符串、函数以及类。它们全部都是对象，而且它们都是从一个类创建而来。
```python
>>> age = 35
>>> age.__class__
<type 'int'>
>>> name = 'bob'
>>> name.__class__
<type 'str'>
>>> def foo(): pass
>>>foo.__class__
<type 'function'>
>>> class Bar(object): pass
>>> b = Bar()
>>> b.__class__
<class '__main__.Bar'>

>>> a.__class__.__class__
<type 'type'>
>>> age.__class__.__class__
<type 'type'>
>>> foo.__class__.__class__
<type 'type'>
>>> b.__class__.__class__
<type 'type'>
```
#### 自定义元类
```python
class UpperAttrMetaclass(type):
    def __new__(cls, name, bases, dct):
        attrs = ((name, value) for name, value in dct.items() if not name.startswith('__'))
        uppercase_attr = dict((name.upper(), value) for name, value in attrs)
        return super(UpperAttrMetaclass, cls).__new__(cls, name, bases, uppercase_attr)
```