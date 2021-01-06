### 2020.12.23

### 项目整体框架搭建，整体设计一

#### 1. 课堂记录

1. 项目框架

2. git\sourcetree\gitee

   git@gitee.com:cpf100/msb-dongbao-mall.git

3. mysql和duck

4. lombok与代码生成器与mybatis-plu

5. @MapperScan("com.msb.msbdongbaoums.mapper")

6. maven的使用，父项目<parent>



### 2020.12.30

1.导入依赖

群组-服务名-版本号：确定唯一jar包

parent依赖的版本号：是否可以不同

2.导入依赖

如何识别其他模块的，install什么情况下用

3.扫包

springbootApplication、mapperscan还可以扫其他模块的包

@SpringBootApplication(scanBasePackages = {"com.msb"}) 还是 @Service 或者 @Component

4.时间

linux设置时间，代码自动修改时间，国际时间标准



#### 扫描、注入、组件

##### 1.扫：扫描其他类

```java
//扫描本模块下的类
@SpringBootApplication
//扫描本模块"com.msb.dongbao.ums"包 ，和"com.msb.dongbao.portal"包 下的类
@SpringBootApplication(scanBasePackages = {"com.msb.dongbao.ums", "com.msb.dongbao.portal"})
//合并，可以写做扫描包"com.msb.dongbao"
@SpringBootApplication(scanBasePackages = {"com.msb.dongbao"})
//那么，是不是也可以扫描包 "com", "org"，这样会扫描所有的引用的jar包下的类
@SpringBootApplication(scanBasePackages = {"com.msb.dongbao"}, "com", "org")
```

##### 2.注入：扫描时为了注入

```java
//注意，扫描是为了注入，扫描其他模块的包"com.msb.dongbao.ums"，就可以其他模块的组件注入
@SpringBootApplication(scanBasePackages = {"com.msb.dongbao"})
@Autowired
private UmsMemberService umsMemberService;

//如果，不需要注入，那么只要引入jar包就行了
private UmsMemberService umsMemberService;
/*
		<dependency>
            <groupId>com.msb</groupId>
            <artifactId>msb-dongbao-ums</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
*/
```

##### 3.组件：接口和实现类只需要接口时组件

```java
public interface UmsMemberService {
}

@Service
public class UmsMemberServiceImpl implements UmsMemberService {
}

/*
只有UmsMemberServiceImpl需要添加 @Component 组件的注解
*/
```



版本1：

- 场景：

  api下无 springboot-pulin

  application无springboot-pulin ， 依赖api 和 service

  service 有 springboot-pulin， 依赖 api

- 结果

  仓库中有 api 包

  service 确定依赖 api 

  application 确定依赖 api 和 service

  

版本3

- 更改

  删除service 中的 springboot-pulin

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

- 结果

  没有任何影响



版本4

- 修改

  service 增加 pom

  ```xml
  <packaging>pom</packaging>
  ```





spring-boot-maven-plugin打包出来的jar是不可依赖的
我们现在整合后的maven项目有一个parent工程，打包类型为pom，下面多个spring-boot工程作为它的module，分别为子模块和moduleA,moduleB。假如moduleA依赖于子模块。如果你在子模块中使用了spring-boot-maven-plugin的默认配置build，或者在parent工程中使用spring-boot-maven-plugin的默认配置build。那么在clean package的时候会发现moduleA找不到子模块中的类。原因就是默认打包出来的jar是不可依赖的。



### 2021.01.05

1.时间（遗留）、公司与宿舍环境、宿舍mysql

2.环境、密码脱敏、注册登录

测试