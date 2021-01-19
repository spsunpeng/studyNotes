#### 遗留问题：

- @PropertySource(value = {"classpath:person.yaml"})   不起作用






### 一、入门搭建1（不使用maven）

- 总结不适用maven导包的方法
- 搭建一个简单的spring demo

#### 1. 创建项目

创建java项目

#### 2. 引入jar包

##### 2.1 下载jar包

下载：https://repo.spring.io/libs-release-local/org/springframework/spring/

本地存储位置：E:\jar\spring\spring-framework-5.2.3.RELEASE\libs

##### 2.2 告诉idea(jar包的路径)

导入jar包，此外Module和包Libraries的作用也是导入jar包，Atifacts的作用是导出jar包

![](pictures\spring\idea导包方式1.png)

###### 2.2.1 idea管理包介绍

- project settings

  - Projects
  - Modules：作用也是导入jar包
  - Libraries：作用是导入jar包

  - facets：【原意方面】
  - Atifacts：作用是导入jar包，【原意神器】

- platform settings

  - sdks
  - global Libraries：作用也是导入jar包

###### 2.2.2 导包方式

- 新建lib文件夹（非必须），将jar包存放在这里
- 右键，选择 Add as Library
- 弹出窗口，选择导入的方式：global 、Modules、Libraries

##### 2.3 此外

运行Spring程序额外还需要commons-logging包，方式一样

下载：http://commons.apache.org/proper/commons-logging/download_logging.cgi

本地：E:\jar

导入方式：一样

#### 3. 创建实体类

```java
public class Person {
    private int id;
    private String name;
    private Long age;
    private String sex;
}
```

#### 4. 配置文件(ioc.xml)

##### 4.1 新建带有spring头信息的xml文件

注：只有在导入spring核心jar包后，idea才会出现此选项

新建 --> XML Configuration File --> Spring Config

![新建spring_xml](D:\Git\gitRepository\studyNotes\myNotes\pictures\spring\新建spring_xml.png)

具有spring头信息的xml文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

</beans>
```

##### 4.2 配置bean

```xml
    <bean id="person" class="com.mashibing.bean.Person">
        <property name="id" value="001"></property>
        <property name="name" value="sunpeng"></property>
    </bean>
```

#### 5. 使用

```java
public class Test1 {
    public static void main(String[] args) {
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("com/mashibing/Ioc.xml");
        Person person = (Person)applicationContext.getBean("person");
        System.out.println(person);
    }
}
```





### 二、入门搭建2（使用maven）

- spring的xml配置
- 使用对象的方式

#### maven优势

- maven管理jar包的优势：简单方便，此外，在导入时会自动导入相关的jar包，比如只导入context，会自动导入bean、core、exception，还会自动导入common

#### 创建bean

```xml
	<!--通过无参构造器和get/set方法-->
    <bean id="person" class="com.mashibing.bean.Person">
        <property name="id" value="10"></property>
        <property name="name" value="sunpeng"></property>
        <property name="sex" value="nan"></property>
        <property name="age" value="20"></property>
    </bean>

    <!--通过有参构造器-->
    <bean id="person2" class="com.mashibing.bean.Person">
        <constructor-arg name="id" value="11"></constructor-arg>
        <constructor-arg name="name" value="sunyue"></constructor-arg>
        <constructor-arg name="sex" value="nv"></constructor-arg>
        <constructor-arg name="age" value="18"></constructor-arg>
    </bean>
```

- spring底层通过反射实现，反射获取对象时需要无参构造器
- <property>由set/get方法的名字决定，不由定义的属性名决定
- <constructor-arg>由由构造器器参数名决定，不由定义的属性名决定

#### 使用

```java
public class Test {
    public static void main(String[] args) {
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("com/mashibing/Ioc.xml");
        Person person = context.getBean("person", Person.class);
    }
}
```

- 代码：ApplicationContext applicationContext = new ClassPathXmlApplicationContext("com/mashibing/Ioc.xml");
  - ClassPathXmlApplicationContext 继承 ApplicationContext
  - ApplicationContext继承Beanfactory

- 获取对象：Person person = context.getBean("person", Person.class);
  - 参数"person"：配置文件中的 bean.id，通过它可以获取唯一的对象
  - 参数Person.class：也可以通过类获取类名获取对象，好处是返回值知道是类型，但是一个类可能会创建多个对象，导致报错
  - 所以，还是两个都给出吧



#### 图表diagrams

类图在idea中的查看方式

![diagrams](pictures\spring\diagrams.png)









