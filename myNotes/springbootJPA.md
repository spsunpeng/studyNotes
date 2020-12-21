# SpringBoot

Spring Boot 是一个快速开发框架，可以迅速搭建出一套基于 Spring 框架体系的应用，是 Spring Cloud 的基础。

Spring Boot 开启了各种自动装配，从而简化代码的开发，不需要编写各种配置文件，只需要引入相关依赖就可以迅速搭建一个应用。

- Spring Boot 整合Spring MVC

1、不需要 web.xml

2、不需要 springmvc.xml

3、不需要 tomcat，Spring Boot 内嵌了 tomcat

4、不需要配置 JSON 解析，支持 REST 架构

5、个性化配置非常简单

- Spring Boot 整合 JPA

jpa：是orm框架的规范(解决持久层设计上的差异)，仅定义了一些接口

Hibermate：是对jpa规范的实现；

springData：不是jpa规范的实现，仅是抽象上的含义。

jdba：是访问数据库的规范(解决各个数据库使用的差异)与实现

### 实例

#### 1.添加依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>GroupID2</groupId>
    <artifactId>springbootJPA</artifactId>
    <version>1.0-SNAPSHOT</version>


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
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.21</version>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

</project>
```

#### 2.entity层

```java
package com.entity;

import javax.persistence.*;

@Entity                    //表示跟表映射
@Table(name = "student")  //同样，表名和类名相同的话可省略
public class Student {

    @Id                                                   //表示主键
    @GeneratedValue(strategy = GenerationType.IDENTITY)   //自动增加
    private Long id;
    @Column
    private String name;
    @Column
    private Double score;
    @Column
    private Integer age;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Double getScore() {
        return score;
    }

    public void setScore(Double score) {
        this.score = score;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "Student{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", score=" + score +
                ", age=" + age +
                '}';
    }
}

```

#### 3.dao层

```java
package com.Repository;


import com.entity.Student;
import org.springframework.data.domain.Example;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.JpaSpecificationExecutor;
import org.springframework.data.jpa.repository.Query;
import org.springframework.data.repository.query.Param;

import javax.persistence.criteria.CriteriaBuilder;
import java.util.List;

public interface StudentRepository extends JpaRepository<Student, Long>, JpaSpecificationExecutor<Student> {

//查2. 自定义函数接口： Or And In Like Between等
    //public Student getById(Long id);
    public List<Student> getById(Long id);
    public List<Student> getByAge(int age);
    public List<Student> getByName(String name);

    public Student getByNameAndScore(String name, Double score);
    public List<Student> getByNameOrScore(String name, Double score);
    public List<Student> findByNameLike(String name);

    public List<Student> findByScoreLessThan(Double max);
    public List<Student> findByAgeBetween(int min, int max);




//查3. jpal查询
    //占位从1开始：？1
    @Query("from Student  where  name = ?1 or age = ?2")
    List<Student> findByCondition2(String name, Integer age);

    @Query("from Student  where  name = :name or age = :age")
    List<Student> findByCondition3(@Param("name") String untitled1, @Param("age") Integer untitled2);
    //:name 和 @Param("name") 名字必须一样，和String untitled1可以不一样

    //传入对象Bean，那么jpql的写法为：固定写法，没有为什么
    @Query("from Student s where  s.name = :#{#student.name} or s.age = :#{#student.age}")
    List<Student> findByCondition4(@Param("student") Student student);



//查4. mysql查询
    @Query(value = "select * from student s where  s.name = ?1 or s.age = ?2", nativeQuery = true)
    List<Student> findByMysql(String name, Integer age);
}
/*
问1：
* 接口继承父类，那么有什么作用？
* 特点：首先，接口肯定是无法实现的，那么就只能由父类实现。
* 作用：自己封装第三方提供的接口
* 实现：没有试出来
* */
```

#### 4.handle层1(MVCHandle)，着重MVC

```java
package com.controller;

import com.Repository.StudentRepository;
import com.entity.Student;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/MVC")
public class MVC_DemoHandle {

    @Autowired
    private StudentRepository studentRepository;

    //@GetMapping
    @GetMapping("/findByNameOrScore1/{name}/{score}")
    public List<Student> findByNameOrScore1(
            @PathVariable("name") String name,
            @PathVariable("score") Double score){
        return studentRepository.getByNameOrScore(name, score);
    }
    //访问：http://localhost:8080/MVC/findByNameOrScore1/sp1/80


    //@RequestMapping
    @RequestMapping(value = "/findByNameOrScore2",method = RequestMethod.GET,params = {"name","score"})
    public List<Student> findByNameOrScore2(
            @RequestParam("name") String name,
            @RequestParam("score") Double score){
        return studentRepository.getByNameOrScore(name, score);
    }
    //访问：http://localhost:8080/MVC/findByNameAndScore2?name=sp1&score=80
}
```

#### 4.handle层2(StudentHandle)，着重jpa

```java
package com.controller;

