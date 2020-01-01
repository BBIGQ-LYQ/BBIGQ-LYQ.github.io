---
title: 串口编程Demo
date: 2020-01-01 16:35:12
tags: 
      - study
      - UART
      
top: 1
---

## 在Linux上学习串口编程  
<!--more-->

### 认识串口

> 串口通信：硬件与硬件之间的一种通信方式，也是我们在项目中最简单的通信方式。
> 除了串口的这种通信方式以外还有：USB、I2C、SPI、CAN、LIN等。
> 串口是双向通信的。

----

### 打开串口
直接通过open来打开串口驱动文件
> int uartFd=open("/dev/ttySAC1",O_RDWR|O_NOCTTY);

通过**uartFd**的值判断是否成功打开串口
                int uartFd=open("/dev/ttySAC1",O_RDWR|O_NOCTTY);
                if(uartFd < 0)
	        {
		        perror("open fail:");
		        return -1;
	        }
	        else
	        {
		        //打开串口成功!!
		        printf("open uart ok\n");
	        } 

### 配置串口
> 串口的唯一难点就是配置串口属性
> 这里需要添加一个头文件**#include <termios.h>**
```
int set_serial_uart(int ser_fd)
{
	struct termios new_cfg,old_cfg;                                 //串口配置结构体
		
	/*保存并测试现有串口参数设置，在这里如果串口号等出错，会有相关的出错信息*/ 
	if	(tcgetattr(ser_fd, &old_cfg) != 0)
	{
		perror("tcgetattr");
		return -1;
	}
	
	bzero( &new_cfg, sizeof(new_cfg));
	/*原始模式*/
	/* 设置字符大小*/
	
	new_cfg = old_cfg;  //结构体变量赋值
	
	cfmakeraw(&new_cfg); /* 配置为原始模式 */ 

	/*波特率为115200*/
	cfsetispeed(&new_cfg, B115200); 
	cfsetospeed(&new_cfg, B115200);
	
	new_cfg.c_cflag |= CLOCAL | CREAD;
	
	/*8位数据位*/
	new_cfg.c_cflag &= ~CSIZE;
	new_cfg.c_cflag |= CS8;

	/*无奇偶校验位*/
	new_cfg.c_cflag &= ~PARENB;

	/*1位停止位*/
	new_cfg.c_cflag &= ~CSTOPB;
	/*清除串口缓冲区*/
	tcflush(ser_fd,TCIOFLUSH);
	new_cfg.c_cc[VTIME] = 0;
	new_cfg.c_cc[VMIN] = 1;
	tcflush ( ser_fd, TCIOFLUSH);
	/*串口设置使能*/
	tcsetattr( ser_fd ,TCSANOW,&new_cfg);
}
```

----

> struct termios结构体原型：

```
struct termios {
tcflag_t c_iflag;      /* input modes  输入模式 */
tcflag_t c_oflag;      /* output modes 输出模式 */
tcflag_t c_cflag;      /* control modes 控制模式*/
tcflag_t c_lflag;      /* local modes   本地模式*/
cc_t   c_cc[NCCS];     /* special characters 控制字符 */
}

```
----

### 使用串口
> 对于串口的读写，使用read()、write()函数即可

----

### 额外的接口

1.获取串口的原始状态 
int tcgetattr(int fd, struct termios *termios_p);
参数一：需要获取的串口设备描述符  
参数二：获取状态后保存的地址。
返回值：0 成功 
       -1 失败 
	   
2.把新的串口配置为原始模式  
void cfmakeraw(struct termios *termios_p);
参数一：需要配置的串口 

3.设置新串口的属性  
设置速率：
speed_t cfgetispeed(const struct termios *termios_p);
speed_t cfgetospeed(const struct termios *termios_p);

数据位，奇偶校验，停止位的设置具体看文档。  

//刷新串口
int tcflush(int fd, int queue_selector);
例子：
tcflush(ser_fd,TCIOFLUSH);

----

## 使用QT提供的接口下使用串口
库文件/头文件
> Header: #include <QSerialPort> 
> qmake: QT += serialport

```
        //设置需要初始化的串口
        QSerialPort *myPort = new QSerialPort("/dev/ttySAC3");
        //打开串口设备
        myPort->open(QIODevice::ReadWrite);                             //设置打开权限
        //设置串口属性
        myPort->setBaudRate(QSerialPort::Baud9600);                     //设置波特率  9600
        myPort->setDataBits(QSerialPort::Data8);                        //设置数据位  8位
        myPort->setStopBits(QSerialPort::OneStop);                      //设置停止位  1位停止位
        myPort->setParity(QSerialPort::NoParity);                       //设置奇偶校验 无奇偶校验 
        myPort->setFlowControl(QSerialPort::NoFlowControl);             //设置控制流  无控制流

        //关联可读信号
        connect(myPort, SIGNAL(readyRead()), this, SLOT(readAllData()));//关联串口可读信号
```

QT中使用接口对串口的读写操作也是直接使用write和read
**用例**  
> myPort->readAll();
> myPort->write(BUFF, BUFF.size());


----
> [我的GITHUB](https://github.com/BBIGQ-LYQ)

