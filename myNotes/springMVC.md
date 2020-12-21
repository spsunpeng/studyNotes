### Spring MVC

Spring MVC 是目前主流的实现 MVC 设计模式的企业级开发框架，Spring 框架的一个子模块，无需整合，开发起来更加便捷。

#### 什么是 MVC 设计模式？

将应用程序分为 Controller、Model、View 三层，Controller 接收客户端请求，调用 Model 生成业务数据，传递给 View。

Spring MVC 就是对这套流程的封装，屏蔽了很多底层代码，开放出接口，让开发者可以更加轻松、便捷地完成基于 MVC 模式的 Web 开发。

#### Spring MVC 的核心组件

- DispatcherServlet：前置控制器，是整个流程控制的核心，控制其他组件的执行，进行统一调度，降低组件之间的耦合性，相当于总指挥。
- Handler：处理器，完成具体的业务逻辑，相当于 Servlet 或 Action。
- HandlerMapping：DispatcherServlet 接收到请求之后，通过 HandlerMapping 将不同的请求映射到不同的 Handler。
- HandlerInterceptor：处理器拦截器，是一个接口，如果需要完成一些拦截处理，可以实现该接口。
- HandlerExecutionChain：处理器执行链，包括两部分内容：Handler 和 HandlerInterceptor（系统会有一个默认的 HandlerInterceptor，如果需要额外设置拦截，可以添加拦截器）。
- HandlerAdapter：处理器适配器，Handler 执行业务方法之前，需要进行一系列的操作，包括表单数据的验证、数据类型的转换、将表单数据封装到 JavaBean 等，这些操作都是由 HandlerApater 来完成，开发者只需将注意力集中业务逻辑的处理上，DispatcherServlet 通过 HandlerAdapter 执行不同的 Handler。
- ModelAndView：装载了模型数据和视图信息，作为 Handler 的处理结果，返回给 DispatcherServlet。
- ViewResolver：视图解析器，DispatcheServlet 通过它将逻辑视图解析为物理视图，最终将渲染结果响应给客户端。

#### Spring MVC 的工作流程

- 客户端请求被 DisptacherServlet 接收。
- 根据 HandlerMapping 映射到 Handler。
- 生成 Handler 和 HandlerInterceptor。
- Handler 和 HandlerInterceptor 以 HandlerExecutionChain 的形式一并返回给 DisptacherServlet。
- DispatcherServlet 通过 HandlerAdapter 调用 Handler 的方法完成业务逻辑处理。
- Handler 返回一个 ModelAndView 给 DispatcherServlet。
- DispatcherServlet 将获取的 ModelAndView 对象传给 ViewResolver 视图解析器，将逻辑视图解析为物理视图 View。
- ViewResovler 返回一个 View 给 DispatcherServlet。
- DispatcherServlet 根据 View 进行视图渲染（将模型数据 Model 填充到视图 View 中）。
- DispatcherServlet 将渲染后的结果响应给客户端。

![image-20200813091802946](C:\Users\sunpeng.SINO\AppData\Roaming\Typora\typora-user-images\image-20200813091802946.png)

Spring MVC 流程非常复杂，实际开发中很简单，因为大部分的组件不需要开发者创建、管理，只需要通过配置文件的方式完成配置即可，真正需要开发者进行处理的只有 Handler 、View。

