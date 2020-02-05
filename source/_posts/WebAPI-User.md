---
title: WebAPI-User
date: 2020-01-22 13:05:52
tags: 
      - study
      - API
      
top: 2
---
# Web API 协议

<!--more-->

## Web API一般采用HTTP作为底层协议

> HTTP 请求机制如下：

+ 1:客户端向服务器发送一个请求；
+ 2：服务器给客户端一个响应，告诉客户端是否可以完成它请求的工作；
----
### HTTP协议包含的内容：

+ 1：URL(API调用地址)       ---> http://example.com
+ 2：Method(请求方式)       ---> POST/GET/PUT/...
+ 3：Header(请求头)         ---> User-Agent/认证信息等等
+ 4：Body(请求体)           ---> Data
----
#### 请求方式Method：
+ GET：请求服务器获取一个资源
+ POST：请求雾浮起创建要给新的资源
+ PUT：请求服务器编辑或者更新一个已经存在的资源
+ DELETE：请求服务器删除一个资源
----
#### 请求头(Header)
+ 提供了请求的元信息，是一个简单的项目列表，其中有客户端发送请求的时间和请求大小、身份认证等信息
----
#### 请求体(Body)
+ 包含了客户端希望发送给服务器的数据

----

## 服务端向客户端返回内容
> + 当成功调用API后，除了返回数据外，还会包含一个状态码，处理成功返回的2xx：

|HTTP状态码|语义|
|:--:|:--:|
200 OK-[GET]|服务器成功返回用户请求数据
201 CREATED-[POST/PUT/PATCH]|用户新建或修改数据成功
202 Accepted-[*]|表示一个请求已经进入后台排队(异步任务)
204 NO CONTENT-[DELETE]|用户删除数据成功

----

> + API未调用成功，则返回错误码。服务器错误码是5xx，表示服务不可用（此时一般建议使用重试或者联系商品页面的API服务商）

|错误代码|HTTP状态码|语义|解决方案|
|:--:|:--:|:--:|:--:|
Internal|500|API网关内部错误|建议重试
Failed To Invoke Backend Service|500|底层服务错误|API提供者底层服务错误，建议重试
Service Unavailable|503|服务不可用|建议稍后重试
Async Service|504|后端服务超市|建议稍后重试

> + 客户端错误码一般为4xx类型，种类最多，其中主要是参数错误、签名错误、请求方式有误或者被流控限制等，这里就不一一阐述了

需要查错误码表，可以看这里[aliyun错误码表][1]。

[1]:https://help.aliyun.com/document_detail/43906.html "aliyun错误码表"

***注意：有些API错误码是用户自己添加的，根据不同的提供商，有不同的错误码***

## 返回数据格式
> 一般返回的数据格式为JSON，部分为XML格式的

----

## API简单身份认证(APPCODE方式)
认证方式主要根据前面所说的Header中包含的信息
可以通过APPCODE的方式，实现到被调用接口的身份认证，获取访问相关API的调用权限
> 使用方法：
+ 请求Header中添加的Authorization字段
+ 配置Authorization字段的值为"APPCODE + 半角空格 + APPCODE值"

> 格式：
+ Authorization:APPCODE AppCode值

> 事例：
+ Authorization:APPCODE xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx

## API签名认证(AppKey & AppSecret)
AppKey和AppSecret相当于当前账户的另外一套账号和密码机制，在云市场购买API之后，就可以在控制台找到对应的AppKey和AppSecret。
