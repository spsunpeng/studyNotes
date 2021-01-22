### 遗留问题：

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

- xmlns:xsi -》xmlns全名xml namespace，那么它的意思就是xml的命名空间是xsi，这个命名空间解释文件中的标签
- 此外，还有xmlns:p命名空间

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





### 二、配置

- maven优势：maven管理jar包的优势：简单方便，此外，在导入时会自动导入相关的jar包，比如只导入context，会自动导入bean、core、exception，还会自动导入common

#### 1. 基础配置

#####  1.1 创建bean

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

##### 1.2 使用

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



##### 1.3 图表diagrams

类图在idea中的查看方式

![diagrams](pictures\spring\diagrams.png)

#### 2. 复杂参数bean配置

```xml
<bean name="person3" class="com.mashibing.bean.Person">
    <property name="id" value="10"></property>
    <property name="name" value="sunpeng"></property>
    <property name="sex" value="nan"></property>
    <property name="age" value="20"></property>
    <!--[]-->
    <property name="like" >
        <array>
            <value>book</value>
            <value>woman</value>
        </array>
    </property>
    <!--list-->
    <property name="list">
        <list>
            <value>xiaoming</value>
            <value>xiaohong</value>
        </list>
    </property>
    <!--object-->
    <property name="address">
        <bean class="com.mashibing.bean.Address">
            <property name="province" value="shanxi"></property>
            <property name="city" value="xian"></property>
        </bean>
    </property>
    <!--set<object>-->
    <property name="addresses">
        <set>
            <!--使用bean标签-->
            <bean class="com.mashibing.bean.Address">
                <property name="province" value="shanxi"></property>
                <property name="city" value="xian"></property>
            </bean>
            <!--bean标签中的home标签无用，其他地方调用不到-->
            <bean name="company" class="com.mashibing.bean.Address">
                <property name="province" value="shanxi"></property>
                <property name="city" value="xian"></property>
            </bean>
            <!--ref标签使用外面的bean-->
            <ref bean="home"></ref>
        </set>
    </property>

    <!--map<string,object>-->
    <property name="map">
        <map>
            <!--map<string,string>-->
            <entry key="aa" value="11"></entry>
            <!--map<string,list>，这里array不能用-->
            <entry key="bb">
                <list>
                    <value>21</value>
                    <value>22</value>
                </list>
            </entry>
            <!--map<string,object>-->
            <entry key="cc">
                <bean class="com.mashibing.bean.Address">
                    <property name="city" value="weinan"></property>
                </bean>
            </entry>
            <!--value = value-ref-->
            <entry key="dd" value-ref="home"></entry>
        </map>
    </property>
</bean>

<bean name="home" class="com.mashibing.bean.Address">
    <property name="province" value="shanxi2"></property>
    <property name="city" value="xian2"></property>
</bean>
```

- 标签
  - bean：object
  - array：[]
  - ref：引用bean
  - entry：条目，map内键值对需要的标签
- 建议：设计实体类的时候建议用包装类代替基本类型，用list等代替数组
  - Integer 代替 int：标签type="java.lang.Integer"，这里就不能用int
  - map<string,list> 代替 map<string,array> ：map中使用array标签打印出来的时hashcode，没有toSting重载，不能使用



#### 3. 工厂模式

##### 3.1 xml

```xml
<!--静态工厂-->
<bean name="personF1" class="com.mashibing.factory.PersonStaticFactory" factory-method="getInstance"></bean>
<!--工厂方法-->
<bean name="personInstanceFactory" class="com.mashibing.factory.PersonInstanceFactory"></bean>
<bean name="personF2" class="com.mashibing.bean.Person" factory-bean="personInstanceFactory" factory-method="getInstance"></bean>
<!--FactoryBean：创建bean对象的一种补充-->
<bean name="personFactoryBean" class="com.mashibing.factory.PersonFactoryBean"></bean>
```

- 工厂方法是单例，不要想当然认为它可能是多例的，因为personInstanceFactory的bean是单例的，

##### 3.2 测试

