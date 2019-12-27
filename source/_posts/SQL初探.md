---
title: SQL初探
date: 2019-12-27 14:20:39
tags: 
      - study
      - SQL
      
top: 1
---

## SQLite 初探

<!--more-->

### 1、认识数据库

常用的数据库|库名|说明
--|:--:|---
MYSQL|ORACLE|SQLITE     
windos系统常用|大型服务器使用|轻量级的数据库，是最嵌入式设备最适用的数据库>

----

### 2、SQLITE  数据库的安装：

下载SQLITE   数据库源代码  
[传送门](https://www.sqlite.org/index.html)

----

### 3、安装数据库的方向键

        sudo apt-get install    libreadline-dev    //（方向键库文件）
        
----

### 4、在家目录解压数据库源程序 

        tar  -xvf  sqlite-autoconf-3300100.tar.gz  

----

### 5、配置sqlite文件  

        Cd  qlite-autoconf-3300100  
        ./configure 

----

### 6、编译源码

        Make    

----

### 7、安装

        Sudo Make install    
        sudo  cp  sqlite3   /bin/ 
        
----

### 8、测试数据库是否可用 
        
        Sqlite3
        
出现以下提示即为成功：
![成功](/blogmat/sqlite.png)

----

### 9、常用命令

SQLTIE3的常用命令|说明
--|--
.help|查看命令手册 
.quit|退出数据库  
.schema|显示表格中的内容 
.table|显示数据库中的表格

----

> 所有的.db文件都是数据库类的文件
> SQL 语句： 每一条sql语句都是以  ;（分） 号结束的！！！ 

基本的数据类型|说明
--|--
INTEGER|整型
REAL|浮点型
TEXT|字符串
BLOB|二进制
NULL|空

#### sqlite用法

        Create  table   表名(
           字段1名称  类型   约束条件，
           字段2名称  类型   约束条件，
           字段3名称  类型   约束条件，
            ....... 
        );

插入数据：
> Insert  into  表名 values(数据列表);
        
删除数据：
> delete  from 表名  where 表达式;
> delete from  student where  name='xxx';   //删除名字为xxx的数据

修改数据：
> update  表名  set  字段1=新的值，字段2=新的值 .... where  表达式;
> update  student  set  name=”小花”, id=198,sex=”女”  where  name=”小陈”;   //把小陈的信息改成小花的

查找数据：
> select  [*|字段名]   from   表名;
> Select  * from  表名;  //查找表格中的所有数据
> select  字段名  from  表名；//查找某一字段的数据

**带条件的查找：** 
>语法： select   [*|字段名]  from  表名   where  表达式；

> 表达式语句可选值： 
> =,  !=,  >,   <,  >= , <= 
> between   //在一定范围
> like       //通配

**例子1**
> select  *  from  student   where   id  between 100  and 200;  //查找id在100到200之间的数据

**例子2**
> select * from  student  where   name  like  “小%”    //查找名字带有小的所有数据
> select * from car  where  carid   like  “%A%”;        //查出车牌号 为粤a 的车，车牌带A 的都显示出来


----
[我的GITHUB](https://github.com/BBIGQ-LYQ)

