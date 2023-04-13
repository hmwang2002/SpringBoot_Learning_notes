# Web核心

## JavaWeb技术栈

- B/S架构：Browser/Server，浏览器/服务器 架构模式，它的特点是，客户端只需要浏览器，应用程序的逻辑和数据都存储在服务器端。浏览器只需要请求服务器，获取Web资源，服务器把Web资源发送给浏览器即可。
  - 静态资源：HTML、CSS、JavaScript、图片等。负责页面展现
  - B/S架构好处：易于维护升级：服务器端升级后，客户端无需任何部署就可以使用到新的版本

- 动态资源：Servlet，JSP等。负责逻辑处理

- 数据库：负责存储数据
- HTTP协议：定义通信规则![截屏2023-03-23 14.12.45](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2023-03-23%2014.12.45.png)

## HTTP

超文本传输协议，规定了浏览器和服务器之间数据传输的规则。

![截屏2023-03-23 14.28.02](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2023-03-23%2014.28.02.png)

对于多次请求不能共享数据的问题：Java使用（Cookie、Session）来解决这个问题。

### 请求数据格式

![截屏2023-03-23 14.31.03](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2023-03-23%2014.31.03.png)

![截屏2023-03-23 14.31.58](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2023-03-23%2014.31.58.png)

GET请求和POST请求区别：

1. GET请求参数在请求行中，没有请求体。POST请求请求参数在请求体中。
2. GET请求请求参数大小有限制，POST没有

![截屏2023-03-23 14.37.03](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2023-03-23%2014.37.03.png)

### 响应数据格式

![截屏2023-03-23 14.38.43](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2023-03-23%2014.38.43.png)

![截屏2023-03-23 14.39.38](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2023-03-23%2014.39.38.png)

![截屏2023-03-23 14.57.34](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2023-03-23%2014.57.34.png)

重要的状态码：200 OK

400 Bad Request

404 Not found

500 服务器出错了



## Tomcat

Web服务器是一个应用程序（软件），对HTTP协议的操作进行封装，使得程序员不必直接对协议进行操作，让web开发更加便捷。主要功能是“提供网上信息浏览服务”。

Tomcat也被称为Web容器、Servlet容器。Servlet需要依赖Tomcat才能运行。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   

![image-20230325221451098](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230325221451098.png)

![image-20230325222120957](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230325222120957.png)

## Servlet

![image-20230325224118028](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230325224118028.png)