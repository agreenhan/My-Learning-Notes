# SpringBoot

# Xml和JavaConfig

## 什么是JavaConfig

-  是 Spring 提供的使用 java 类配置容器。 配置 Spring IOC 容器的纯 Java 方法。
- 优点
  1. 可以使用面像对象的方式， 一个配置类可以继承配置类，可以重写方法
  2. 避免繁琐的 xml 配置

## Xml配置容器

- 在spring的配置文件中配置bean

## JavaConfig

- 主要使用的注解

  - @Configuration：放在类的上面， 这个类相当于 xml 配置文件，可以在其中声明 bean
  - @Bean：放在方法的上面， 方法的返回值是对象类型， 这个对象注入到 spring ioc 容器

- 实例代码

  ```java
  /**
   * @Configuration：表示当前类是 xml 配置文件的作用 * 在这个类中有很多方法， 方法的返回值是对象。
   * 在这个方法的上面加入@Bean， 表示方法返回值的对象放入到容器中。
   * @Bean 相当于 <bean></bean>
   */
  @Configuration
  public class JavaConfigTest {
      /**
       * 定义方法， 方法的返回值是对象。
       * @Bean：表示把对象注入到容器中。
       * 位置： 方法的上面
       * @Bean 没有使用属性，默认对象名称是方法名
       * name ：指定对象的名称
       */
      @Bean(name = "lisiStu")
      public Student formStudent(){
          Student student = new Student();
          student.setAge("20");
          student.setName("李四");
          student.setSno("02119");
          return student;
      }
  }
  ```

- 测试方法

  ```java
  @org.junit.Test
  public void test02(){
      ApplicationContext ctx = new AnnotationConfigApplicationContext(JavaConfigTest.class);
      Student stu = (Student) ctx.getBean("lisiStu");
      System.out.println(stu);
  }
  ```

## @ImportResource

- @ImportResource是导入xml配置，等同于xml文件的resources

- 使用方法：

  - 创建数据类型：

    ```
    public class Student {
        private String name;
        private String age;
        private String sno;
    }
    ```

  - 创建配置文件

    ```
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
        <bean id="myStu" class="com.mytest.Student">
            <property name="name" value="张三"></property>
            <property name="sno" value="1122233"></property>
            <property name="age" value="30"></property>
        </bean>
    </beans>
    ```

  - 创建配置类

    ```
    @Configuration
    @ImportResource(value = "classpath:beans.xml")
    public class ImportResourceTest {
    
    }
    ```

  - 创建测试方法

    ```
    @org.junit.Test
        public void test03(){
            ApplicationContext ctx = new AnnotationConfigApplicationContext(ImportResourceTest.class);
            Student stu = (Student) ctx.getBean("myStu");
            System.out.println(stu);
        }
    ```

## @PropertyResource

- 该注解用于读取属性配置文件

- 使用方法：

  - 属性配置文件（如果输入中文，记得修改编码格式）

    ```
    stu.name=王五
    stu.age=30
    stu.sno=0222
    ```

  - 创建数据类

    ```
    @Component("stu")
    public class Student {
        @Value("${stu.name}")
        private String name;
        @Value("${stu.age}")
        private String age;
        @Value("${stu.sno}")
        private String sno;
    }
    ```

  - 创建配置类

    ```
    @Configuration
    @PropertySource(value = "classpath:config.properties")
    @ComponentScan(value = "com.mytest")
    public class PropertiesResourceTest {
    }
    ```

  - 创建测试方法

    ```
    @org.junit.Test
    public void test04(){
        ApplicationContext ctx = new AnnotationConfigApplicationContext(PropertiesResourceTest.class);
        Student stu = (Student) ctx.getBean("stu");
        System.out.println(stu);
    }
    ```

# SpringBoot入门

## 创建SpringBoot项目

### 第一种方式

- https://start.spring.io

- 使用 spring boot 提供的初始化器向导的方式，完成 spring boot 项目的创建： 使用方便。

- 步骤：

  - 新建项目

    ![image-20220731093007911](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220731093007911.png)

  - 选择依赖

    ![image-20220731093136142](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220731093136142.png)

  - SpringBoot的项目结构：

    <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220731093306404.png" alt="image-20220731093306404" style="zoom:50%;" />	<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220731093325697.png" alt="image-20220731093325697" style="zoom:50%;" />

- 起步依赖

  - <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220731095101288.png" alt="image-20220731095101288" style="zoom:50%;" />

### 第二种方式

- 同第一种，使用的是国内地址（网速快）：https://start.springboot.io

### 第三种方式

- 使用maven向导创建项目

- 步骤

  - 创建一个普通的maven项目

  - 修改项目的目录

    ![image-20220731095357362](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220731095357362.png)

  - 添加 Spring Boot 依赖

    ```xml
    <dependencies>
     <dependency>
     <groupId>org.springframework.boot</groupId>
     <artifactId>spring-boot-starter-web</artifactId>
     </dependency>
     <dependency>
     <groupId>org.springframework.boot</groupId>
     <artifactId>spring-boot-starter-test</artifactId>
     <scope>test</scope>
     </dependency>
     </dependencies> <build>
     <plugins>
     <plugin>
     <groupId>org.springframework.boot</groupId>
     <artifactId>spring-boot-maven-plugin</artifactId>
     </plugin>
     </plugins>
    </build>
    ```

  - 创建启动类：加入@SpringBootApplication 注解

    ```java
    package com.bjpowernode;
    @SpringBootApplication
    public class MySpringBootMain {
     public static void main(String[] args) {
     SpringApplication.run(MySpringBootMain.class,args);
     } 
     }
    ```

## SpringBoot详解

### 重要注解

- @SpringBootApplication ： @SpringBootApplication 是 一 个 复 合 注 解 ， 是 由@SpringBootConfiguration， @EnableAutoConfiguration ，@ComponentScan 联合在一起组成的。

- @SpringBootConfiguration

  - 源码：

    ```java
    @Target({ElementType.TYPE})
    @Retention(RetentionPolicy.RUNTIME)
    @Documented
    @Configuration
    @Indexed
    public @interface SpringBootConfiguration {
        @AliasFor(
            annotation = Configuration.class
        )
        boolean proxyBeanMethods() default true;
    }
    ```

  - @SpringBootConfiguration ： 就 是 @Configuration 这 个 注 解 的 功 能 ， 使 用@SpringBootConfiguration 这个注解的类就是配置文件的作用。

