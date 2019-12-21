---
title: Qt HTTP demo
date: 2019-12-19 16:17:40
tags: 
      - study
      - QT
      
top: 1

---

## 基于 Qt 的 HTTP demo          
        
<!--more-->

----

> Header:#include <QNetworkAccessManager> 
> qmake:QT += network

> Header:#include <QNetworkRequest> 
> qmake:QT += network


> Header:#include <QNetworkReply> 
> qmake:QT += network


----

## 新建一个HTTP请求管理器  QNetworkAccessManager：（请求管理器）
        QNetworkAccessManager *manager = new QNetworkAccessManager(this); 
       
## 关联管理器的请求结束信号
               //发出者            //关联信号              //接收者      //槽函数
        connect(manager, SIGNAL(finished(QNetworkReply *)), this, SLOT(read_data(QNetworkReply *)));

## QNetworkRequest(QUrl(URL))初始化请求地址
        manager->get(QNetworkRequest(QUrl("http://t.weather.sojson.com/api/weather/city/101280101")));

## 读取请求后的返回信息
        //读取请求后的返回信息
        void MainWindow::read_data(QNetworkReply *data)
        {
            //读取服务器返回的数据
            QString msg = data->readAll();
            ui->textBrowser->setText(msg);
        }
        
        
