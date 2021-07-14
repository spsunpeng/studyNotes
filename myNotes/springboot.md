# 一、雷峰阳

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



# 二、连鹏举

互联网高级工程师就业班

### 1、rest风格

rest是一种设计模式，指从资源的角度去考虑问题，而非操作的角度，体现在url上就是有post、get等之分。



### 2、springboot项目打包执行

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

如果没有spring-boot-maven-plugin组件依赖，那么打包时就没有下面红框中的文件，在cmd中执行时（java -jar），会报错：没有主文件

![spring-booot-maven-plugin](pictures\springboot\spring-booot-maven-plugin.png)

### 3、springboot与spring

#### 3.1 yaml文件

springboot推荐使用yaml文件，spring不能识别yaml文件，只能用properties

#### 3.2 外部文件

spring在使用外部配置文件时，需要在ioc.xml文件中声明。

```xml
<context:property-placeholder location="classpath:userConfig.properties"></context:property-placeholder>
```

### 4、配置文件优先级

file:./config/   >   file:./   >   classpath:./config/   >  classfile:./

### 5、配置文件与环境

application.yml

application-dev.yml

application-sit.yml

```yaml
spring:
  profiles:
    active: sit
```

### 6、springboot整合servlet

#### 6.1 http

```java
@WebServlet(name = "myServlet",urlPatterns = "/servlet")
public class HelloServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("123");
        super.doGet(req, resp);
    }
}
```

```java
@ServletComponentScan
```

#### 6.2 过滤器

```java
@WebFilter(filterName = "MyFilter", urlPatterns = "/*")
public class MyFilter implements Filter {

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println("init");
    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("doFilter");
        filterChain.doFilter(servletRequest, servletResponse);
    }

    @Override
    public void destroy() {
        System.out.println("destroy");
    }
}
```

```java
    @Bean
    public ServletRegistrationBean<HelloServlet> getServletRegistrationBean(){
        ServletRegistrationBean<HelloServlet> bean = new ServletRegistrationBean<>(new HelloServlet());
        ArrayList<String> url = new ArrayList<>();
        url.add("/helloServlet");
        bean.setUrlMappings(url);
        bean.setLoadOnStartup(1);
        return bean;
    }
```

#### 6.3 监听器

```java
public class MyListen implements HttpSessionListener {

    public static int online=0;

    @Override
    public void sessionCreated(HttpSessionEvent se) {
        online++;
        System.out.println("创建+1");
    }

    @Override
    public void sessionDestroyed(HttpSessionEvent se) {
        System.out.println("销毁-1");
    }
}
```

```java
@Bean
public ServletListenerRegistrationBean listenerRegist(){
    ServletListenerRegistrationBean srb = new ServletListenerRegistrationBean();
    srb.setListener(new MyListen());
    System.out.println("listener");
    return srb;
}
```





# 三、马东阳

目前学习到：第12阶段-springboot的使用，第38节-拓展_@ImportResource

### springboot配置

#### 1、application

父依赖的引入，使得springboot会默认读取**/application*.yml、**/application*.yam、**/application*.properties

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.4.5</version>
</parent>
```

```xml
<build>
  <resources>
    <resource>
      <directory>${basedir}/src/main/resources</directory>
      <filtering>true</filtering>
      <includes>
        <include>**/application*.yml</include>
        <include>**/application*.yaml</include>
        <include>**/application*.properties</include>
      </includes>
    </resource>
    <resource>
      <directory>${basedir}/src/main/resources</directory>
      <excludes>
        <exclude>**/application*.yml</exclude>
        <exclude>**/application*.yaml</exclude>
        <exclude>**/application*.properties</exclude>
      </excludes>
    </resource>
  </resources>
</build>
```

properties 优先级高于 yaml，而非覆盖。

#### 2、启动器

```xml
<!--名字在后，spring提供的启动器，spring整合第三方-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<!--名字在前，第三方提供的启动器，第三方支持spring-->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.1.3</version>
</dependency>
```

#### 3、打包

springboot打包可以分为jar、war、pom，其中pom用在父依赖中

##### 3.1 jar

###### 3.1.1 依赖

```xml
<!--默认打jar包-->
<packaging>jar</packaging>
<!--打包插件-->
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <configuration>
                <fork>true</fork>
            </configuration>
        </plugin>
    </plugins>
