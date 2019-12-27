---
title: Qt多媒体播放器
date: 2019-12-26 14:53:11
tags: 
      - study
      - QT
      
top: 1
---

## 基于mplayer的多媒体播放器

<!--more-->

**废话不多说，直接上源码**
[SOURCE](https://github.com/BBIGQ-LYQ/Qt--)

**详细步骤：**

### 第一步：移植mplayer到ARM开发板上去

mplayer的移植就不一一细说了，网上有很多大牛讲的都特别详细
这里直接将mplayer复制到ARM的/bin文件下

----
### 第二部：编写代码

> 需要实现的功能是：
>         1：视频播放、快进退、声音调大小、上下首切换等等;
>         2：音乐的功能大致与视频播放没有太多的差别，只多了刷歌词的功能;

#### 根据功能设计程序

> 基于视频和音乐的视窗大致相同，所以这里将功能视窗抽象出来，然后后续使用时只需要继承该类，并且编写相关独特功能即可！
> 同时基于对视频和音乐的控制功能也大致一样，都是播放/暂停、快进退、声音调大小、上下首切换等等，抽象出来做 class ToolsWindow

**功能窗口类：class FuncWindow 代码区**

```
#ifndef FUNCWINDOW_H
#define FUNCWINDOW_H

#include <QMainWindow>

#include <QListWidgetItem>
//鼠标事件重写
#include <QMouseEvent>
#include <QInputEvent>
#include <QEvent>
//进程头文件
#include <QProcess>
//定时器
#include <QTimer>

//工具栏头文件
#include "toolswindow.h"

//读取文件
#include <QDir>
#include <QStringList>

#define FilePath "/home/bbigq/workspace/QT/ARM_player/mat/"   //该处填写音视频素材的文件路径

namespace Ui {
class FuncWindow;
}

class FuncWindow : public QMainWindow
{
    Q_OBJECT

public:
    //工具栏
    ToolsWindow * toolwin;
    //视频 1/音乐 2
    int variety = 0;
    //工具表触发状态  0为未触发  1为触发
    int state = 1;
    //播放状态
    int playTime = 0;
    //播放器进程
    QProcess *mPro;
    //当前播放的文件
    QString nowFile = "";
    //当前事件总时间
    double allTime = 0;
    //当前进度(百分比)
    int precent = 0;
    //当前时间
    double preTime = 0;
    //当前音量值
    int volume = 50;
    //定时器
    QTimer *timer;
    //文件列表
    QStringList fileList;

    explicit FuncWindow(QWidget *parent = nullptr);
    ~FuncWindow();

    //重写单击触发文件列表
    void mousePressEvent(QMouseEvent *event);

signals:
    void reflashRate(int);

public slots:
    //播放
    virtual void play();
    //上一首
    virtual void prevPlay();
    //下一首
    virtual void nextPlay();
    //快退
    virtual void quickRetreat();
    //快进
    virtual void fastForward();
    //音量加
    virtual void VolumePlus();
    //音量减
    virtual void volumeDN();
    //推出当前界面
    virtual void out();
    //获取当前进度
    virtual void getRate();
    //读取当前播放信息
    virtual void readData();
    //读取文件路径
    virtual void readFileName();
    //刷新链表
    virtual void refreshList();
    //停止定时器
    virtual void stopTimer();
    //跳转到目标
    virtual void jumpAim(int);
    //播放前数据准备
    virtual void readyData(){}
    //播放结束回调
    virtual void overHandle();


private slots:
    void on_listWidget_itemClicked(QListWidgetItem *item);

    void on_listWidget_itemDoubleClicked(QListWidgetItem *item);

private:
    Ui::FuncWindow *ui;
};

#endif // FUNCWINDOW_H

```

**功能窗口类：class ToolsWindow 代码区**

```
#ifndef TOOLSWINDOW_H
#define TOOLSWINDOW_H
#include <QWidget>
#include <QDebug>
namespace Ui {
class ToolsWindow;
}

class ToolsWindow : public QWidget
{
    Q_OBJECT

public:


    explicit ToolsWindow(QWidget *parent = nullptr);
    ~ToolsWindow();

//自定义信号
signals:
    void PLAYSIG();         //执行音频文件类
    void PREVSIG();         //上一首/个
    void NEXTSIG();         //下一首/个
    void MOVEPRE();         //快进
    void RETURNB();         //倒退
    void VOLADD();          //音量加
    void VOLSUB();          //音量减
    void STOPTIMER();       //停止定时器
    void SKIP(int);         //滑动进度条
    void OUT();             //退出

public slots:
    void reflashRateRealTime(int ); //刷新进度条

private slots:
    void on_but_out_clicked();      //退出按钮被点击

    void on_but_rev_clicked();      //倒退按钮被点击

    void on_but_frev_clicked();     //上一首按钮被点击

    void on_but_play_clicked();     //播放按钮被点击

    void on_but_next_clicked();     //下一首按钮被点击

    void on_but_adv_clicked();      //快进按钮被点击

    void on_but_volup_clicked();    //音量加按钮被点击

    void on_but_voldown_clicked();  //音量减按钮被点击

    void on_precent_seek_sliderPressed();//进度条被按下

    void on_precent_seek_sliderReleased();//进度条松开

private:
    Ui::ToolsWindow *ui;
};

#endif // TOOLSWINDOW_H

```
**继承说明：**
> class FuncWindow 是继承于QMainWindow的
> class ToolsWindow 继承于 QWidget

![ToolsWindow](/blogmat/toolsWindow.png)

在FuncWindow中默认包含了一个工具栏： **ToolsWindow * toolwin** 指针，那么只要在构造函数中对其进行分配空间，就可以直接使用该工具栏了;
![main](/blogmat/main.png)
![inherit tools](/blogmat/music.png)

工具栏上所有的操作都由信号发出，所以有很多自定义的槽函数;
在触发到相应的按钮和操作时，通过emit发出，由FuncWindow的对象中接收并处理

```
//自定义信号
signals:
    void PLAYSIG();         //执行音频文件类
    void PREVSIG();         //上一首/个
    void NEXTSIG();         //下一首/个
    void MOVEPRE();         //快进
    void RETURNB();         //倒退
    void VOLADD();          //音量加
    void VOLSUB();          //音量减
    void STOPTIMER();       //停止定时器
    void SKIP(int);         //滑动进度条
    void OUT();             //退出
    
```

FuncWindow 类中有很多对应的处理函数都是虚函数，用户可以根据自己的要求来**继承后重写**

#### 继承与重写
----

> 针对视频播放页面，大致的功能在父类FuncWindow中已经可以实现了，所以不需要重写，当然可以根据自己要求进行重写
> 所以这里只需要对 variety 赋值为1即可，因为我默认variety = 1 为视频继承，variety = 2 为音乐继承

----
> 针对音乐播放页面，由于需要歌词的随动，所以在musicWindow中继承FuncWindow过去后，需要添加播放歌词的窗体;
> 本次demo中添加了一个listWidget作为歌词的视窗，用来实时显示歌词

```

listWidget = new QListWidget(this);                     //开辟空间
listWidget->setGeometry(QRect(50, 50, 500, 350));       //设置listWideget的起始位置(x,y)和大小
listWidget->setMinimumSize(QSize(500, 350));            //设置listWidget的最小设置
listWidget->setMaximumSize(QSize(500, 350));            //设置listWidget的最大设置

```
----

为了显示歌词，在musicWindow中新加了一个获取歌词的函数和一个播放歌词的函数

**获取歌词 getLrc 代码**
```
void musicWindows::getLrc()
{
    if(!lrcTimeList.isEmpty())
    {
        lrcTimeList.clear();
        listWidget->clear();
    }
    QStringList temp;
    //临时存储时间
    QStringList timeTemp;
    QStringList parseTime;
    QString name = nowFile;
    QString lrcPath = FilePath + name.remove("mp3") + "lrc";    //通过当前播放的音乐名字去修改后缀去匹配同名的歌词文件

    QFile lrcFile(lrcPath);
    if (!lrcFile.open(QIODevice::ReadOnly | QIODevice::Text))
              return;
    while (!lrcFile.atEnd()) {                                  //将歌词文件中的歌词与时间段剪切，保存到两个列表中
                QString line = lrcFile.readLine();
                temp = line.split("]");
                if(temp.at(1).size() > 1)
                    listWidget->addItem(temp.at(1));
                QStringList buff = temp.at(0).split("[");
                timeTemp.append(buff.at(1));
          }

    for(int i = 5; i < timeTemp.size(); i++)                    //处理时间列表保存的数据，转成double保存，方便后续比较，保存到lrcTimeList
    {
        parseTime = timeTemp.at(i).split(":");
        QString min = parseTime.at(0);
        QString sec = parseTime.at(1);

        double group = min.toDouble() * 60 + sec.toDouble();


        lrcTimeList.append(QString::number(group));

    }
}

```

----
**获取歌词 showLrc 代码**

```

void musicWindows::showLrc()
{
    int now = 0;
    double thisTime = preTime;
    qDebug() << "showLrc " << thisTime;
    for(int i = 0; i < lrcTimeList.size()-1; i ++)              //通过匹配当前音乐播放的时间在歌词列表中的区间，来播放当前的歌词段
    {
        if(thisTime >= lrcTimeList.at(i).toDouble() && thisTime <= lrcTimeList.at(i+1).toDouble())
        {
            now = i;
            listWidget->setCurrentRow(now);
            listWidget->scrollToItem(listWidget->currentItem(), QAbstractItemView::PositionAtCenter);
            break;
        }
    }
}

```
----

**重点！！！**

> listWidget->setCurrentRow(now);                                                               //自动移动listWidget当前的选中区，输入第几行
> listWidget->scrollToItem(listWidget->currentItem(), QAbstractItemView::PositionAtCenter);     //保持当前显示的歌词在listWidget中间 

----

**读取数据函数的重写**
```
void musicWindows::readData()
{
    QString msg = mPro->readAll();
    qDebug() << "music process " << msg ;
    if(msg.indexOf("ANS_TIME_POSITION") > -1)
    {
        msg.remove(0, msg.lastIndexOf("=")+1);
        QString thisMsg = msg;
        preTime = thisMsg.toDouble();
         //通过计算时间去改变当前百分比
        precent = (int)(preTime/allTime *100);
        emit reflashRate(precent);
        //刷字幕
        showLrc();                                                              //在父类的基础上添加这一句，用来随着定时器的提醒来更新字幕(歌词)                                                              
        qDebug() << "allTime : " << allTime << "preTime : " << preTime << "precent : " << precent;
    }
    else if(msg.indexOf("ANS_LENGTH") > -1)
    {
        //获取总的时间
        msg.remove(0, msg.lastIndexOf("=")+1);
        QStringList temp = msg.split("\n");
        allTime = temp.at(0).toDouble();
    }
}

```
----
**读取数据函数的重写**

```
void musicWindows::readyData()
{
    getLrc();                   //获取歌词信息
    Title->setText(nowFile);    //打印当前播放的音乐名字
}

```

----

[我的GITHUB](https://github.com/BBIGQ-LYQ)