- @EnableAutoConfiguration：开启自动配置， 把一些对象加入到 spring 容器中。

- @ComponentScan：组件扫描器， 扫描注解，根据注解的功能，创建 java bean，给属性赋值等等。组件扫描器默认扫描的是 @ComponentScan 注解所在的类， 类所在的包和子包。

### SpringBoot核心配置文件

- Spring Boot 的核心配置文件用于配置 Spring Boot 程序，名字必须以 application 开始

#### properties文件

- 默认使用该文件作为核心配置文件

- 使用示例：（在properties中写）

  ```properties
  #设置访问端口号
  server.port=9092
  #应用的 contextpath
  server.servlet.context-path=/boot
  ```

#### .yml文件

- yml 是一种 yaml 格式的配置文件，主要采用一定的空格、换行等格式排版进行配置。

- yaml 是一种直观的能够被计算机识别的的数据序列化格式，容易被人类阅读，yaml 类似于 xml，但是语法比 xml 简洁很多，值与前面的冒号配置项必须要有一个空格， yml 缀也可以使用 yaml 后缀

- 示例：

  ```yml
  server:
   port: 9091
   servlet:
     context-path: /boot
  ```

- 注意 ： 当两种格式配置文件同时存在 ，在 SpringBoot2.4 开始， 使用的是 yml 配置文件，修改配置名称都为 application，推荐使用yml格式配置文件。

#### 多环境配置

- 在实际开发的过程中，我们的项目会经历很多的阶段（开发->测试->上线），每个阶段的配置也会不同，例如：端口、上下文根、数据库等，那么这个时候为了方便在不同的环境之间切换，SpringBoot 提供了多环境配置，具体步骤如下项目名称：006-springboot-multi-environment为每个环境创建一个配置文件，命名必须以 application-环境标识.properties|yml

- 示例

  - ![image-20220731105254573](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220731105254573.png)

  - application.properties

    ```properties
    #指定使用的环境文件
    #spring.profiles.active=dev
    spring.profiles.active=test
    ```

  - application-dev.properties

    ```properties
    #开发环境的信息
    server.port=9091
    server.servlet.context-path=/mydev
    ```

  - application-product.properties

    ```properties
    server.port=9093
    server.servlet.context-path=/myprod
    ```

  - application-test.properties

    ```properties
    #开发环境的信息
    server.port=9091
    server.servlet.context-path=/mydev
    ```

#### SpringBoot自定义配置

- SpringBoot 的核心配置文件中，除了使用内置的配置项之外，我们还可以在自定义配置，然后采用如下注解去读取配置的属性值

##### @Value 注解

- @Value("${key}") ， key 来自 application.properties(yml)
- application.properties：添加两个自定义配置项 school.name 和school.website。在 IDEA 中可以看到这两个属性不能被 SpringBoot 识别，背景是桔色的

##### @ConfigurationProperties

- 将整个文件映射成一个对象，用于自定义配置项比较多的情况

- 示例

  - 在 com.bjpowernode.springboot.config 包下创建 SchoolInfo 类，并为该 类加上Component 和 ConfigurationProperties 注解，prefix 可以不指定，如果不指定，那么会去配置文件中寻找与该类的属性名一致的配置，prefix 的作用可以区分同名配置

  - 实体类

    ```java
    @ConfigurationProperties(prefix = "student")
    @Component
    public class StudentInfo {
        private String name;
        private String age;
        private String sno;
    }
    ```

  - properties

    ```properties
    student.name=赵六
    student.age=88
    student.sno=9090
    ```

  - 主类

    ```java
    @Controller
    public class HelloSpringBoot {
        @Resource
        private StudentInfo info;
        
        @RequestMapping("/info")
        @ResponseBody
        public String infoSpringBoot(){
            return info.toString();
        }
    }
    ```

  

##### 警告解决

- 在 SchoolInfo 类中使用了 ConfigurationProperties 注解，IDEA 会出现一个警告，不影响程序的执行
- 点击 open documentnation 跳转到网页，在网页中提示需要加一个依赖，我们将这个依赖拷贝，粘贴到 pom.xml 文件中

##### 中文乱码

如果在 SpringBoot 核心配置文件中有中文信息，会出现乱码：

- 一般在配置文件中，不建议出现中文（注释除外）
- 如果有，可以先转化为 ASCII 码

##### 提示

- 如果是从别的地方拷贝的配置文件，空格一定要删干净

## 在SpringBoot中使用JSP

- SpringBoot不建议使用JSP，而是使用模板技术代替JSP

- 步骤

  1. 在pom.xml文件中配置相关依赖

     ```xml
     <!--引入 Spring Boot 内嵌的 Tomcat 对 JSP 的解析包，不加解析不了 jsp 页面-->
     <!--如果只是使用 JSP 页面，可以只添加该依赖-->
     <dependency>
      <groupId>org.apache.tomcat.embed</groupId>
      <artifactId>tomcat-embed-jasper</artifactId>
     </dependency>
     
     <!--如果要使用 servlet 必须添加该以下两个依赖-->
     <!-- servlet 依赖的 jar 包-->
     <dependency> 
     <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
     </dependency>
     
     <!-- jsp 依赖 jar 包-->
     <dependency>
      <groupId>javax.servlet.jsp</groupId>
      <artifactId>javax.servlet.jsp-api</artifactId>
      <version>2.3.1</version>
     </dependency>
     
     <!--如果使用 JSTL 必须添加该依赖-->
     <!--jstl 标签依赖的 jar 包 start-->
     <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>jstl</artifactId>
     </dependency>
     ```

  2. 创建一个存放JSP的目录，一般叫做Webapp

     - 需要将Webapp设置为web源文件

  3. 在pom.xml指定JSP文件编译后的存放目录

     - SpringBoot 要求 jsp 文件必须编译到指定的 META-INF/resources 目录下才能访问，否则访问不到。其实官方已经更建议使用模板技术

       ```xml
       <!—
        SpringBoot 要求 jsp 文件必须编译到指定的META-INF/resources 目录下才能访问，否则访问不到。其它官方已经建议使用模版技术
       -->
       <resources>
        	<resource>
        		<!--源文件位置-->
        		<directory>src/main/webapp</directory>
       		 <!--指定编译到META-INF/resource，该目录不能随便写-->
        		<targetPath>META-INF/resources</targetPath>
        		<!--指定要把哪些文件编译进去，**表示 webapp 目录及子目录，*.*表示所有文件-->
        		<includes>
        			<include>**/*.*</include>
        		</includes>	
        	</resource>
        </resources>
       ```

       

  4. 创建Controller，访问JSP

  5. 在application.properties文件中配置视图解析器

     ```
     spring.mvc.view.prefix=/
     spring.mvc.view.suffix=.jsp
     ```