</build>
```

###### 3.1.2 打包

- maven打包

![maven打包](pictures\springboot\maven打包.png)

- 打包结果

  ![springboot打包结果](pictures\springboot\springboot打包结果.png)

##### 3.2 war

由于springboot集成了tomcat，所以运行web程序时用jar包即可。如果非要打war包，按照如下步骤：

- 依赖

  - 排除web启动中自动依赖的tomcat插，再加入单独的tomcat依赖（仅在编译时生效）。

    ```xml
    <!--配置SpringBoot的web启动器-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <!--排除web启动中自动依赖的tomcat插件-->
        <exclusions>
            <exclusion>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-tomcat</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
    <!--
        手动依赖tomcat插件，但是表明项目打包时该依赖不会被打进去，目的主要是保证开发阶段本地SpringBoot
        项目可以正常运行
    -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-tomcat</artifactId>
        <!--打包的时候可以不用包进去，别的设施会提供。事实上该依赖理论上可以参与编译，测试，运行等周期。
            相当于compile，但是打包阶段做了exclude操作-->
        <scope>provided</scope>
    </dependency>
    ```

  - 打包插件

    ```xml
    <!--默认打jar包-->
    <packaging>war</packaging>
    <!--打包插件-->
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <fork>true</fork>
                </configuration>
            </plugin>
        </plugins>
    </build>
    ```

- 启动类

  正常springboot启动要初始化web的配置，现在没有了tomcat依赖，正常springboot无法启动成功。

  ```java
  @SpringBootApplication
  public class MyApplication extends SpringBootServletInitializer {
      //重写配置方法
      @Override
      protected SpringApplicationBuilder configure(SpringApplicationBuilder application) {
          return application.sources(MyApplication.class);
      }
      public static void main(String[] args) {
          //启动SpringBoot
          SpringApplication.run(MyApplication.class,args);
      }
  }
  ```

- 打包

  用maven-package打包，然后放到tomcat下运行。



#### 4、参数

##### 4.1 spring读取方式

```java
@Component
@PropertySource(value = "classpath:bookConfig.yaml")
public class Book {
    @Value("${springboot-base.book.name}")
    private String name;
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
}
```

```properties
springboot-base.book.name=shuixu
```

##### 4.2 springboot读取方式

```java
@Component
@PropertySource(value = "classpath:bookConfig.properties")
@ConfigurationProperties(prefix = "springboot-base.book")
public class Book {
    //@Value("${springboot-base.book.name}")
    private String name;
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
}
```

```yaml
springboot-base:
  book:
    name: dsddd
