# 遗留：

- 使用notepad编写程序编译运行、使用idea创建程序、使用idea创建web程序(整合tomcat)、

- idea集成git -> 9.git

- maven -> 9.maven

- 多态实现工厂模式，代码练习

- 内部类实现lambad表达式，原理和C++比较

- object.hashCode(); 原理和使用

- junit：在java项目中使用

  

# 第一阶段：java开篇

### 1. java核心

java核心：不同于C++的地方

- 垃圾处理机制
- 跨平台性

### 2. java跨平台性

- 过程

  .java

  编译：javac.exe -》 JDK编译

  .class

  执行：java.exe  -》JVM解释.class文件，JVM针对不同系统对.class进行不同的解释

- 总结：
  - 一次编译，到处执行
  - javac.exe和java.exe文件都在JVM，java.exe会调用JVM
  - “可执行文件”在命令行中就是“命令”。

### 3. C语言跨平台性

C语言在多个操作系统执行的方式

- window：VS，所以VS是微软的

- Linux：gcc/g++，需要编写makfefile文件



### 4. 终端命令

| linux    | win/doc命令                                           |
| -------- | ----------------------------------------------------- |
| 无       | 切换盘符，直接输入盘符，如：D:  d:  C:  c:            |
| cd       | C:/User 不能直接进入 D:/git，必须切换盘符，再切换目录 |
| ls       | dir                                                   |
| clear    | cls                                                   |
| mkdir    | md                                                    |
| rm -r    | rd                                                    |
| 复制文件 | copy                                                  |
| rm       | del                                                   |



### 5. 一个文件中多个类

- 一个源文件中可以有多个类，只能有一个类被public修饰，源文件的名字必须跟public修饰的那个类名保持一致。
- 多个类会产生独立的字节码文件，即一个 .java 产生多个 .class



### 6. 进入要编译文件所在的目录的终端

- 方法1：文件路径上输入 cmd 
- 方法2：在notepad中右键文件 --》打开文件夹所在命令行



### 7. 配置环境变量

- path：配置JVM中可执行文件的路径 -》D:\Java\jdk1.8.0_131\bin

  D:\Java\jdk1.8.0_131\jre\bin -》 配置jre中可执行文件，可以不配置，目前还不知道用在哪里

- classpath：配置字节码文件（.class）的位置，可配可不配

- JAVA_HOME：D:\Java\jdk1.8.0_131

  解决tomcat在执行startup.bat的时候会出现闪退问题

### 8. 字节码文件

- 打开是16进制数字，需要专业的文件打开
- 反编译软件，资料中有

### 9. 转义字符：\t

- \t：制表符，每8位一个格子，不足则补全，eg

### 10. JDK、JRE、JVM

- JRE作用：JDK里的工具（如javac）几乎是用Java所编写，同属于Java应用程序，因此要使用JDK所附的工具来开发Java程序，所以自身需要附一套JRE才能运行。
- 安装了两个JRE：系统会先到 JDK 的子级目录找，然后到同级目录找，也就是说另外安装的JER没用
- JVM+Lib=JRE：JVM无法单独进行翻译，需要类库lib



# 第三阶段：idea

### 1.设置

- 修改类头的文档注释信息

  ```java
  /**
  @author sunpeng
  @Date ${YEAR}-${MONTH}-${DAY} ${TIME}
  */
  ```

- 

### 2.快捷键

注意：不起作用，可能是和输入法的快捷键重复

- 搜索
  - 搜索当前文件：alt + f
  - 搜索全部文件：alt + F
- 



# 第四阶段：面向对象

**类的组成：属性，方法，构造器，代码块，内部类**

### 1. java和C++的区别

| 特性             | java                                                         | C++                                          |
| ---------------- | ------------------------------------------------------------ | -------------------------------------------- |
| 构造器重写       | 重写会使得默认无参构造器失效，需重新提供                     | 与系统的无参构造形成重载                     |
| 对象所占用内存   | People p1 = new People(); (变量再栈内容在堆)                 | People p1; (栈)   People *p1 = new People(); |
| this             | 1.构造器调用构造器；2.解决字段和构造器形参重名，详2.4        | 一般缺省，因为显式无加强用法                 |
| 继承关键字       | extends                                                      | :                                            |
| 继承权限         | public                                                       | public、private、protected                   |
| 多重继承         | 不支持，但接口中实现了此功能                                 | 支持，且可能会有菱形继承                     |
| 构造器的继承     | 缺省为 super();                                              | 缺省合法，也可以使用初始化列表               |
| 派生类中访问基类 | 关键字：super                                                | 使用作用域 -》类名::                         |
| 多态             | Base b = new Derive();                                       | Base  *p = new Derive();                     |
| 抽象函数         | 一般将覆盖关系的基类成员方法都声明为抽象类                   | 纯虚函数                                     |
| 抽象类           | 关键字：abstract，仅仅为了继承用，本省就不需要也无法构造对象了，详见3.3 | 抽象类                                       |
| 接口             |                                                              | 无                                           |