## SpringBoot中使用ApplicationContext

- 在 main 方法中 SpringApplication.run()方法获取返回的 Spring 容器对象，再获取业务 bean进行调用。

  ```
  // 或者Application
  ConfigurationApplicationContext ctx = SpringApplication.run(Application.class, args);
  ctx.getBean("");
  ```

## CommandLineRunner接口

- 开发中可能会有这样的情景。需要在容器启动后执行一些内容。比如读取配置文件，数据库连接之类的。SpringBoot 给我们提供了两个接口来帮助我们实现这种需求。
- 这两个接口分别为 CommandLineRunner 和 ApplicationRunner。他们的执行时机为容器启动完成的时候
- 这两个接口中有一个 run 方法，我们只需要实现这个方法即可。
- 这两个接口的不同之处在于： 
  - ApplicationRunner中run方法的参数为 ApplicationArguments 
  - CommandLineRunner接口中 run 方法的参数为 String 数组

# SpringBoot和Web组件

## 拦截器

### SpringMVC拦截器

- 自定义拦截器

  1. 创建类实现SpringMVC 框架的HandlerInterceptor接口

  2. 在SpringMVC的配置文件中，声明拦截器

     ```xml
     <mvc:interceptors>
     	<mvc:interceptor>
             <mvc:path="url"/>
             <bean class="拦截器的全限定名称"/>
         </mvc:interceptor>
     </mvc:interceptors>
     ```

- 系统的拦截器

### SpringBoot使用拦截器

1. 创建类实现HandlerInterceptor接口

   ```java
   public class SpringBootInterceptor implements HandlerInterceptor {
       @Override
       public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
           System.out.println("拦截器启动了");
           return true;
       }
   }
   ```

2. 注册拦截器对象

   ```java
   @Configuration
   public class MyAppConfig implements WebMvcConfigurer {
   
       // 添加拦截器对象，注入到容器中
       @Override
       public void addInterceptors(InterceptorRegistry registry) {
           // 创建拦截器对象
           HandlerInterceptor interceptor = new SpringBootInterceptor();
           // 指定拦截的请求uri地址
           String[] path = {"/user/**"};
           // 指定不拦截的地址
           String[] excludePath = {"/user/login"};
           // 配置拦截器
           registry.addInterceptor(interceptor).addPathPatterns(path).excludePathPatterns(excludePath);
       }
   }
   ```

3. 测试拦截器

   ```java
   @Controller
   public class TestController {
       @RequestMapping("/user/test")
       @ResponseBody
       public String test(){
           return "test界面";
       }
   
       @RequestMapping("/user/login")
       @ResponseBody
       public String login(){
           return "login界面";
       }
   }
   ```

## Servlet

- ServletRegistrationBean 用来做在 servlet 3.0+容器中注册 servlet 的功能，但更具有SpringBean 友好性。

1. 创建servlet

   ```java
   public class MyServlet extends HttpServlet {
   
       @Override
       protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           doPost(req,resp);
       }
   
       @Override
       protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           // 使用HttpServletResponse输出数据，应答结果
           resp.setContentType("text/html;charset=utf-8");
           PrintWriter out = resp.getWriter();
           out.println("执行Post方法");
           out.flush();
           out.close();
       }
   }
   ```

2. 注册servlet

   ```java
   @Configuration
   public class ServletConfig {
       // 定义方法，注册Servlet对象
       @Bean
       public ServletRegistrationBean servletRegistrationBean(){
           ServletRegistrationBean bean = new ServletRegistrationBean(new MyServlet(), "/myServlet");
           /*
               第二种方法:
               ServletRegistrationBean bean = new ServletRegistrationBean();
               bean.setServlet(new MyServlet());
               bean.setUrlMappings("/myServlet");
           */
           return bean;
       }
   }
   ```

## Filter过滤器

- FilterRegistrationBean 用来注册 Filter 对象
- Filter是Servlet规范中的过滤器，可以处理请求，对请求的参数、属性进行调整，常常在过滤器中处理字符编码

1. 创建Filter对象

   ```java
   public class MyFilter implements Filter {
       @Override
       public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
           System.out.println("使用了Servlet中的Filter对象");
           filterChain.doFilter(servletRequest,servletResponse);
       }
   }
   ```

2. 注册Filter对象

   ```java
   @Configuration
   public class FilterConfig {
       @Bean
       public FilterRegistrationBean filterRegistrationBean(){
           FilterRegistrationBean bean = new FilterRegistrationBean();
           // 使用哪个过滤器
           bean.setFilter(new MyFilter());
           // 过滤器的地址
           bean.addUrlPatterns("/user/*");
           return bean;
       }
   }
   ```

## 字符集过滤器

### 非配置文件方法

1. 创建servlet，输出中文数据

   ```java
   public class MyServlet extends HttpServlet {
   
       @Override
       protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           doPost(req,resp);
       }
   
       @Override
       protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           // 使用HttpServletResponse输出数据，应答结果
           resp.setContentType("text/html");
           PrintWriter out = resp.getWriter();
           out.println("执行Post方法");
           out.flush();
           out.close();
       }
   }
   ```

2. 注册servlet和filter

   ```java
   @Configuration
   public class FilterConfig {
       @Bean
       public FilterRegistrationBean filterRegistrationBean(){
           FilterRegistrationBean bean = new FilterRegistrationBean();
           // 创建过滤器CharacterEncodingFilter
           CharacterEncodingFilter filter = new CharacterEncodingFilter();
           // 设置编码
           filter.setEncoding("utf-8");
           // 强制request和response使用encoding的编码
           filter.setForceEncoding(true);
           // 使用哪个过滤器
           bean.setFilter(filter);
           // 过滤器的地址
           bean.addUrlPatterns("/*");
           return bean;
       }
   
       // 定义方法，注册Servlet对象
       @Bean
       public ServletRegistrationBean servletRegistrationBean(){
           ServletRegistrationBean bean = new ServletRegistrationBean(new MyServlet(), "/myServlet");
           return bean;
       }
   }
   
   ```

