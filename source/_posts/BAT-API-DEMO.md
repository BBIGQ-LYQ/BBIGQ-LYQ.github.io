---
title: BAT API DEMO
date: 2019-12-21 14:43:23
tags: 
      - study
      - QT
      
top: 1

---


## 百度API接口的使用小demo

<!--more-->

### 第一步：
> 获取需要的API
![第一步](/blogmat/getAPI.PNG)

----

### 第二步：
> 下载SDK
![第二步](/blogmat/2.PNG)
![第二步](/blogmat/3.PNG)
----
### 第三步：
> 根据提示安装库文件
![第三步](/blogmat/4.PNG)
----

```

CURL:    sudo apt-get install   libcurl4-openssl-dev
jsoncpp： sudo apt-get install   libjsoncpp-dev
openssl： sudo apt-get install   libssl-dev

```


#### 第四步：
> 配置环境变量

```

export CPLUS_INCLUDE_PATH=$CPLUS_INCLUDE_PATH:/usr/include/jsoncpp

```
#### 第五步：编写代码

```

    // 设置APPID/AK/SK
        std::string app_id = "18078217";
        std::string api_key = "rtYVoXqqeR7278NI0qGh99q9";
        std::string secret_key = "u9FauW1mwvCfg9zdkwo58WzkYNKh6QNp";

        aip::Imageclassify client(app_id, api_key, secret_key);
        

        Json::Value result;

        std::string image;
        aip::get_file_content("./assets/timg.jpeg", &image);    //此处填入对应的图片路径

        // 调用车型识别
        result = client.car_detect(image, aip::null);

        // 如果有可选参数
        std::map<std::string, std::string> options;
        options["top_num"] = "3";
        options["baike_num"] = "5";

        // 带参数调用车型识别
        result = client.car_detect(image, options);
        
        std::cout << result << endl;;
    
}

```


#### 第六步：编译

```
g++   main.cpp     -std=c++11   -lcurl -lcrypto  -ljsoncpp

```
> 运行结果
![结果](/blogmat/5.png)
[我的GITHUB](https://github.com/BBIGQ-LYQ)
----
