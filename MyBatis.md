# MyBatis

MyBatis是一款优秀的持久层框架，用于简化JDBC开发。

## 持久层

- 负责将数据保存到数据库的那一层
- JavaEE三层架构：表现层、业务层、持久层

## 框架

- 框架就是一个半成品软件，是一套可重用的、通用的、软件基础代码模型
- 在框架的基础之上构建软件编写更加高效、规范、通用、可扩展

![image-20230316145412416](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230316145412416.png)

## Mapper代理开发

![截屏2023-03-16 18.48.00](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2023-03-16%2018.48.00.png)

步骤：

1. 定义与SQL映射文件同名的Mapper接口，并且将Mapper和SQL映射文件放置在同一目录下
2. 设置SQL映射文件的namespace属性为Mapper接口全限定名
3. 在Mapper接口中定义方法，方法名就是SQL映射文件中sql语句的id，并保持参数类型和返回值类型一致
4. 编码
   - 通过SqlSession的getMapper方法获取Mapper接口的代理对象
   - 调用对应方法完成sql的执行



如何让xml配置文件和你写的class在一个文件夹下：

1. 手动拖到里面。不推荐，因为在实际工作场景中要求resource和java类分开。
2. 在resource文件夹下新建同名文件夹。注意名称中不能用.来分割，因为Maven运行时不会识别，用/在编译生成的class文件中就会存放在同一个文件夹中。





## MyBatis核心配置文件

MyBatis 的配置文件包含了会深深影响 MyBatis 行为的设置和属性信息。 配置文档的顶层结构如下：

- configuration（配置）
  - properties（属性）
  - settings（设置）
  - typeAliases（类型别名）
  - typeHandlers（类型处理器）
  - objectFactory（对象工厂）
  - plugins（插件）
  - environments（环境配置）
    - environment（环境变量）
      - transactionManager（事务管理器）
      - dataSource（数据源）
  - databaseIdProvider（数据库厂商标识）
  - mappers（映射器）

### environments

配置数据库连接环境信息，可以配置多个environment，通过default属性切换不同的environment。

当然这些配置信息最后也会被更上层的Spring接管。

### typeAliases

配置别名，比如把数据类型文件夹名称XXXX.pojo添加进去，这样子在XXXMapper.xml中返回类型就只需要写一个类名就行了。

并且添加别名后不区分大小写。

## 配置文件完成增删改查

![截屏2023-03-16 19.34.41](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2023-03-16%2019.34.41.png)

熟练使用idea。。。不会真有傻子自己一个一个打字吧

![截屏2023-03-16 19.53.31](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2023-03-16%2019.53.31.png)

之后使用Spring框架之后，只用写一句即可：

```java
List<Brand> brands = brandMapper.selectAll();
```



三步：编写接口方法->编写SQL->执行方法。

**一个问题：**数据库表的字段名称和实体类的属性名称不一样，则不能自动封装数据。

**解决方案**：

1. 你在xml里面的sql语句中起一个别名。缺点：每次查询都要定义一次别名。
2. ![截屏2023-03-16 20.00.23](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2023-03-16%2020.00.23.png)

sql片段，然后语句中引用。

```xml
<include refid="XXX" />
```

当然这也会造成重复劳动。

3. resultMap。

```xml
<resultMap id="brandResultMap" type="brand">
  <result column="brand_name" property="brandName" />
  <result column="company_name" property="companyName"/>
</resultMap>
```

- Step1：定义\<resultMap>标签
- Step2：在select标签中使用resultMap属性替换resultType属性。也就是替换返回类型。

![截屏2023-03-16 20.19.56](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2023-03-16%2020.19.56.png)

参数占位符：

1. #{}:会将其替换为?，为了防止SQL注入
2. $():拼sql，会存在SQL注入的问题

所以传参时候记得用#{}，面试时候也会问这种问题。

在表名或者列名不固定的情况下，会用${}。

### xml特殊字符

- 转义字符
  - 比如<，用&lt

- CDATA区
  - 输入CD，自动补全一个CDATA区，里面正常输入





### 多条件查询

散装多条件需要加上注解，以表明用在哪个条件里面。

如果都是一个类里面的，直接传一个类也行。

也可以封装成一个Map集合，Map的key需要和参数名称保持一致。

![截屏2023-03-16 20.35.13](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2023-03-16%2020.35.13.png)

#### 散装参数

需要使用注解。

> 对于需要迷糊匹配的数据，需要将用户的数据进行程序上的处理，比如加上%或者_。

#### 对象参数

对象的属性名称要和参数占位符属性名称一致。

参数处理完后封装成对象就可以了。

#### Map集合参数

```java
Map map = new HashMap();
map.put("xx", "xxx");
```

### 动态查询

![image-20230318144013736](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230318144013736.png)

```xml
<if test="XXX != null">
    xxx = #{xxx}
</if>
与运算用and
```

动态条件查询

1. if：条件判断
   - test：逻辑表达式
   - 问题：会出现where后面一个条件都没有的情况。解决方案：1. 用恒等式过渡一下，这个方法比较low。2. mybatis提供了where标签。

![image-20230318144815707](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230318144815707.png)

![image-20230318144901039](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230318144901039.png)

#### 单条件的动态条件查询

从多个条件中选择一个。

choose(when, otherwise):选择，类似于Java中的switch语句。

![image-20230318145137935](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230318145137935.png)

otherwise可以不写，如果没有满足的条件，where标签就不会出现在sql语句中。

### 添加

![image-20230318145735526](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230318145735526.png)

需要提交事务

```java
sqlSession.commit();
```

也可以设置为自动提交事务

```java
SqlSession sqlSession = sqlSessionFactory.openSession(true);
```

![image-20230318150551835](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230318150551835.png)

返回添加数据的主键

![image-20230318150721869](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230318150721869.png)

### 修改

#### 修改全部字段

![image-20230318150847491](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230318150847491.png)

#### 修改动态字段

也就是修改的字段不固定。

![image-20230318151017610](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230318151017610.png)

### 删除

#### 删除一个

![image-20230318151439046](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230318151439046.png)

#### 批量删除

![image-20230318151609211](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230318151609211.png)

也就是改成where id in

![image-20230318151830721](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230318151830721.png)

## MyBatis参数传递

MyBatis接口方法中可以接收各种各样的参数，MyBatis底层对于这些参数进行不同的封装处理方式

单个参数：

1. POJO类型
2. Map集合
3. Collection：可以用@Param注解来替换MyBatis默认的key名
4. List：可以用@Param注解来替换MyBatis默认的key名
5. Array：可以用@Param注解来替换MyBatis默认的key名
6. 其他类型

多个参数：使用@Param注解

MyBatis提供了ParamNameResolver类来进行参数封装。

## 注解开发

写注解开发会比配置文件开发更加方便。但注解开发一般用来完成一些简单的语句。复杂的操作建议用xml，以保证效率。

![image-20230318204522015](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230318204522015.png)