```java
Person personF1 = context.getBean("personF1", Person.class);
Person personF2 = context.getBean("personF2", Person.class)
Person personFactoryBean = context.getBean("personFactoryBean", Person.class);
```

##### 3.3 静态工厂

```java
public class PersonStaticFactory {
    public static Person getInstance(){
        System.out.println("静态工厂");
        Person person = new Person();
        person.setId(1);
        person.setName("jia");
        person.setAge(20L);
        return person;
    }
}
```

##### 3.4 工厂方法

```java
public class PersonInstanceFactory {
    public Person getInstance(){
        System.out.println("工厂方法");
        Person person = new Person();
        person.setId(2);
        person.setName("yi");
        person.setAge(21L);
        return person;
    }
}
```

##### 3.5 FectoryBean

```java
/*
* 此方式是创建bean对象的一种补充，
* （1）用户可以按照自己的需求创建对象（person），创建的对象由ioc容器管理(person)，但是（PersonFactoryBean）这个类本身还是需要配置
* （2）是单例模式，但只会在使用时创建
* */
public class PersonFactoryBean implements FactoryBean<Person> {

    @Override
    public Class<?> getObjectType() {
        return Person.class;
    }

    @Override
    public boolean isSingleton() {
        return true;
    }

    @Override
    public Person getObject() throws Exception {
        System.out.println("FactoryBean");
        Person person = new Person();
        person.setId(10);
        person.setName("zhangsan");
        return person;
    }
}
```



#### 4. 配置外部类

```xml
<!--配置外部类-->
<bean name="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
    <property name="username" value="root"></property>
    <property name="password" value="610527"></property>
    <property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
    <property name="url" value="jdbc:mysql://127.0.0.1:3306/mydatabase"></property>
</bean>
```



#### 5. 引用其他配置文件

##### 5.1 引入context命名空间

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       https://www.springframework.org/schema/context/spring-context.xsd">
</beans>
```

##### 5.2 配置

```xml
<bean name="dataSource2" class="com.alibaba.druid.pool.DruidDataSource">
    <property name="username" value="${jdbc.username}"></property>
    <property name="password" value="${jdbc.password}"></property>
    <property name="driverClassName" value="${jdbc.driverClassName}"></property>
    <property name="url" value="${jdbc.url}"></property>
</bean>
<context:property-placeholder location="classpath:db.properties"></context:property-placeholder>
```

##### 5.3 外部配置文件

```properties
jdbc.username=root
jdbc.password=610527
jdbc.driverClassName=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://127.0.0.1:3306/msb_dongbao
```

- 错误1：Driver无法转换成string

  错误：name="driver"：

  正确：name="driverClassName"：

- 错误2：找不到类“com.mysql.jdbc.Driver”

  错误：jdbc.driverClassName=“com.mysql.jdbc.Driver”

  正确：jdbc.driverClassName=com.mysql.jdbc.Driver



#### 6. 自动装配

```xml
<!--自动装配：autowire=byName/byType-->
<bean name="address001" class="com.mashibing.bean.Address">
    <property name="city" value="xian"></property>
</bean>
<bean name="person001" class="com.mashibing.bean.Person" autowire="byName"></bean>
```

- 注解的基础



#### 7. SpEL

- Spring Expression Language

```xml
<!--SpEL-->
<bean name="address002" class="com.mashibing.bean.Address">
    <property name="city" value="xian"></property>
</bean>
<bean name="person002" class="com.mashibing.bean.Person">
    <!--运算符-->
    <property name="id" value="#{12-1}"></property>
    <!--bean-->
    <property name="addresses" value="#{address002}"></property>
    <!--bean的属性-->
    <property name="name" value="#{address002.city}"></property>
    <!--bean的方法-->
    <property name="like" value="#{address002.getCity()}"></property>
    <!--静态方法，不依靠bean-->
    <property name="age" value="#{T(System).currentTimeMillis()}"></property>
</bean>
```



#### 8. 其他bean标签

其他标签：继承、抽象类、初始化、销毁、加强初始化等标签，需要时百度吧

scope：scope="singleton" / "prototype"



### 三、注解



