import com.Repository.StudentRepository;
import com.entity.Student;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.domain.*;
import org.springframework.data.jpa.domain.Specification;
import org.springframework.web.bind.annotation.*;

import javax.persistence.criteria.CriteriaBuilder;
import javax.persistence.criteria.CriteriaQuery;
import javax.persistence.criteria.Predicate;
import javax.persistence.criteria.Root;
import java.util.ArrayList;
import java.util.List;


@RestController
@RequestMapping("/student")
public class StudentHandle {

    @Autowired
    private StudentRepository studentRepository;


//问2：@GetMapping、@PostMapping等标识的区分有什么用呢？按理标识@Mapping("网址映射")这样就以经能标识出来具体执行哪个方法了，再去区分增啥改查又有什么用
//增删改
    @PostMapping("/save")
    public Student save(@RequestBody Student s1){
        return studentRepository.save(s1);
    }

    @PutMapping("/put")
    public Student updata(@RequestBody Student s1){
        return studentRepository.save(s1);
    }

    @DeleteMapping("/delete/{id}")
    public String deleteById(@PathVariable("id") Long id){
        studentRepository.deleteById(id);
        return "delete success";
    }



//查1. 继承接口JpaRepository：主要方法findAll
    @GetMapping("/findAll")
    public List<Student> findAll(){
        return studentRepository.findAll();
    }
    //分页查询
    @GetMapping("/findByIdPage")
    public Page<Student> findByIdPage(){
        Pageable pageable = PageRequest.of(0,4);
        return studentRepository.findAll(pageable);
    }
    //排序
    @GetMapping("/findAllSort")
    public List<Student> findAllSort(){
        Sort sort = new Sort(Sort.Direction.ASC, "score");
        return studentRepository.findAll(sort);
    }
    //分页查询
    @GetMapping("/findByIdPageSort")
    public Page<Student> findByIdPageSort(){
        Sort sort = new Sort(Sort.Direction.DESC, "score");
        Pageable pageable = PageRequest.of(0,4, sort);
        return studentRepository.findAll(pageable);
    }
    //findAllById
    @GetMapping("/findAllById")
    private List<Student> findAllById(){
        List<Long> ids = new ArrayList<Long>();
        ids.add(4L);
        ids.add(5L);
        return studentRepository.findAllById(ids);
    }
    //求数目
    @GetMapping("/AllCount")
    public long AllCount(){
        return studentRepository.count();
    }



//查2. 自定义函数接口： Or And In Like Between等
    @GetMapping("/findbyId/{id}")
    public List<Student> findbyId(@PathVariable("id") Long id){
        return studentRepository.getById(id);
    }
    @GetMapping("/findByAge")
    public List<Student> findByAge(){
        return studentRepository.getByAge(76);
    }
    @GetMapping("/findByName")
    public List<Student> findByName(){
        return studentRepository.getByName("sp1");
    }
    //Or
    @GetMapping("/findByNameOrScore/{name}/{score}")
    public List<Student> findByNameOrScore(
            @PathVariable("name") String name,
            @PathVariable("score") Double score){
        return studentRepository.getByNameOrScore(name, score);
    }
    //And
    @GetMapping("/findByNameAndScore")
    public Student findByNameAndScore(){
        return studentRepository.getByNameAndScore("sp1", 50D);
    }
    //like
    @GetMapping("/findByNameLike")
    private List<Student> findByNameLike(){
        return studentRepository.findByNameLike("%un%");
    }
    //<
    @GetMapping("/findByScoreLessThan")
    public List<Student> findByScoreLessThan(){
        return studentRepository.findByScoreLessThan(80D);
    }
    //Between

    @GetMapping("/findByAgeBetween")
    public List<Student> findByAgeBetween(){
        return studentRepository.findByAgeBetween(20, 100);
    }


//查3. jpal查询
    @GetMapping("/findByCondition2")
    List<Student> findByCondition2(String name, Integer age){
        return  studentRepository.findByCondition2("sp1", 73);
    }
    @GetMapping("/findByCondition3")
    List<Student> findByCondition3(){
        return studentRepository.findByCondition3("sp1", 73);
    }
    @GetMapping("/findByCondition4")
    List<Student> findByCondition4(){
        Student student = new Student();
        student.setName("sp1");
        student.setAge(73);
        return studentRepository.findByCondition4(student);
    }



//查4. mysql查询
    @GetMapping("/findByMysql")
    public List<Student> findByMysql(String name, Integer age){    //必须要有public，否则就只是本包作用域，就无法和外卖呢的mysql对接
        return  studentRepository.findByMysql("sp2", 73);
    }



//5. Specifications动态查询   //问3：还有很多内容
    @GetMapping("/FindSpecification")
    public List<Student> FindSpecification(){

        final String name = null;
        final Integer age = 0;

        //root: 实体对象，可以通过它获取属性值
        //criteriaQuery：用于生成sql语句
        //criteriaBuilder：用于拼接查询条件
        List<Student> students = studentRepository.findAll(new Specification<Student>() {
            public Predicate toPredicate(Root<Student> root, CriteriaQuery<?> criteriaQuery, CriteriaBuilder criteriaBuilder) {
                List<Predicate> predicates = new ArrayList<Predicate>();
                if(name != null){
                    predicates.add(criteriaBuilder.equal(root.get("name"), name));
                }
                if(age != 0){
                    predicates.add(criteriaBuilder.equal(root.get("age"), age));
                }
                return criteriaBuilder.and(predicates.toArray(new Predicate[]{})); //and全部 , or为空
                //criteriaQuery用法：return criteriaQuery.where(predicates.toArray(new Predicate[predicates.size()])).getRestriction();
            }
        });
        return students;
    }

