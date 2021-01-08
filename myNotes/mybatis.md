### 一、mysql依赖和配置

注：mysql server使用的是5.7版本

#### mysql-connector-java 8.0.15

```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.15</version>
</dependency>
```

```properties
driver=com.mysql.cj.jdbc.Driver
url=jdbc:mysql://127.0.0.1:3306/mydatabase?useSSL=false&useUnicode=true&characterEncoding=utf-8&serverTimezone=Asia/Shanghai
username=root
password=610527
```

#### mysql-connector-java 5.1.47

```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.47</version>
</dependency>
```

```properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://127.0.0.1:3306/mydatabase
username=root
password=610527
```

#### 错误一

场景：mysql-connector-java 5.1.47 配置 driver=com.mysql.cj.jdbc.Driver

错误：启动报错，com.mysql.cj.jdbc.Driver not found

补充：反之，只会报个警告。

#### 错误二

场景：mysql-connector-java 8.0.15 配置 url=jdbc:mysql://127.0.0.1:3306/mydatabase

错误：执行中当使用数据库时报错：无法识别多个时区，需要在jdbc配置时区（即使没有用到datatime）。

Error querying database.  Cause: java.sql.SQLException: The server time zone value 'ÖÐ¹ú±ê×¼Ê±¼ä' is unrecognized or represents more than one time zone. You must configure either the server or JDBC driver (via the serverTimezone configuration property) to use a more specifc time zone value if you want to utilize time zone support.





### 二、入门搭建

#### 1.建项目导包

##### 1.1 新建maven项目

##### 1.2 pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>mybatis</artifactId>
        <groupId>com-sunpeng</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>simpleMyBatis</artifactId>

    <dependencies>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.4</version>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.15</version>
        </dependency>

        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13</version>
        </dependency>
    </dependencies>
</project>
```

#### 2.mybaties全局配置：mybaties-config.xml

##### 2.1 mybaties-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <properties resource="db.properties"></properties>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="StudentMapper.xml"/>
    </mappers>
</configuration>
```

##### 2.2 db.properties

```properties
driver=com.mysql.cj.jdbc.Driver
url=jdbc:mysql://127.0.0.1:3306/mydatabase?useSSL=false&useUnicode=true&characterEncoding=utf-8&serverTimezone=Asia/Shanghai
username=root
password=610527
```

#### 3.编码与sql映射

##### 3.1 entity

```java
public class Student {
    private Long id;
    private String name;
    private Double score;
    private Integer age;
    
    //get、set
}
```

##### 3.2 Mapper.java

```java
package com.sunpeng.dao;
import com.sunpeng.entity.Student;
public interface StudentMapper {
    public void select();
    public Integer insert();
    public Integer update();
    public Integer delete();
    public Student selectById( Integer id);
}
```

##### 3.2 Mapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.sunpeng.dao.StudentMapper">

    <select id="selectById" resultType="com.sunpeng.entity.Student">
      select * from student where id = #{id}
    </select>

</mapper>
```

#### 4.启动测试

```java
public class Test01 {

    @Test
    public void test01() throws IOException {
        String resource = "mybatis-config.xml";
        InputStream inputStream = Resources.getResourceAsStream(resource);
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);

        SqlSession sqlSession = sqlSessionFactory.openSession();
        StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);
        Student student = mapper.selectById(10);

        System.out.println(student);
    }
}
```





### 三、springboot整合mybatis-plus

#### 1.新建spring Initializr工程

选择spingboot、mybatis、mysql依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.4.1</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.sunpeng</groupId>
    <artifactId>mybatis</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>mybatis</name>
    <description>Demo project for Spring Boot</description>

    <properties>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.0.5</version>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
    </dependencies>

</project>
```

#### 2.配置application.yml

```yml
spring:
  datasource:
    username: root
    password: 610527
    url: jdbc:mysql://127.0.0.1:3306/mydatabase?useSSL=false&useUnicode=true&characterEncoding=utf-8&serverTimezone=Asia/Shanghai
    driver-class-name: com.mysql.cj.jdbc.Driver
  profiles:
    active: dev
mybatis-plus:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
  global-config:
    db-config:
      logic-delete-value: 1
      logic-not-delete-value: 0
  #mapper-locations: classpath:UmsMemberMapper.xml
  #mapper-locations: classpath*:mapper/*.xml
  mapper-locations:
  - classpath*:/com/msb/msbdongbaoums/mapper/xml/*.xml
```

#### 3.编码与Mapper映射

##### 3.1 entity

```java
public class Student {
    private Long id;
    private String name;
    private Double score;
    private Integer age;
    
    //get、set
}
```

##### 3.2 Mapper.java

```java
import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.sunpeng.mybatis.entity.Student;
import org.springframework.stereotype.Repository;

@Repository
public interface StudentMapper extends BaseMapper<Student> {
    public Student selectById(Integer id);
}
```

##### 3.2 Mapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.sunpeng.mybatis.mapper.StudentMapper">

    <select id="selectById" resultType="com.sunpeng.mybatis.entity.Student">
      select * from student where id = #{id}
    </select>

</mapper>
```

#### 4.运行与测试

##### 4.1Application.class

```java
import org.mybatis.spring.annotation.MapperScan;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
@MapperScan("com.sunpeng.mybatis.mapper")
public class MybatisApplication {
    public static void main(String[] args) {
        SpringApplication.run(MybatisApplication.class, args);
    }
}
```

##### 4.2测试

```java
import com.sunpeng.mybatis.mapper.StudentMapper;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

@SpringBootTest
class MybatisApplicationTests {

    @Autowired
    StudentMapper studentMapper;

    @Test
    void contextLoads() {
        System.out.println(studentMapper.selectById(10));
    }
}
```

#### 5.总结

##### 5.1 springboot如何整合mybatis-plus

- 全局配置：因为springboot有全局配置文件Application.yml文件，所以全局配置可以放到这里

- 自动注入：studentMapper只是接口，如何创建对象并调用方法

  普通：通过工厂才能创建出studentMapper的实体对象

  ```java
  String resource = "mybatis-config.xml";
  InputStream inputStream = Resources.getResourceAsStream(resource);
  SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
  SqlSession sqlSession = sqlSessionFactory.openSession();
  StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);
  //mapper是实现StudentMapper的对象，才能待用具体方法
  Student student = mapper.selectById(10);
  ```

  springboot整合：springboot整合mybatis-plus

  - 设置组件：在StudentMapper.class类上加注解 @Repository

  - 实现接口：在Application.class类上加注解 @MapperScan("com.sunpeng.mybatis.mapper")

  - 使用：@Autowired就可以自动导入了，实现的问题 @MapperScan 已经解决

##### 5.2 mybatis-plus与mybatis的不同

1）StudentMapper 继承BaseMapper<Student>，使得基本接口已经实现，不用再手动实现

```java
import com.baomidou.mybatisplus.core.mapper.BaseMapper;
public interface StudentMapper extends BaseMapper<Student> {}
```

