3. 在 application.properties , 禁用 Spring Boot 中默认启用的过滤器

   ```properties
   # SpringBoot中默认配置了CharacterEncodingFilter，编码默认为ISO-8858-1
   # 设置enabled为false，作用是关闭SpringBoot配置好的过滤器，使用自定义的
   server.servlet.encoding.enabled=false
   ```

### 配置文件方法

- Spring Boot 项目默认启用了 CharacterEncodingFilter, 设置他的属性就可以

  ```properties
  #设置 spring boot 中 CharacterEncodingFitler 的属性值
  server.servlet.encoding.enabled=true 
  #强制 request， response 使用 charset 他的值 utf-8
  server.servlet.encoding.charset=utf-8 
  server.servlet.encoding.force=true
  ```



# ORM操作MySQL

## 使用步骤

1. mybatis起步依赖：完成MyBatis对象自动配置，对象放容器中

2. pom.xml指定把src/main/java目录中的xml文件包含到classpath中

   ```java
   <build>
           <!-- resources插件 -->
           <resources>
               <resource>
                   <directory>src/main/java</directory>
                   <includes>
                       <include>**/*.xml</include>
                   </includes>
               </resource>
           </resources>
   </build>
   ```

3. 创建实体类Student(Setting、Getting、toString)

   ```java
   public class Student {
       private Integer id;
       private String name;
       private Integer age;
   }
   ```

4. 创建Dao接口StudentDao，创建一个查询学生的方法

   ```java
   @Mapper
   public interface StudentDao {
       Student selectById(@Param("stuId") Integer id);
   }
   ```

5. 创建Dao接口对应的Mapper文件、xml文件，写SQL语句

   ```java
   <?xml version="1.0" encoding="UTF-8" ?> <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="com.example.dao.StudentDao">
       <select id="selectById" parameterType="int" resultType="com.example.model.Student">
           select id, name, age from student where id=#{stuId}
       </select>
   </mapper>
   ```

6. 创建Service层对象，创建StudentService接口及其实现类，调用Dao层方法，完成对数据库的操作

   ```java
   public interface StudentService {
       Student queryStudent(Integer id);
   }
   
   @Service
   public class StudentServiceImpl implements StudentService {
       @Resource
       private StudentDao studentDao;
   
       public StudentServiceImpl() {
       }
   
       public Student queryStudent(Integer id) {
           return this.studentDao.selectById(id);
       }
   }
   ```

7. 创建Controller层对象，访问Service层

   ```java
   @Controller
   public class StudentController {
       @Resource
       private StudentService studentService;
   
       public StudentController() {
       }
   
       @RequestMapping({"/student/query"})
       @ResponseBody
       public String queryStudent(Integer id) {
           return this.studentService.queryStudent(id).toString();
       }
   }
   ```

8. 写application.properties文件配置数据库的连接信息

   ```properties
   # 连接数据库
   spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
   spring.datasource.url=jdbc:mysql://localhost:3306/SpringBootdb?useUnicode=true&characterEncoding=UTF-8&serverTimezone=GMT%2B8
   spring.datasource.username=root
   spring.datasource.password=******
   ```

## 第一种方式：@Mapper

- 放在Dao接口的上面，每个接口都需要使用这个注解。

```
@Mapper
public interface StudentDao{

}
```

## 第二种方式：@MapperScan

```
 主类上添加注解包扫描：@MapperScan("包名")
```

## 第三种方式：分开管理

- Mapper文件和java代码分开管理，mapper文件放在resources目录下，java代码放在src/main/java

- 实现步骤：

  1. 在resources创建自定义目录，例如mapper，存放xml文件

  2. 在application.properties配置文件中指定映射文件的位置，这个配置只有接口和映射文件不在同一个包的情况下，才需要指定。

     ```properties
     #指定 mybatis 的配置， 相当于 mybatis 主配置文件的作用
     mybatis:
      mapper-locations: classpath:mapper/*.xml
      configuration:
      log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
     ```

  3. 注意需要配置pom.xml文件的resources标签，使resources文件下的所有文件都被编译

     ```xml
     <resources>
                 <resource>
                     <directory>src/main/resources</directory>
                     <includes>
                         <include>**/*.*</include>
                     </includes>
                 </resource>
     </resources>
     ```

  4. 配置mybatis的日志

     ```properties
     mybatis.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl
     ```

## 事务支持

- Spring Boot 使用事务非常简单，底层依然采用的是 Spring 本身提供的事务管理
- Spring中的事务
  1. 管理事务的对象：事务管理器（接口，接口有很多的实现类）
     - 使用mybatis或JDBC访问数据库，使用的事务管理器：DataSourceTransactionManager
  2. 声明式事务：在xml配置文件或者使用注解说明事务控制的内容
     - 控制事务：隔离级别、传播行为、超时时间
  3. 事务处理方式：
     - Spring框架中的@Transactional
     - aspectj框架可以在xml配置文件中，声明事务控制的内容
- SpringBoot中的事务：上的两种方式都能使用
  1. 在业务方法的上面加入@Transactional，加入注解后，方法就有事务功能了
  2. 明确的在主启动类的上面，加入@EnableTransactionManager
- 注意：
  1. 在 Application 主类上，@EnableTransactionManagement 开启事务支持@EnableTransactionManagement 可选，但是@Service 必须添加事务才生效
  2. @Transactional：表示方法的事务支持
     - 默认：使用库的隔离级别；传播行为：REQUIRED；超时时间：-1

## Mybatis逆向工程

1. 在pom.xml中添加插件

   ```xml
   			<!--mybatis代码自动生成插件-->
               <plugin>
                   <groupId>org.mybatis.generator</groupId>
                   <artifactId>mybatis-generator-maven-plugin</artifactId>
                   <version>1.3.6</version>
                   <configuration>
                       <!--配置文件的位置-->
                       <configurationFile>GeneratorMapper.xml</configurationFile>
                       <verbose>true</verbose>
                       <overwrite>true</overwrite>
                   </configuration>
               </plugin>
   ```

