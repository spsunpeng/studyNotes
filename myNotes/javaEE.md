### 前言

代码：git@github.com:spsunpeng/javaWeb.git



### 一、以前

#### 1、概念

- 文本交互语言：由文本和标记符组成，文本是内容，标记符是就是标记。如HTML，标记可以告诉浏览器如何显示文本（如：文字如何处理，图片如何排版）。

- HTML：页面内容，是文本标记语，标记可以告诉浏览器如何显示文本。

- CSS：页面布局，是层叠样式表单。是用于(增强)控制网页样式并允许将样式信息与网页内容分离的一种标记性语言。 增强html的功能

- jsp：动态页面

- javaScript：交互性（信息动态交互）、安全性（不允许直接访问本地硬盘）、跨平台性（只要是可以解释JS的浏览器都行，和平台无关）

- Query：js最常用的类库

- xml: 可扩展标记语言，可扩展的意思是可以自定义标签 ，用来保存数据而且这些数据有自我描述性，也可以作为项目或模块的配置 文件

- javaWeb: 所有通过java语言编写，可以通过浏览器访问的程序的总称

-  Servlet：server applet
   - Servlet是javaEE的规范之一。规范就是接口。
   - Servlet是javaWeb三大组件之一，三大组件：Servlet程序、Filter过滤器、Listener监听器。
   - Servlet是运行再服务器上的程序，可以接受和相应客户端的请求。

- Tomcat：常用的web服务器。Tomcat =  Servlet支持 + jsp支持 + javaWeb容器



#### 2、Static Web项目

##### 2.1 新建项目

- idea -> new -> project -> Static Web ->  Static Web

##### 2.2 HTML

右键项目名 -> 新建HTML

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

- 单标签：<单标签/>

- 双标签：<双标签头></双标签尾>



#### 3、tomcat

- 下载安装：http://tomcat.apache.org ，然后直接解压

##### 3.1 访问静态资源

1. 将静态资源放到webapps目录下

2. 启动 tomcat ：【双击】bin /startup.bat

3. 访问：127.0.0.1:8080/路径/资源，资源是html、jsp等文件，缺省为index.jsp

##### 3.2 访问动态（运行war）

- 可能是把war包放到webapps，然后再启动运行。（已无用）



#### 4、servlet项目

##### 4.1 新建工程

- 方法一：new Project/Model -> Java Enterprise，选择Tomcat，选中Web Application（简单，因为Enterprise是企业版） 

- 方法二：平民版

  - 新建项目：new Project/Model -> Java，选中Web Application 

  - 启动类：file -> settins -> bulid -> Applicaton server -> '+' -> Tomcat Server -> 选择Tomcat 安装的路径

  ![启动服务器tomcat](pictures\javaWeb\启动服务器tomcat.png)

  - 导包：servlet.jar、jsp.jar

  ![idea导包方式3](pictures\javaWeb\idea导包方式3.png)

##### 4.2 继承HttpServlet

```java
public class HelloServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String username = req.getParameter("username");
        req.getSession().setAttribute("username", username);
        //转发
        req.getRequestDispatcher("index.jsp").forward(req, resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}

```

##### 4.3 web.xml配置 

```xml
    <servlet>
        <servlet-name>helloServlet</servlet-name>
        <servlet-class>com.mahsibing.Controller.HelloServlet</servlet-class>
    </servlet>
    
    <servlet-mapping>
        <servlet-name>helloServlet</servlet-name>
        <url-pattern>/hello</url-pattern>
    </servlet-mapping>
```

##### 4.4 index.hsp视图 

```jsp
<html>
  <head>
    <title>$Title$</title>
  </head>
  <body>
  ${username}
  </body>
</html>
```





### 二、javaEE

##### 1、http

- 场景：web，浏览器和服务器之间传输数据

- http：超文本传输协议。解决不同浏览器向服务器传输数据时的差异

- servlet：Server applet，web的小服务程序。解决动态(实时)数据相应的问题，固定的页面可以直接返回，但多数情况都不是预想配置好的页面，这时就需要将数据转化为页面，这个转化的功能就叫Server applet。

  java与网络

![java与网络](pictures\javaEE\java与网络.png)

http请求报文

![http请求报文](pictures\javaEE\http请求报文.png)

http响应报文

![http相应报文](pictures\javaEE\http相应报文.png)

客户端到服务端

![客户端请求服务端](pictures\javaWeb\客户端请求服务端.png)

myTomcat

![](pictures\javaWeb\手动实现tomcat容器.png)



##### jsp

- jsp：Java Server Pages，java服务器页面，是一种动态网页开发技术。它**使用JSP标签在HTML网页中插入Java代码**。（java代码与html页面的交互）

作用1：帮助浏览器转发，并且可以设置访问方式

页面访问：127.0.0.1/method.jsp

转发：127.0.0.1/method（post方法）

method.jsp

```jsp
<html>
<head>
    <title>Title</title>
</head>
<body>
    <form action="method" method="post">
        <input type="submit" value="转发">
    </form>
</body>
</html>
```

##### servlet

404 资源错误

405 方法错误

500 服务内部错误

HttpServlet源码中也是通过service调用doGet/doPost











