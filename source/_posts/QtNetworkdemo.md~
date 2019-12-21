---
title: QtNetworkdemo
date: 2019-12-19 13:15:16
tags: 
      - study
      - QT
      
top: 1
---

## 网络编程          
  
<!--more-->

----
## Linux网络编程
**网络的分层模型： 4层**

应用层： 用户自定义的应用协议，HTTP，TFTP，SSH，FTP...
传输层： TCP/UDP   TCP：可靠的传输协议  UDP：不可靠的传输协议
网络层： IPV4,IPV6,3G，4G
物理层： 网卡，路由器，网线，基站...


**TCP搭建流程：**

服务器端：
1：创建网络通信tcp socket
2：绑定服务器的IP地址信息
3：监听客户端的连接请求
4：等待客户机的连接
5：进行通信
6：关闭通信
                
客户端：
1：创建网络通信 tcp socket
2：连接服务器
3：进行通信
4：关闭通信


**UDP的通信流程**

接受端：
1：创建UDP通信socket
2：绑定UDP通信socket
3：接受数据
4：关闭通信
              
发送端：
1：创建UDP通信socket
2：往绑定的IP地址中发送数据
3：关闭通信
                

通信方式：单播、组播、广播

----
## QT网络编程demo

----

相关类|静态函数|函数说明
--|:--:|---
QNetworkInterface|QNetworkInterface::allAddresses()|获得所有的IP地址
QNetworkInterface|QNetworkInterface::allInterface()|获得所有的网卡信息
QNetworkInterface|QNetworkInterface::hardwateAddress()|warning 消息框 


> 更多接口详见
> Header:#include <QNetworkInterface> 
> qmake:QT += network

**用例**
> QNetworkInterface::allAddresses()
```
         QList<QHostAddress> addrs = QNetworkInterface::allAddresses();
```

----
相关类|静态函数|函数说明
--|:--:|---
QTcpSocket|QTcpSocket()|创建QTCP通信socket

**用例**
> QTcpSocket()
```
        clien = new QTcpSocket(this);
        //设置服务端IP
        QHostAddress MYip("192.168.95.99");
        //设置端口
        unsigned short PORT = 9046;
        //连接服务器
        clien->connectToHost(MYip, PORT);
```

> 关联有数据可读信号
> connect(clien, SIGNAL(readyRead()), this, SLOT(readData()));  //readData()为用户编写的槽函数

> 数据的发送，发送数据，注意数据类型的转化
> sclien->write(msg.toUtf8());       //注意发送的类型为char *

> 关闭通信
> clien->close()

> 更多接口详见
> QTcpSocket
> QHostAddress

----

