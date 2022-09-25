# 前言

1. 什么是分布式框架
   - 分布式系统是若干独立系统的集合，但是用户使用起来像是在使用一套系统
2. 为什么需要分布式系统
   - 规模的逐步扩大和业务的复杂，单台计算机扛不住双十一那样的流量
3. 应用架构的发展演变
   1. 单一架构
      - 当网站流量小时，将所有应用放到一台服务器上，打包运行公司管理系统、超时收银系统等
      - 优点：开发简单，部署简单
      - 缺点：扩展不容易，维护不容易，性能提升难
   2. 垂直应用架构
      - 将大应用拆分成小应用，根据不同的访问频率决定各自业务部署的服务器数量
      - 优点：扩展容易
      - 缺点：页面一改，可能造成整个项目的重新部署，业务和界面没有分离，随着业务种类的增加，难以解决业务之间的互相调用问题，订单服务器和用户服务器交互效率问题
   3. 分布式架构（基于RPC：远程过程调用）
      - 将业务拆分后，用某种方式实现各种业务模块的远程调用和复用，这时一个好的RPC框架就决定了你的分布式架构的性能。

# 初识Dubbo

## 为什么Dubbo性能高

- 高性能要从底层说起，既然是一个RPC框架，主要实现远程过程（方法）调用，那么提升性能就要从两个方面入手：序列化和网络通信
- 序列化：本地的对象要在网络中传输，必须要实现Serializable接口，也就是必须序列化。序列化的方法有很多，效率最高的就是二进制流（计算机就是二进制类的）。Dubbo采用的就是效率最高的二进制流。
- 网络通信：不同于HTTP需要进行7步走（三次握手和四次挥手），Dubbo采用Socket通信机制，一步到位，提升了通信效率，并且可以建立长连接，不用反复连接，直接传输数据。

## 其他RPC框架

- gRPC
- Thrift
- HSF

# Dubbo框架

## 概述

- Apache Dubbo是一款高性能、轻量级的开源Java RPC框架，它提供了三大核心能力：面向接口的远程方法调用、智能容错和负载均衡、服务自动注册和实现。
- Dubbo是一个分布式服务框架
- 官网：http://dubbo.apache.org

## Dubbo基本架构

- ​	基本架构：![image-20220806091335658](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220806091335658.png)
- Provider：服务提供者，暴露服务的服务提供方，服务提供方在启动时，向注册中心注册自己提供的服务
- Consumer：服务消费者，调用远程服务的服务消费方，服务消费者在启动时，向注册中心订阅自己所需的服务，服务消费者，从提供者地址列表中，基于软负载均衡算法，选一台提供者进行调用，如果调用失败，再选另一台调用。
- Registry：注册中心，注册中心返回服务提供者地址列表给消费者，如果有变更，注册中心将基于长连接推送更新数据给消费者。
- Monitor：监控中心，服务消费者和提供者，在内存中累计调用次数和调用时间，定时每分钟发送一次统计数据到监控中心。
- 调用关系说明：
  - 服务容器负责启动、加载、运行服务器提供者
  - 服务提供者在启动时，向注册中心注册自己提供的服务
  - 服务消费者在启动时，向注册中心订阅自己所需的服务
  - 注册中心返回服务提供者地址列表给消费者，如果有变更，注册中心将基于长连接推送更新数据给消费者。

## Dubbo支持的协议

- 支持多种协议：dubbo、hessian、rmi、http、webservice、thrift、memcached、redis
- 官方推荐使用dubbo协议，dubbo协议默认端口号为20880

## 直连方式

### 创建服务提供者

1. 创建实体类，并且实体类要实现Serializable接口

2. 编辑pom.xml，引入相关依赖，并设置java编译版本

   ```xml
   <dependencies>
       <dependency>
         <groupId>junit</groupId>
         <artifactId>junit</artifactId>
         <version>4.11</version>
         <scope>test</scope>
       </dependency>
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-context</artifactId>
         <version>5.2.5.RELEASE</version>
       </dependency>
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-webmvc</artifactId>
         <version>5.2.5.RELEASE</version>
       </dependency>
   
       <dependency>
         <groupId>com.alibaba</groupId>
         <artifactId>dubbo</artifactId>
         <version>2.6.2</version>
       </dependency>
     </dependencies>
     
     <build>
     	  <plugins>
     		<plugin>
             <artifactId>maven-compiler-plugin</artifactId>
             <version>3.1</version>
             <configuration>
               <source>1.8</source>
               <target>1.8</target>
             </configuration>
           </plugin>
         </plugins>
     </build>
   ```

