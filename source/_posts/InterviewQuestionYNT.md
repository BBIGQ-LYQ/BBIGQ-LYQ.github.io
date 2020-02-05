---
title: InterviewQuestionYNT
date: 2020-01-17 18:41:00
tags: 
      - study
      - interview
      
top: 2
---

# Summary of minor interview problems

<!--more-->

## First blood
> **printf**

 C中的printf计算参数时，是从右到左压栈的！！！并不是单纯的从左到右去分析，示例如下：

 ```
 int arr[] = {6, 7, 8, 9};
    int  *ptr = arr;
    *(ptr++) += 123;

    printf("%d,%d \n", *ptr, *(++ptr));
```
这里输出的结果是：**8,8**
 
----
## Second blood
> **if**

初学者一般使用if等判断语句的时候，都会直接使用比如以下的写法
```
if(a == 123)
{
    /*
    ***
    */
}
```
但是以下这种写法会比较好
```
if(123 == a)
{
    /*
    ***
    */
}
```
原因是:如果一个工程较大的时候，当错误的将 '==' 写成 '=' 的时候，第一种写法会直接判断通过，并且修改了a的值，会给后续Debug带来极大的障碍；
而第二种就可以避免了，常量无法被赋值，会直接编译不通过或者报错，可以快速的定位到出错的地方；


