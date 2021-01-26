问：1.自动装配

​		2.springBoot的@test

### 一、翻译

1. Component 组件，成分，零件，要素
1. Application 申请，运用，应用软件
3. Configuration 布置，构造，配置
4. scan扫描
5. Enable使能够
6. auto自动
7. Request要求、请求
8. Response回复
9. Repository仓库
10. rest 休息、剩余部分、支架
11. Variable可变的、变量
12. Controller控制器
13. Mapping映射
14. url：Uniform Resource Locator：统一资源定位符
15. param参数



### 二、注解

@SpringBootApplication：包含了@ComponentScan、@Configuration和@EnableAutoConfiguration注解。其中@ComponentScan让spring Boot扫描到Configuration类并把它加入到程序上下文。

@Configuration 等同于spring的XML配置文件；使用Java代码可以检查类型安全。

@EnableAutoConfiguration 自动配置。

@ComponentScan 组件扫描，可自动发现和装配一些Bean。

@Component可配合CommandLineRunner使用，在程序启动后执行一些基础任务。

@RestController注解是@Controller和@ResponseBody的合集,表示这是个控制器bean,并且是将函数的返回值直 接填入HTTP响应体中,是REST风格的控制器。

@Autowired自动导入。

@PathVariable获取参数。

@JsonBackReference解决嵌套外链问题。

@RepositoryRestResourcepublic配合spring-boot-starter-data-rest使用。



@RequestMapping实例

@RequestMapping(value = "/index",method = RequestMethod.GET,params = {"name","id=10"})



### 三、注解：分层注解

基本注解

@Component  原意是组件，它是spring最基本的注解，作用是将类注入到bean

@Controller @Service @Respository 都是组件的注解，没有区别

@RestController 由@Controller + @ResponseBody组成，此外，ServiceComb





