3. 创建dao、service层

4. dubbo配置文件dubbo-stuservice-provider.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd">
       
       <!-- 服务提供者声明名称:必须保证服务名称唯一性，它的名称是dubbo内部使用的唯一标识 -->
       <dubbo:application name="001-link-stuservice-provider"/>
       
       <!-- 访问服务协议的名称及端口号，dubbo官方推荐使用的dubbo协议，端口号默认20880 -->
       <!-- name:指定协议的名称,port:指定协议的端口号 -->
       <dubbo:protocol name="dubbo" port="20880"/>
       
       <!-- 暴露服务接口 ——>dubbo:service -->
       <!-- interface：暴露服务接口的全限定类名
            ref:接口引用的实现类在spring容器中的表示
            registry:如果不使用注册中心，则值为：N/A
        -->
       <dubbo:service interface="com.mytest.dubbo.service.StudentService" ref="stuService" registry="N/A"/>
       
       <!-- 将接口的实现类加载到Spring容器中 -->
       <bean id="stuService" class="com.mytest.dubbo.service.impl.StudentServiceImpl"/>
   
   </beans>
   ```

5. 配置web.xml文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
       <context-param>
           <param-name>contextConfigLocation</param-name>
           <param-value>classpath:dubbo-stuservice-provider.xml</param-value>
       </context-param>
       <listener>
           <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
       </listener>
   </web-app>
   ```

6. 将项目打jar包，供消费者引入依赖。

### 创建服务消费者

1. 引入服务提供者的jar包（即依赖）

   ```
       <!-- 依赖服务提供者 -->
       <dependency>
         <groupId>org.example</groupId>
         <artifactId>Dubbo-001</artifactId>
         <version>1.0-SNAPSHOT</version>
       </dependency>
   ```

2. 创建对应controller

3. 配置application.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:mvc="http://www.springframework.org/schema/mvc"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">
       <!-- 扫描组件 -->
       <context:component-scan base-package="com.mytest.dubbo.web"/>
       <!-- 配置注解驱动 -->
       <mvc:annotation-driven></mvc:annotation-driven>
   
       <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
           <property name="prefix" value="/"/>
           <property name="suffix" value=".jsp"/>
       </bean>
   </beans>
   ```

4. 配置dubbo-consumer.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd">
       <!-- 声明服务消费者的名称:保持唯一性 -->
       <dubbo:application name="dubbo-consumer"/>
       <!--
           引用远程服务接口：
           id:远程调用接口对象名称
           interface：调用远程接口的全限定类名
           url：访问服务器接口的地址
           registry：不使用注册中心，值为：N/A
       -->
       <dubbo:reference id="stuService" interface="com.mytest.dubbo.service.StudentService" url="dubbo://localhost:20880" registry="N/A"/>
   </beans>
   ```

## dubbo服务化最佳实践

### 打包

- 建议将服务接口、服务模型等均放在公共包中

### 粒度

- 服务接口尽可能大粒度，每个服务方法代表一个功能，而不是某功能的一个步骤
- 服务接口建议以业务场景为单位划分，并对相近业务做抽象，防止接口数量爆炸
- 不建议使用过于抽象的通用接口，如：Map query(Map)，这样的接口没有明确语义，会给后期维护带来不便。

### 版本

- 每个接口都应定义版本号，区分同一接口的不同实现。如：<dubbo:service interface="com.xxx.XxxService" version="1.0"/>

## 直边方式-改造

### 创建接口（即公共包）

1. 新建普通Java工程
2. 创建实体类对象，并实现Serializable接口
3. 创建service接口（只写接口，不写实现）

### 创建服务提供者

1. 新建JavaWeb工程

2. 导入公共包

   ```xml
       <dependency>
         <groupId>org.example</groupId>
         <artifactId>interface-001</artifactId>
         <version>1.0-SNAPSHOT</version>
       </dependency>
   ```

3. 将公共包中的接口的实现，写出实现类

