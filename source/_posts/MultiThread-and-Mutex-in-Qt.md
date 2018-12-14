---
title: MultiThread and Mutex in Qt
date: 2018-11-13 10:29:43
tags: [Qt, MultiThread]
---
#### Qt中的多线程和锁
如果有多个线程共用一个Mutex，在同时访问这个Mutex的时候，只有一个线程可以获得所有权，其他的线程会被挂起，直到前面的线程放弃Mutex的所有权。
一个示例程序
```cpp
//runobject.h
#include <QObject>

class RunObject : public QObject
{
    Q_OBJECT
public:
    explicit RunObject(QObject *parent = 0);

signals:

public slots:
    void startRunAndLock();

private:
    QString m_uuid;
};
```
```cpp
//runobject.cpp
#include "runobject.h"
#include <QMutex>
#include <QMutexLocker>
#include <QUuid>
#include <QThread>
#include <QDebug>

QMutex global_mutex;

RunObject::RunObject(QObject *parent) : QObject(parent)
{
    m_uuid = QUuid().createUuid().toString();
}

void RunObject::startRunAndLock()
{
    qDebug() << m_uuid << " before get mutext";
    QMutexLocker locker(&global_mutex);
    qDebug() << m_uuid << " now get mutext";

    QThread::currentThread()->sleep(10);

    qDebug() << m_uuid << " after long time work, release mutex";
}
```
```cpp
//main.cpp
#include <QCoreApplication>
#include <QThread>
#include <QDebug>
#include <QMetaObject>
#include "runobject.h"

int main(int argc, char *argv[])
{
    QCoreApplication a(argc, argv);

    RunObject obj1;
    RunObject obj2;
    RunObject obj3;

    QThread thread1;
    QThread thread2;
    QThread thread3;

    thread1.start();
    thread2.start();
    thread3.start();

    obj1.moveToThread(&thread1);
    obj2.moveToThread(&thread2);
    obj3.moveToThread(&thread3);

    qDebug() << "start multi jobs";

    QMetaObject::invokeMethod(&obj1, "startRunAndLock");
    QMetaObject::invokeMethod(&obj2, "startRunAndLock");
    QMetaObject::invokeMethod(&obj3, "startRunAndLock");

    return a.exec();
}
```
输出结果：
```
start multi jobs
"{7488acab-e215-4b92-8671-2fb853be89f3}"  before get mutext
"{c03aab6f-0f2b-4753-8a17-96b1918777cb}"  before get mutext
"{9a26aae0-41b7-4004-a273-24f6b6ae18fe}"  before get mutext
"{7488acab-e215-4b92-8671-2fb853be89f3}"  now get mutext
"{7488acab-e215-4b92-8671-2fb853be89f3}"  after long time work, release mutex
"{c03aab6f-0f2b-4753-8a17-96b1918777cb}"  now get mutext
"{c03aab6f-0f2b-4753-8a17-96b1918777cb}"  after long time work, release mutex
"{9a26aae0-41b7-4004-a273-24f6b6ae18fe}"  now get mutext
"{9a26aae0-41b7-4004-a273-24f6b6ae18fe}"  after long time work, release mutex
```
可以看出现在是等一个线程工作完成之后，下个线程才开始工作

#### 扩展Mutex
* `QMutex`中有一个`tryLock(int timeout = 0)`方法，会尝试获得Mutex的所有权，成功则返回true，否则返回false。如果timeout为负数，则和`lock()`方法是等效的。
* QLockFile 在多个线程或进程中锁定文件。
* QReadWriteLock，读写锁，允许多个只读用户同时存在，但写只能存在一个。