```

- springboot在spring的基础上，还提供了注解：@ConfigurationProperties
- springboot在spring的基础上，还支持yaml文件。
- spring没有默认指定文件，必须指定，springboot默认了指定文件application
- @PropertySource是sping提供，不支持yaml格式文件，特例是application.yaml，可能是springboot做了转换
- 对于springboot而言，配置文件有默认application.yaml的和指定的bookConfig.properties。如果两者做了，相同的配置。@Value会使用application.yaml，@ConfigurationProperties会使用bookConfig.properties。

- @ConfigurationProperties不支持大写字母，支持连字符-



### 整合web

#### 1、前端

##### 1.1 jsp

优点：
1、功能强大，可以写java代码
2、支持jsp标签（jsp tag）
3、支持表达式语言（el）
4、官方标准，用户群广，丰富的第三方jsp标签库
缺点：
性能问题。不支持前后端分离

##### 1.2 freemarker

FreeMarker是一个用Java语言编写的模板引擎，它基于模板来生成文本输出。FreeMarker与Web容器无关，即在Web运行时，它并不知道Servlet或HTTP。它不仅可以用作表现层的实现技术，而且还可以用于生成XML，JSP或Java 等。
目前企业中:主要用Freemarker做静态页面或是页面展示

优点：
1、不能编写java代码，可以实现严格的mvc分离
2、性能非常不错
3、对jsp标签支持良好
4、内置大量常用功能，使用非常方便
5、宏定义（类似jsp标签）非常方便
6、使用表达式语言
缺点：
1、不是官方标准
2、用户群体和第三方标签库没有jsp多

##### 1.3 Thymeleaf

Thymeleaf是个XML/XHTML/HTML5模板引擎，可以用于Web与非Web应用。
Thymeleaf的主要目标在于提供一种可被浏览器正确显示的、格式良好的模板创建方式，因此也可以用作静态建模。你可以使用它创建经过验证的XML与HTML模板。相对于编写逻辑或代码，开发者只需将标签属性添加到模板中即可。接下来，这些标签属性就会在DOM（文档对象模型）上执行预先制定好的逻辑。Thymeleaf的可扩展性也非常棒。你可以使用它定义自己的模板属性集合，这样就可以计算自定义表达式并使用自定义逻辑。这意味着Thymeleaf还可以作为模板引擎框架。
优点：静态html嵌入标签属性，浏览器可以直接打开模板文件，便于前后端联调。springboot官方推荐方案。
缺点：模板必须符合xml规范

VUE: 前后端分离,最多,未来趋势

#### 2、拦截器

##### 2.1 拦截器

和spring一样

```java
@Component
public class WebInterceptor implements HandlerInterceptor {

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("拦截器");
        return true;
    }
}
```

##### 2.2 拦截器配置

将spring的xml配置改为springboot的配置类

```java
@Configuration
public class InterceptorConfig implements WebMvcConfigurer {

    @Autowired
    private WebInterceptor webInterceptor;

    @Override
    public void addInterceptors(InterceptorRegistry registry) {       registry.addInterceptor(webInterceptor).addPathPatterns("/**").excludePathPatterns("/hi");
    }
}
```



### 整合mybatis

ctrl + alt + shift + h : 调整idea编译检查的级别

#### 1、接口的注入

##### 1.1 dao层的接口需要注入

一般的接口是我们自己实现，接口都有自己的实现类，注入时会注入实现类。但是dao层的类我我们定义接口，mybatis/jpa自动实现，那么在编译如何让它通过。

dao层接口：

```java
public interface AccountMapper {
    List<Account> queryAccount();
}
```

###### 1.1.1 mybatis

```java
//在启动类上添加注释，使编译时通过
@MapperScan("com.msb.springboot_mybatis.mapper")     //mybatis实现
@ComponentScan("com.msb.springboot_mybatis.mapper")  //spring实现
//或者在接口类上添加注释，使编译时通过
@Mapper  //mybatis实现
```

###### 1.1.2 jpa

```java
//继承JpaRepository
public interface AccountMapper extends JpaRepository<Account, Long>{
    List<Account> queryAccount();
}
```

##### 1.2 接口多种实现时的注入

当接口有多个实现时

```java
public interface Person {
}
@Compoment
public class Man implements Person {
}
@Compoment
public class Woman implements Person {
}
```

###### 1.2.1 单独注入一个

```java
public class PersonService{
    @Autowired
	private List<Person> personList;
}
```

###### 1.2.2 注入多个

```java
//注入一个
public class PersonService{
    @Autowired
	private Person person
}
//只让一个成为组件
public interface Person {
}
@Compoment
@ConditionalOnExpression("true")
public class Man implements Person {
}
@Compoment
@ConditionalOnExpression("false")
public class Woman implements Person {
}
```

#### 2、配置

##### 2.1 application.yaml

```yaml
spring:
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    password: 610527
    username: root
    url: jdbc:mysql://127.0.0.1:3306/mydatabase?useUnicode=true&characterEncoding=utf-8