4. 配置dubbo-userservice-provider.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd">
       <!-- 声明dubbo服务提供者的名称：保证唯一性 -->
       <dubbo:application name="provider-001"/>
       <!-- 设置dubbo使用的协议端口 -->
       <dubbo:protocol name="dubbo" port="20880"/>
       <!-- 暴露接口 -->
       <dubbo:service interface="com.mytest.dubbo.service.UserService" ref="userService" registry="N/A"/>
       <!-- 加载业务接口的实现类到spring容器中 -->
       <bean id="userService" class="com.mytest.dubbo.service.impl.UserServiceImpl"/>
   </beans>
   ```

5. 配置web.xml

   ```xaml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
       <context-param>
           <param-name>contextConfigLocation</param-name>
           <param-value>classpath:dubbo-userservice-provider.xml</param-value>
       </context-param>
       <listener>
           <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
       </listener>
   </web-app>
   ```

### 创建服务消费者

1. 新建JavaWeb工程

2. 导入公共包

   ```xml
       <dependency>
         <groupId>org.example</groupId>
         <artifactId>interface-001</artifactId>
         <version>1.0-SNAPSHOT</version>
       </dependency>
   ```

3. 创建controller

4. 配置applicationContext.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:mvc="http://www.springframework.org/schema/mvc"
          xmlns:context="http://www.springframework.org/schema/context"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
       <!-- 扫描组件 -->
       <context:component-scan base-package="com.mytest.dubbo.web"/>
   
       <!-- 配置注解驱动 -->
       <mvc:annotation-driven/>
   
       <!-- 视图解析器 -->
       <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
           <property name="suffix" value=".jsp"/>
           <property name="prefix" value="/"/>
       </bean>
   </beans>
   ```

5. 配置dubbo-consumer.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd">
       <!-- 声明服务消费者名称：唯一性 -->
       <dubbo:application name="consumer-001"/>
       <!-- 引用远程接口服务 -->
       <dubbo:reference id="userService" interface="com.mytest.dubbo.service.UserService" url="dubbo://localhost:20880" registry="N/A"/>
   </beans>
   ```

6. 配置web.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
       <servlet>
           <servlet-name>dispatcherServlet</servlet-name>
           <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
           <init-param>
               <param-name>contextConfigLocation</param-name>
               <param-value>classpath:applicationContext.xml,classpath:dubbo-consumer.xml</param-value>
           </init-param>
       </servlet>
       <servlet-mapping>
           <servlet-name>dispatcherServlet</servlet-name>
           <url-pattern>/</url-pattern>
       </servlet-mapping>
   </web-app>
   ```

## dubbo常用标签

### 公用标签

- ```
  <dubbo:application/>和<dubbo:registry/>
  ```

- 配置注册中心

  ```
  <dubbo:registry address="ip:port" protocol="协议"/>
  ```

- 配置应用信息

  ```
  <dubbo:application name="服务的名称"/>
  ```

### 服务提供者标签

- 配置暴露的服务

  ```
  <dubbo:service interface="服务接口名" ref="服务实现对象 bean"/>
  ```

### 服务消费者标签

- 配置服务消费者引用远程服务

  ```
  <dubbo:reference id="服务引用bean的id" interface="服务接口名"/>
  ```

# 注册中心-Zookeeper

## 注册中心概述

- 对于服务提供方，它需要发布服务，而且由于应用系统的复杂性，服务的数量、类型也不断膨胀；对于服务消费方，它最关心如何获取到它所需要的服务，而面对复杂的应用系统，需要管理大量的服务调用。
- 对于提供方和消费方来说，他们还有可能兼容两种角色，即需要提供服务，有需要消费服务。通过将服务统一管理起来，可以有效的优化内部应用对服务发布/使用的流程和管理。
- 服务注册中心可以通过特定协议来完成服务对外的统一。Dubbo提供的注册中心有如下类型可选：
  - Multicast注册中心：组播方式
  - Redis注册中心：使用Redis作为注册中心
  - Simple注册中心：就是一个Dubbo服务，作为注册中心，提供查找服务的功能
  - Zookeeper注册中心：使用Zookeeper作为注册中心（推荐使用）

## 注册中心工作方式

- ![image-20220807195522903](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220807195522903.png)

## Zookeeper注册中心

### 下载地址

- https://zookeeper.apache.org/releases.html

### 使用步骤

1. 将下载好的Zookeeper压缩包解压缩

2. 配置Zookeeper，修改 zookeeper-3.8.0-bin/conf/目录下配置文件，复制 zoo-sample.cfg 并改名为 zoo.cfg

3. 修改zoo.conf

   dataDir : zookeeper 数据的存放目录

   admin.serverPort=8888

   原因：zookeeper 3.8.0 内部默认会启动一个应用服务器，默认占用8080 端口

4. 启动Zookeeper，双击zkServer.cmd

## Dubbo案例使用Zookeeper

### 创建接口

1. 创建普通java工程
2. 创建实体类，并实现Serializable接口
3. 创建service接口

### 创建服务提供者

1. 创建javaweb项目

