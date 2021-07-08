## 1、概述

JDBC（Java DataBase Connectivity,  Java数据库连接） ,是一种用于执行SQL语句的Java API，为多种关系数据库提供统一访问,它由一组用Java语言编写的类和接口组成。

SUN公司是JDBC规范制定者（连接数据库规范），数据库厂商都遵守这个规范。从代码角度上说，jdbc就是接口，数据库提供jar包实现这些接口。

![JDBC](pictures\JDBC\JDBC.png)



## 2、使用

### 2.1 步骤

- 加载一个Driver驱动
- 创建数据库连接（Connection）
- 创建SQL命令发送器Statement
- 通过Statement发送SQL命令并得到结果
- 处理结果（select语句）
- 关闭数据库资源ResultSet  Statement  Connection

### 2.2 示例

```java
//启动
Class.forName("com.mysql.jdbc.Driver");
//连接
Connection connection = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/mydatabase?useUnicode=true&characterEncoding=utf-8", "root", "610527");
//发送器
Statement statement = connection.createStatement();
//发送
statement.executeUpdate("SQL"); //增删改
statement.executeQuery("SQL");  //查
//关闭连接
statement.close();
connection.close();
```





## 3、预编译

### 3.1 示例

```java
String sql = "insert into student values(?, ?,) ";
PreparedStatement preparedStatement = connection.prepareStatement(sql);
preparedStatement.setLong(1, 106);
preparedStatement.setString(2, "yi");
ResultSet resultSet = preparedStatement.executeQuery();
```

### 3.2 作用

- 防止注入攻击

  ```java
  //场景，登录是通过特殊账户和密码破坏sql语句
  //普通sql拼接
  String name = "guest";
  String password = "' or 'a' = 'a"; 
  String sql = "select * from account where name = '"+name+"' and password='"+password+"'";
  //select * from account where name = 'guest' and password='' or 'a' = 'a'
  
  //预编译
  String sql = "select * from account where name = ? and password = ? ";
  PreparedStatement preparedStatement = connection.prepareStatement(sql);
  preparedStatement.setString(1, name);
  preparedStatement.setString(2, password);
  ResultSet resultSet = preparedStatement.executeQuery();
  //SELECT * FROM users WHERE  userName = 'xiaoming' AND password = 'anything\'   OR\'x\'=\'x\''
  ```

- 提高效率

  - 多条语句插入时。重复编译同样的sql语句浪费效率。可以把编译好的sql的字节码文件存在内存上，提高效率。

  - 使用预编译不是一定会缓存sql语句，还需要开启此功能。需在Connection中url中添加参数

    ```java
    "&useServerPrepStmts=true&cachePrepStmts=true"; 
    ```



## 4、事务

- connection.setAutoCommit(false);  //关闭自动提交
- connection.rollback(); //回滚
- connection.commit();  //提交
- 一般关闭自动提交后，不用写回滚和提交，系统会帮我们完成。但是如果异常后try...catch掉异常，并且不显式地回滚，然后再显式地提交connection.commit();，会使得数据不一致

```java
//场景：转账时，转入转出必须保持一致。
public void test1() throws ClassNotFoundException, SQLException {
        Class.forName("com.mysql.jdbc.Driver");
        String url = "jdbc:mysql://127.0.0.1:3306/mydatabase?useUnicode=true&characterEncoding=utf-8";
        Connection connection = DriverManager.getConnection(url, "root", "610527");
        connection.setAutoCommit(false);  //关闭自动提交
        String sql="update account set money =money- ? where id = ?";
        PreparedStatement ps = connection.prepareStatement(sql);

        try {
            //转入
            ps.setDouble(1, 100);
            ps.setLong(2,1);
            ps.executeUpdate();
            //异常
            int a = 1/0;
            //转出
            ps.setDouble(1, -100);
            ps.setLong(2,2);
            ps.executeUpdate();
        }catch (Exception e){
            connection.rollback(); //回滚
        }finally {
            connection.commit();  //提交
        }
        connection.close();
    }

```



## 5、api

| api                                                          | 作用                     |
| ------------------------------------------------------------ | ------------------------ |
| Connection connection = DriverManager.getConnection(url, 账号, 密码); | 获取连接                 |
|                                                              |                          |
| Connection                                                   |                          |
| Statement connection.createStatement()                       | 获取Statement            |
| PreparedStatement prepareStatement(String sql)               | 获取预编译的Statement    |
| void setAutoCommit (boolean autoCommit)                      | 启停自动提交             |
| void rollback();                                             | 回滚                     |
| void commit();                                               | 提交                     |
|                                                              |                          |
| int rows =  statement.executeUpdate(sql);                    | 增删改，返回修改的行数   |
| ResultSet resultSet = statement.executeQuery(sql);           | 查                       |
| int rows =  preparedStatement.executeUpdate(sql);            | 增删改，返回修改的行数   |
| ResultSet resultSet = preparedStatement.executeQuery();      | 查                       |
| preparedStatement.setInt(int pos, Int value);                | 为sql中的?按照顺序设置值 |
| preparedStatement.setString(int pos, String value);          | 为sql中的?按照顺序设置值 |
|                                                              |                          |
| ResultSet                                                    |                          |
| Long getLong(String parameter);                              | 根据字段名从结果中取数据 |
| Long getString(String parameter);                            | 根据字段名从结果中取数据 |



## 6、连接池

- 作用：和数据库的连接不断创建和修改，会耗费大量的资源。
- 实现：维护一个连接池，用了拿，不用了放回来，用完了新创建，放满了就销毁。
- 代码：逻辑有错，暂不修改

```java
//逻辑有错，暂不修改
package com.mysql.test;

import org.junit.Test;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.util.LinkedList;

public class ConnectionPool {

    private static LinkedList<Connection> connectionList;

    private static String driver ="com.mysql.jdbc.Driver";
    private static String url="jdbc:mysql://127.0.0.1:3306/mydatabase?useUnicode=true&characterEncoding=utf-8";
    private static String user="root";
    private static String password="610527";
    private static int connectionNum = 3;

    /**
     * 初始化
     */
    static {
        try {
            Class.forName(driver);
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
        connectionList = new LinkedList<>();
        for (int i = 0; i < connectionNum; i++) {
            connectionList.add(initConnection());
        }
        System.out.println(connectionList.size());
    }

    /**
     * 连接
     * @return
     */
    private static Connection initConnection(){
        try {
            return DriverManager.getConnection(url, user, password);
        } catch (SQLException e) {
            e.printStackTrace();
        }
        return null;
    }

    /**
     * 对外提供获取连接的
     * @return
     */
    public Connection getConnection(){
        if(connectionList.isEmpty()){
            System.out.println("获取成功：连接已使用完,创建新的连接"+connectionList.size());
             return this.initConnection();
        }
        System.out.println("获取成功：从连接池中获取连接"+connectionList.size());
        return connectionList.remove();
    }

    /**
     * 对外提供关闭连接
     * @param connection
     */
    public void closeConnection(Connection connection){
        if(connectionList.size() < connectionNum){
            System.out.println("关闭成功：实际并没有关闭，而是回到连接池中"+connectionList.size());
            connectionList.add(connection);
        }else {
            try {
                System.out.println("关闭成功：连接池已满，关闭此连接" +connectionList.size() );
                connection.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }

    @Test
    public void testConnectionPool(){
        getConnection();
        getConnection();
        getConnection();
        getConnection();
        getConnection();
        Connection connection = getConnection();
        closeConnection(connection);
    }
}

```