2. 写GeneratorMapper.xml配置文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE generatorConfiguration
           PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
           "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
   
   <generatorConfiguration>
   
       <!-- 指定连接数据库的JDBC驱动包所在位置，指定到你本机的完整路径 -->
       <classPathEntry location="D:\mysql-jar\mysql-connector-java-8.0.28\mysql-connector-java-8.0.28.jar"/>
   
       <!-- 配置table表信息内容体，targetRuntime指定采用MyBatis3的版本 -->
       <context id="tables" targetRuntime="MyBatis3">
   
           <!-- 抑制生成注释，由于生成的注释都是英文的，可以不让它生成 -->
           <commentGenerator>
               <property name="suppressAllComments" value="true" />
           </commentGenerator>
   
           <!-- 配置数据库连接信息 -->
           <jdbcConnection driverClass="com.mysql.cj.jdbc.Driver"
                           connectionURL="jdbc:mysql://127.0.0.1:3306/SpringBootdb"
                           userId="root"
                           password="jht123456">
           </jdbcConnection>
   
           <!-- 生成model类，targetPackage指定model类的包名， targetProject指定生成的model放在eclipse的哪个工程下面-->
           <javaModelGenerator targetPackage="com.example.model"
                               targetProject="C:\Users\Administrator\IdeaProjects\SpringBootMybatis-001\SpringBootMybatis-001\src\main\java">
               <property name="enableSubPackages" value="false" />
               <property name="trimStrings" value="false" />
           </javaModelGenerator>
   
           <!-- 生成MyBatis的Mapper.xml文件，targetPackage指定mapper.xml文件的包名， targetProject指定生成的mapper.xml放在eclipse的哪个工程下面 -->
           <sqlMapGenerator targetPackage="mapper" targetProject="src/main/resources">
               <property name="enableSubPackages" value="false" />
           </sqlMapGenerator>
   
           <!-- 生成MyBatis的Mapper接口类文件,targetPackage指定Mapper接口类的包名， targetProject指定生成的Mapper接口放在eclipse的哪个工程下面 -->
           <javaClientGenerator type="XMLMAPPER" targetPackage="com.example.dao" targetProject="src/main/java">
               <property name="enableSubPackages" value="false" />
           </javaClientGenerator>
   
           <!-- 数据库表名及对应的Java模型类名 -->
           <table tableName="student" domainObjectName="Student"
                  enableCountByExample="false"
                  enableUpdateByExample="false"
                  enableDeleteByExample="false"
                  enableSelectByExample="false"
                  selectByExampleQueryId="false"/>
       </context>
   
   </generatorConfiguration>
   ```

3. 在maven中运行插件

# 接口架构风格RESTful

## 认识REST

- **REST（英文：Representational State Transfer，简称 REST，中文为表现层状态转移）**

- 一种互联网软件架构设计的风格，但它并不是标准，它只是提出了一组客户端和服务器交互时的架构理念和设计原则，基于这种理念和原则设计的接口可以更简洁，更有层次，REST这个词，是 Roy Thomas Fielding 在他 2000 年的博士论文中提出的。

- 任何的技术都可以实现这种理念，如果一个架构符合 REST 原则，就称它为 RESTFul 架构
  比如我们要访问一个 http 接口：http://localhost:8080/boot/order?id=1021&status=1
  采用 RESTFul 风格则 http 地址为：http://localhost:8080/boot/order/1021/1

- 使用http中的动作（请求方式），表示对资源的操作（CURD）

  - GET：查询资源

    ```
    处理单个资源：
    http://localhost:8080/boot/order/1021/1
    处理多个资源：
    http://localhost:8080/boot/orders/1021/1002
    ```

  - POST：创建资源

    ```
    URL：http://localhost:8080/boot/order
    在post请求中传递数据（可以通过form表单）:
    <form action="http://localhost:8080/boot/order" method="post">
    	订单：<input type="text" name="name" />
    	数量：<input type="text" name="num"/>
    </from>
    ```

  - PUT：更新资源

    ```
    URL：http://localhost:8080/boot/order/1021
    在post请求中传递数据（可以通过form表单）:
    <form action="http://localhost:8080/boot/order/1" method="post">
    	订单：<input type="text" name="name" />
    	数量：<input type="text" name="num"/>
    		 <input type="hidden" name="_method" value="PUT"/>
    </from>
    ```

  - DELETE：删除资源

    ```
    <a href="http://localhost:8080/myboot/student/1">删除1的数据</a>
    ```

## RESTful的注解

### @PathVariable

- 作用：获取url中的数据

- 示例：当路径变量名和形参名一致时，@PathVariable的value可以省略

  ```java
  @RestController
  public class MyRestController {
      @GetMapping("/student/{studentId}")
      public String queryStudent(
          @PathVariable(value = "studentId") 
          Integer stuId){
          return "查询信息为：" + stuId;
      }
  }
  ```

### @PostMapping

- 作用：支持Post请求的方式，等同于@RequestMapping(method=RequestMethod.POST)

### @DeleteMapping

- 作用：支持Delete请求方式，等同于@RequestMapping(method=RequestMethod.DELETE)

### @PutMapping

- 作用：支持Put请求方式，等同于@RequestMapping(method=RequestMethod.PUT)

### @GetMapping

- 作用：支持Get请求方式，等同于@RequestMapping(method=RequestMethod.GET)	

### @RestController：

- 作用：符合注解，是@Controller和@ResponseBody组合
- 在类的上面使用@RestController，表示当前类者的所有方法都加入了@ResponseBody

## RESTful优点

- 轻量，直接基于 http，不再需要任何别的诸如消息协议get/post/put/delete 为 CRUD 操作
- 面向资源，一目了然，具有自解释性。
- 数据描述简单，一般以 xml，json 做数据交换。
- 无状态，在调用一个接口（访问、操作资源）的时候，可以不用考虑上下文，不用考虑当前状态，极大的降低了复杂度。
- 简单、低耦合

## Postman工具

- 用于测试POST、GET、DELETE等请求

## 请求路径冲突

- 下述路径访问会失败， 路径有冲突。

  ```
  @GetMapping(value = "/student/{studentId}/{classname}")
  @GetMapping(value = "/student/{studentId}/{schoolname}")
  ```

- 解决：设计路径，必须唯一， 路径 uri 和 请求方式必须唯一。

## 在页面或者ajax中支持put、delete请求

- 在SpringMVC中有一个过滤器，支持post请求转为put和delete请求

- 过滤器：org.springframework.web.filter.HiddenHttpMethodFilter

- 作用：把post请求转化为put、delete请求 

- SpringBoot使用方式

  1. application.yml(properties)：开启使用HiddenHttpMethodFilter过滤器

     ```properties
     # 启用支持put、delete
     spring.mvc.hiddenmethod.filter.enabled=true
     ```

  2. 在请求页面中，包含_method参数，值为put、delete，发起这个请求使用的post方式

     ```html
     <form action="student" method="post">
             <input type="hidden" name="_method" value="put"/>
             <input type="submit" value="测试请求方式"/>
     </form>
     ```

## 总结

- 增 post 请求、删 delete 请求、改 put 请求、查 get 请求

- 请求路径不要出现动词例如：

  查询订单接口/boot/order/1021/1（推荐）

  /boot/queryOrder/1021/1（不推荐）

- 分页、排序等操作，不需要使用斜杠传参数

  例如：订单列表接口

  /boot/orders?page=1&sort=desc

  一般传的参数不是数据库表的字段，可以不采用斜杠

# SpringBoot集成Redis

- Redis 是一个 NoSQL 数据库， 常作用缓存 Cache 使用。 通过 Redis 客户端可以使用多种语言在程序中，访问 Redis 数据。java 语言中使用的客户端库有 Jedis，lettuce, Redisson等。
- Spring Boot 中使用 RedisTemplate 模版类操作 Redis 数据。
- 示例实现功能：完善根据学生 id 查询学生的功能，先从 redis 缓存中查找，如果找不到，再从数据库中查找，然后放到 redis 缓存中

## 配置Windows版本的Redis

- 仅用于方便学习，项目中Rdies均运行于Linux系统
- Redis-x64-3.2.100.zip ：解压到非中文目录
- redis-server.exe：服务端，启动后，不能关闭
- redis-cli.exe：客户端，访问Redis数据
- redisclient-win32.x86_64.2.0.jar：Redis图形界面工具
  - 在该文件目录下cmd，执行java -jar redisclient-win32.x86_64.2.0.jar

## pom依赖

```
<!--redis 起步依赖，spring boot 会在容器中创建两个对象 RedisTemplate ,StringRedisTemplate--> <dependency>
 <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