1. 创建maven工程，porm.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   
   <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
     <modelVersion>4.0.0</modelVersion>
   
     <groupId>com.southwind</groupId>
     <artifactId>aispringMVC</artifactId>
     <version>1.0-SNAPSHOT</version>
     <packaging>war</packaging>
   
     <name>aispringMVC Maven Webapp</name>
     <!-- FIXME change it to the project's website -->
     <url>http://www.example.com</url>
   
     <properties>
       <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
       <maven.compiler.source>1.7</maven.compiler.source>
       <maven.compiler.target>1.7</maven.compiler.target>
     </properties>
   
     <dependencies>
       <dependency>
         <groupId>junit</groupId>
         <artifactId>junit</artifactId>
         <version>4.11</version>
         <scope>test</scope>
       </dependency>
   
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-webmvc</artifactId>
         <version>5.0.2.RELEASE</version>
       </dependency>
     </dependencies>
   
     <build>
       <finalName>aispringMVC</finalName>
       <pluginManagement><!-- lock down plugins versions to avoid using Maven defaults (may be moved to parent pom) -->
         <plugins>
           <plugin>
             <artifactId>maven-clean-plugin</artifactId>
             <version>3.1.0</version>
           </plugin>
           <!-- see http://maven.apache.org/ref/current/maven-core/default-bindings.html#Plugin_bindings_for_war_packaging -->
           <plugin>
             <artifactId>maven-resources-plugin</artifactId>
             <version>3.0.2</version>
           </plugin>
           <plugin>
             <artifactId>maven-compiler-plugin</artifactId>
             <version>3.8.0</version>
           </plugin>
           <plugin>
             <artifactId>maven-surefire-plugin</artifactId>
             <version>2.22.1</version>
           </plugin>
           <plugin>
             <artifactId>maven-war-plugin</artifactId>
             <version>3.2.2</version>
           </plugin>
           <plugin>
             <artifactId>maven-install-plugin</artifactId>
             <version>2.5.2</version>
           </plugin>
           <plugin>
             <artifactId>maven-deploy-plugin</artifactId>
             <version>2.8.2</version>
           </plugin>
         </plugins>
       </pluginManagement>
     </build>
   </project>
   ```

2. 在webapp->WEB-INF->web.xml中配置DispatcherServlet

   ```xml
   <!DOCTYPE web-app PUBLIC
    "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
    "http://java.sun.com/dtd/web-app_2_3.dtd" >
   
   <web-app>
     <display-name>Archetype Created Web Application</display-name>
   
     <servlet>
       <servlet-name>dispatcherServlet</servlet-name>
       <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
       <init-param>
         <param-name>contextConfigLocation</param-name>
         <param-value>classpath:springmvc.xml</param-value>  <!--读springmvc.xml-->
       </init-param>
     </servlet>
   
     <servlet-mapping>
       <servlet-name>dispatcherServlet</servlet-name>
       <url-pattern>/</url-pattern> <!--拦截所有请求-->
     </servlet-mapping>
   
   </web-app>
   ```

3. 配置springmvc.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:aop="http://www.springframework.org/schema/aop"
          xsi:schemaLocation="http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.3.xsd
          http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.3.xsd
         http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.3.xsd
         ">
   
       <!--自动扫描，自动生成@Controller等标志的对象，-->
       <context:component-scan base-package="com.southwind"></context:component-scan>
   
       <!--视图解析器，解析@RequestMapping标志的方法-->
       <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
           <property name="prefix" value="/"></property>
           <property name="suffix" value=".jsp"></property>
       </bean>
   
   </beans>
   ```

4. 创建 Handler

   ```java
   package com.southwind;
   
   import org.springframework.stereotype.Controller;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RequestMethod;
   import org.springframework.web.bind.annotation.RequestParam;
   
   @Controller       //1.交给IOC 2.控制器的作用
   public class HelloHandle {
   
       @RequestMapping(value = "/index", method = RequestMethod.GET, params = {"name", "id=10"}) //value = "/index"映射网址
       public String index(@RequestParam("name") String myname, int id){ //参数通过params和@RequestParam对应，没有时通过变量名对应
           System.out.println("name: "+myname);
           System.out.println("执行了index...");
           return "index";       //逻辑视图 "index"结合springmvc.xml的视图解析器，映射到物理视图index.jsp文件
       }
   }
   ```

5. *.jsp文件，webapp功臣会自动创建一个物理视图index.jsp

   ```jsp
   <html>
   <body>
   <h2>Hello World 333!</h2>
   </body>
   </html>
   ```

6. 测试：访问：http:127.0.0.1:8080/index?name=sunpeng&id=10



Spring MVC 注解

- @Controller

@Controller 在类定义处添加，将该类交个 IoC 容器来管理（结合 springmvc.xml 的自动扫描配置使用），同时使其成为一个控制器，可以接收客户端请求。

- @RequestMapping 

Spring MVC 通过 @RequestMapping 注解将 URL 请求与业务方法进行映射，在 Handler 的类定义处以及方法定义处都可以添加 @RequestMapping ，在类定义处添加，相当于客户端多了一层访问路径。

@RequestMapping (value="网址映射"，method=访问方法，params={"参数1","参数2"})

其中，method访问方法是枚举类型RequestMethod，params要用方法的参数接收，举例：

 @RequestMapping(value = "/index", method = RequestMethod.GET, params = {"name", "id=10"})

public String index(@RequestParam("name") String myname, int id){}



