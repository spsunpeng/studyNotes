### 1.问题

问1：springboot 简化 spring

问2：springboot 简化 打包部署

### 2.打包

第一步：添加依赖

```xml
<!-- 这个插件，可以将应用打包成一个可执行的jar包；-->
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

第二步：Maven -》 项目名 -》 Lifecycle -》 package【双击】

第三步：测试

​		    运行jar：java -jar name.jar

比较：运行java：javac name.java

​				   java name

### 3.场景启动器

#### 3.1 prom：parent

父项目一般用于依赖的版本管理

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>1.5.9.RELEASE</version>
</parent>

他的父项目是
<parent>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-dependencies</artifactId>
  <version>1.5.9.RELEASE</version>
  <relativePath>../../spring-boot-dependencies</relativePath>
</parent>
他来真正管理Spring Boot应用里面的所有依赖版本；
```

1. spring-boot-starter-parent，表示spring-boot-starter的所以要父项目
2. 父项目一般做依赖管理
3. spring-boot-starter-parent 依赖 spring-boot-dependencies
4. spring-boot-dependencies才是真正所有依赖的版本管理，所以它又叫版本仲裁
5. 所以后面导入的依赖不需要写版本；（没有在dependencies里面管理的依赖自然需要声明版本号）

#### 3.2 启动器（依赖）

比如：导入spring-boot-starter-web，它就不用写版本号

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

1. spring-boot-starter：场景启动器，将所有工程场景都抽取出来，做成一个个场景启动器
2. spring-boot-starter-web：web场景启动器，导入所有web需要的依赖，对比以前，简化各种各样的依赖导入
3. spring-boot-dependencies: 版本仲裁，web需要多个依赖，它解决了版本的兼容问题(记得我遇到过springdata、mybatis、mysql间不兼容问题)



### 4.主程序类/主入口类

```java
/**
 *  @SpringBootApplication 来标注一个主程序类，说明这是一个Spring Boot应用
 */
@SpringBootApplication
public class HelloWorldMainApplication {

    public static void main(String[] args) {

        // Spring应用启动起来
        SpringApplication.run(HelloWorldMainApplication.class,args);
    }
}
```

@**SpringBootApplication**:    Spring Boot应用标注在某个类上说明这个类是SpringBoot的主配置类，SpringBoot就应该运行这个类的main方法来启动SpringBoot应用；

