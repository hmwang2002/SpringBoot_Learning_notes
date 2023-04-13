# Maven

Maven是专门用于管理和构建Java项目的工具，它的主要功能有：

- 提供了一套标准化的项目结构
- 提供了一套标准化的构建流程（编译，测试，打包，发布......）
- 提供了一套依赖管理机制

Maven提供了一个标准化的项目结构

![image-20230311153336227](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230311153336227.png)

## 依赖管理

依赖管理其实就是管理你项目所依赖的第三方资源（jar包、插件...）

如果没使用Maven：

1. 下载jar包
2. 复制jar包到项目
3. 将jar包加入工作环境

使用Maven：

1. Maven使用标准的坐标配置来管理各种依赖
2. 只需要简单的配置就可以完成依赖管理

![image-20230311160902527](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230311160902527.png)

![image-20230311161117704](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230311161117704.png)

## 常用命令

mvn + 命令

compile：编译

clean：清理

test：测试

package：打包

install：安装

## Maven生命周期

![image-20230311162418837](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230311162418837.png)

## Maven坐标

什么是坐标？

- Maven中的坐标是资源的唯一标识
- 使用坐标来定义项目或引入项目中需要的依赖

Maven坐标主要组成

- groupId：定义当前Maven项目隶属组织名称（通常是域名反写，例如：com.NJU)
- artifactId：定义当前Maven项目名称（通常是模块名称，例如order-service、goods-service）
- version：定义当前项目版本号

## 作用范围

![image-20230311170259875](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230311170259875.png)