mybatis:
  mapper-locations: classpath:mybatis/*.xml
  type-aliases-package: com.msb.entity
```

##### 2.2 Mapper.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.msb.springboot_mybatis.mapper.AccountMapper">
    <select id="queryAccount" resultType="com.msb.springboot_mybatis.entity.Account">select * from account</select>
</mapper>
```

#### 3、分页（整合PageHelper）

##### 3.1 依赖

```xml
<dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper-spring-boot-starter</artifactId>
    <version>1.2.12</version>
</dependency>
```

##### 3.2 使用

```java
//提前调用分页方法
Page<Account> pages = PageHelper.startPage(pageNum, pageSize);
//然后再查询
List<Account> accounts = accountMapper.queryAccount();
//将结果存入PageInfo中，因为Page中数据不稳定
PageInfo<Account> pi =new PageInfo<>(accounts);
//结果
System.out.println("当前页"+pi.getPageNum());
System.out.println("总页数"+pi.getPages());
System.out.println("页大小"+pi.getSize());
System.out.println("总记录数"+pi.getTotal());
System.out.println("当前页数据"+pi.getList());
```

#### 4、连接池（整合druid）

##### 4.1 依赖

```xml
<!--数据库连接池-->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid-spring-boot-starter</artifactId>
    <version>1.1.10</version>
</dependency>
```

##### 4.2 配置

```yaml
spring:
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    password: 610527
    username: root
    url: jdbc:mysql://127.0.0.1:3306/mydatabase?useSSL=false&useUnicode=true&characterEncoding=utf-8&serverTimezone=Asia/Shanghai
    type: com.alibaba.druid.pool.DruidDataSource
  druid:
    # 连接池的配置信息
    # 初始化大小，最小，最大
    initial-size: 5
    min-idle: 5
    maxActive: 20
    # 配置获取连接等待超时的时间
    maxWait: 60000
    # 配置间隔多久才进行一次检测，检测需要关闭的空闲连接，单位是毫秒
    timeBetweenEvictionRunsMillis: 60000
    # 配置一个连接在池中最小生存的时间，单位是毫秒
    minEvictableIdleTimeMillis: 300000
    validationQuery: SELECT 1
    testWhileIdle: true
    testOnBorrow: false
    testOnReturn: false
    # 打开PSCache，并且指定每个连接上PSCache的大小
    poolPreparedStatements: true
    maxPoolPreparedStatementPerConnectionSize: 20
    # 配置监控统计拦截的filters，去掉后监控界面sql无法统计，'wall'用于防火墙
    filters: stat,wall,slf4j
    # 通过connectProperties属性来打开mergeSql功能；慢SQL记录
    connectionProperties: druid.stat.mergeSql\=true;druid.stat.slowSqlMillis\=5000
    # 配置DruidStatFilter
    web-stat-filter:
    enabled: true
    url-pattern: "/*"
    exclusions: "*.js,*.gif,*.jpg,*.bmp,*.png,*.css,*.ico,/druid/*"
    # 配置DruidStatViewServlet
    stat-view-servlet:
    url-pattern: "/druid/*"
    # IP白名单(没有配置或者为空，则允许所有访问)
    allow: 127.0.0.1,192.168.8.109
    # IP黑名单 (存在共同时，deny优先于allow)
    deny: 192.168.1.188
    #  禁用HTML页面上的“Reset All”功能
    reset-enable: false
    # 登录名
    login-username: admin
    # 登录密码
    login-password: 123456
```



### 整合logback

#### 模板

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<configuration>
    <!-- 控制台输出 -->
    <appender name="Stdout" class="ch.qos.logback.core.ConsoleAppender">
        <!-- 日志输出格式 -->
        <layout class="ch.qos.logback.classic.PatternLayout">
            <!--格式化输出：%d表示日期，%thread表示线程名，%-5level：级别从左显示5个字符宽度%msg：日志消息，%n是换行符-->
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n
            </pattern>
        </layout>
    </appender>

    <!-- 按照每天生成日志文件 -->
    <property name="LOG_HOME" value="${catalina.base}/logs/" /> <!--定义日志文件的存储地址 勿在 LogBack 的配置中使用相对路径-->
    <appender name="RollingFile"  class="ch.qos.logback.core.rolling.RollingFileAppender">
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!--日志文件输出的文件名-->
            <FileNamePattern>${LOG_HOME}/server.%d{yyyy-MM-dd}.log</FileNamePattern>
            <MaxHistory>30</MaxHistory>
        </rollingPolicy>
        <layout class="ch.qos.logback.classic.PatternLayout">
            <!--格式化输出：%d表示日期，%thread表示线程名，%-5level：级别从左显示5个字符宽度%msg：日志消息，%n是换行符-->
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n
            </pattern>
        </layout>
        <!--日志文件最大的大小-->
        <triggeringPolicy class="ch.qos.logback.core.rolling.SizeBasedTriggeringPolicy">
            <MaxFileSize>10MB</MaxFileSize>
        </triggeringPolicy>
    </appender>

    <!--日志异步到数据库 -->
    <appender name="DB" class="ch.qos.logback.classic.db.DBAppender">
        <!--日志异步到数据库-->
<!--        <connectionSource class="ch.qos.logback.core.db.DriverManagerConnectionSource">
           &lt;!&ndash;连接池&ndash;&gt;
           <dataSource class="com.mchange.v2.c3p0.ComboPooledDataSource">
              <driverClass>com.mysql.jdbc.Driver</driverClass>
              <url>jdbc:mysql://127.0.0.1:3306/mydatabase</url>
              <user>root</user>
              <password>610527</password>
            </dataSource>
        </connectionSource>
      </appender>-->

    <!-- 日志输出级别 -->
    <root level="info">
        <appender-ref ref="Stdout" />
        <appender-ref ref="RollingFile" />
    </root>
    <!--com.msb.mapper包下日记级别调整为DEBUG，可以打印sql语句-->
    <logger name="com.msb.mapper" level="DEBUG"></logger>
</configuration>
```

#### 1、整合

![springboot整合logback](pictures\springboot\springboot整合logback.png)

springboot默认使用logback

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
private static final Logger LOGGER = LoggerFactory.getLogger(Class clazz);
//接口（协议）都是slf4j，springboot默认实现是logback
```

#### 2、日志级别

xml格式

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<configuration>
	<!-- 日志输出级别 -->
    <root level="info">
        <appender-ref ref="Stdout" />
        <appender-ref ref="RollingFile" />
    </root>
    <!--com.msb.mapper包下日记级别调整为DEBUG，可以打印sql语句-->
    <logger name="com.msb.mapper" level="DEBUG"></logger>
<configuration>
```

properties格式

```properties
logging.level.root=info
logging.level.com.msb.mapper=DEBUG
```

#### 3、输出格式

##### 3.1 打印方式

日志输出不仅有控制台打印，还可以写入文件，存入数据库

```xml
<root level="info">
    <appender-ref ref="Stdout" />       <!--控制台打印-->
    <appender-ref ref="RollingFile" />  <!--写入文件-->
    <appender-ref ref="DB" />           <!--存库-->
</root>
```
##### 3.2 日志格式

```properties
log4j.rootLogger=error,logfile
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.err
log4j.appender.stdout.layout=org.apache.log4j.SimpleLayout
log4j.appender.logfile=org.apache.log4j.FileAppender
log4j.appender.logfile.File=d:/msb.log
log4j.appender.logfile.layout=org.apache.log4j.PatternLayout
log4j.appender.logfile.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss} %l %F %p %m%n
```

%p：输出日志信息的优先级，即DEBUG，INFO，WARN，ERROR，FATAL。

%d：输出日志时间点的日期或时间，默认格式为ISO8601，也可以在其后指定格式，如：%d{yyyy/MM/dd HH:mm:ss,SSS}。

%r：输出自应用程序启动到输出该log信息耗费的毫秒数。

%t：输出产生该日志事件的线程名。

%l：输出日志事件的发生位置，相当于%c.%M(%F:%L)的组合，包括类全名、方法、文件名以及在代码中的行数。例如

test.TestLog4j.main(TestLog4j.java:10)。
		%c：输出日志信息所属的类目，通常就是所在类的全名。
       %M：输出产生日志信息的方法名。
       %F：输出日志消息产生时所在的文件名称。
       %L:：输出代码中的行号。
       %m:：输出代码中指定的具体日志信息。
       %n：输出一个回车换行符，Windows平台为"rn"，Unix平台为"n"。
       %x：输出和当前线程相关联的NDC(嵌套诊断环境)，尤其用到像java servlets这样的多客户多线程的应用中。
       %%：输出一个"%"字符。



### 自动部署

#### 1、导入依赖


```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-devtools</artifactId>
        <version>2.4.5</version>
        <optional>true</optional>
    </dependency>
```
#### 2、修改idea自动编译

<img src="pictures\springboot\idea自动编译.png" alt="idea自动编译" style="zoom:75%;" />

#### 3、修改Reigstry

Ctrl+Shift+Alt+/，然后点击弹出框中Registry

![idea注册](pictures\springboot\idea注册.png)

#### 4、使用

- 前端页面修改很方便，修改的部分直接编译到字节文件上。
- 后端代码修改不是很快捷，服务还是要重启，只不过系统帮我们重启而已。



## 整合junit

依赖使用spring-boot-starter-test启动器，启动时只需要使用springboot提供的注解@SpringBootTest。

1、依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
```

2、使用

```java
package com.msb;
import com.msb.pojo.Emp;
import com.msb.service.EmpService;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import java.util.List;

@SpringBootTest(classes = Springboot03Application.class)
class Springboot03AppliactionTests {
    @Autowired
    private EmpService empService;
    @Test
    public void testFindAll() {
        List<Emp> list = empService.findAll();
        list.forEach(System.out::println);
    }
}
```



## 注解

### 1、@Configuration

@Configuration相比@Component仅仅多了参数proxyBeanMethods，作用是类中的方法是否使用代理。

@Configuration从功能上来说，就是表明这个类是配置类。实现就是借助@Component实现。

proxyBeanMethods=false 称之为Lite模式  特点启动快

proxyBeanMethods=true  称之为Full模式  特点依赖spring容器控制bean单例

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component
public @interface Configuration {
    @AliasFor(
        annotation = Component.class
    )
    String value() default "";

    boolean proxyBeanMethods() default true;
}
```

#### 2、构建bean

```java
@Configuration(proxyBeanMethods=false)
//@Component
//@Import(value = Person.class) //id="com.msb.config.Person" , type=Person.class 可以放到任意位置
//@EnableConfigurationProperties(value = Person.class) //person-com.msb.config.Person
//@ImportResource("classpath:beans.xml")
public class MyConfig {
    @Bean
    public Person getPerson(){
        return new Person();
    }
}
```

方法一：@Configuration

方法二：@Component

方法三：@Bean + @Configuration/@Component

方法四：@Import(value = Person.class)

方法五：@ImportResource("classpath:beans.xml") + beans.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="person" class="com.msb.config.Person">
        <property name="name" value="jia"></property>
    </bean>
</beans>
```

方法六：@EnableConfigurationProperties(value = Person.class) + @ConfigurationProperties(prefix = "person") + application.yaml

```java
@ConfigurationProperties(prefix = "person")
public class Person {
    private String name;
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
}
```

```yaml
person:
  name: sunpeng
```

#### 3、@Conditional 条件装配

满足Conditional指定的条件,则进行组件注入

```java
@Configuration(proxyBeanMethods=false)
@ConditionalOnClass(value = Person.class)
public class MyConfig {
    @Bean
    @ConditionalOnProperty(name = "person.name", havingValue = "sunpeng")
    public Person getPerson(){
        return new Person();
    }
}
```

子类：ctrl + h

![Conditional](pictures\springboot\Conditional.png)



#### 4、其他注解

@PropertySource("classpath:userConfig.properties")

@Value

@ConfigurationProperties(prefix = "person")

@Autowird

@Qualifier(value = "")



















