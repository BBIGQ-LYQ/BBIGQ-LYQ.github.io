---
title: Qt Process Demo
date: 2019-12-21 16:59:24
tags: 
      - study
      - QT
      
top: 1
---

## Qt上进程的使用

<!--more-->



**Header:#include <QProcess>**
**qmake:QT += core**

### 重要的接口：
----

#### 开启一个进程
> void start(const QString &program, const QStringList &arguments, QIODevice::OpenMode mode = ReadWrite)
> void start(const QString &command, QIODevice::OpenMode mode = ReadWrite)
> void start(QIODevice::OpenMode mode = ReadWrite)

----

#### 等待进程结束并回收进程资源
> bool waitForFinished(int msecs = 30000)

----

#### 结束进程
> void kill()

----
### 信号 Signals
```
void errorOccurred(QProcess::ProcessError error)
void finished(int exitCode, QProcess::ExitStatus exitStatus)    //进程结束后会发送该信号
void readyReadStandardError()
void readyReadStandardOutput()
void started()
void stateChanged(QProcess::ProcessState newState)

```


**用例**

```
//新建一个QT进程
QProcess *myPro = new QProcess(this);

//用该进程去执行任务
myPro->start("对应的任务");
```
----
[我的GITHUB](https://github.com/BBIGQ-LYQ)