### 2.  面向对象基本内容

#### 2.1 格式

- 属性：静态描述类的特征：权限修饰符 [特征修饰符] 数据类型 属性名字 [=值]

- 方法：静态描述类的行为：权限修饰符 [特征修饰符] 返回值类型 方法的名字（参数列表） 【抛出异常】 方法执行体

#### 2.2 构造器

- 格式：权限修饰符 与类名一样的方法名([参数列表]) 【抛出异常】 方法执行体

- 作用：用来创建当前类的对象

- 构造和写法：没有返回类型 有返回值

- 调用：通过new关键字

- 特点：每一个类都有默认无参数的构造方法，我们定义的和默认的构成了重载

#### 2.3 程序块(代码块)

- 可以理解是一个非常特殊的方法

- 没有修饰符 没有返回类型 没有名字 没有参数

- 只有一个方法的执行体{}

- 程序块是在每一次调用构造方法之前都默认执行

- 程序块没有重载，却存在多个，按照创建的顺序逐一运行

#### 2.4 this关键字的使用

- 指代词，代替的是调用属性/方法时的当前对象

- this既然时一个对象，可以调用一般的属性，可以调用一般的方法，放置在类成员的任何位置(四个成员都可以)

- this也可以调用构造器：this(); 但是此句只能放到第一行，不然就无法识别两个对象了？

- 用途：解决构造函数中属性和传参重名的问题，而C++中就没有解决这个。



### 3. 继承

#### 3.1 Base b = new Derive();

1. b有实无名，即"名"(类型)是基类，"实"(内存)是子类。
2. 那么如果子类的方法或字段中存在public，b能不能访问子类的变量呢？答案是不能，即使实际存在，不知道叫什么，自然也无法访问。
3. "有实"却无法访问，那有没有"实"又有什么用呢？答案覆盖（基类和子类的同函数名、同参数、兼容返回值的成员方法）和多态（表现为同一个入口却调用不同的方法）。
4. 编译时，对象在调用成员方法时，编译器会查看对象的内存，调用覆盖的子类方法，当然非覆盖方法它还是调用不了，就和（3）一样。
5. 如果子类中有一方法继承自基类，同时它还调用了非继承的方法，那么这是不是就间接钻篓了？答案是允许合法。
6. 通过（5）可以访问到非继承的，但还可以通过强制类型转换访问，如 ((Derive)b).方法。

#### 3.2 final

表示不允许被继承，默认/不写认为是可以继承

#### 3.3 abstract：

- 含义：抽象，基类中不需要实现的可以声明为抽象方法，类中只要有一个抽象方法，类就也需要加abstract，认为是抽象类，C++中叫纯虚函数和抽象类。

- 一般将覆盖关系的基类成员方法都声明为抽象类。

- 抽象类不能构造对象：Base()，它构造对象以经没有意义了，只是作为一个抽象的祖先概念，构造出的实体对象对现今而言没有意义。所以，一般它只用在多态中：Base b = Deriver()；



### 4.接口

- 构造器：接口没有构造器，不能定义对象，有些像式抽象类，没有全部实现抽象方法就是抽象类
- 一旦实现一个接口，那么实现类要实现接口的所有抽象方法。
- 访问接口中的常量
- 继承与实现
  - 继承：手机 extends 照相机 “is-a” 手机是一个照相机
  - 实现：手机 implements 拍摄功能“has-a” 手机具有拍摄功能
- 多态的应用场合：

  - 接口当作方法的形参，传入具体实现类的对象

  - 接口作为方法的返回值，返回的是具体实现类的对象
- 在JDK1.8之前，接口只有两部分内容
  - 常量：固定的修饰符：public static final
  - 抽象方法：固定修饰符：public abstract
  - 问：为什么JDK1.8之后接口中加入非抽象方法？ 答：因为如果修改接口，对实现接口的类们影响太大。



### 5.static

