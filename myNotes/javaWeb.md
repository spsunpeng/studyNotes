# 2020.09.17

#### 1.HTML 和 CSS

CSS：页面布局

HTML：页面内容

#### 2.结构

javaSE: C/S结构，Client / Server

Web:  B/S结构，Broswer/Server

#### 3.前端开发流程

设计页面(UI)Ps -> 静态页面HTML -> 动态页面jsp

#### 4.网页组成

内容：HTML 

布局：CSS

行为(交互)：javaScript

#### 5.HTML介绍

HTML：文本标记语言

HTML就是文本和标记符，标记符就是 <标记>。标记可以告诉浏览器如何显示文本（如：文字如何处理，图片如何排版）。

#### 6.创建HTML页面

idea -> new -> project -> Static Web ->  Static Web

设置项目名和路径

右键项目名 -> 新建HTML

#### 7.HTML语法

```HTML
<!DOCTYPE html> <!--约束声明-->
<html lang="zh_CH"> <!--标签：表示开始，lang="en"表示语言是中文-->
<head> <!--标签：表示网页的标题-->
    <meta charset="UTF-8">
    <title>标题</title>
</head>
<body> <!--标签：表示主体-->
    hello
    <button onclick="alert('提示内容')">按钮</button> <!--标签：按钮-->
    上面<br/>下面 <!--单标签：换行-->
</body>
</html>
```

标签分为

单标签：<单标签/>

双标签：<双标签头></双标签尾>

#### 7.1HTML执行访问

虽然在网络上展示成功，但这就是个静态文件，并不牵扯web。

#### 8.CSS定义

CSS是层叠样式表单。是用于(增强)控制网页样式并允许将样式信息与网页内容分离的一种标记性语言。 

作用：1.增强html的功能，2.复用

#### 9.javaScript

特点：

1. 交互性（信息动态交互）
2. 安全性（不允许直接访问本地硬盘）
3. 跨平台性（只要是可以解释JS的浏览器都行，和平台无关）

#### 10.jQuery

jQuery是js最常用的类库

#### 11.xml

xml: 可扩展标记语言

可扩展的意思是，可以自定义标签  

1. 用来保存数据，而且这些数据有自我描述性
2. 它可以作为项目或模块的配置 文件
3. 数据传输，现在已经被json替代

#### 12.javaWeb

javaWeb: 所有通过java语言编写，可以通过浏览器访问的程序的总称，叫JavaWeb

请求：request

响应：response

web资源按照实现和呈现的效果不同，分为静态资源和动态资源

静态资源：html、css、js、txt、jpg图片、MP4视频

动态资源：jsp页面、Servlet程序

#### 13.常用的web服务器

Tomcat：由Apache组织提供的一种Web服务器，提供对jsp和Servlet的支持，它是一种轻量级的javaWeb容器。

#### 14.tomcat安装

1. 下载：http://tomcat.apache.org
2. 安装：解压文件

#### 15.启动tomcat

tomcat->bin-> 双击startup.bat，出现命令行

验证：浏览器访问127.0.0.1:8080，得到一个tomcat猫网页，默认访问127.0.0.1:8080/root/index,

​	root默认路径名，index默认资源名

第二种启动方式：

1. cd tomcat->bin

2. catalina run

关闭：

1. 关闭命令行
2. 用命令Ctrl+C
3. tomcat->bin-> 双击shutdown.bat

#### 16.设置端口号

mysql默认端口号：3306

tomcat默认端口号：8080，在tomcat->conf->server.xml文件中可以修改，重启生效

http协议默认端口：80，访问网址的时候端口默认值就是80

#### 18.访问

127.0.0.1:8080/路径/资源

127.0.0.1表示本地ip

8080是tomcat默认端口号

路径是tomcat->webapps下的路径

资源是路径下的html、jsp等文件

另外：路径的缺省值是root，资源的缺省值是index，意思是127.0.0.1:8080/  《=》 127.0.0.1:8080/root/index

#### 18.5 Tomcat的使用

（1）启动：tomcat->bin-> 双击startup.bat

（2）在tomcat->webapps下，新建文件夹作为工程路径，开发 .html 文件作为访问资源

（3）访问：127.0.0.1:8080/工程路径/资源路径

#### 19.部署web方式

1.把web文件放到webapps下：访问127.0.0.1:8080/路径/资源

