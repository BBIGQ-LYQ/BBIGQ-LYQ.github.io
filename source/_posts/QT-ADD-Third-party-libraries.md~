---
title: QT ADD Third-party libraries
date: 2019-12-21 15:22:08
tags: 
      - study
      - QT
      
top: 1
---

## QT上使用第三方库的教程，附带demo

<!--more-->

### 第一步：

> 在新建的工程中的xxx.pro文件

### 第二步：
> 使用BAT API的demo为例子
[BAT API DEMO](https://bbigq-lyq.github.io/2019/12/21/BAT-API-DEMO/)

> 添加头文件

```

INCLUDEPATH+=/home/bbigq/baiduAPI/image/aip-cpp-sdk-0.8.5

```

### 第三步：

> 添加库文件
```

LIBS+= -lcurl -lcrypto -ljsoncpp

```


**源码**

```
#include "mainwindow.h"
#include "ui_mainwindow.h"
//使用QFileDialog打开一张图
#include <QFileDialog>

//JSON相关头文件
#include <QJsonDocument>
#include <QJsonObject>
#include <QJsonParseError>
#include <QJsonValue>
#include <QJsonArray>
//debug
#include <QDebug>

//添加第三方头文件
#include "ocr.h"
#include "image_classify.h"

MainWindow::MainWindow(QWidget *parent)
    : QMainWindow(parent)
    , ui(new Ui::MainWindow)
{
    ui->setupUi(this);
}

MainWindow::~MainWindow()
{
    delete ui;
}


void MainWindow::on_pushButton_clicked()
{
    //获得文件的名字
    QString fileName = QFileDialog::getOpenFileName(this,
          tr("Open Image"), "/home/bbigq", tr("*"));
    //打开一张图片
    QPixmap pic(fileName);

    //设置大小
    pic.scaled(ui->show->width(), ui->show->height());
    //显示图片
    ui->show->setPixmap(pic);

    // 设置APPID/AK/SK
    std::string app_id = "18078217";
    std::string api_key = "rtYVoXqqeR7278NI0qGh99q9";
    std::string secret_key = "u9FauW1mwvCfg9zdkwo58WzkYNKh6QNp";

    aip::Imageclassify client(app_id, api_key, secret_key);


    Json::Value result;

    std::string image;
    aip::get_file_content(fileName.toUtf8().data(), &image);

    // 调用车型识别
    result = client.car_detect(image, aip::null);

    // 如果有可选参数
    std::map<std::string, std::string> options;
    options["top_num"] = "3";
    options["baike_num"] = "5";

    // 带参数调用车型识别
    result = client.car_detect(image, options);

    //每次操作JSON时都先判断该字符串是否为JSON格式的
    QJsonParseError err;
    QJsonDocument json = QJsonDocument::fromJson(result.toStyledString().data(), &err);
    if(err.error == QJsonParseError::NoError)
        qDebug() << "DATA IS OK";

    //判断完之后，转换成对象
    QJsonObject obj = json.object();
    QJsonValue msg = obj.take("result");
    //这里匹配到的result是数组对象，需要转
    QJsonArray msgArray = msg.toArray();
    //将包含所需名字的信息的数组对象成员提取出来转成单独的对象
    QJsonObject aim = msgArray.at(msgArray.size() - 1).toObject();
    //匹配对应信息
    QJsonValue name = aim.take("name");
    qDebug() << name;
    ui->textBrowser->setText(name.toString());

}

```

![结果](/blogmat/carDemo.png)

[我的GITHUB](https://github.com/BBIGQ-LYQ)