- RedisTemplate使用的是lettuce客户端库
- 在程序中使用RedisTemplate类的方法操作Redis数据，实际上就是调用的lettuce客户端中的方法

## 配置application.properties(yml)

```properties
# 指定redis(host, ip, password)
spring.redis.host=localhost
spring.redis.port=6379
# spring.redis.password=123456
```

## 对比StringRedisTemplate和RedisTemplate

- StringRedisTemplate：把key-value都是作为String处理，使用的是String的序列化，可读性好

- RedisTemplate：把key-value经过了序列化存到redis，key、value是序列化的内容，不能直接识别，默认使用的是JDK的序列化，可以修改为其他的序列化

- 使用示例

  ```java
  @RestController
  public class RedisController {
  
      /**
       * 注入RedisTemplate
       * 三种写法：
       *      RedisTemplate
       *      RedisTemplate<String,String>
       *      RedisTemplate<Object,Object>
       *
       * 注意：RedisTemplate对象的名称redisTemplate，方便注入spring容器
       */
      @Resource
      private RedisTemplate redisTemplate;
  
      @Resource
      private StringRedisTemplate stringRedisTemplate;
  
      // 添加数据到redis
      @PostMapping("/redis/addString/{key}/{value}")
      public String addToRedis(@PathVariable String key,@PathVariable String value){
          // 操作String类型数据，先获取ValueOperations对象
          ValueOperations valueOperations = redisTemplate.opsForValue();
          valueOperations.set(key,value);
          return "向redis添加数据";
      }
  
      // 从redis获取数据
      @GetMapping("/redis/getData/{key}")
      public String getData(@PathVariable String key){
          // 操作String类型数据，先获取ValueOperations对象
          ValueOperations valueOperations = redisTemplate.opsForValue();
          Object o = valueOperations.get(key);
          return "向redis查数据:" + o.toString();
      }
  
      // 添加数据到redis
      @PostMapping("/redis/add/{key}/{value}")
      public String add(@PathVariable String key,
                        @PathVariable String value){
          ValueOperations valueOperations = stringRedisTemplate.opsForValue();
          valueOperations.set(key, value);
          return "添加成功";
      }
  
      // 从redis获取数据
      @GetMapping("/redis/get/{key}")
      public String get(@PathVariable String key){
          ValueOperations valueOperations = stringRedisTemplate.opsForValue();
          valueOperations.get(key);
          return key;
      }
  }
  ```

### Idea生成序列化版本号

- File->Settings->Editor->Code Style->Insepections其中的Serializable class without 'serialVersionUID'

### 序列化

- 序列化：把对象转化为可传输的字节序列过程称为序列化

- 反序列化：把字节序列还原为对象的过程称为反序列化

- 设置RedisTemplate的序列化，可以单独设置key，可以单独设置value，也可以key和value同时设置：

  - 设置序列化格式：字符串

    ```java
    // 设置序列化格式
        @PostMapping("/redis/set/{key}/{value}")
        public String set(@PathVariable String key,
                          @PathVariable String value){
            // 设置key的序列化格式
            redisTemplate.setKeySerializer(new StringRedisSerializer());
            // 设置value的序列化格式
            redisTemplate.setValueSerializer(new StringRedisSerializer());
            // 添加数据
            redisTemplate.opsForValue().set(key, value);
            return "设置序列化格式";
        }
    ```

  - 设置序列化格式：JSON

    ```java
        // json序列化
        @PostMapping("/redis/setjson/{name}/{age}")
        public String setJson(@PathVariable String name, @PathVariable Integer age){
            Student stu = new Student(1,name, age);
            redisTemplate.setKeySerializer(new StringRedisSerializer());
            redisTemplate.setValueSerializer(new Jackson2JsonRedisSerializer(Student.class));
            redisTemplate.opsForValue().set("stu",stu);
            return "插入成功";
        }
    
        // 反序列化
        @GetMapping("/redis/getjson")
        public String getJson(){
            redisTemplate.setKeySerializer(new StringRedisSerializer());
            redisTemplate.setValueSerializer(new Jackson2JsonRedisSerializer(Student.class));
            Object stu = redisTemplate.opsForValue().get("stu");
            return stu.toString();
        }
    ```

- 为什么需要序列化

  ```
  序列化最终的目的是为了对象可以跨平台存储和进行网络传输。然而进行跨平台存储和网络传输的方式就是IO，IO支持的数据格式就是字节数组，必须在把对象转成字节数组的时候就制定一种规则（序列化），那么从IO流里读出数据的时候再以这种规则把对象还原回来
  ```

- 什么情况下需要序列化

  ```
  本质上存储和网络传输都需要经过把一个对象状态保存成一种跨平台识别的字节个事，然后其他的平台才可以通过字节信息解析还原对象信息
  ```