2.在conf建立配置文件：abc.xml

abc.xml：

```xml
<Context path="/abc" docBas="E:/book">
```

web文件可以放在任何地方，path映射docBas

访问：127.0.0.1:8080/abc/资源

#### 20.IDEA整合Tomcat

file -> settins -> bulid -> Applicaton server -> '+' -> Tomcat Server -> 选择Tomcat 安装的路径

#### 21.web目录

​	web：web目录专门用来存放web工程的资源文件：html页面、css文件、js文件等等

​	WEB-INF：WEB-INF目录是受服务器保护，目的浏览器无法直接访问此目录下的内容，必须通过程序

​	web.xml：动态web工程的配置文件，可以配置web工程组件：Servlet程序、Filter过滤器、Listener监听器等

​	index.jsp：缺省时访问页面，http://localhost:8080/工程路径

#### 22.web工程中导入jar包

1. 在 web->WEB-INF 下新建directory，命名为lib
2. 将jar包复制到lib下
3. 设置为应用：右键jar包  ->  add as library

#### 23.Servlet是什么

1. Servlet是javaEE的规范之一。规范就是接口。
2. Servlet是javaWeb三大组件之一，三大组件：Servlet程序、Filter过滤器、Listener监听器。
3. Servlet是运行再服务器上的程序，可以接受和相应客户端的请求。
4. java: servlet <==> C++: boost库 <==> Linux: TCP协议

#### 24.学习计划

javaWeb(html, css, js、xml, tomcat, servlet)

spring, springMVC, springboot

jenkins, git, gradle, redis, k8s, kfk

serviceComb, pass, springCloud

java(流、多线程), jvm, 内存

jba, mysql, mybatis

#### 25.手动实现servlet程序

1. 编写一个类实现servlet接口
2. 实现service方法，处理请求，并响应数据
3. 到web.xml中去配置servlet

27. Servlet小程序

#### 26.建立web工程

静态工程：new Project/Model -> Static Web

动态工程：new Project/Model -> Java Enterprise，选择Tomcat，选中Web Application 

#### 27.servlet小程序

（1）新建动态web工程：new Project/Model -> Java Enterprise，选择Tomcat，选中Web Application 

（2）处理程序

实现Servlet接口，核心service()方法

```java
package com.servlet;

import javax.servlet.*;
import java.io.IOException;
import java.util.ArrayList;

public class HelloServlet implements Servlet {

    public HelloServlet() {
        System.out.println("HelloServlet()");
    }

    @Override
    public void init(ServletConfig servletConfig) throws ServletException {
        System.out.println("HelloServlet.init()");
    }

    @Override
    public ServletConfig getServletConfig() {
        return null;
    }

    /**
     * service是处理请求和访问的
     * @param servletRequest
     * @param servletResponse
     * @throws ServletException
     * @throws IOException
     */
    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        System.out.println("HelloServlet.service()");
    }

    @Override
    public String getServletInfo() {
        return null;
    }

    @Override
    public void destroy() {
        System.out.println("HelloServlet.destroy()");
    }
}
```

（3）web.xml配置

当访问来临时，告诉系统访问哪里

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <!--servlet标签是给Tomcat配置Servlet程序-->
    <servlet>
        <!--servlet-name标签给Servlet程序起别名，一般是类名-->
        <servlet-name>HelloServlet</servlet-name>
        <servlet-class>com.servlet.HelloServlet</servlet-class>
    </servlet>

    <!--servlet-mapping标签给servlet程序配置访问地址-->
    <servlet-mapping>
        <!--servlet-name的作用是告诉服务器，我们当前配置地址是哪个servlet程序使用-->
        <servlet-name>HelloServlet</servlet-name>
        <!--urt-pattern标签配置访问地址
            /斜杠在服务器解析时，表示地址：Application->Url: http://localhost:8080/工程路径
            /hello：表示http://localhost:8080/工程路径/hello
        -->
        <url-pattern>/hello</url-pattern>
    </servlet-mapping>

</web-app>
```

好麻烦，就两条信息：<servlet-class>com.servlet.HelloServlet</servlet-class> 和 <url-pattern>/hello</url-pattern>，就不能写在一起。

#### 28.Tomcat

越想越觉得Tomcat多余，C++中就是VS直接调用boost

#### 学习进展

尚硅谷2020最新版JavaWeb：126 -> 127