**SpringBootApplication**新加入，没有就会报错，那么之前的main有什么缺陷吗？

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(
    excludeFilters = {@Filter(
    type = FilterType.CUSTOM,
    classes = {TypeExcludeFilter.class}
), @Filter(
    type = FilterType.CUSTOM,
    classes = {AutoConfigurationExcludeFilter.class}
)}
```

@SpringBootConfiguration：springboot的配置类

​		@Configuration：配置类标注这个注解，简化以前的配置文件，配置类也是容器的一个组件：@Component

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Configuration
public @interface SpringBootConfiguration {
}
```

@EnableAutoConfiguration：开启在自动配置

​	@AutoConfigurationPackage：自动配置包

```
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@AutoConfigurationPackage
@Import({EnableAutoConfigurationImportSelector.class})
```

@Import({Registrar.class})

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@Import({Registrar.class})
public @interface AutoConfigurationPackage {
}
```

**springboot 与 spring注解版比较学习**





**2020-10-13 14:10:45.601  INFO 41976 --- [           main] s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/hello]}" onto public java.lang.String com.atguigu.controller.HelloController.hello(**)
2020-10-13 14:10:45.604  INFO 41976 --- [           main] s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/error]}" onto public org.springframework.http.ResponseEntity<java.util.Map<java.lang.String, java.lang.Object>> org.springframework.boot.autoconfigure.web.BasicErrorController.error(javax.servlet.http.HttpServletRequest)
2020-10-13 14:10:45.604  INFO 41976 --- [           main] s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/error],produces=[text/html]}" onto public org.springframework.web.servlet.ModelAndView org.springframework.boot.autoconfigure.web.BasicErrorController.errorHtml(javax.servlet.http.HttpServletRequest,javax.servlet.http.HttpServletResponse)



# 2020.10.14

#### 1.springboot：创建

1. idea: 创建maven工程

2. prom: 导入依赖

3. 创建启动类:

   ```java
   @SpringBootApplication
   public class HelloWorldMainApplication {
       public static void main(String[] args) {
           SpringApplication.run(HelloWorldMainApplication.class, args);
       }
   }
   ```

4. 开发

#### 2.springboot：快速创建

- 新建 -》 project -》Spring Initializr

- New Project 下选择：

![](\图片\快速创建springboot.png)

​	type：Maven Project

​	Packaging：jar（自动打包的打包方式）

​	java Version：8

​	Package：项目路径（系统会自动创建Application，放在此目录下）

#### 3、IDEA：使用 Spring Initializer快速创建项目

IDE都支持使用Spring的项目创建向导快速创建一个Spring Boot项目；

选择我们需要的模块；向导会联网创建Spring Boot项目；

默认生成的Spring Boot项目；

- 主程序已经生成好了，我们只需要我们自己的逻辑
- resources文件夹中目录结构
  - static：保存所有的静态资源； js css  images；以前springWeb工程静态资源是保存在Web中，而springboot没有web文件夹
  - templates：保存所有的模板页面；（Spring Boot默认jar包使用嵌入式的Tomcat，默认不支持JSP页面）；可以使用模板引擎（freemarker、thymeleaf）；
  - application.properties：Spring Boot应用的配置文件；可以修改一些默认设置；



4.语法

- 空格缩进
- 键值对间 ": "



#### 5.springboot：注解

- 自动注入免去了bean的配置，但是设置值还是需要配置

  @ConfigurationProperties(prefix = "person") ： 表示在全局配置文件（application.xml 或 application.properties）中属于person的

  举例：

  ```java
  @PropertySource(value = {"classpath:person.properties"})
  @Component
  @ConfigurationProperties(prefix = "person")
  public class Person {
  
      private String lastName;
      private Integer age;
      private Boolean boss;
      private Date birth;
  
      private Map<String, Object> maps;
      private List<Object> lists;
      private Dog dog;
  }
      
  ```

```yml

server:
  port: 8081

person:
  lastName: hello
  age: 18
  boss: false
  birth: 2017/12/12
  maps: {k1: v1,k2: 12}
  lists:
    - lisi
    - zhaoliu
  dog:
    name: 小狗
    age: 12
```

```properties
server.port=8081

person.last-name=张三
person.age=15
person.birth=2017/12/13
person.boss=false
person.maps.k1=v1
person.maps.k2=12
person.lists=a,b,c
person.dog.name=dog
person.dog.age=12
```

- @ConfigurationProperties(prefix = "person") 仅识别全局的，配置内容必须放到application中，导致application臃肿，所以：

  @PropertySource(value = {"classpath:person.properties"}) 可以改变访问路径

- 但springboot仍然提供了spring配置文件支持

```xml
<bean id="helloWorldService" class="com.atguigu.springboot.service.HelloWorldService"></bean>
```

​	但仅这样，springboot不会识别的，还需要加入注解：@ImportResource(locations = {"classpath:beans.xml"})

```java
//@ImportResource(locations = {"classpath:beans.xml"})
@SpringBootApplication
public class SpringBoot01HelloworldQuickApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringBoot01HelloworldQuickApplication.class, args);
    }

}
```

- 但一般不建议这样使用：而是使用 @Configuration 和 @Bean

  ```java
  @Configuration
  public class MyAppConfig {
  
      //将方法的返回值添加到容器中；容器中这个组件默认的id就是方法名
      @Bean
      public HelloWorldService helloWorldService02(){
          return new HelloWorldService();
      }
  }
  ```



# 2020.10.19

#### 1.jar执行

- 打包：package
- 执行：在idea的Terminal执行

E:\IDEAProject\spring-boot-02\target>java -jar spring-boot-02-0.0.1-SNAPSHOT.jar --spring.config.location=C:\Users\sunpeng.SINO\Desktop\application.properties

#### 2.加载顺序

- 本地：

/config/application.properties

/application.properties

/classfile/config/application.properties

/classfile/application.properties

- 运维：

/命令行（参数）：如： --server.port=8087

/命令行（加载外部文件）：如：--spring.config.location=C:\Users\sunpeng.SINO\Desktop\application.properties

/classfile/config

/classfile/

没有/config/application.properties和/application.properties，因为打jar包只会打包：src、External Libraries

![](\图片\打jar包.png)







