---
title: Qt常用窗体
date: 2019-12-18 20:20:00
tags: 
      - study
      - QT
      
top: 1
---

## QT自带的窗体使用

----

<!--more-->

相关类|静态函数|函数说明
--|:--:|---
QMessageBox|QMessageBox::question|question 消息框
QMessageBox|QMessageBox::information|information 消息框
QMessageBox|QMessageBox::warning|warning 消息框      
QMessageBox|QMessageBox::critical|critical 消息框
QMessageBox|QMessageBox::about|about 消息框
QMessageBox|QMessageBox::abouutQt|abouut Qt 消息框

**用例**
> QMessageBox::warning
```
        int ret = QMessageBox::warning(this, tr("请看题"),   //标题
                                            tr("吃饭不?"),         //提示内容
                                            QMessageBox::Yes | QMessageBox::No //选择内容
                                            | QMessageBox::Cancel);
```


----


相关类|静态函数|函数说明
--|:--:|---
QFileDialog|getOpenFileName()|获得用户选择的文件名
QFileDialog|getSaveFileName()|获得用户保存的文件名
QFileDialog|getExistingDirectory()|获得用户选择的已存在的目录名
QFileDialog|getOpenFileNames()|获得用户选择的文件名列表
        
**用例**    
> getOpenFileNames
```
        QString fileName = QFileDialog::getOpenFileName(this,                   //父类的指针
                                tr("Open File"),                        //打开窗口的标题
                                "/home/bbigq",                          //打开默认路径
                                tr("Images (*.png *.xpm *.jpeg *.bmp)") //文件过滤器 这里只能打开*.png *.xpm *.jpg
                                );
```
        
        
        
> getOpenFileNames()                 //获得用户选择的文件名列表

```     
        
        QStringList temp = QFileDialog::getOpenFileNames(
                                this,                                   //父类的指针
                                "Select one or more files to open",     //打开窗口的标题
                                "/home/bbigq/workspace/QT",             //获取的路径
                                "Anyfile (*)");                         //获取所有文件
```

----


相关类|静态函数|函数说明
--|:--:|---
QColorDialog|getColor()|获得用户选择的颜色值


**用例**    
> getColor()

```
        QColor color = QColorDialog::getColor();                    //获取颜色

        QString choise_color = QString("color: %1;").arg(color.name()); //拼接颜色信息，将颜色信息填入choise_color中

        ui->label->setStyleSheet(choise_color);                         //设置字体颜色

```

相关类|静态函数|函数说明
--|:--:|---
QFontDialog|getFont()|获得用户选择的字体


**用例**    
> getFont()

```
        QFont font = QFontDialog::getFont();                    //获取字体
```