- static修饰变量
  - 在类加载的时候一起加载入方法区中的静态域中
  - 先于对象存在
  - 访问方式： 对象名.属性名    类名.属性名（推荐）

- static修饰方法
  - 静态方法不能访问非静态成员，因为静态式属于全局，不属于所在类中，所以没有this指针
  - 静态方法访问静态变量：类名.属性名（推荐），如果是类内可以直省略类名



### 6.代码块

代码块分类：普通块，构造块，静态块，同步块（多线程）

代码块执行顺序：

- 最先执行静态块，只在类加载的时候执行一次，所以一般以后实战写项目：创建工厂，数据库的初始化信息都放入静态块。
  一般用于执行一些**全局性的初始化操作**。

- 再执行构造块，（不常用）
- 再执行构造器，
- 再执行方法中的普通块。



### 7.Object

- toString()：idea可以直接生成

- equals(Object o)：idea可以直接生成，可用用到运算符 instanceof， if(a instanceof b)，判断a是否是b的实例化

  

### 8.内部类

内部类：成员内部类 (静态的，非静态的) 和  局部内部类（位置：方法内，块内，构造器内）

**局部内部类**

- 作用：如果类B在整个项目中只使用一次，那么就没有必要单独创建一个B类，使用内部类就可以了
- 在局部内部类中访问到的变量必须是被final修饰的
- 局部内部类 -> 匿名内部类 -> lambad表达式

```java
public class TestOuter {
    //1.在局部内部类中访问到的变量必须是被final修饰的
    public void method(){
        final int num = 10;
        class A{
            public void a(){
                //num = 20;
                System.out.println(num);
            }
        }
    }
    //2.如果类B在整个项目中只使用一次，那么就没有必要单独创建一个B类，使用内部类就可以了
    public Comparable method2(){
        class B implements Comparable{
            @Override
            public int compareTo(Object o) {
                return 100;
            }
        }
        return new B();
    }
    public Comparable method3(){
        //3.匿名内部类
        return new Comparable(){
            @Override
            public int compareTo(Object o) {
                return 200;
            }
        };
    }
    public void teat(){
        Comparable com = new Comparable(){
            @Override
            public int compareTo(Object o) {
                return 200;
            }
        };
        System.out.println(com.compareTo("abc"));
    }
}
```



### 9.类与类的关系

对于继承、实现这两种关系没多少疑问，它们体现的是一种类和类、或者类与接口间的纵向关系。其他的四种关系体现的是类和类、或者类与接口间的引用、横向关系，是比较难区分的，有很多事物间的关系要想准确定位是很难的。前面也提到，这四种关系都是**语义级别**的，所以从代码层面并不能完全区分各种关系，但总的来说，后几种关系所表现的强弱程度依次为：**组合>聚合>关联>依赖**





# 第五阶段：javaSE进阶

### 1. 异常

#### 1.1 关键字

- try：捕获

- catch：处理

  - 处理方式

    ```java
    //1.什么都不做
    
    //2.为用户打印信息
    System.out.println("出现异常");
    
    //3.为程序员打印详细信息
    System.out.println(e);
    System.out.println(e.getMessage());
    e.printStackTrace();
    
    //4.抛出
    throw e;
    ```

  - 仍会导致中断
    - throw抛出异常的情况
    - catch中没有正常的进行异常捕获
    - 在try中遇到return
  - catah使用
    - try中出现异常以后，将异常类型跟catch后面的类型依次比较，按照代码的顺序进行比对，执行第一个与异常类型匹配的catch语句
    - 一旦执行其中一条catch语句之后，后面的catch语句就会被忽略了！
    - 在安排catch语句的顺序的时候，一般会将特殊异常放在前面（并列），一般化的异常放在后面。
      先写子类异常，再写父类异常。

  - 使用总结

    ```java
    try{
        Scanner sc = new Scanner(System.in);
        System.out.println("请录入第一个数：");
        int num1 = sc.nextInt();
        System.out.println("请录入第二个数：");
        int num2 = sc.nextInt();
        System.out.println("商："+num1/num2);
    }catch(ArithmeticException ex){
        System.out.println("对不起，除数不可以为0");
    }catch(InputMismatchException ex){
        System.out.println("对不起，你录入的数据不是int类型的数据");
    }catch(Exception ex){
        System.out.println("对不起，你的程序出现异常");
    }finally {
        System.out.println("----谢谢你使用计算器111");
    }
    ```

- throw：抛出

- throws：