- 序列化的方式

  ```
  序列化只是一种拆装组装对象的规则，那么这种规则肯定也多种多样，比如常见的序列化方式有：JDK(不支持跨语言),JSON,XML,Hessian,Kryo(不支持化语言),Thrift,Protostuff
  ```

- java的序列化：把java对象转化为byte[]，二进制数据

  ```
  json序列化：json序列化功能将对象转换为JSON格式或从JSON格式转换对象，例如把一个Student对象转换为JSON字符串，反序列化将JSON字符串转换为Student对象。
  ```

# SpringBoot集成Dubbo

- 阿里巴巴提供了 dubbo 集成 springBoot 开源项目，可以到 GitHub 上 https://github.com/apache/dubbo-spring-boot-project 查看入门教程

## 创建接口（公共包）

1. 新建普通java工程
2. 创建实体类并实现Serializable接口
3. 创建service接口

## 创建服务提供者

1. 新建SpringBoot工程

2. 在pom.xml文件中引入公共包、dubbo依赖、Zookeeper依赖

   ```xml
   <dependencies>
           <dependency>
            <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter</artifactId>
           </dependency>
   
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-test</artifactId>
               <scope>test</scope>
           </dependency>
           <!-- 公共项目的gav -->
           <dependency>
               <groupId>com.myspringboot</groupId>
               <artifactId>SpringBoot-learning-dubbo</artifactId>
               <version>1.0-SNAPSHOT</version>
           </dependency>
   
           <!-- dubbo依赖 -->
           <dependency>
               <groupId>org.apache.dubbo</groupId>
               <artifactId>dubbo-spring-boot-starter</artifactId>
               <version>2.7.8</version>
           </dependency>
   
           <!-- Zookeeper -->
           <dependency>
               <groupId>org.apache.dubbo</groupId>
               <artifactId>dubbo-dependencies-zookeeper</artifactId>
               <version>2.7.8</version>
               <type>pom</type>
               <!-- 如果报SLF4j 依赖重复导入错误，则添加下列代码，取消导入SLF4j,日志依赖 SLF4j 多次加入了。只需要依赖一次就可以 -->
   <!--            <exclusions>
                   <exclusion>
                       <groupId>org.slf4j</groupId>
                       <artifactId>slf4j-log4j12</artifactId>
                   </exclusion>
               </exclusions>-->
           </dependency>
       </dependencies>
   ```
   
3. 编写接口的实现类，重点是@DUbboService注解

   ```java
   @DubboService(interfaceClass = StudentService.class, version = "1.0.0", timeout = 5000)
   public class StudentServiceImpl implements StudentService {
       @Override
       public Student queryStudentById(Integer id) {
           return new Student(id, "测试名", 18);
       }
   }
   ```

4. 在主启动类上添加@EnableDubbo注解，用于将代理对象注入到spring容器

   ```java
   @SpringBootApplication
   @EnableDubbo
   public class SpringBootLearningDubboProviderApplication {
   
       public static void main(String[] args) {
           SpringApplication.run(SpringBootLearningDubboProviderApplication.class, args);
       }
   }
   ```

5. 配置application.properties文件

   ```xml
   # 服务名称,相当于<dubbo:application name=""/>
   spring.application.name=SpringBoot-learning-dubbo-provider
   
   # dubbo注解所在的包，@DubboService
   dubbo.scan.base-packages=com.myspringboot.springbootlearningdubboprovider.service
   
   # 配置dubbo协议
   dubbo.protocol.name=dubbo
   dubbo.protocol.port=20880
   
   # Zookeeper
   dubbo.registry.address=zookeeper://localhost:2181
   ```

## 创建服务消费者

1. 新建springboot工程，选择web依赖

2. 在pom.xml中添加dubbo、Zookeeper、公共包依赖，如上

3. 编写对应的controller，重点是@DubboReference注解

   ```java
   @RestController
   public class StudentController {
       // 引用远程服务，把创建好的代理对象，注入给studentService
       @DubboReference(interfaceClass = StudentService.class, version = "1.0.0")
       private StudentService studentService;
   
       @GetMapping("/query")
       public String queryStudentById(Integer id){
           return studentService.queryStudentById(id).toString();
       }
   }
   ```

4. 在主启动类上添加@EnableDubbo注解，用于将代理对象注入到spring容器

5. 配置application.properties

   ```xml
   # 服务名称
   spring.application.name=SpringBoot-learning-dubbo-consumer
   
   # Zookeeper
   dubbo.registry.address=zookeeper://localhost:2181
   ```

## 测试

1. 先启动Zookeeper
2. 运行服务提供者
3. 运行服务消费者
4. 打开浏览器测试

## SLF4j依赖

- 日志依赖 SLF4j 多次加入了。只需要依赖一次就可以。

- 解决方式：排除多余的 SLF4j 依赖, 提供者和消费者项目都需要这样做

  ```xml
  <dependency>
   	<groupId>org.apache.dubbo</groupId>
   	<artifactId>dubbo-dependencies-zookeeper</artifactId>
  	<version>2.7.8</version>
   	<type>pom</type>
  	<exclusions>
  	 	<exclusion>
  	 	<artifactId>slf4j-log4j12</artifactId>
  	 	<groupId>org.slf4j</groupId>
  	 	</exclusion>
  	</exclusions>
  </dependency>
  ```

# SpringBoot打包

- Spring Boot 可以打包为 war 或 jar 文件。 以两种方式发布应用

## SpringBoot打包为war

1. 创建Spring Boot web项目

2. 在pom.xml文件中配置内嵌Tomcat对jsp的解析包

   ```xml
   <dependency>
               <groupId>org.apache.tomcat.embed</groupId>
               <artifactId>tomcat-embed-jasper</artifactId>
   </dependency>
   ```

3. 指定jsp文件的编译目录

   ```xml
   <resources>
               <!-- resources插件，把jsp编译到指定目录 -->
               <resource>
                   <directory>src/main/webapp</directory>
                   <targetPath>META-INF/resources</targetPath>
                   <includes>
                       <include>**/*.*</include>
                   </includes>
               </resource>
               <!-- 如果使用了mybatis，而且mapper文件放在了src/main/java目录下 -->
               <resource>
                   <directory>src/main/java</directory>
                   <includes>
                       <include>**/*.xml</include>
                   </includes>
               </resource>
               <!-- 把src/main/resources下面的所有文件，都包含到classes目录 -->
               <resource>
                   <directory>src/main/resources</directory>
                   <includes>
                       <include>**/*.*</include>
                   </includes>
               </resource>
   </resources>
   ```

