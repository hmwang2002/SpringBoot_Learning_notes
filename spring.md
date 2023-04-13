# 笔记

## 核心概念

### IoC(Inversion of Control)控制反转

使用对象时，由主动new产生对象转换为由**外部**提供对象，此过程中对象创建控制权由程序转移到**外部**，此思想称为控制反转

- Spring技术对IoC思想进行了实现
  - Spring提供了一个容器，称为IoC容器，用来充当IoC思想中的“外部”
  - IoC容器负责对象的创建，初始化等一系列工作，被创建或被管理的对象在IoC容器中统称为Bean

![image-20230301221214508](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230301221214508.png)

![image-20230301221326152](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230301221326152.png)