# 2020-09

#### 1、StringUtils判空

import org.apache.commons.lang.StringUtils

包必须手动导入，因为jdk自带的util包没有 isBlank() 方法。

1）boolean StringUtils.isEmpty(String str)  -》 str为 null 或 “” 为空

2）boolean StringUtils.isBlank(String str)  -》 str为 null 或 “”  或 “ ” 或 “   ” 或  “\n” 或 ” 或 "\t“ 都为空

具体：https://www.cnblogs.com/guiblog/p/7787569.html





#### 2、@Valided

@Valided 判空注解

问题引入：@ApiModelProperty(require = true)注解表明这个参数必填，即 param != null，否则服务无法调用

局限：对于基本类型它可以保证，但是对于String和对象它并不能完全确保有值。

解决：引入@Valided注解，以及连带的注解：

##### 2.1 控制层

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

##### 2.2 Model层

@NotNull(massage="")		不能为 null

@NotEmpty(massage="")    字符串不能为 null/""

@NotBlank(massage="")     字符串不能为 null/""/空格/转移字符  

@Size(min=1, massage="")  数组最小为1  

@Valid             					   对象不能为空    





#### 3、Tyora图片

绝对路径：直接粘贴，粘贴的就是绝对路径，但是这样的话，图片一经迁移就无法访问

相对路径：将图片和Tyora存放在同一个目录下，使用相对路径，图片和文件可以一起迁移

另外：Tyora也可以认为是文本标记语言，加标记可以使字体加粗、倾斜等。图片同样，加标记可以改变尺寸、背景等。



#### 4、环境

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



#### 5、win进程

- 通过端口号查看PID：netstat -aon|findstr "port"
- 通过PID查看信息：tasklist|findstr "PID"
- 通过PID杀死进程：taskkill /T /F /PID PID号

任务管理器方法：任务管理器：win10【详细信息】、win7【进程，如果没有PID字段，则查看->选择列->勾选PID】



#### 6、工作经验之数据库字段默认值

- 场景
  - aliyunCode字段 数据库中 不能为空
  - aliyunCode字段 接口中 不是必传， 
  - template 和mapper两个表关联

- 结果
  - 每次必须判断aliyunCode为不为空，为空要么不增，要么给默认值
  - 两张表结合导致 复杂度 指数上升
  - 导致3个bug：都是因为数据库为空

- 补充：如果就是这种场景应用，记得，不但需要入参的aliyunCode为不为空，还需要判断数据库中查询大的结果是否为空





# 2021-03

#### 1、公司框架

- 启动类：idea和gradle都可以启动jvm，用哪个自己选择，公司本地启动使用idea
- runWith：启动前执行一个gradle小程序，把gradle文件生成为application.properties
- 打tar包：公司代码也可以打包，执行，生成的文件中含有application.properties，其他环境会直接覆盖这个文件。
- base组件：（1）可以在服务启动时做点什么，只需要继承一个接口.（2）日志级别设置；（3）敏感字段加密：一个类的to/from方法。



#### 2、 打印堆栈

- Logger.error("http error", e) 中的e打印的就是堆栈，且先打印堆栈，再打印自己的描述信息

- Logger.error("http error", e) 中的e没用，打印堆栈就是帮助定位信息，描述信息已经很清晰地定位到信息了

- 那么会不会不清楚try{}中哪一行的错误？不会。因为IOException只会捕捉IOException，不过异常类型不明确的话另说。

- java或框架的自动异常处理（打印堆栈）

  - 打印堆栈是为了帮助定位错误，用于不发预见的执行时异常

  - 检查异常需要自己处理，打印时我觉得就没必要打印堆栈了，直接打印描述信息不香吗

  - 至于检查异常在自己没有打印堆栈时，仍打印堆栈，这都是因为后面的代码要用到，但是前面的已经异常了，没有生产出数据，还使用的话就空指针了

  - springboot统一异常处理会捕捉异常（实现一个接口就行），这样代码就不会再往下执行，也就不会再打印堆栈了。

  - serviceComb框架内会先于springboot统一异常处理，处理异常，所以，一有异常serviceComb就会打印异常堆栈，往往把正常的业务情况打印出来，建议关掉。

    


#### 3、时区

- 情景1：时区不同，本地和dev的时区不一样，导致token的过期时间。解决方法：数据库配置，设定时区

- 情景2：数据库不同，dev和sit环境数据库不同，导致token错误


#### 4、jsoup

.data(map) 中map的值不能为空

解决：

注解：@JsonInclude(JsonInclude.Include.NON_NULL)

可以在 new ObjectMapper().convertValue(object, Map.class)时，不将object中的空转换

#### 5、string和list<>之间的转换

String strList = objectMapper.writeValueAsString(arrayList);

