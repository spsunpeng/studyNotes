# 2020.09.02

#### 1.StringUtils判空

import org.apache.commons.lang.StringUtils

包必须手动导入，因为jdk自带的util包没有 isBlank() 方法。

1）boolean StringUtils.isEmpty(String str)  -》 str为 null 或 “” 为空

2）boolean StringUtils.isBlank(String str)  -》 str为 null 或 “”  或 “ ” 或 “   ” 或  “\n” 或 ” 或 "\t“ 都为空

具体：https://www.cnblogs.com/guiblog/p/7787569.html

#### 2.jpa的Entity层 与 数据库表

jpa的Entity层         数据库表    启动成功否           结论 

​    无@id                      任意             否                 程序只检查自己的语法，jpa必须要有主键，即使有相应的表

​    @id                          任意             是                 有主键就启动成功，mysql若没有表则建立

@id + @id                   任意             否                  程序只检查自己的语法，即使有相应的数据库表

@id+@id+@IdClass   任意             是                 正确，无论数据库表自己有几个主键

结论：实际中要一一对应，不要因为能启动成功就随意使用，以免后续的增删改查操作发生未知错误。

#### 3.Web

请求方式

1. 请求方式分为get、post等，get对应getMapping，post对应postMapping，不可混用

2. 传参分为两种：params  和  body

params

1. 可以由params传入，params可以是任意类型/对象。
2. 按照名字为 传递的参数 和 接受的参数 做对应
3. 默认，传参和入参随意，不必一一对应
4. 只有设置@ApiParams(require = true) 等注解时，才会规定入参必须需要那些穿参
5. 入参是对象时，在接受传参时，会调用同名set方法。
6. 入参是对象的字段和其他入参名同名时，两个都会赋值
7. body的列表从功能上说，也是params，而非body

body

1. body接受必须要有注解@Requestbody，这样才能认为是主体
2. 加入@Requestbody后，那么这一项就是必填的了
3. @Requestbody注解的都是对象必穿，但不表示其中的字段都必须有，要求比穿也需要require = true
4. 同样前后按名字对应，调用set方法
5. 传参可以是txt、xml、json等，但一般都是json

总结

1. 穿参和入参对应，get请求和@getMapping对应，参数和请求之间没有必然的关系

2. 但是一般用法主要有两种：

   第一种：get请求、@getMapping、params

   第二种：post请求、@postMapping、body(json)、@Requestbody

#### 4.注解：分层注解

基本注解

@Component  原意是组件，它是spring最基本的注解，作用是将类注入到bean中

MVC各层注解

@Controller 控制层注解，作用是@Component和表明他是控制层(后者好像没有实际意义)

