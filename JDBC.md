# JDBC

JDBC是使用Java语言操作关系数据库的一套API。Java DataBase Connectivity

![image-20230307092606054](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230307092606054.png)

## 本质

- 官方（sun公司）定义的一套操作所有关系型数据库的规则，即接口。
- 各个数据库厂商去实现这套接口，提供数据库驱动jar包。
- 我们可以使用这套接口（JDBC）编程，真正执行的代码时驱动jar包中的实现类。

## 使用

需要注意mySQL 8.0之后注册驱动变了

```java
//驱动程序名
        String driver = "com.mysql.cj.jdbc.Driver";
        //URL指向要访问的数据库名java  java:库名
        String url = "jdbc:mysql://localhost:3306/[你的数据库名称]?useSSL=false&serverTimezone=UTC";
```

```java
import java.sql.*;

public class MySQLDemo {

    public static void main(String[] args) {
        //声明Connection对象
        Connection con;
        //驱动程序名
        String driver = "com.mysql.cj.jdbc.Driver";
        //URL指向要访问的数据库名java  java:库名
        String url = "jdbc:mysql://localhost:3306/employees?useSSL=false&serverTimezone=UTC";
        //MySQL配置时的用户名
        String user = "root";
        //MySQL配置时的密码
        String password = "Whm18051964286";
        //遍历查询结果集
        try {
            //加载驱动程序
            Class.forName(driver);
            //1.getConnection()方法，连接MySQL数据库！！
            con = DriverManager.getConnection(url,user,password);
            if(!con.isClosed())
                System.out.println("Succeeded connecting to the Database!");
            //2.创建statement类对象，用来执行SQL语句！！
            Statement statement = con.createStatement();
            //要执行的SQL语句
            String sql = "select * from employees";
            ResultSet rs = statement.executeQuery(sql);
            while (rs.next()) System.out.println(rs.getString("gender"));
            rs.close();
            con.close();
        } catch(ClassNotFoundException e) {
            //数据库驱动类异常处理
            System.out.println("Sorry,can`t find the Driver!");
            e.printStackTrace();
        } catch(SQLException e) {
            //数据库连接失败异常处理
            e.printStackTrace();
        }
        finally{
            System.out.println("数据库数据成功获取！！");
        }
    }
}
```

### DriverManager

驱动管理类：

1. 注册驱动
2. 获取数据库连接

![image-20230307104238393](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230307104238393.png)

![image-20230307104128975](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230307104128975.png)

### Connection

数据库连接对象：

1. 获取SQL的对象
2. 管理事务

```java
// 普通执行SQL对象
Statement createStatement();
// 预编译SQL的执行SQL对象：防止SQL注入
PreparedStatement prepareStatement(sql);
// 执行存储过程的对象
CallableStatement prepareCall(sql);
```

事务管理

```sql
// MySQL事务管理
// 开启事务
Begin; / START TRANSACTION;
// 提交事务
COMMIT;
// 回滚事务
ROLLBACK;

// MySQL默认自动提交事务
```

JDBC事务管理：Connection接口中定义了3个对应的方法

```java
// 开启事务
setAutoCommit(boolean autoCommit);//true为自动提交事务;false为手动提交事务，即为开启事务
// 提交事务
commit()
// 回滚事务
rollback()
```



### Statement

Statement的作用：执行SQL语句

![image-20230307151538998](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230307151538998.png)

### ResultSet

![image-20230307161520901](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230307161520901.png)

定义一个Employee类。小技能：alt + insert可以快速创建getter和setter方法。

```java
package org.example;

public class Employee {
    int id;
    String gender;
    String name;

    public Employee(int id, String gender, String name) {
        this.id = id;
        this.gender = gender;
        this.name = name;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getGender() {
        return gender;
    }

    public void setGender(String gender) {
        this.gender = gender;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Employee{" +
                "id=" + id +
                ", gender='" + gender + '\'' +
                ", name='" + name + '\'' +
                '}';
    }
}

```

将每行部分数据创建为一个对象存入arraylist。

```java
package org.example;

import java.sql.*;
import java.util.ArrayList;

public class MySQLDemo {

    public static void main(String[] args) {
        ArrayList<Employee> employees = new ArrayList<>();
        //声明Connection对象
        Connection con;
        //驱动程序名
        String driver = "com.mysql.cj.jdbc.Driver";
        //URL指向要访问的数据库名java  java:库名
        String url = "jdbc:mysql://localhost:3306/employees?useSSL=false&serverTimezone=UTC";
        //MySQL配置时的用户名
        String user = "root";
        //MySQL配置时的密码
        String password = "Whm18051964286";
        //遍历查询结果集
        try {
            //加载驱动程序
            Class.forName(driver);
            //1.getConnection()方法，连接MySQL数据库！！
            con = DriverManager.getConnection(url,user,password);
            if(!con.isClosed())
                System.out.println("Succeeded connecting to the Database!");
            //2.创建statement类对象，用来执行SQL语句！！
            Statement statement = con.createStatement();
            //要执行的SQL语句
            String sql = "select * from employees";
            ResultSet rs = statement.executeQuery(sql);
            while (rs.next()) {
                int id = rs.getInt(1);
                String name = rs.getString(3) + " " + rs.getString(4);
                String gender = rs.getString(5);
                Employee e = new Employee(id, gender, name);
                employees.add(e);
                System.out.println(employees.get(employees.size() - 1).toString());
            }
            rs.close();
            statement.close();
            con.close();
        } catch(ClassNotFoundException e) {
            //数据库驱动类异常处理
            System.out.println("Sorry,can`t find the Driver!");
            e.printStackTrace();
        } catch(SQLException e) {
            //数据库连接失败异常处理
            e.printStackTrace();
        }
        finally{
            System.out.println("数据库数据成功获取！！");
        }


    }

}
```

### PreparedStatement

![image-20230307164318518](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230307164318518.png)

![image-20230307165445077](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230307165445077.png)



# 数据库连接池

![image-20230307171055554](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230307171055554.png)

![image-20230307171428386](http://kiyotakawang.oss-cn-hangzhou.aliyuncs.com/img/image-20230307171428386.png)