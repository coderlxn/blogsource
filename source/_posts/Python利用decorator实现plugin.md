---
title: Python利用decorator实现plugin
date: 2018-11-21 16:05:24
tags: Python
---
插件模式应该有的一个基本功能就是扩展简单，在扩展的时候只需要按照固定的格式开发自己的内容，然后插件系统自动的去发现和加载插件进入系统中。
* 注册机制
在开发的插件时向插件系统进行注册，但又要求不能修改现有的代码，这个和decorator的功能非常相似。所以首先考虑通过decorator来提供插件的注册机制，在开发插件的时候默认像插件系统进行注册。
```python
# registry.py
REGISTERED_CLASSED = []


def registered_class(cls):
    REGISTERED_CLASSED.append(cls)
    print("Class {} registered".format(cls))
    return cls
```
这样在开发的时候需要添加一个registered_class的装饰器即可
```python
# plugina.py
from .abstractplugin import AbstractPlugin
from .classregistry import registered_class


@registered_class
class PluginA(AbstractPlugin):
    def __init__(self):
        print("Plugin A init")

    def run(self):
        print("Plugin A run")

```
这样在导入插件模块的时候会将插件注册到插件列表中。
另外的一个问题就是现在必须要明确的导入插件文件才会触发decorator的运行，最好的情况是只需要将插件文件放到指定目录下就会自动的进行进行导入并注册。
* 自动导入
通过importlib可以实现模块的导入功能，可以在运行时通过importlib将插件的模块导入来触发插件的注册。
在插件package的__init__.py文件中变量当前的所有文件并依次进行导入。
```python
# __init__.py
import importlib
from .classregistry import REGISTERED_CLASSED
from os.path import basename, dirname, join
from glob import glob

pwd = dirname(__file__)
lst = glob(join(pwd, "*.py"))
for x in lst:
    file_name = basename(x)
    if not file_name.startswith("__"):
        importlib.import_module(".{}".format(file_name[:-3]), __package__)

__all__ = ["REGISTERED_CLASSED"]
```
通过__all__来限制外部的访问
* 其他问题
在相对路径导入的时候，需要提供package的名称，并且要主要package的相对地址
