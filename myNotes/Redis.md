### springBoot整合Redis实例

1. 建立maven工程，加入依赖

   ```xml
   <parent>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-parent</artifactId>
       <version>2.1.5.RELEASE</version>
   </parent>
   
   <dependencies>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-web</artifactId>
       </dependency>
   
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-data-redis</artifactId>
       </dependency>
   
       <!--连接池，redis需要-->
       <dependency>
           <groupId>org.apache.commons</groupId>
           <artifactId>commons-pool2</artifactId>
       </dependency> 
   </dependencies>
   ```

2. 创建与数据库关联的类: Student

   注意：序列化：implements Serializable

   ```java
   package com.entity;
   
   import java.io.Serializable;
   import java.util.Date;
   
   //redis数据存储在内存，所以需要序列化，可以防止redis-server退出时，数据丢失
   public class Student implements Serializable {
       private Integer id;
       private String name;
       private Double sorce;
       private Date birthday;
   
       public Integer getId() {
           return id;
       }
   
       public void setId(Integer id) {
           this.id = id;
       }
   
       public String getName() {
           return name;
       }
   
       public void setName(String name) {
           this.name = name;
       }
   
       public Double getSorce() {
           return sorce;
       }
   
       public void setSorce(Double sorce) {
           this.sorce = sorce;
       }
   
       public Date getBirthday() {
           return birthday;
       }
   
       public void setBirthday(Date birthday) {
           this.birthday = birthday;
       }
   }
   ```

3. 访问接口: StudentHandle

   ```java
   package com.controller;
   
   import com.entity.Student;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.data.redis.core.RedisTemplate;
   import org.springframework.web.bind.annotation.*;
   
   @RestController
   public class StudentHandler {
       @Autowired
       private RedisTemplate redisTemplate;
   
       @PostMapping("/set")
       public void set(@RequestBody Student student){     //@RequestBody,把json转换为对象
           redisTemplate.opsForValue().set("student", student);
       }
   
       @GetMapping("/get/{key}")
       public Student get(@PathVariable("key") String key){
           return (Student)redisTemplate.opsForValue().get(key);
       }
   
       @DeleteMapping("/delete/{key}")
       public boolean delete(@PathVariable("key") String key) {
           redisTemplate.delete(key);
           return redisTemplate.hasKey(key);
       }
   }
   ```

4. application.xml

   ```yml
   spring:
     redis:
       database: 0
       host: 127.0.0.1
       port: 6379
   ```

5. 启动类

   ```java
   package com;
   
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   
   @SpringBootApplication
   public class Application {
       public static void main(String[] args) {
           SpringApplication.run(Application.class, args);
       }
   }
   ```

   

