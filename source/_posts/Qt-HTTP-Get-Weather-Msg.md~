---
title: Qt HTTP Get Weather Msg
date: 2019-12-21 10:13:00
tags: 
      - study
      - QT
      
top: 1
---

## 使用QT的接口实现HTTP请求获取天气信息

<!--more-->

**所需头文件**
JSON相关头文件

```
#include <QJsonDocument>
#include <QJsonObject>
#include <QJsonParseError>
#include <QJsonValue>
#include <QJsonArray>
```

添加HTTP网络管理器

```
#include <QNetworkAccessManager>
```

请求地址

```
#include <QNetworkRequest>
```

debug

```
#include <QDebug>
```

**注意**
> #include <QNetworkReply> 该头文件需要添加在MianWindow下，因为在MianWindow类的定义中定义了：

        public slots:
        void readDate(QNetworkReply *);
        
**源码**   
```
#include "mainwindow.h"
#include "ui_mainwindow.h"
//JSON相关头文件
#include <QJsonDocument>
#include <QJsonObject>
#include <QJsonParseError>
#include <QJsonValue>
#include <QJsonArray>

//添加HTTP网络管理器
#include <QNetworkAccessManager>

//请求地址
#include <QNetworkRequest>

//debug
#include <QDebug>

MainWindow::MainWindow(QWidget *parent)
    : QMainWindow(parent)
    , ui(new Ui::MainWindow)
{
    ui->setupUi(this);

    //新建一个请求HTTP管理器
    QNetworkAccessManager *manager = new QNetworkAccessManager(this);

    //关联管理器请求
    connect(manager, SIGNAL(finished(QNetworkReply *)), this, SLOT(readDate(QNetworkReply *)));
    //发送HTTP请求
    manager->get(QNetworkRequest(QUrl("http://t.weather.sojson.com/api/weather/city/101280101")));

}

MainWindow::~MainWindow()
{
    delete ui;
}

void MainWindow::readDate(QNetworkReply *data)
{
    //读取服务器返回的所有数据
    QString msg = data->readAll();
    ui->textBrowser->setText(msg);

    //每次操作JSON时都先判断该字符串是否为JSON格式的
    QJsonParseError err;
    QJsonDocument json = QJsonDocument::fromJson(msg.toUtf8(), &err);
    if(err.error == QJsonParseError::NoError)
        qDebug() << "DATA IS OK";

    //判断完之后，转换成对象
    QJsonObject obj = json.object();
    //提取信息中指定的城市信息这个对象
    QJsonValue value = obj.take("cityInfo");

    //将提取出来的信息转成一个对象
    QJsonObject CityInfo = value.toObject();
    //分别提取相关信息
    value = CityInfo.take("city");
    qDebug() << value.toString() ;

    value = CityInfo.take("citykey");
    qDebug() << value.toString() ;

    value = CityInfo.take("parent");
    qDebug() << value.toString() ;

    value = CityInfo.take("updateTime");
    qDebug() << value.toString() ;
}


```

**当我们取出的value是一个数组时，就将它转成array来处理**

```
QJsonArray arr = value.toArray();

QJsonValue msg = arr.at(0);
QJsonValue msg = arr.at(1);
QJsonValue msg = arr.at(2);
....

```

**效果展示**
![效果展示](/blogmat/weather.png)


[我的GITHUB](https://github.com/BBIGQ-LYQ)

