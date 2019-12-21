---
title: QTcpserver
date: 2019-12-19 14:30:42
tags: 
      - study
      - QT
      
top: 1
---


## 基于 QTcpserver 的 demo       
  
<!--more-->
        
----


Header:#include <QTcpServer> 
qmake:QT += network


----
### 创建服务器socket

        QTcpServer *ser;                //定义服务器对象
        ser = new QTcpServer(this);     //创建服务器socket
        
### 绑定和监听

        ser->listen(QHostAddress::Any, PORT); //设置为监听模式   这里的Any代表默认获取本地的IP，如果需要指定IP：ser->listen(QHostAddress("IP"), PORT);
        
### 获取服务端的IP和PORT

        QString serIP = ser->serverAddress().toString();
        QString serPORT = QString::number(ser->serverPort());

        ui->label->setText("IP: " + serIP);
        ui->label_2->setText("PORT: " + serPORT);
        
### 监听这里可以判断一下是否成功监听

        if(ser->listen(QHostAddress::Any, PORT))
        {
                qDebug() << "listen ok";
        }
        else
                qDebug() << "listen false";
                
### 监听到有新的连接成功之后会受到一个newConnection()信号，这个时候需要通过connect去关联成功连接信号，同时写一个回调的槽函数newConnect()

        connect(ser, SIGNAL(newConnection()), this, SLOT(newConnect()));//关联连接请求信号  newConnection() 新的连接请求信号   newConnect() 用户编写的槽函数
        
----  

### 关联新连接的槽函数的编写

        //处理客户断的连接请求
        void MainWindow::newConnect()
        {
            //服务器之负责接受连接请求，不负责通信
            //产生新的通信对象

            QTcpSocket *new_clien = ser->nextPendingConnection();
            //打印连接者的主机名、端口、IP
            QString name = new_clien->peerName();
            QString port = QString::number(new_clien->peerPort());
            QString ip   = new_clien->peerAddress().toString();

            ui->textBrowser->append(name);
            ui->textBrowser->append(port);
            ui->textBrowser->append(ip);
            
            //当需要实现服务端能接收信息   （必须写在 new_clien 创建/接收成功后）
            connect(new_clien, SIGNAL(readyRead()), this, SLOT(readData())); //关联可读信息信号  readyRead() 可读信息信号    readData() 用户编写的读数据槽函数
        }

### 接收可读信号的槽函数

        void MainWindow::readData()
        {
            QString msg = new_clien->readAll();

            ui->msg_browser->append(msg);
        }
        
----        
