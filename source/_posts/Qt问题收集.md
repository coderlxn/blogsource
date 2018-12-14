---
title: Qt问题收集
date: 2018-12-13 14:44:11
tags: qt
---
#### connect nullptr
如果sender是nullptr，会有一条提示信息输出，如果receiver是nullptr，会导致崩溃
```cpp
QObject *obj1 = new QObject();
QObject *obj2 = new QObject();
QObject *obj3 = nullptr;
QObject::connect(obj1, &QObject::destroyed, obj2, &QObject::destroyed);
QObject::connect(obj3, &QObject::destroyed, obj2, &QObject::destroyed);		//QObject::connect: invalid null parameter
QObject::connect(obj1, &QObject::destroyed, obj3, &QObject::destroyed);		//Crash here!
```