4. 打包后war文件的名称

   ```xml
   <!--指定打包后的 war 文件名称--> 
   <finalName>myweb</finalName>
   ```

5. 在src/main下创建webapp目录，并指定webapp是web应用目录

6. 在webapp下创建jsp文件，并编写controller完成简单数据展示。

7. 配置application.properties，设置视图解析器

   ```
   server.port=9001
   server.servlet.context-path=/myjsp
   
   spring.mvc.view.prefix=/
   spring.mvc.view.suffix=.jsp
   ```

8. 主启动类继承SpringBootServletInitializer，并重写configure方法

   - 继承 SpringBootServletInitializer 可以使用外部 tomcat。SpringBootServletInitializer 就是原有的 web.xml 文件的替代。使用了嵌入式 Servlet,默认是不支持 jsp。

   ```
   /**
    * 继承SpringBootServletInitializer，才能够使用独立tomcat服务器
    */
   @SpringBootApplication
   public class SpringBootLearningWarApplication extends SpringBootServletInitializer {
   
       public static void main(String[] args) {
           SpringApplication.run(SpringBootLearningWarApplication.class, args);
       }
   
       @Override
       protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {
           return builder.sources(SpringBootLearningWarApplication.class);
       }
   }
   ```

9. 指定项目package是war

   ```
   <!--指定 packing 为 war--> 
   <packaging>war</packaging>
   ```

10. maven package打包

    ![image-20220808190225345](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220808190225345.png)

11. 发布打包后的war到tomcat

    - target 目录下的 war 文件拷贝到 tomcat 服务器 webapps 目录中，启动 tomcat。在浏览器访问 web 应用。

## SpringBoot打包为jar

1. 创建Spring Boot web项目

2. 在pom.xml文件中配置内嵌Tomcat对jsp的解析包

3. 指定jsp文件的编译目录

4. 打包后jar文件的名称

5. 在src/main下创建webapp目录，并指定webapp是web应用目录

6. 在webapp下创建jsp文件，并编写controller完成简单数据展示。

7. 配置application.properties，设置视图解析器

8. 带有jsp项目打包成jar时，必须 maven 插件版本

   ```xml
   <plugins>
               <plugin>
                   <groupId>org.springframework.boot</groupId>
                   <artifactId>spring-boot-maven-plugin</artifactId>
                   <!-- 打包jar，有jsp文件时，必须指定maven-plugin插件版本为1.4.2.RELEASE -->
                   <version>1.4.2.RELEASE</version>
               </plugin>
   </plugins>
   ```

9. 执行maven package

10. 执行 jar，启动内置的 tomcat，指令：java –jar mybootjar.jar

## Spring Boot部署和运行方式总结

- 在 IDEA 中直接运行 Spring Boot 程序的 main 方法（开发阶段）

- 用 maven 将 Spring Boot 安装为一个 jar 包，使用 Java 命令运行

  java -jar springboot-xxx.jar 

  可以将该命令封装到一个 Linux 的一个 shell 脚本中（上线部署）

  ◼ 写一个 shell 脚本：

  \#!/bin/sh

  java -jar xxx.jar

  ◼ 赋权限 chmod 777 run.sh

  ◼ 启动 shell 脚本： ./run.sh

- 使用 Spring Boot 的 maven 插件将 Springboot 程序打成 war 包，单独部署在 tomcat 中运行（上线部

  署，常用）

## war和jar区别

- war需要服务器才能执行，jar可以直接执行（内嵌服务器）
- 但是war能够充分利用服务器的能力，jar的服务器能力较弱。

# 总结

## 注解

### 创建对象

```
@Controller：放在类上，创建控制器对象，注入容器中

@RestController：是@Controller和@ResponseBody的复合注解，在这个类下的所有方法返回值都是数据

@Service：放在业务层的实现类上，创建service对象，注入到容器

@Repository：放在dao层的实现类上面，创建dao对象，放到容器中，没有使用这个注解，是因为现在使用MyBatis框架，dao对象是Mybati			  s通过代理生成，不需要@Repository

@Component：放在类的上面，创建此类的对象，放入到容器中

```

### 赋值

```
@Value：简单类型的赋值，例如:在属性上面使用。还可以通过@Value获取配置文件的数据（properties或yml），例如:						@Value("${server.port}")

@Autowired:引用类型赋值自动注入，支持byName，byType，默认是byType。放在属性的上面，也可以放在构造方法的上面。推荐是放在构造		   方法的上面

@Qualifer：给引用类型赋值，使用byName方式。@Autowired和@Qualifer都是Spring框架提供的。

@Resource：来自jdk中的定义，javax.annotation。实现引用类型的自动注入，支持byName、byType。默认是byName，如果byName失败，			再使用byType注入，在属性上使用。
```

### 其他

```
@Configuration：放在类的上面，表示这是个配置类，相当于xml配置文件

@Bean：放在方法上面，把方法的返回值对象，注入到Spring容器中

@ImportResource：加载其他的xml配置文件，把文件中的对象注入到Spring容器中

@PropertySource：读取其他的properties属性配置文件

@ComponentScan：扫描器，指定包名，扫描注解

@ResponseBody：放在方法上，表示方法的返回值为数据

@RequestBody：把请求体中的数据，读取出来并转为java对象使用

@ControllerAdvice：控制器增强，放在类上，表示此类提供了方法，可以对controller增强功能

@ExceptionHandler：用来处理异常，放在方法上

@Transaction：处理事务的，放在service实现类的public方法上面，表示此方法有事务
```

### SpringBoot

```
@SpringBootApplication：放在启动类上面，包含了@SpringBootConfiguration、@EnableAutoConfiguration、@ComponentScan


```

### Mybatis相关注解

```
@Mapper：放在类的上面，让Mybatis找到接口，创建其代理对象

@MapperScan：放在主类上面，指定扫描的包，把这个包中的所有接口都创建其代理对象，并注入到容器中

@Param：放在dao接口的方法的形参前面，作为命名参数使用
```

### Duboo相关注解

```
@DubboService：在提供者端使用，用于暴露服务，放在接口的实现类上

@DubboReference：在消费者使用的，引用远程服务，放在属性上面

@EnableDubbo：放在主类上面，表示当前应用启用Duboo功能
```