    @GetMapping("/FindSpecificationPage")
    public Page<Student> FindSpecificationPage(){

        final String name = null;
        final Integer age = 0;
        Pageable pageable = PageRequest.of(0, 4);

        Page<Student> students = studentRepository.findAll(new Specification<Student>() {

            public Predicate toPredicate(Root<Student> root, CriteriaQuery<?> criteriaQuery, CriteriaBuilder criteriaBuilder) {
                List<Predicate> predicates = new ArrayList<Predicate>();
                if(name != null){
                    predicates.add(criteriaBuilder.equal(root.get("name"), name));
                }
                if(age != 0){
                    predicates.add(criteriaBuilder.equal(root.get("age"), age));
                }
                return criteriaBuilder.and(predicates.toArray(new Predicate[]{})); //and全部 , or为空
            }
        }, pageable);

        return students;
    }



//6. Example动态查询
    public void findByExample1(){
        Student student = new Student();
        student.setId(1L);
        //studentRepository.findAll(Example.of(student));  //问4：Example.of报错
    }

}

/*
查询总结：
1.使用父接口
    1.1 findAllById 根据主键查
    1.2 findAll() 重载，可以排序，分页
2.方法命名规则
    例2.1：findByName("sunpeng") ，系统解析：
        1.去掉findBy，
        2.将剩余的 Name("sunpeng") 与 select * from student where 拼接
        3. 拼接的结果为select * from student where Name = sunpeng;
    例2.2：findByNameLike("%un%"),系统解析为select * from student where Name like un;
    注2.1：findByIdIn(id1,id2)、findByIdIn(List<id>)和findAllById(List<id>)的用途一样
    注2.2：OrderBy 与 findAll(sort)一样
3.jpql查询
    它是jpa定义的语言，用来统一不同sql语句之间的差异，使用起来和mysql一模一样，只需把表名换成类名，表的字段换成类的字段
    3.1 占位从1开始：？1
        如：@Query("from Student s where  s.name = ?1 or s.age = ?2")
    3.2 :name 和 @Param("name") 名字必须一样，和String untitled1可以不一样
        @Query("from Student  where  name = :name or age = :age")
        List<Student> findByCondition3(@Param("name") String untitled1, @Param("age") Integer untitled2);
    3.3 传入对象Bean，那么jpql的写法为：固定写法，没有为什么
        @Query("from Student s where  s.name = :#{#student.name} or s.age = :#{#student.age}")
        List<Student> findByCondition4(@Param("student") Student student);
4.本地sql查询
    @Query中写原生的sql语句，这样的话也就是针对单一的数据库了
5.Specifications动态查询
 */

```

#### 5.springBoot启动入口

```java
package com;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class,args);
    }
}
```

#### 6.application.yml

```yml
spring:
  datasource:
    url: jdbc:mysql://127.0.0.1:3306/mydatabase?useUnicode=true&characterEncoding=utf-8
    username: root
    password: 610527
    driver-class-name: com.mysql.jdbc.Driver
  jpa:
    show-sql: true
    properties:
      hibernate:
        format_sql: ture
```



#### 7.测试：目录 test->java->com

```java
package com;

import com.Repository.StudentRepository;
import com.entity.Student;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;

import java.util.List;

@RunWith(SpringRunner.class)
@SpringBootTest()//classes = Application.class，与Application包名不一致的时候需要添加
public class Test1 {

    @Autowired
    private StudentRepository studentRepository;

    @Test
    public void fun1(){
        List<Student> students = studentRepository.findAll();
        System.out.println(students);
    }

    @Test
    public void fun2(){
        System.out.println("hello");
    }
}
```

### 遇到的问题：

1. 原因就是mysql版本冲突：程序直接挂掉：Application run failed
   mysql Server是5.7，mysql依赖是8.0.15

   将mysql依赖降为5.1.17

2. application.yml中用户名关键字错误
   name: root -》username: root