2. 添加相关依赖，并设置java编译版本

   ```xml
   <dependencies>
       <dependency>
         <groupId>junit</groupId>
         <artifactId>junit</artifactId>
         <version>4.11</version>
         <scope>test</scope>
       </dependency>
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-context</artifactId>
         <version>5.2.5.RELEASE</version>
       </dependency>
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-webmvc</artifactId>
         <version>5.2.5.RELEASE</version>
       </dependency>
       <dependency>
         <groupId>com.alibaba</groupId>
         <artifactId>dubbo</artifactId>
         <version>2.6.2</version>
       </dependency>
       <!-- 接口工程依赖 -->
       <dependency>
         <groupId>org.example</groupId>
         <artifactId>zk-interface-001</artifactId>
         <version>1.0-SNAPSHOT</version>
       </dependency>
       <!-- zookeeper依赖 -->
       <dependency>
         <groupId>org.apache.curator</groupId>
         <artifactId>curator-framework</artifactId>
         <version>4.1.0</version>
       </dependency>
     </dependencies>
     
   	<build>	  
     	  <plugins>
     		<plugin>
             <artifactId>maven-compiler-plugin</artifactId>
             <version>3.1</version>
             <configuration>
               <source>1.8</source>
               <target>1.8</target>
             </configuration>
           </plugin>
         </plugins>
       </build>
   ```

3. 编写service的实现类

4. 配置dubbo-zk-userservice-provider.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd">
       <!-- 声明dubbo服务提供者的名称：保证唯一性 -->
       <dubbo:application name="zk-provider-001"/>
       <!-- 声明dubbo使用的协议名称和端口号 -->
       <dubbo:protocol name="dubbo" port="20880"/>
       <!-- 使用Zookeeper注册中心，指定注册中心地址和端口号，如果是linux系统，则localhost改为linux的ip地址 -->
       <dubbo:registry address="zookeeper://localhost:2181"/>
       <!-- 暴露服务接口 -->
       <dubbo:service interface="com.mytest.dubbo.service.UserService" ref="userServiceImpl"/>
       <!-- 加载接口实现类 -->
       <bean id="userServiceImpl" class="com.mytest.service.impl.UserServiceImpl"/>
   </beans>
   ```

5. 配置web.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
       <context-param>
           <param-name>contextConfigLocation</param-name>
           <param-value>classpath:dubbo-zk-userservice-provider.xml</param-value>
       </context-param>
       <listener>
           <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
       </listener>
   </web-app>
   ```

### 创建服务消费者

1. 新建Javaweb工程

2. 添加相应依赖及java编译版本（参考上一步）

3. 编写controller，调用服务（service）

4. 配置application.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:mvc="http://www.springframework.org/schema/mvc"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">
       <!-- 扫描组件 -->
       <context:component-scan base-package="com.mytest.dubbo.web"/>
       <!-- 配置注解驱动 -->
       <mvc:annotation-driven/>
       <!-- 视图解析器 -->
       <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
           <property name="prefix" value="/"/>
           <property name="suffix" value=".jsp"/>
       </bean>
   </beans>
   ```

5. 配置dubbo-zk-consumer.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd">
       <!-- 声明dubbo服务消费者名称：保证唯一性 -->
       <dubbo:application name="dubbo-zk-consumer"/>
   
       <!-- 指定注册中心，如果是linux系统，则localhost改为linux的ip地址 -->
       <dubbo:registry address="zookeeper://localhost:2181"/>
   
       <!-- 引用远程接口调用 -->
       <dubbo:reference id="userService" interface="com.mytest.dubbo.service.UserService"/>
   </beans>
   ```