- finally：一定执行

  - 用途：关闭数据库资源，关闭IO流资源，关闭socket资源。
  - 优先于return等
  - 只用 System.exit(0);//终止当前的虚拟机执行 会导致finally不执行

#### 1.2 throw和throws的区别

- 位置不同：
  throw：方法内部
  throws: 方法的签名处，方法的声明处

- 内容不同：
  throw+异常对象（检查异常，运行时异常）
  throws+异常的类型（可以多个类型，用，拼接）

- 作用不同：
  throw：异常出现的源头，制造异常。
  throws:在方法的声明处，告诉方法的调用者，这个方法中可能会出现我声明的这些异常。然后调用者对这个异常进行处理：
  要么自己处理要么再继续向外抛出异常

#### 1.3 分类

- object.Throwable
  - Error：JCM虚拟机错误
    - 栈溢出
  - Exception
    - 检查异常：可以预见的，系统会画横线提醒，必须要解决，一般通过try-catch解决
    - 运行时异常：RuntimeException：相反，一般就throw

#### 1.4 处理异常

- 检查异常：系统可以预见的，系统会画横线提醒，必须要解决
  - 因为可以预见所以必须解决，不解决启动不了
  - 一般用try-catch解决，因为知道异常类型，所以就地解决
  - 也可以用throw-throws，由于必须解决，所以会一层一层向上抛出。用途是抛到最后一起解决，但最后还是要try-catch，不然最后抛出main方法后，抛给虚拟机执行默认解决方法—printStackTrace()。
- 运行时异常：系统只有在运行后才知道，程序员难以完全预见的，不是必须解决的
  - 可以说都是未知异常
  - 一般用throw抛出，因为不是必须解决的，所以不用throws
  - 也可以使用try-catch解决，但实际中大部分运行场景都是无法预料的，所以难以使用

- 自定义异常：自定义异常不是第三类异常，它需要继承Exception或RuntimeException，为业务而生
  - 继承Exception：自定义异常都是可以遇见的，按理说继承Exception比较好，但这么做，系统会充斥这大量的try-catch，逻辑混乱且没有代码复用性。
  - 继承RuntimeException：只需要throw即可，但是自定义异常都是需要处理的，不然把它定义出来干什么。
  - 解决方法：自定义异常继承RuntimeException，自定义异常继承实现框架异常处理接口，如此实现了统一处理，并且此方法也会同时处理RuntimeException。



### 2. 常用类

#### 2.1 基础数据类型对象

| 源 src  | 目标 tag | 方法1                             | 方法2                     |
| ------- | -------- | --------------------------------- | ------------------------- |
| int     | Integer  | 自动包装                          |                           |
| Integer | int      | 自动解包 Integer tag= 100;        | tag =  src.intValue();    |
| int     | String   | String tag = 100;                 | String tag = 100+'0';     |
| String  | int      | tag = Integer.parseInt(src, pos); |                           |
| Integer | String   | tag = src.toString()              | tag = String.valueOf(src) |
| String  | Integer  | tag = Integer.valueOf(src);       |                           |

#### 2.2 date 

|              | Date                                                         | LocalData                                                    |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 创建         | Date date = new Date();                                      | LocalDateTime localDateTime = LocalDateTime.now();           |
| get          | 无                                                           | localDateTime.getYear();                                     |
| set          | 无                                                           | LocalDateTime newLocalDateTime = localDateTime.withMonth(8);原有的不改变，也不能改变，新创建出来的是改变后的 |
| DateFormat   | DateFormat df = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss")； | DateTimeFormatter df = DateTimeFormatter.ofPattern("yyyy-MM-dd hh:mm:ss"); |
| stringToDate | Date d = df.parse("2019-4-6 12:23:54")                       | TemporalAccessor parse2 = df3.parse("2020-06-15 03:22:03");  |
| DateToString | String format = df.format(new Date());                       | String format = df3.format(LocalDateTime.now());             |
|              |                                                              |                                                              |

#### 2.3 math

math类属于java.lang，不用导入

```java
public class Test01 {
    //这是一个main方法，是程序的入口：
    public static void main(String[] args) {
        System.out.println("取大的那个值："+Math.max(3, 6));
        System.out.println("取小的那个值："+Math.min(3, 6));
    }
}
```

静态导入：静态方法可以直接调用，但是重名危险很大

```java
//静态导入：
import static java.lang.Math.*;
public class Test01 {
    public static void main(String[] args) {
        System.out.println("取大的那个值："+max(3, 6));
        System.out.println("取小的那个值："+min(3, 6));
    }
}
```

