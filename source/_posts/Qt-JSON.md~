---
title: Qt JSON
date: 2019-12-19 17:09:15
tags: 
      - study
      - QT
      
top: 1

---


## Qt 中的JSON 运用与解析

<!--more-->

----

> Header:#include <QJsonDocument> 
> qmake:QT += core
> JSON 数据管理器

> Header:#include <QJsonObject> 
> qmake:QT += core
> JSON项目

> Header:#include <QJsonParseError> 
> qmake:QT += core
> 判断数据是不是JSON

> Header:#include <QJsonValue> 
> qmake:QT += core
> JSON值

----

**用例**

定义一个JSON数据的一串字符

        QString json_data = "{\"name\":\"john\", \"age\":18}";
 
判断json_data是不是JSON格式数据

        QJsonParseError err;
        QJsonDocument json = QJsonDocument::fromJson(json_data.toUtf8(), &err);
        
判断是不是JSON数据

        if(err.error == QJsonParseError::NoError)
                qDebug() << "RIGHT";
        else
                qDebug() << "ERROR";

转化成一个CJSON对象

        QJsonObject root = json.object();

通过键获取值

        QJsonValue value = root.take("name");
        qDebug() << value.toString();

        value = root.take("age");
        qDebug() << value.toInt();       
        
        
[我的GITHUB](https://github.com/BBIGQ-LYQ)