List<Student> students = objectMapper.readValue(listJsonStr, new TypeReference<List<Student>>()

#### 6、加密

##### 6.1 加密

base64：编解码，不是加密，所以可以反解码

散列算法：md5【不可反解码，但由于对于主题的加密结果一样，可以通过彩虹表（对应表）破解】

对称加密：两方持有相同的密钥，加解密只需要密钥即可

非对称加密：公加私解，私加公解

##### 6.2、非堆成加密

**非堆成加密**：生成私钥和公钥，传输时消息和公钥可能会被截取。

| 加密/签名    | 解密/验签    | 泄露                         | 篡改                             |
| ------------ | ------------ | ---------------------------- | -------------------------------- |
| **公钥加密** | **私钥解密** | 防泄露，因为只有私钥才能解密 | 能篡改，因为公钥就可以为信息加密 |
| **私钥签名** | **公钥验签** | 会泄露，因为公钥就可以验签   | 防篡改，因为只有私钥才能签名     |

- 如果既要防泄露又要防篡改，需要双方都生成公私钥，消息在通信时既要加密又要签名。
- 非堆成加密：RSA加密算法

参考：https://www.cnblogs.com/pcheng/p/9629621.html



# 2021-04

#### 1、springboot配置

```java
public class myConfig{
    private String name;
    
	@Value("${supplier-access.nmyvirtual}")
	private String hobby;
}

@ConfigurationProperties(prefix = "supplier-access.nmyvirtual") //正确
@ConfigurationProperties(prefix = "supplier-access.nmyVirtual") //错误  
```

```properties
supplier-access.nmyvirtual.name=sunpeng   //可以不配置
supplier-access.nmyvirtual.hobby=women    //必须配置 
```

- @ConfigurationProperties中不能出现大小字母

- @Value必须配置，不然就会启动失败

  

#### 2、异常：类找不到

- ClassNotfoundException 编译时找不到类（常见）
  - 注入时的，找不到配置，找不到配置等等。

- NoClassDefFoundError 运行时找不到类（不常见）
  - 一种情况就是因为静态变量加载不到原因
  - 工程里没有将jar添加到classpath，maven项目的，需要根据项目情况排查



#### 3、postman

1.1 切换环境实现不同环境下的测试

- 切换环境达到不同环境
- 在请求中通过变量{{supplier-access}}达到不同服务

图1：不同环境

![postman不同服务url](pictures\quickNotes\postman不同环境.png)

图2：不同服务

![postman不同服务](pictures\quickNotes\postman不同服务.png)

图3：不同接口

![postman不同接口](pictures\quickNotes\postman不同接口.png)



#### 4、postman账号密码

账户：476567162@qq.com

密码：sp602157



# 2021-05

#### 1、删除module

先移除再删除

#### 2、yaml文档

- 根据缩进判断等级
- -代表数组

#### 3、rpc协议

什么是RPC协议？
RPC是一种远程过程调用的协议，使用这种协议向另一台计算机上的程序请求服务，不需要了解底层网络技术的协议。

在 RPC 中，发出请求的程序是客户程序，而提供服务的程序是服务器。

HTTP是一种超文本传输协议。是WWW浏览器和WWW服务器之间的应用层通讯协议。

RPC协议与HTTP协议的区别
1、RPC是一种API，HTTP是一种无状态的网络协议。RPC可以基于HTTP协议实现，也可以直接在TCP协议上实现。

2、RPC主要是用在大型网站里面，因为大型网站里面系统繁多，业务线复杂，而且效率优势非常重要的一块，这个时候RPC的优势就比较明显了。

HTTP主要是用在中小型企业里面，业务线没那么繁多的情况下。

3、HTTP开发方便简单、直接。开发一个完善的RPC框架难度比较大。

4、HTTP发明的初衷是为了传送超文本的资源，协议设计的比较复杂，参数传递的方式效率也不高。开源的RPC框架针对远程调用协议上的效率会比HTTP快很多。

5、HTTP需要事先通知，修改Nginx/HAProxy配置。RPC能做到自动通知，不影响上游。

6、HTTP大部分是通过Json来实现的，字节大小和序列化耗时都比Thrift要更消耗性能。RPC，可以基于Thrift实现高效的二进制传输。



#### 4、学习计划

javaEE -> springmvc -> springboot

数据库/jdbc -> mybatis/jpa  -> springboot

spring -> springboot

spring + springmvc + mybatis/jpa  -> springboot

springboot + 环境/组件 -> springcloud

设计模式/坦克大战项目 -> spring/springmvc/springboot源码



# 2021-07

#### 1、md文件表格换行和插入行

- ctrl + enter ：插入行

- shift + erter ：换行



#### 2、工作经验：yaml格式文件空格

每行前缩进的空格是yaml文件的格式/语法，一定注意。

##### 2.1 复制粘贴不规范导致错误

在复制粘贴keycloak文件时，由于复制粘贴没有考虑空格，导致只有第一行空格正确，其余几行都错误。

![错误复制yaml内容方法](pictures\quickNotes\错误复制yaml内容方法.png)

正确做法为从顶头位置复制，从顶头位置复制/粘贴。

![正确复制yaml内容方法](pictures\quickNotes\正确复制yaml内容方法.png)

##### 2.2 修改文件类型导致错误

由于servicecomb契约文件的原因，需要先将契约文件改为txt文件，程序启动成功后，再改为yaml文件格式，并将打印出来的文件粘贴到契约文件。注意，一定要先改文件类型，再改内容。反之，空格会乱。



#### 3、日志

日志配置

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



