#### 2.4 随机数 Random

用无参构造器就行，不用种种子

```java
//（2）利用空参构造器创建对象：
Random r2 = new Random();//表面是在调用无参数构造器，实际底层还是调用了带参构造器
System.out.println(r2.nextInt(10));//在 0（包括）和指定值（不包括）之间均匀分布的 int 值。
System.out.println(r2.nextDouble());//在 0.0 和 1.0 之间均匀分布的 double 值。
```

### 3. 注解：Annotation

#### 3.1 注解

注解：jdk5.0后加入

注解：junit、文档注解、文档注解、jdk内置注解、

框架 = 注解 + 反射 + 设计模式

#### 3.2 文档注解：

![文档注解](马士兵\文档注解.png)

文档注解用于生成API文档，生成方法见老师笔记

#### 3.3 jdk内置注解

- @Override: 限定重写父类方法，该注解只能用于方法
- @Deprecated: 用于表示所修饰的元素(类,方法，构造器，属性等)已过时。通常是因为所修饰的结构危险或存在更好的选择
- @SuppressWarnings: 抑制编译器警告

#### 3.4 元注解

注解用于修饰类变量方法等，而元注解用于修饰注解

- @Rentention：指定修饰的那个注解的生命周期

  - 源码有效：RetentionPolicy.SOURCE 

  - 编译有效：RetentionPolicy.CLASS

  - 运行有效：RetentionPolicy.RUNTIME

- @Target：指定被修饰的注解能用于修饰哪些程序元素

  - 类
  - 成员变量
  - 成员方法
  - 构造器

- @Documented：和文档注解一样用于API文档

  Documented注解修饰了Deprecated注解，那么Deprecated注解就会在javadoc提取的时候，提取到API中

- @Inherited：被它修饰的Annotation将具有继承性

  ```java
  //注解
  @Inherited //元注解
  public @interface MyAnnotation {
      String[] value();
  }
  
  //父类，注解MyAnnotation
  @MyAnnotation({"ab", "cd"})
  public class Person {
  }
  
  //由于元注解@Inherited的作用，子类也会继承MyAnnotation
  public class Person extends student{
  }
  ```



### 4.测试：Junit

#### 4.1 Junit

| JUnit       | 导包                                                         | 注意 |
| ----------- | ------------------------------------------------------------ | ---- |
| java        | 没有使用maven，但还是maven导的包，估计idea本身集成maven。不用手动导包，也不用加依赖，idea完成的 |      |
| spring      | maven                                                        |      |
| springboot  | maven                                                        |      |
| servicecomb | maven                                                        |      |

#### 4.2 使用

- @Test   ->  import org.junit.Test; 
- Assert.assertEquals(期望值, 测试值);  ->  import org.junit.Assert;    

### 5. 反射：Reflect

#### 5.1 类

- 类 

  包 注解 类 继承父类 实现接口 { 构造器  属性  方法 }

  Package Annotation class Superclass Interface  {Constructor  Field  Method}

  - 构造器
  
    构造器{参数1，参数2}
  
    Constructor {int.class,  double.class}
  
  - 属性
  
    属性：修饰符 类型  名字
  
    Field ：modifier  type  name
  
  - 方法
  
    方法：注解 修饰符 类型  名字（参数1，参数2）异常{}
  
    Method：Annotation modifier  Return name  Exception {Parameter} 

#### 5.2 使用

```java
Class cls = Student.class;
//获取对象
Object o = cls.newInstance();
//获取参数并赋值
Field score = cls.getField("score");
score.set(o, 20);
//获取方法并调用
Method myMethod = cls.getMethod("myMethod");
myMethod.invoke(o，12);
```



## 第十五阶段：

### 1. JVM

- java  ->  java 8.0

- jdk -> 1.8
- jre

- jvm ：java虚拟机，跨语言跨平台
  - 相当于操作系统，执行class文件，只要是能编译成class文件的都可以在jvm上运行
  - 是一种规范 HotSpot是主流用的，由oracle提供，8.0后估计要收费



### 2. class文件

- 打开：

  - 2进制文件

  - 普通文本文件打开时乱码的，需要专门的2进制文件查看器才能查看，比如 ：

    idea的插件：BinED

    notepag的插件：HEX-Editor
  
  - 反编译：idea生成的out目录下的文件就是2进制文件，但双击打开并不是直接打开，二十反编译再打开
  
    常用的反编译软件还有，idea...





































