# SpringBoot配置

## 配置文件分类

![截屏2023-04-12 18.28.39](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2023-04-12%2018.28.39.png)

两种方式：properties采用句柄方式，yml对于所属的属性采用缩进的方式

![截屏2023-04-12 18.29.10](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2023-04-12%2018.29.10.png)

### 小结

- SpringBoot提供了2种配置文件类型：properties和yml/yaml
- 默认配置文件名称：application
- 在同一级目录下优先级为：properties > yml > yaml

## YAML

![截屏2023-04-12 18.37.30](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2023-04-12%2018.37.30.png)

![截屏2023-04-12 18.41.23](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2023-04-12%2018.41.23.png)

### 基本语法

- 大小写敏感
- 数据值前边必须有空格，作为分隔符
- 使用缩进表示层级关系
- 缩进时不允许使用Tab键，只允许使用空格（各个系统Tab对应的空格数目可能不同，导致层次混乱）。
- 缩进的空格数目不重要，只要相同层级的元素左侧对齐即可
- #表示注释，从这个字符一直到行尾，都会被解析器忽略

### 数据格式

- 对象（map）：键值对的集合（冒号后面要加空格！）

  ```yaml
  person:
  	name: zhangsan
  # 行内写法
  person: (name: zhangsan)
  ```

- 数组：一组按次序排列的值

  ```yaml
  address:
  	- beijing
  	- shanghai
  # 行内写法
  address: [beijing, shanghai]
  ```

- 纯量：单个的、不可再分的值

  ```yaml
  msg1: 'hello \n world' # 单引号忽略转义字符
  msg2: "hello \n world" # 双引号识别转义字符
  ```

  ### 参数引用
  
  ```yaml
  name: lisi
  
  person:
  	name: ${name} #引用上边定义的name值
  ```
  
  

## 获取数据

1. @Value
2. Environment
3. @ConfigurationProperties(prefix = "")

## Profile

我们在开发Spring Boot应用时，通常同一套程序会被安装到不同环境，比如：开发、测试、生产等。期中数据库地址、服务器端口等等配置都不同，如果每次打包时，都要修改配置文件，那么非常麻烦。profile功能就是来进行动态配置切换的。

### profile配置方式

- 多profile文件方式
- yml多文档方式

### 激活方式

- 配置文件
- 虚拟机参数
- 命令行参数

## 内部配置加载顺序

![image-20230415212315208](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230415212315208.png)