@Service 服务层注解，作用是@Component和表明他是服务层(后者好像没有实际意义）

@Respository 持久层注解，作用是@Component和表明他是持久层(后者好像没有实际意义）

其他注解

@RestController 由@Controller和@ResponseBody组成，此外，ServiceComb





# 2020.09.03

#### 1.mysql

设置字段的默认值：alter table users_info alter column role_id set default 1

#### 2.注解：@Valided

@Valided 判空注解

问题引入：@ApiModelProperty(require = true)注解表明这个参数必填，即 param != null，否则服务无法调用

局限：对于基本类型它可以保证，但是对于String和对象它并不能完全确保有值。

解决：引入@Valided注解，以及连带的注解：

1. 控制层：

   类前：@Valided     

   函数参数前：@Valid

   ```java
   @RestController
   @Valided    
   public class StudentHandle {
   
       @PostMapping("/save")
       public Student save(@RequestBody @Valid SaveRequest param){
       }
   }
   ```

2. Model层

   @NotNull(massage="")		不能为 null

   @NotEmpty(massage="")    字符串不能为 null/""

   @NotBlank(massage="")     字符串不能为 null/""/空格/转移字符  

   @Size(min=1, massage="")  数组最小为1  

   @Valid             					   对象不能为空    

   

# 2020.09.04

#### 1.java和C++的字符串区别

1. java好像没有考虑过C++中 length(char*) 和 length(string) 的问题
2. 那么 java 的字符串和字符 的区别也是 \n 吗？

#### 2.mysql的varchar(1)

mysql的varchar(1)同时兼容java和C++中的char、char[]、string吗？

#### 3.jba注解之@Column

@Column(name="columnName", length=32) 

private String columnName;

可以直接映射到表中的字段，但是对于以经存在的字段，此句并不能更改varchar()的大小。

#### 4.jpa脱离数据库表的思想

jpa主张脱离表，如不使用sql语句，移植时不移植表。

1）建表：

1. jpa通过注解@Entity 建表、建立字段。
2. jpa通过注解@Column(name="columnName", length=32) 建立字段。
3. jpa通过注解@Id、@IdClass 建立主键、联合主键。

2）增删查改：脱离sql语句

3）设置字段属性：

​	字段默认值：代码实现，新增前固定给该字段赋值。

​	非空控制等：代码实现，代码自己判断。

4）表数据：

​	代码移植时：表如何移植

1. 表的数据存储在项目中，程序启动时自动初始化数据

 	2. 不给表设置多余的属性，依靠1）和3）保证

#### 5.时间戳

sql定义了三个类：Time   Date  Timestamp

```java
import org.junit.Test;

import java.sql.Time;
import java.sql.Date;
import java.sql.Timestamp;

public class Test2 {

    @Test
    public void fun1(){
        Date d = new Date(System.currentTimeMillis()); //获取当前日期
        System.out.println(d);                         //2020-09-04

        Time t = new Time(System.currentTimeMillis()); //获取当前时间
        System.out.println(t);                         //10:30:36

        Timestamp dt = new Timestamp(System.currentTimeMillis()); //获取当前日期时间
        System.out.println(dt);                                   //2020-09-04 10:30:36.219
    }
}
```

重点：jpa的字段 java.sql.Timestamp; 映射到mysql是datetime





# 2020.09.09

#### 1.mysql不等于

mysql中不等于 <>  和 != 都可以表示不等于，不过 <> 在所有的sql语句中都是通用的。





# 2020.09.14

#### 1.Linux: 查看文件内容

| 命令 | 解释                   | 常用                    | 解释             |
| ---- | ---------------------- | ----------------------- | ---------------- |
| cat  | 从第一行开始显示       | **cat filename**        | -n可以显示行号   |
| tac  | 从最后一行开始显示     |                         |                  |
| more | 一页一页的显示档案内容 |                         | d：向前翻页      |
| less | 相比more，可以翻页     | shift+g 移动到最后一行  | b：向后翻页      |
| head | 只看头几行             | head-n 20 filename      | 头10页，默认为10 |
| tail | 只看尾巴几行           | **tail -n 20 filename** | 尾10页，默认为10 |

#### 2.linux: cat

1. 命令格式：

   cat [选项] [文件]...

2. 命令功能：

   cat主要有三大功能：

   1. 一次显示整个文件: cat filename

   2. 从键盘创建一个文件: cat > filename 只能创建新文件,不能编辑已有文件.

   3. 将几个文件合并为一个文件: cat file1 file2 > file

3. 命令参数：

   -n, --number   对输出的所有行编号,由1开始对所有输出的行数编号

#### 3. grep 参数 "搜索条件" fileName

eg：grep -rn "getDictData.*7," servicelog.log

- 参数

  - -r ：进入目录搜索
  - -n :  显示行号

- 搜索条件：支持正则表达式

  - "getDictData" ： 搜索含有getDictData的

  - "getDictData.*7,"：搜索含有 getDictData和7，的

- fileName：文件，可以是文件，也可以是目录，注意目录要加 -r 



# 2020.09.17 

#### 1.Tyora：图片

绝对路径：直接粘贴，粘贴的就是绝对路径，但是这样的话，图片一经迁移就无法访问

相对路径：将图片和Tyora存放在同一个目录下，使用相对路径，图片和文件可以一起迁移

另外：Tyora也可以认为是文本标记语言，加标记可以使字体加粗、倾斜等。图片同样，加标记可以改变尺寸、背景等。





# 2020.09.25

#### 1.Specification：cb.and()

cb.and(predicate1, null)

cd.and：and毋庸置疑就是且，但是 predicate1 and null 结果是 predicate1 

cd.or：or毋庸置疑就是或，但是 predicate1 or null 结果是 null

综上所述，Specification认为null表示所有，而非一个没有

 

# 2020.10.10

#### 1.gitLab：Replace

在gitLab上更改内容

1. 选择要更改的文件
2. 选择 Replace
3. 导入更改的版本
4. merger两者



# 2020.10.13

#### 1.环境

| 缩写     | 全程                        | 中文                 |
| -------- | --------------------------- | -------------------- |
| DEV      | development                 | 开发                 |
| SIT      | System Integrate Test       | 系统整合测试（内测） |
| UAT      | User Acceptance Test        | 用户验收测试         |
| PET      | Performance Evaluation Test | 性能评估测试（压测） |
| SIM      | simulation                  | 仿真                 |
| PRD/PROD | production                  | 产品/正式/生产       |

四个主要环境：pro、pre、test、dev环境，中文名字：生产环境、灰度环境、测试环境、开发环境：

pro环境：生产环境，面向外部用户的环境，连接上互联网即可访问的正式环境。

pre环境：灰度环境，外部用户可以访问，但是服务器配置相对低，其它和生产一样。

test环境：测试环境，外部用户无法访问，专门给测试人员使用的，版本相对稳定。

dev环境：开发环境，外部用户无法访问，开发人员使用，版本变动很大。



# 2020.10.14

#### 1.断点调试

![断点调试](pictures\quickNotes\断点调试.png)

```java
@ControllerAdvice
public class MyExceptionHandler {

    @ExceptionHandler(value =Exception.class)
	public String exceptionHandler(Exception e){
		System.out.println("发生了一个异常"+e);
       	return e.getMessage();
    }
}

@ControllerAdvice
public class GlobalExceptionHandler {

    /**
     * 处理自定义异常
     *
     */
    @ExceptionHandler(value = DefinitionException.class)
    @ResponseBody
    public Result bizExceptionHandler(DefinitionException e) {
        return Result.defineError(e);
    }

    /**
     * 处理其他异常
     *
     */
    @ExceptionHandler(value = Exception.class)
    @ResponseBody
    public Result exceptionHandler( Exception e) {
        return Result.otherError(ErrorEnum.INTERNAL_SERVER_ERROR);
    }
}
```



# 2020.10.20

#### 1.win进程

- 通过端口号查看PID：netstat -aon|findstr "port"
- 通过PID查看信息：tasklist|findstr "PID"
- 通过PID杀死进程：taskkill /T /F /PID PID号

任务管理器方法：任务管理器：win10【详细信息】、win7【进程，如果没有PID字段，则查看->选择列->勾选PID】



# 2020.11.09

#### 1.Typora：分级目录

一级：实心圆；二级：空心圆；三级：实心正方形

快捷键：减号+空格



# 2020.11.11

#### 1. JPA：事务

```java
  @Modifying
  @Transactional
  @Query("delete from User u where u.active = false")
  void deleteInactiveUsers();
```

为什么@Modifying下一定要有@Transactional

- @Modifying的主要作用是声明执行的SQL语句是更新（增删改）操作，（仅仅只是声明）。
- @Transactional的主要作用是提供事务支持（JPA默认会依赖JDBC默认隔离级别，即默认只读，所以增删改需要此注解支持）。



#### 2. 注入值

##### 1. 配置文件版

```xml
<bean id="student" class="com.springIoc.entity.StudentEntity">
    <property name="id" value="1"></property>
    <property name="name" value="张三"></property>
    <property name="age" value="22"></property>
</bean>
```

#####  2. 注解+配置文件

@Component 代替 <bean id="student" class="com.springIoc.entity.StudentEntity"></bean>

@ConfigurationProperties(prefix = "student")

```properties
student.id=1
student.name="张三"
student.age=22
```



##### 3. 注解

```java
//@Value("${person.last-name}")
private String lastName;
//@Value("#{11*2}")
private Integer age;
//@Value("true")
private Boolean boss;
```



# 2020.11.16

#### 1. Ajax

网页异步刷新：Web2.0和核心，Ajax的核心



# 2020.11.25

#### 1. springMVC：传map

#### 2. 新生代、老年代

#### 3. k8s上linux命令：jmap、grep



# 2020.12.02

#### 1. vi：查看

1）上下翻动：向上箭头、向下箭头：每次滚动一条日志，这条日这可能占据多行

2）前后翻页：ctrl+f、ctrl+b：f就是forword，b就是backward

3）首行末行：首行（gg），末行（shift+g）

4）查找：?string：查找字符串，N向前，n向后

5）分页分屏：k8s中的vi也支持分页/分屏操作





# 编译打包运行：

### 1. 单jar包

例子：创建一个java项目，编写两个类Welcome.java和Teacher.java，其中类Welcome含有main函数，是程序的入口，并调用Teacher中的greeting。

```java
package com.sunpeng;
import com.sunpeng.impl.Teacher;
public class Welcome {
    public static void main(String[] args) {
        Teacher.greeting();
    }
}
```

```java
package com.sunpeng.impl;
public class Teacher {
    public static void greeting(){
        System.out.printf("Welcome study jar!");
    }
}
```

命令在src目录下运行

#### 1.1 编译

|        | 编译                                                         |
| ------ | ------------------------------------------------------------ |
| JVM    | javac com/sunpeng/Welcome.java           编译Welcome时因导入Teacher，所以Teacher并不需要再执行编译命令 |
| idea   | Build                                                        |
| maven  | mvn package                              同时编译打包        |
| gradle | gradle package                           同时编译打包        |

#### 1.2 打包

|        | 编译                                                         |
| ------ | ------------------------------------------------------------ |
| JVM    | 1）jar -cvfm welcome.jar META-INF/MANIFEST.MF com/sunpeng/Welcome.class com/sunpeng/impl/Teacher.class |
| idea   | 2）将需要打包的文件放到target中，然后 jar -cvfm welcome.jar META-INF/MANIFEST.MF * |
| maven  | mvn package                同时编译打包                      |
| gradle | gradle package             同时编译打包                      |

```java
#/MANIFEST.MF文件
Manifest-Version: 1.0
Created-By: 1.8.0_231 (Oracle Corporation)
Main-Class: com.sunpeng.Welcome
```

#### 1.3 运行

|        | 编译                                                         |
| ------ | ------------------------------------------------------------ |
| JVM    | (1) 运行字节码文件：java Welcome     (2) 运行jar文件：java -jar welcome.jar |
| idea   | Run                                                          |
| maven  | java -jar welcome.jar                                        |
| gradle | java -jar welcome.jar                                        |

### 2. 调用jar

例子：模块一：创建一个java项目，编写Teacher.java，含有方法greeting。模块二：创建一个java模块，编写Welcome.java，含有main函数，并调用模块一Teacher中的greeting。

#### 2.1 Teacher编译打包

和第一节的方法一样

|        | 编译                                                         |
| ------ | ------------------------------------------------------------ |
| JVM    | (1) 编译 javac com/sunpeng/Welcome.java    (2) 打包 jar -cvf com/sunpeng/impl/Teacher.class |
| idea   | (1) 编译build  (2)打包 jar -cvf com/sunpeng/impl/Teacher.class |
| maven  | mvn package                同时编译打包                      |
| gradle | gradle package             同时编译打包                      |

#### 2.2 编写/识别

welcome识别teacher.jar，实际编写代码时并不需要识别，只需要在编译运行时指明jar包路径即可

|        | 编译                                                         |
| ------ | ------------------------------------------------------------ |
| JVM    | 普通编译器根本没有识别功能                                   |
| idea   | jar包右键-->add a library  ，如果不识别idea就会报错，就没有链接提醒 |
| maven  | mvn install 导入仓库中，再在Welcome中加入依赖                |
| gradle | 待补充                                                       |

```xml
<dependency>
    <groupId>com.sunpeng</groupId>
    <artifactId>teacher</artifactId>
    <version>1.0-SNAPSHOT</version>
</dependency>
```

#### 2.3 编译、打包、运行

|      | JVM                                                          |
| ---- | ------------------------------------------------------------ |
| 编译 | javac -cp ..\lib\teacher.jar com\sunpeng\Welcome.java -d ..\target |
| 打包 | jar -cvfm welcome.jar META-INF/MANIFEST.MF *     MANIFEST.MF文件需要添加Class-Path |
| 运行 | java -jar welcome.jar                                        |

```java
#/MANIFEST.MF文件
Manifest-Version: 1.0
Created-By: 1.8.0_231 (Oracle Corporation)
Main-Class: com.sunpeng.Welcome
Class-Path: ../lib/teacher.jar
```

### 3. 细节补充

#### 3.1 编译

在Terminal中执行javac命令，具体：

javac com/sunpeng/Welcome.java

- 之需要显示地编译Welcome.java即可，因为在导包时会编译Teacher：import com.sunpeng.impl.Teacher;
- 必须在src目录下执行，因为系统不仅会编译Welcome，也要编译Teacher，所以必须找到Teacher。Teacher导入的路径为：import com.sunpeng.impl.Teacher; com目录在src下，所以必须是src。

#### 3.2 打包

jar -cvf com/sunpeng/Welcome.class com/sunpeng/impl/Teacher.class

打包并生成META-INF/MANIFEST.MF文件，如下

```java
Manifest-Version: 1.0
Created-By: 1.8.0_231 (Oracle Corporation)
```

但是生成的META-INF/MANIFEST.MF也并不知道程序入口，需要添加：

```java
Main-Class: com.sunpeng.Welcome
```

将Welcome和Teacher放到一起，并指明程序的入口

然后执行：

jar -cvfm welcome.jar META-INF/MANIFEST.MF com/sunpeng/Welcome.class com/sunpeng/impl/Teacher.class

- -c 表示要创建一个新的jar包，
- -v 表示创建的过程中在控制台输出创建过程的一些信息，
- -f 表示给生成的jar包命名
- -m 表示要定义MANIFEST文件。之后再重新执行

#### 3.3 编译打包

步骤：

1. 新建target

2. javac com/sunpeng/Welcome -d ../target

3. 将META-INF/MANIFEST.MF复制到target中

4. 进入target目录
5. jar -cvfm welcome.jar META-INF/MANIFEST.MF

此外，可以在命令行确定main函数入口

java -cp welcome.jar com.sunpeng.Welcome

cp表示classpath，后面跟main函数的路径

#### 3.4 查看jar文件目录

jar -tf filename.jar





# 2020.12.18

#### 1.动态打印日志：tailf

tailf  filename