6. 配置web.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
       <servlet>
           <servlet-name>dispatcherServlet</servlet-name>
           <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
           <init-param>
               <param-name>contextConfigLocation</param-name>
               <param-value>classpath:application.xml,classpath:dubbo-zk-consumer.xml</param-value>
           </init-param>
       </servlet>
       <servlet-mapping>
           <servlet-name>dispatcherServlet</servlet-name>
           <url-pattern>/</url-pattern>
       </servlet-mapping>
   </web-app>
   ```

### 启动Zookeeper

1. 在安装包bin目录下，双击zkServer.cmd

### 测试

1. 先启动Zookeeper，再启动服务提供者服务器，最后启动服务消费者服务器，然后进行测试。

## 注册中心的高可用

- 高可用可以理解为将自己作为服务向其他注册中心注册自己。这样可以形成一组互相注册的服务中心。这样服务清单可以**互相同步**，达到高可用的效果。

# dubbo的配置

## 配置原则

- 在服务提供者配置访问参数，因为服务提供者更了解服务的各种参数

## 关闭检查

- dubbo缺省会在启动时检查依赖的服务是否可用，不可用时会抛出异常，阻止Spring初始化完成，以便上线时，能及早发现问题，默认check=true。通过check=false关闭检查。比如，测试时，有些服务不关心，或者出现了循环依赖，必须有一方先启动。

- 关闭某个服务的启动检查

  ```
  <dubbo:reference interface="接口全限定类名" check="false"/>
  ```

- 关闭注册中心启动时检查

  ```
  <dubbo:registry check="false"/>
  ```

  - 默认启动服务时检查注册中心存在并已运行。注册中心不启动会报错。

## 重试次数

- 消费者访问提供者，如果访问失败，则切换重试访问其他服务器，但重试会带来更长延迟。访问时间变长，用户的体验较差。多次重新访问服务器可能访问成功。可通过retries="2"来设置重试次数（不含第一次）。

- 配置如下：

  ```
  <dubbo:service retries="2"/>
  或
  <dubbo:reference retries="2"/>
  ```

## 超时时间

- 由于网络或服务端不可靠，会导致调用出现一种不确定的中间状态（超时）。为了避免超时导致客户端资源（线程）挂起耗尽，必须设置超时时间。
- timeout：调用远程服务超时时间（毫秒）

### dubbo消费端

```
指定接口超时配置
<dubbo:reference interface="接口名" timeout="2000"/>
```

### dubbo服务端

```
<dubbo:service interface="接口名" timeout="2000"/>
```

## 版本号

- 每个接口都应定义版本号，区分同一接口的不同实现。如：<dubbo:service interface="com.xxx.XxxService" version="1.0"/>

### 使用实例

- 以Dubbo案例中的项目为例。

- 服务提供者添加接口的实现类UserService2，并修改dubbo-zk-userservice-provider.xml：

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
         xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd">
      <!-- 声明dubbo服务提供者的名称：保证唯一性 -->
      <dubbo:application name="zk-provider-001"/>
      <!-- 声明dubbo使用的协议名称和端口号 -->
      <dubbo:protocol name="dubbo" port="20880"/>
      <!-- 使用Zookeeper注册中心，指定注册中心地址和端口号 -->
      <dubbo:registry address="zookeeper://localhost:2181"/>
      <!-- 暴露服务接口 -->
      <!-- 不管是否一个接口有多个实现类，只要服务提供者提供接口服务时指定了版本号，那么消费者引用远程接口服务时必须指定版本号 -->
      <dubbo:service interface="com.mytest.dubbo.service.UserService" ref="userServiceImpl" version="1.0.0"/>
      <dubbo:service interface="com.mytest.dubbo.service.UserService" ref="userServiceImpl2" version="2.0.0"/>
      <!-- 加载接口实现类 -->
      <bean id="userServiceImpl" class="com.mytest.service.impl.UserServiceImpl"/>
      <bean id="userServiceImpl2" class="com.mytest.service.impl.UserServiceImpl2"/>
  </beans>
  ```

- 服务消费者修改dubbo-zk-consumer.xml文件，直接在controller中调用即可。

  ```
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
         xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd">
      <!-- 声明dubbo服务消费者名称：保证唯一性 -->
      <dubbo:application name="dubbo-zk-consumer"/>
  
      <!-- 指定注册中心 -->
      <dubbo:registry address="zookeeper://localhost:2181"/>
  
      <!-- 引用远程接口调用 -->
      <dubbo:reference id="userService1" interface="com.mytest.dubbo.service.UserService" version="1.0.0"/>
      <dubbo:reference id="userService2" interface="com.mytest.dubbo.service.UserService" version="2.0.0"/>
  </beans>
  ```

# 监控中心

## 什么是监控中心

- Dubbo的使用，其实只需要有注册中心、消费者、提供者这三个就可以使用了，但是并不能看到有哪些消费者和提供者，为了更好的调试、发现问题、解决问题，因此引入dubbo-admin。通过dubbo-admin可以对消费者和提供者进行管理。可以在dubbo应用部署做动态的调整、服务的管理。
- dubbo-admin
  - 图形化的服务管理页面，安装时需要指定注册中心地址，即可从注册中心中获取到所有的提供者/消费者进行配置管理。
- dubbo-monitor-simple:
  - 简单的监控中心

## 发布配置中心

### 下载监控中心：

- https://github.com/apache/dubbo-admin

### 运行管理中心后台dubbo-admin

- 到dubbo-admin-0.0.1-SNAPSHOT.jar所在的目录。执行下面命令java -jar dubbo-admin-0.0.1-SNAPSHOT.jar

### 修改配置dubbo-properties文件

- 

## 监控中心的数据来源

## 应用监控