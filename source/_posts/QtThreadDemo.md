---
title: QtThreadDemo
date: 2019-12-21 16:18:13
tags: 
      - study
      - QT
      
top: 1
---

## Qt中使用线程小demo

<!--more-->

> **注意：在Qt中使用线程和linux中使用是一样的，如pthread.h  pthread_create等**
> **像UI界面时不能存在死循环和延时的，但是在线程中可以哦！**

----

### 第一种用法：使用linux中的pthread.h

```
pthread_t pid;
pthread_create(&pid, NULL, func, null)


void *func(void *arg)
{
        /*
        按需编写
        */
}


```

### 第二种用法：使用Qt中的QThread 

> Header:#include <QThread> 
> qmake:QT += core

> 开启线程的任务，这里会自动调用类内部的run函数，run函数是一个虚函数，需要用户继承后在子类中**重写!!!**
> void start(QThread::Priority priority = InheritPriority)


**这里需要注意一点十分重要：QT中的线程必须重写run方法，即需要继承QThread类，然后对虚函数run，用户自行编写**

```
virtual void run()

```
**例子**
```
//重写线程类中的run函数
class myThread:public QThread
{
public:
    void run()
    {
        qDebug() << "任务线程已经重写";
    }
};


//定义一个线程
myThread *tid;

//为线程对象分配空间
tid = new myThread;

//开启线程的任务
tid->start();


```

#### 线程中传递UI

```
pthread_t pid;
pthread_create(&pid, NULL, func, (void *)ui)


void *func(void *arg)
{
        Ui::MainWindow *ui = (Ui::MainWindow *)arg;
        qDebug() << "func" << (void *)ui << endl;
}

```

[我的GITHUB](https://github.com/BBIGQ-LYQ)
