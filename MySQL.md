# MySQL

![image-20230303173819508](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230303173819508.png)

MySQL实际上是一个数据库管理系统。

## 关系型数据库

关系型数据库是建立在关系模型基础上的数据库，简单说，关系型数据库是由多张能互相连接的二维表组成的数据库。

**优点**：

- 都是使用表结构，格式一致，易于维护。
- 都是通用的SQL语言操作，使用方便，可用于复杂查询。
- 数据存储在磁盘中，安全。

## SQL

Structure Query Language结构化查询语言，一门操作关系型数据库的编程语言。定义了操作所有关系型数据库的统一标准。对于同一个需求，每一种数据库操作的方式可能会存在一些不一样的地方，我们称为“方言”。

### SQL通用语法

1. SQL语句可以单行或多行书写，以分号结尾。
2. MySQL数据库的SQL语句不区分大小写，关键字建议使用大写。
3. 注释
   - 单行注释：-- [空格]注释内容（注意--后面一定要加空格） 或 #注释内容（MySQL特有）
   - 多行注释：/* 注释 */

![image-20230303185408940](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230303185408940.png)

### DDL

数据库操作

```sql
show databases; #查询有多少个数据库
create database db1;
create database if not exists db1;
drop database db1; #删除数据库
drop database if exists db1;

use 数据库名称; #使用数据库
select database(); #查看当前使用的数据库
```

表操作

```sql
show tables;
desc 表名; #查询表结构
create table 表名 (
	字段名1 数据类型1,
    字段名2 数据类型2,
    ...
    字段名n 数据类型n
);
```

### SQL数据类型

![image-20230303194735645](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230303194735645.png)

age int

score double(总长度，小数点后保留的位数)

定长字符串和变长字符串：

name char(10)  存储性能高 浪费空间

name varchar(10)  存储性能低 节约空间

varchar会根据数据的长度再决定存储空间的大小，而char是指定多少存储空间。当然，都有一个限定的最大长度。

### DML

对表中数据进行增删改的操作。

 ```sql
 -- 查询所有数据
 select * from 表
 -- 给指定列添加数据
 insert into 表名(列名1，列名2，...) values(值1，值2，...);
 -- 修改表数据
 update 表名 set 列名1=值1,列名2=值2，... [WHERE 条件];
 update stu set sex = '女' where name = '张三';
 -- 删除
 delete from 表名 [where 条件];
 ```

![image-20230304225215383](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230304225215383.png)

### DQL

#### 基础查询

1. 查询多个字段

```sql
select 字段列表 from 表名;
select * from 表名; -- 查询所有数据
```

2. 去除重复记录

```sql
select distinct 字段列表 from 表名;
```

3. 起别名

```sql
AS: AS 也可以省略
```

#### 条件查询

```sql
select 字段列表 from 表名 where 条件列表;
```

在sql里面与符号既可以是&&，也可以是and。

![image-20230305213609333](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230305213609333.png)

sql的等于不能用==，用=就可以了

![image-20230305214837794](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230305214837794.png)

null值的比较不能使用=或!=，需要使用is或is not。

![image-20230305215135462](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230305215135462.png)



关于模糊查询like：

1. _：代表单个任意字符
2. %：代表任意个数字符

```sql
select * from where name like '马%';
select * from where name like '_花%';
select * from where name like '%德%';
```

#### 排序查询

![image-20230305220327514](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230305220327514.png)

![image-20230305222540597](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230305222540597.png)

**注意：**有多个排序条件时，当前边的条件值一样时，才会根据第二条件进行排序。

#### 聚合函数

1. 概念：将一列数据作为一个整体，进行纵向计算。
2. 聚合函数分类：

![image-20230305222954227](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230305222954227.png)

3. 聚合函数语法：

```sql
select 聚合函数名（列名） from 表;
```

注意：null值不参与所有聚合函数运算

#### 分组查询

1. 分组查询语法

```sql
select 字段列表 from [where 分组前条件限定] group by 分组字段名 [having 分组后条件过滤];
```

![image-20230305223846000](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230305223846000.png)

![image-20230305224014200](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230305224014200.png)

注意：分组之后，查询的字段为聚合函数和分组字段，查询其他字段无任何意义

#### 分页查询

1. 分页查询语法

```sql
select 字段列表 from limit 起始索引，查询条目数;
```

![image-20230305225030615](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230305225030615.png)

### 约束

1. 约束的概念

- 约束是作用于表中列上的规则，用于限制加入表中的数据
- 约束的存在保证了数据库中数据的正确性、有效性和完整性

2. 约束的分类

| 约束名称 | 描述                                                         | 关键字       |
| -------- | ------------------------------------------------------------ | ------------ |
| 非空约束 | 保证列中所有数据不能有null值                                 | NOT NULL     |
| 唯一约束 | 保证列中所有数据各不相同                                     | UNIQUE       |
| 主键约束 | 主键是一行数据的唯一标识，要求非空且唯一                     | PRIMARY KEY  |
| 检查约束 | 保证列中的值满足某一条件                                     | CHECK        |
| 默认约束 | 保存数据时，未指定值则采用默认值                             | DEFUAULT     |
| 外键约束 | 外键用来让两个表的数据之间建立链接，保证数据的一致性和完整性 | FOREIGHN KEY |

**Tips：**MySQL不支持检查约束

![image-20230306184402765](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230306184402765.png)

自增长：auto increment

![image-20230306195125526](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230306195125526.png)

#### 外键约束

将不同的表建立连接关系。

##### 概念

外键用来让两个表的数据之间建立链接，保证数据的一致性和完整性

##### 语法

1. 添加约束

![image-20230306195612502](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230306195612502.png)

2. 删除约束

![image-20230306195631564](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230306195631564.png)

## 表关系

- 一对一
- 一对多（多对一）
- 多对多

![image-20230306212420495](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230306212420495.png)

## 多表查询

1. 连接查询
   - 内连接：相当于查询A B交集数据
   - 外连接：
     - 左外连接：相当于查询A表所有数据和交集部分数据
     - 右外连接：相当于查询B表所有数据和交集部分数据
2. 子查询

### 内连接

![image-20230306213728419](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230306213728419.png)

### 外连接

![image-20230306214042338](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230306214042338.png)

### 子查询

查询中嵌套查询，称嵌套查询为子查询。

![image-20230306214517542](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230306214517542.png)

![image-20230306214607282](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230306214607282.png)

![image-20230306215644063](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230306215644063.png)

## 事务

- 数据库的事务是一种机制、一个操作序列，包含了一组数据库操作命令
- 事务把所有的命令作为一个整体一起向系统提交或撤销操作请求，即这一组数据库命令要么同时成功，要么同时失败
- 事务是一个不可分割的工作逻辑单元





![image-20230306220721597](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230306220721597.png)
