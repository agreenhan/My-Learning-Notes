# 第1章 MQ的简介

## 1.1 为什么要引入MQ

### 1.1.1 系统之间直接调用实际工程落地和存在的问题

- 在微服务架构之后，链式调用是一般流程，为了完成一整个功能会将其拆分成多个子模块。比如模块A调用模块B，模块B调用模块C，模块C调用模块D。但是在大型分布式应用中，系统间的RPC交互繁杂，这种架构存在以下问题：

#### 1.1.1.1 系统之间接口耦合比较严重

- 每新增一个下游功能，都要对上游的相关接口进行改造

  ```
  比如：系统A要发送数据给系统B和C，发送给每个系统的数据可能有差异，因此系统A要对发送给每个系统的数据进行了组装，然后逐一发送。
  ```

- 如果增加了新的需求：

  ![image-20220817144628318](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220817144628318.png)

#### 1.1.1.2 面对大流量并发时，容易被冲垮

- 每个接口模块的吞吐能力是有限的，当大流量来临，容易被冲垮

- 例子：秒杀业务

  ```
  上游系统发起下单购买操作：只有下单操作
  下游系统完成秒杀业务逻辑：读取订单，库存检查，库存冻结，余额检查，余额冻结，订单生成，余额扣减，库存扣减，生成流水，余额解冻，库存解冻
  ```

#### 1.1.1.3 等待同步存在性能问题

- RPC接口基本上是同步调用，整体的服务性能遵循"木桶理论"，即整体系统的耗时取决于链路中最慢的那个接口。

- 比如A调用B/C/D都是50ms，但是此时B又调用了B1，花费2000ms，那么直接拖累了整个服务性能。

  ![image-20220817145032159](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220817145032159.png)

## 1.2 MQ的作用定义

### 1.2.1 定义

- 面向消息的中间件(message-oriented middleware)MOM能够很好地解决以上问题。

- MOM是指利用高效可靠的消息传递机制进行与平台无关的数据交流，并基于数据通信来进行分布式系统的集成

- 通过提供**消息传递**和**消息排队**模型在分布式环境下提供应用解耦、弹性伸缩、冗余存储、流量削峰、异步通信、数据同步等功能。

- 大致流程：

  发送者把消息发送给消息服务器，消息服务器将消息存放在若干**队列/主题topic**中，在合适的时候，消息服务器会将消息转发给接受者。在这个过程中，发送和接受是异步的，也就是发送无需等待，而且发送者和接收者的生命周期也没有必然联系；尤其在发布pub/订阅sub模式下，也可以完成一对多的通信，即让一个消息有多个接收者

- ![image-20220817150057702](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220817150057702.png)

### 1.2.2 特点

#### 1.2.2.1 采用异步处理模式

- 消息发送者可以发送一个消息而无须等待响应。消息发送者将消息发送到一条虚拟的通道（主题或队列）上；
- 消息接收者则订阅或监听该通道。一条信息可能最终转发给一个或多个消息接收者，这些接收者都无需对消息发送者做出同步回应。整个过程都是异步的。
- 举例：
  - 一个系统跟另外一个系统之间进行通信时，加入A系统希望发送一个消息给B系统，让他去处理。但是系统A不关注系统B到底怎么处理或者有没有处理好，所以系统A把消息发送给MQ，然后就不管这条消息的“死活”了，接着系统B从MQ里消费出来处理即可。至于怎么处理，是否处理完毕，什么时候处理，都是B系统的事，与A系统无关。
  - ![image-20220817151055871](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220817151055871.png)
  - 这样的一种通信方式，就是所谓的“异步”通信方式对于系统A来说，只要把消息发给MQ，然后系统B就会异步地去进行处理了，系统A不需要同步等待系统B处理完。好处就是可以解耦合。

## 1.3 实现的功能

- 实现高可能、高性能、可伸缩、易用和安全的企业级面向消息服务的系统
- 异步消息的消费和处理
- 控制消息的消费顺序
- 可以和spring/springboot整合简化代码
- 配置集群容错的MQ集群

# 第2章 ActiveMQ安装和控制台

## 2.1 下载地址

- http://activemq.apache.org/

## 2.2 Linux环境下安装

1. 下载压缩包
2. 解压到/opt目录下
3. 在/bin根目录下新建文件夹myactiveMQ
4. 将解压缩的文件拷贝到myactiveMQ中
5. 普通启动ActiveMQ：./activemq start
6. activemq的默认进程端口为61616，查看服务：ps或者netstat -anp | grep 61616或者lsof -i:61616
7. 带运行日志的启动方式：./activemq start > /myactiveMQ/run_activemq.log

## 2.3 控制台

- 前台访问地址：http://localhost:8161/admin/

- 默认的用户名和密码均为admin

- 如果开放端口不能访问，注意修改bin/conf/jetty.xml文件中host值，127.0.0.1改为0.0.0.0

  ![img](https://img-blog.csdnimg.cn/img_convert/40600a0f1ce31dc8598eabc4d5e155d4.png)

# 第3章 实现ActiveMQ通讯

## 3.1 新建maven工程

- 普通java工程即可

## 3.2 导入依赖，pom.xml

```
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
    </dependency>
    <dependency>
      <groupId>org.apache.activemq</groupId>
      <artifactId>activemq-all</artifactId>
      <version>5.16.4</version>
    </dependency>
    <dependency>
      <groupId>org.apache.xbean</groupId>
      <artifactId>xbean-spring</artifactId>
      <version>3.16</version>
    </dependency>
    <dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>slf4j-api</artifactId>
      <version>1.7.25</version>
    </dependency>
    <dependency>
      <groupId>ch.qos.logback</groupId>
      <artifactId>logback-classic</artifactId>
      <version>1.2.3</version>
    </dependency>
    <dependency>
      <groupId>org.projectlombok</groupId>
      <artifactId>lombok</artifactId>
      <scope>provided</scope>
    </dependency>
  </dependencies>
```

## 3.3 JMS编码总体架构

- 流程图：

  ![image-20220817185934572](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220817185934572.png)

- 队列和主题

  - 示意图

    ![image-20220817190821073](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220817190821073.png)

  - 队列是一对一，主题是一对多

## 3.4 目的地为队列（queue）点对点

### 3.4.1 启动ActiveMQ

- 参考上文

### 3.4.2 生产者

```java
public class JmsProduce {
    public static final String ACTIVEMQ_URL = "tcp://192.168.238.129:61616";
    public static final String QUEUE_NAME= "queue01";
    public static void main(String[] args) throws JMSException {
        // 1、创建连接工场
        ActiveMQConnectionFactory activeMQConnectionFactory = new ActiveMQConnectionFactory(ACTIVEMQ_URL);
        // 2、通过连接工场，获取连接connection请启动访问
        Connection connection = activeMQConnectionFactory.createConnection();
        connection.start();
        // 3、创建会话session
        // 两个参数，第一个是事务，第二个是签收
        Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
        // 4、创建目的地（具体是队列还是主题topic）
        Queue queue = session.createQueue(QUEUE_NAME);
        // 5、创建消息的生产者
        MessageProducer messageProducer = session.createProducer(queue);
        // 6、通过使用messageProducer生产3条消息发送到MQ的队列里面
        for(int i = 1; i <= 3; i++){
            // 7、创建消息
            TextMessage textMessage = session.createTextMessage("msg----" + i);
            // 8、通过messageProducer发送给MQ
            messageProducer.send(textMessage);
        }
        // 9、关闭资源
        messageProducer.close();
        session.close();
        connection.close();

        System.out.println("消息发布到MQ成功");
    }
}
```

### 3.4.3 消费者（同步阻塞方式）

- receive()，订阅者或接收者调用MessageConsumer的receive()方法来接收消息，receive方法在能够接收到消息之前（或超时之前）将一直阻塞。

```java
public class JmsConsumer {
    public static final String ACTIVEMQ_URL = "tcp://192.168.238.129:61616";
    public static final String QUEUE_NAME= "queue01";
    public static void main(String[] args) throws JMSException {
        // 1、创建连接工场
        ActiveMQConnectionFactory activeMQConnectionFactory = new ActiveMQConnectionFactory(ACTIVEMQ_URL);
        // 2、通过连接工场，获取连接connection请启动访问
        Connection connection = activeMQConnectionFactory.createConnection();
        connection.start();
        // 3、创建会话session
        // 两个参数，第一个是事务，第二个是签收
        Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
        // 4、创建目的地（具体是队列还是主题topic）
        Queue queue = session.createQueue(QUEUE_NAME);
        // 5、创建消费者
        MessageConsumer messageConsumer = session.createConsumer(queue);
        // 6、
        while(true){
            TextMessage textMessage = (TextMessage) messageConsumer.receive();
            if(null != textMessage){
                System.out.println("接收到的消息为：" + textMessage.getText());
            } else {
                break;
            }
        }
        messageConsumer.close();
        session.close();
        connection.close();
    }
}
```

### 3.4.4 消费者（异步非阻塞方式）

- 通过监听的方式来获取消息：MessageConsumer

- 订阅者或接收者通过MessageConsumer的setMessageListener(MessageListener listener)注册一个消息监听器，当消息到达之后，系统自动调用监听器MessageListener的onMessage(Message message)方法

  ```java
  public class JmsConsumer {
      public static final String ACTIVEMQ_URL = "tcp://192.168.238.129:61616";
      public static final String QUEUE_NAME= "queue01";
      public static void main(String[] args) throws JMSException, IOException {
          // 1、创建连接工场
          ActiveMQConnectionFactory activeMQConnectionFactory = new ActiveMQConnectionFactory(ACTIVEMQ_URL);
          // 2、通过连接工场，获取连接connection请启动访问
          Connection connection = activeMQConnectionFactory.createConnection();
          connection.start();
          // 3、创建会话session
          // 两个参数，第一个是事务，第二个是签收
          Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
          // 4、创建目的地（具体是队列还是主题topic）
          Queue queue = session.createQueue(QUEUE_NAME);
          // 5、创建消费者
          MessageConsumer messageConsumer = session.createConsumer(queue);
          messageConsumer.setMessageListener(new MessageListener() {
              @Override
              public void onMessage(Message message) {
                  if(null != message && message instanceof TextMessage){
                      TextMessage textMessage = (TextMessage) message;
                      try {
                          System.out.println("消费者收到消息：" + textMessage.getText());
                      } catch (JMSException e) {
                          e.printStackTrace();
                      }
                  }
              }
          });
          System.in.read();
          messageConsumer.close();
          session.close();
          connection.close();
      }
  }
  ```

### 3.4.5 消费生产关系

- 先生产，只启动1号消费者，1号消费者可以消费消息。
- 先生产，先启动1号消费者，再启动2号消费者，结果：1号消费者可以消费，2号不能。
- 先启动2个消费者，再生产6条消息，消费情况为轮询消费，1号和2号都消费3条消息。

### 3.4.6 两种消费方式

#### 3.4.6.1 同步阻塞方式

- receive()，订阅者或接收者调用MessageConsumer的receive()方法来接收消息，receive方法在能够接收到消息之前（或超时之前）将一直阻塞。

#### 3.4.6.2 异步非阻塞方式

- 订阅者或接收者通过MessageConsumer的setMessageListener(MessageListener listener)注册一个消息监听器，当消息到达之后，系统自动调用监听器MessageListener的onMessage(Message message)方法

### 3.4.7 JMS开发的基本步骤

1. 创建一个connection factory

2. 通过connection factory来创建JMS connection

3. 启动JMS connection

4. 通过connection创建JMS session

5. 创建JMS destination

6. 创建JMS producer或者创建JMS message并设置destination

7. 创建JMS consumer或者注册一个JMS message listener

8. 发送或接收JMS message

9. 关闭所有的JMS资源（connection、session、producer、consumer等）

10. 示意图

    ![image-20220818101720094](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220818101720094.png)

### 3.4.8 点对点消息传递域的特点

- 每个消息只能有一个消费者，类似1对1的关系。

- 消息的生产者和消费者之间没有时间上的相关性。无论消费者在生产者发送消息时是否处于运行状态，消费者都可以提取消息。

- 消息被消费后队列中不会在存储，所以消费者不会消费到已经被消费掉的消息

- 模型图示：

  ![image-20220818102330721](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220818102330721.png)

## 3.5 目的地为主题（topic）

### 3.5.1 发布/订阅消息传递域的特点

- 生产者将消息发布到topic中，每个消息可以有多个消费者，属于1：N的关系

- 生产者和消费者之间有时间上的相关性。订阅某个主题的消费者只能消费自它订阅之后发布的消息。

- 生产者生产时，topic不保存消息它是无状态的不落地，假如无人订阅就去生产，那就是一条废消息，所以一般先启动消费者再启动生产者

- JMS规范允许客户创建持久订阅，这在一定程度上放松了时间上的相关性要求。持久订阅允许消费者消费它在未处于激活状态时发送的消息。

- 模型示例

  ![image-20220818103135079](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220818103135079.png)

### 3.5.2 发布主题生产者

```
public class JmsProduce_Topic {
    public static final String TOPIC_NAME = "topic1";
    public static final String ACTIVE_URL = "tcp://192.168.238.129:61616";

    public static void main(String[] args) throws JMSException {
        // 1 创建连接工场
        ActiveMQConnectionFactory activeMQConnectionFactory = new ActiveMQConnectionFactory(ACTIVE_URL);
        // 2 通过连接工场，获得连接connection并启动
        Connection connection = activeMQConnectionFactory.createConnection();
        connection.start();
        // 3 创建session对话域
        Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
        // 4 创建目的地
        Topic topic = session.createTopic(TOPIC_NAME);
        // 5 创建消费者
        MessageProducer messageProducer = session.createProducer(topic);
        // 6 通过使用messageProducer生产消息到MQ
        for(int i = 1; i <= 3; i++){
            TextMessage textMessage = session.createTextMessage("TOPIC_NAME----" + i);
            messageProducer.send(textMessage);
        }
        // 关闭资源
        messageProducer.close();
        session.close();
        connection.close();
        System.out.println("发布消息成功");
    }
}
```

### 3.5.3 订阅主题消费者

```
public class JmsConsumer_Topic {
    public static final String TOPIC_NAME = "topic1";
    public static final String ACTIVE_URL = "tcp://192.168.238.129:61616";

    public static void main(String[] args) throws JMSException, IOException {
        System.out.println("我是3号消费者");
        // 1 创建连接工场
        ActiveMQConnectionFactory activeMQConnectionFactory = new ActiveMQConnectionFactory(ACTIVE_URL);
        // 2 通过连接工场，获得连接connection并启动
        Connection connection = activeMQConnectionFactory.createConnection();
        connection.start();
        // 3 创建session对话域
        Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
        // 4 创建目的地
        Topic topic = session.createTopic(TOPIC_NAME);
        // 5 创建消费者
        MessageConsumer messageConsumer = session.createConsumer(topic);
        messageConsumer.setMessageListener(new MessageListener() {
            @Override
            public void onMessage(Message message) {
                if(null != message && message instanceof TextMessage){
                    TextMessage textMessage = (TextMessage) message;
                    try {
                        System.out.println("消费者收到消息：" + textMessage.getText());
                    } catch (JMSException e) {
                        e.printStackTrace();
                    }
                }
            }
        });
        System.in.read();
        messageConsumer.close();
        session.close();
        connection.close();
    }
}
```

### 3.5.4 两大模式比较

![image-20220818105841276](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220818105841276.png)

# 第4章 JMS规范和落地产品

## 4.1 JMS是什么

### 4.1.1 JavaEE

- JavaEE是一套使用Java进行企业级应用开发的13个核心规范工业标准。

- JavaEE平台提供了一个基于组件的方法来加快设计、开发、装配及部署企业应用程序。

  ```
  JDBC 		数据库连接
  JNDI 		Java的命名和目录接口
  EJB			
  RMI			远程方法调用
  Java IDL	接口定义语言/公用对象请求代理程序体系结构
  JSP			
  Servlet		
  XML			可扩展白标记语言
  JMS			Java消息服务
  JTA			Java事务API
  JTS			Java事务服务
  JavaMail	
  JAF			
  ```

### 4.1.2 什么是Java消息服务

- JMS即Java消息服务（Java Message Service）应用程序接口，是一个Java平台中关于面向消息中间件（MOM）的API，用于在两个应用程序之间，或分布式系统中发送消息，进行异步通信。Java消息服务是一个与具体平台无关的API，绝大多数MOM提供商都对JMS提供支持。

## 4.2 MQ中间件的其他落地产品

- ![image-20220818111846308](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220818111846308.png)

## 4.3 JMS的组成结构和特点

### 4.3.1 JMS message

- 组成部分：消息头、消息体、消息属性

#### 4.3.1.1 消息头

- JMSDestination

  - 消息的目的地

- JMSDeliveryMode

  - 表示持久和非持久模式

    ```
    一条持久性的消息：应该被传送“一次仅仅一次”，这就意味着如果JMS提供者出现故障，该消息并不会丢失，它会在服务器恢复之后再次传递
    一条非持久的消息：最多会传送一次，这意味着服务器出现故障，该消息将永远丢失。
    ```

- JMSExpiration

  - 消息的过期时间

    ```
    可以设置消息在一定时间后过期，默认是永不过期。
    消息过期时间，等于Destination的send方法中的timeToLive值加上发送时刻的GMT时间值
    如果timeToLive值为0，则JMSExpiration被设置为0，表示该消息永不过期
    如果发送后，在消息过期时间之后消息还没有被发送到目的地，则该消息被清除
    ```

- JMSPriority

  - 消息的优先级

    ```
    消息优先级，从0-9十个级别，0到4是普通消息，5到9是加急消息。
    JMS不要求MQ严格按照这十个优先级发送消息，但必须保证加急消息要先于普通消息到达。默认是4级
    ```

- JMSMessageID

  - 唯一识别每个消息的标识，由MQ产生

#### 4.3.1.2 消息体

- 消息体是封装具体的消息数据
- 5中消息体格式
  - TextMessage
    - 普通字符串消息，包含一个string
  - MapMessage
    - 一个Map类型的消息，key为string类型，而值为Java的基本类型
  - BytesMessage
    - 二进制数组消息，包含一个byte[]
  - StreamMessage
    - Java数据流消息，用标准流操作来顺序的填充和读取
  - ObjectMessage
    - 对象消息，包含一个可序列化的Java对象
- 发送和接受的消息体类型必须一致对应

#### 4.3.1.3 消息属性

- 如果需要除消息头字段以外的值，那么可以使用消息属性

- 识别、去重、重点标注等操作非常有用的方法

  ```
  它们是以属性名和属性值对的形式制定的。可以将属性是为消息头得扩展，属性指定一些消息头没有包括的附加信息，比如可以在属性里指定消息选择器。
  消息的属性就像可以分配给一条消息的附加消息头一样。它们允许开发者添加有关消息的不透明附加信息。
  它们还用于暴露消息选择器在消息过滤时使用的数据。
  ```

## 4.4 JMS的可靠性

### 4.4.1 PERSISTENT持久性

- 参数设置说明

  - 持久

    ```
    持久化：当服务器宕机，消息依然存在。
    messageProducer.setDeliveryMode(DeliveryMode.PERSISTENT);
    ```

  - 非持久

    ```
    非持久化：当服务器宕机，消息不存在。
    messageProduce.setDeliveryMode(DeliveryMode.NON_PERSISTENT);
    ```

  - 默认是持久化

- 持久的Queue

  - 队列的默认传送模式是**持久化**
  - 此模式保证这些消息只被传送一次和成功使用一次。对于这些消息，可靠性是优先考虑的因素。

- 持久的Topic

  - 消费者

    ```
    public class JmsConsumer_Topic {
        public static final String TOPIC_NAME = "topic1";
        public static final String ACTIVE_URL = "tcp://192.168.238.129:61616";
    
        public static void main(String[] args) throws JMSException, IOException {
            System.out.println("消费者");
            // 1 创建连接工场
            ActiveMQConnectionFactory activeMQConnectionFactory = new ActiveMQConnectionFactory(ACTIVE_URL);
            // 2 通过连接工场，获得连接connection并启动
            Connection connection = activeMQConnectionFactory.createConnection();
            connection.setClientID("zs");
            // 3 创建session对话域
            Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
            // 4 创建目的地
            Topic topic = session.createTopic(TOPIC_NAME);
            TopicSubscriber topicSubscriber = session.createDurableSubscriber(topic, "remark...");
            connection.start();
    
            Message message = topicSubscriber.receive();
            while(null != message){
                TextMessage textMessage = (TextMessage) message;
                System.out.println("收到的持久化topic:" + textMessage.getText());
                message = topicSubscriber.receive();
            }
            session.close();
            connection.close();
        }
    }
    ```

  - 生产者

    ```
    public class JmsProduce_Topic {
        public static final String TOPIC_NAME = "topic1";
        public static final String ACTIVE_URL = "tcp://192.168.238.129:61616";
    
        public static void main(String[] args) throws JMSException {
            // 1 创建连接工场
            ActiveMQConnectionFactory activeMQConnectionFactory = new ActiveMQConnectionFactory(ACTIVE_URL);
            // 2 通过连接工场，获得连接connection
            Connection connection = activeMQConnectionFactory.createConnection();
            // 3 创建session对话域
            Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
            // 4 创建目的地
            Topic topic = session.createTopic(TOPIC_NAME);
            // 5 创建消费者
            MessageProducer messageProducer = session.createProducer(topic);
            // 6 设置为持久性 并启动connecton
            messageProducer.setDeliveryMode(DeliveryMode.PERSISTENT);
            connection.start();
            // 7 通过使用messageProducer生产消息到MQ
            for(int i = 1; i <= 3; i++){
                TextMessage textMessage = session.createTextMessage("TOPIC_NAME----" + i);
                messageProducer.send(textMessage);
            }
            // 关闭资源
            messageProducer.close();
            session.close();
            connection.close();
            System.out.println("发布消息成功");
        }
    }
    ```

  - 一定要先运行一次消费者，等于向MQ注册

  - 然后再运行生产者发送信息，此时无论消费者是否在线，都会接收到，不在线的话，下次连接的时候，会把没有收过的消息都接受下来。

### 4.4.2 transaction事务

- 如何开启事务

  - ```java
    Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
    ```

  - 如果为false，

  - 如果为true，则在关闭session之前需要使用session.commit()提交才会生效。

    - 回滚session.rollback();

- 事务偏生产者、签收偏消费者

### 4.4.3 Acknowledge 签收

#### 4.4.3.1 非事务

- 自动签收（默认）
  
- Session.AUTO_ACKNOWLEDGE
  
- 手动签收

  - Session.CLIENT_ACKNOWLEDGE

  - 客户端调用acknowledge方法手动签收

    ```
    textMessage.acknowledge();
    ```

- 允许重复消息
  
  - Session.DUPS_OK_ACKNOWLEDGE

#### 4.4.3.2 事务

- 生产事务开启，只有commit之后才能将全部消息变成已消费。

#### 4.4.3.3 签收和事务的关系

- 在事务性会话中，当一个事务被成功提交则消息被自动签收，如果事务回滚，则消息会被再次传送
- 非事务性对话中，消息何时被确认取决于创建会话是的应答模式（acknowledgement mode）

## 4.5 JMS的点对点总结

- 点对点模型是基于队列的，生产者发消息到队列，消费者从队列接受消息，队列的存在使得消息的异步传输成为可能。
- 如果在Session关闭时，有部分消息已被收到但还没有被签收，那当消费者下次连接到相同队列时，这些消息还会被再次接受
- 队列可以长久的保存消息知道消费者收到消息。消费者不需要因为担心消息会丢失而时刻和队列保持激活的连接状态，充分体现了异步传输模式的优势。

## 4.6 JMS的发布订阅总结

### 4.6.1 非持久订阅

- 非持久订阅只有当客户端处于激活状态，也就是和MQ保持连接状态才能收到发送到某个主题的消息
- 如果消费者处于离线状态，生产者发送的主题消息将会丢失作废，消费者永远不会收到。
- 先要订阅注册才能接受到发布，只给订阅者发布消息

### 4.6.2 持久订阅

- 客户端首先向MQ注册一个自己的身份ID识别号，当这个客户端处于离线时，生产者会为这个ID保存所有发送到主题的消息，当客户再次连接到MQ时会根据消费者的ID得到所有当自己处于离线时发送的主题的消息。
- 非持久订阅状态下，不能恢复或重新派送一个未签收的消息。
- 持久订阅才能恢复或重新派送一个未签收的消息。

### 4.6.3 使用规则

- 当所有的消息必须被接收，则用持久订阅。当丢失消息能够被容忍，则用非持久订阅。

# 第5章 ActiveMQ的Broker

## 5.1 不同的conf配置文件模拟不同的实例

- 在Linux中进入安装目录里，启动

  ```
  ./activemq start xbean:file:/myactiveMQ/apache-activemq-5.xx.x/conf/activemq02.xml 
  ```

## 5.2 嵌入式Broker

- 相当于一个ActiveMQ服务器实例
- 简单来说，Broker其实就是实现了用代码的形式启动ActiveMQ将MQ嵌入到Java代码中，以便随时用随时启动，在用的时候再去启动这样能节省了资源，也保证了可靠性
- 用ActiveMQ Broker作为独立的消息服务器来构建Java应用。ActiveMQ也支持在vm中通信基于嵌入式的broker，能够无缝的集成其他java应用

## 5.2.1 步骤

1. pom.xml

   ```
     <dependencies>
       <dependency>
         <groupId>junit</groupId>
         <artifactId>junit</artifactId>
         <version>4.12</version>
       </dependency>
       <dependency>
         <groupId>org.apache.activemq</groupId>
         <artifactId>activemq-all</artifactId>
         <version>5.16.4</version>
       </dependency>
       <dependency>
         <groupId>org.apache.xbean</groupId>
         <artifactId>xbean-spring</artifactId>
         <version>3.16</version>
       </dependency>
       <dependency>
         <groupId>org.slf4j</groupId>
         <artifactId>slf4j-api</artifactId>
         <version>1.7.25</version>
       </dependency>
       <dependency>
         <groupId>ch.qos.logback</groupId>
         <artifactId>logback-classic</artifactId>
         <version>1.2.3</version>
       </dependency>
       <dependency>
         <groupId>org.projectlombok</groupId>
         <artifactId>lombok</artifactId>
         <version>1.18.20</version>
         <scope>provided</scope>
       </dependency>
       <dependency>
         <groupId>com.fasterxml.jackson.core</groupId>
         <artifactId>jackson-databind</artifactId>
         <version>2.13.1</version>
       </dependency>
     </dependencies>
   ```

2. 编写主程序EmbedBroker

   ```
   public class EmberBroker {
       public static void main(String[] args) throws Exception {
           // ActiveMQ支持在vm中通信基于嵌入式的broker
           BrokerService brokerService = new BrokerService();
           brokerService.setUseJmx(true);
           brokerService.addConnector("tcp://localhost:61616");
           brokerService.start();
           System.in.read();
       }
   }
   ```

3. 队列验证

# 第6章 Spring整合ActiveMQ

## 6.1 Maven修改，需要添加Spring支持JMS的包

```xml
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
    </dependency>
    <dependency>
      <groupId>org.apache.activemq</groupId>
      <artifactId>activemq-all</artifactId>
      <version>5.16.4</version>
    </dependency>
    <dependency>
      <groupId>org.apache.xbean</groupId>
      <artifactId>xbean-spring</artifactId>
      <version>3.16</version>
    </dependency>
    <dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>slf4j-api</artifactId>
      <version>1.7.25</version>
    </dependency>
    <dependency>
      <groupId>ch.qos.logback</groupId>
      <artifactId>logback-classic</artifactId>
      <version>1.2.3</version>
    </dependency>
    <dependency>
      <groupId>org.projectlombok</groupId>
      <artifactId>lombok</artifactId>
      <version>1.18.20</version>
      <scope>provided</scope>
    </dependency>
    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-databind</artifactId>
      <version>2.13.1</version>
    </dependency>
  </dependencies>
```

## 6.2 Spring配置文件

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
    <!-- 开启包的自动扫描 -->
    <context:component-scan base-package="org.example.activemq.spring"></context:component-scan>

    <!-- 配置生产者 -->
    <bean id="jmsFactory" class="org.apache.activemq.pool.PooledConnectionFactory" destroy-method="stop">
        <property name="connectionFactory">
            <!-- 真正可以产生Connection的ConnectionFactory，由对应的JMS厂商提供 -->
            <bean class="org.apache.activemq.ActiveMQConnectionFactory">
                <property name="brokerURL" value="tcp://192.168.238.129:61616"/>
            </bean>
        </property>
        <property name="maxConnections" value="100"/>
    </bean>

    <!-- 这个是队列目的地，点对点的 -->
    <bean id="destination" class="org.apache.activemq.command.ActiveMQQueue">
        <constructor-arg index="0" value="spring-active-queue"/>
    </bean>

    <!-- 这个是主题 -->
    <bean id="destinationTopic" class="org.apache.activemq.command.ActiveMQTopic">
        <constructor-arg index="0" value="spring-active-topic"/>
    </bean>

    <!-- Spring提供的JMS工具类，它可以进行消息发送、接收等 -->
    <bean id="jmsTemplate" class="org.springframework.jms.core.JmsTemplate">
        <property name="connectionFactory" ref="jmsFactory"/>
        <!-- 这里ref决定是主题还是队列 -->
        <property name="defaultDestination" ref="destinationTopic"/>
        <property name="messageConverter">
            <bean class="org.springframework.jms.support.converter.SimpleMessageConverter"/>
        </property>
    </bean>
</beans>
```

## 6.3 队列

### 6.3.1 生产者

```java
@Service
public class SpringMQ_Producer {
    @Autowired
    private JmsTemplate jmsTemplate;

    public static void main(String[] args) {
        ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
        SpringMQ_Producer producer = (SpringMQ_Producer) ctx.getBean("springMQ_Producer");
        producer.jmsTemplate.send(new MessageCreator() {
            @Override
            public Message createMessage(Session session) throws JMSException {
                return session.createTextMessage("spring和ActiveMQ的queue整合");
            }
        });
        System.out.println("*********send task over");
    }
}
```

### 6.3.2 消费者

```java
@Service
public class SpringMQ_Consumer {
    @Autowired
    private JmsTemplate jmsTemplate;

    public static void main(String[] args) {
        ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
        SpringMQ_Consumer consumer = (SpringMQ_Consumer) ctx.getBean("springMQ_Consumer");
        String retValue = (String) consumer.jmsTemplate.receiveAndConvert();
        System.out.println("消费者收到的消息:" + retValue);
    }
}
```

## 6.4 主题

- 先启动消费者订阅，再启动生产者

### 6.4.1 生产者

```java
@Service
public class SpringMQ_Producer {
    @Autowired
    private JmsTemplate jmsTemplate;

    public static void main(String[] args) {
        ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
        SpringMQ_Producer producer = (SpringMQ_Producer) ctx.getBean("springMQ_Producer");
        producer.jmsTemplate.send(new MessageCreator() {
            @Override
            public Message createMessage(Session session) throws JMSException {
                return session.createTextMessage("spring和ActiveMQ的topic整合");
            }
        });
        System.out.println("*********send task over");
    }
}
```



### 6.4.2 消费者

```java
@Service
public class SpringMQ_Consumer {
    @Autowired
    private JmsTemplate jmsTemplate;

    public static void main(String[] args) {
        ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
        SpringMQ_Consumer consumer = (SpringMQ_Consumer) ctx.getBean("springMQ_Consumer");
        String retValue = (String) consumer.jmsTemplate.receiveAndConvert();
        System.out.println("消费者收到的消息:" + retValue);
    }
}
```



## 6.5 在spring里面实现消费者不启动，直接通过配置监听完成

- 可以实现不用先启动消费者，也能获得消息。

- 监听器：

  ```java
  @Component
  public class MyMessageListener implements MessageListener {
      @Override
      public void onMessage(Message message) {
          if(message instanceof TextMessage){
              TextMessage textMessage = (TextMessage) message;
              try {
                  System.out.println("监听器输出:" + textMessage.getText());
              } catch (JMSException e) {
                  e.printStackTrace();
              }
          }
      }
  }
  ```

- 配置文件

  ```xml
  	<!-- 配置监听类 -->
      <bean id="jmsContainer" class="org.springframework.jms.listener.DefaultMessageListenerContainer">
          <property name="connectionFactory" ref="jmsFactory"/>
          <property name="destination" ref="destinationTopic"/>
          <property name="messageListener" ref="myMessageListener"/>
      </bean>
  
      <!-- 可以使用@Component -->
      <!--<bean id="myMessageListener" class="org.example.activemq.spring.MyMessageListener"/>-->
  ```

# 第7章 SpringBoot整合ActiveMQ

## 7.1 队列

### 7.1.1 生产者

1. 新建springboot项目

   - 添加spring web和activeMQ依赖

2. 配置application.yml

   ```
   server:
     port: 7777
   spring:
     activemq:
       broker-url: tcp://192.168.238.129:61616
       user: admin
       password: admin
     jms:
       pub-sub-domain: false      #false代表Queue，true代表Topic，不写默认false
   
   # 定义队列名称
   myqueue: boot-activemq-queue
   ```

3. 编写配置类

   ```java
   @Component
   @EnableJms
   public class ConfigBean {
       @Value("${myqueue}")
       private String myQueue;
   
       @Bean
       public Queue queue(){
           return new ActiveMQQueue(myQueue);
       }
   }
   
   ```

4. 编写生产者类

   ```java
   @Component
   public class Queue_Producer {
       @Autowired
       private JmsMessagingTemplate jmsMessagingTemplate;
   
       @Autowired
       private Queue queue;
   
       public void produceMsg(){
           jmsMessagingTemplate.convertAndSend(queue, "*****" + UUID.randomUUID().toString().substring(0, 6));
       }
   
       // 间隔时间3秒钟定投
       @Scheduled(fixedDelay = 3000L)
       public void produceMsgScheduled(){
           jmsMessagingTemplate.convertAndSend(queue, "*****" + UUID.randomUUID().toString().substring(0, 6));
           System.out.println("正在投送....");
       }
   }
   ```

5. 主启动类

   ```java
   @SpringBootApplication
   @EnableScheduling
   public class ActiveMq003Application {
   
       public static void main(String[] args) {
           SpringApplication.run(ActiveMq003Application.class, args);
       }
   
   }
   ```

### 7.1.2 消费者

1. 新建springboot项目

2. 配置application.yml

   ```
   server:
     port: 8888
   spring:
     activemq:
       broker-url: tcp://192.168.238.129:61616
       user: admin
       password: admin
     jms:
       pub-sub-domain: false
   
   # 定义队列名
   myqueue: boot-activemq-queue
   ```

3. 编辑消费者

   ```java
   @Component
   public class Queue_Consumer {
       @JmsListener(destination = "${myqueue}")
       public void receive(TextMessage textMessage) throws JMSException {
           System.out.println("消费者收到消息:" + textMessage.getText());
       }
   }
   ```

4. 主启动类

   ```java
   @SpringBootApplication
   public class ActiveMq004Application {
   
       public static void main(String[] args) {
           SpringApplication.run(ActiveMq004Application.class, args);
       }
   }
   ```

## 7.2 主题

### 7.2.1 生产者

1. 新建springboot项目

2. 配置application.yml

   ```
   server:
     port: 5555
   
   spring:
     activemq:
       broker-url: tcp://192.168.238.129:61616
       user: admin
       password: admin
     jms:
       pub-sub-domain: true # true表示Topic
   
   # 主题名称
   myTopic: boot-activemq-topic
   ```

3. 编写配置类

   ```java
   @Component
   public class ConfigBean {
       @Value("${myTopic}")
       private String topicName;
   
       @Bean
       public Topic topic(){
           return new ActiveMQTopic(topicName);
       }
   }
   ```

4. 编写生产者

   ```
   @Component
   public class Topic_Produce {
       @Autowired
       private JmsMessagingTemplate jmsMessagingTemplate;
   
       @Autowired
       private Topic topic;
   
       @Scheduled(fixedDelay = 3000)
       public void produceTopic(){
           jmsMessagingTemplate.convertAndSend(topic, "主题消息：" + UUID.randomUUID().toString().substring(0,6));
           System.out.println("正在发送....");
       }
   }
   ```

5. 主启动类

   ```java
   @SpringBootApplication
   @EnableScheduling
   public class ActiveMqTopic005Application {
       public static void main(String[] args) {
           SpringApplication.run(ActiveMqTopic005Application.class, args);
       }
   }
   ```

### 7.2.2 消费者

1. 新建springboot项目

2. 配置application.ym

   ```
   server:
     port: 5555
   
   spring:
     activemq:
       broker-url: tcp://192.168.238.129:61616
       user: admin
       password: admin
     jms:
       pub-sub-domain: true # true表示Topic
   
   # 主题名称
   myTopic: boot-activemq-topic
   ```

3. 编写消费者

   ```java
   @Component
   public class Topic_Consumer {
       @JmsListener(destination = "${myTopic}")
       public void receive(TextMessage textMessage) throws JMSException {
           System.out.println("消费者收到订阅的主题:" + textMessage.getText());
       }
   }
   ```

4. 主启动类

   ```java
   @SpringBootApplication
   public class ActiveMqTopic006Application {
   
       public static void main(String[] args) {
           SpringApplication.run(ActiveMqTopic006Application.class, args);
       }
   
   }
   ```

   

# 第8章 ActiveMQ的传输协议

## 8.1 简介

- ActiveMQ支持的client-broker通讯协议有：TCP、NIO、UDP、SSL、Http(s)、VM。

- 其中配置Transport Connector的文件在activeMQ安装目录的conf/activemq.xml中的\<transportConnector\>标签之内。

- 如下图所示

  ![image-20220819141625839](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220819141625839.png)

  ```
  在上图给出的配置信息中，URI描述信息的头部都是采用协议名称，例如：
  描述amqp协议的监听端口号时，采用的URI描述格式为“amqp://....”
  
  唯独在进行openwire协议描述时，URI头却采用的“tcp://.....”。这是因为ActiveMQ中默认的消息协议就是openwire
  ```

## 8.2 传输协议种类

### 8.2.1 Transmission Control Protocol(TCP)

- 这是默认的Broker配置，TCP的Client监听端口61616
- 在网络传输数据前，必须要序列化数据，消息是通过一个叫wire protocol的来序列化成字节流。默认情况下ActiveMQ把wrie protocol叫做OpenWire，它的作用是促使网络上的效率和数据快速交互。
- TCP连接的URI形式如：tcp://hostname:port?key=value&key=value，后面参数是可选的
- TCP传输的优点
  - TCP协议传输可靠性高，稳定性强
  - 高效性：字节流方式传递，效率很高
  - 有效性、可用性：应用广泛，支持任何平台
  - 关于Transport协议的可配置参数可以参考：http://activemq.apache.org/configuring-version-5-transports.html

### 8.2.2 New I/O API Protocol(NIO)

- NIO协议和TCP协议类似，但NIO更侧重于底层的访问操作，它允许开发人员对同一资源可更多的client调用和服务端有更多的负载
- 适合使用NIO协议的场景
  - 可能有大量的Client去连接到Broker上，一般情况下，大量的Client去连接Broker是被操作系统的线程所限制的。因此NIo的实现比TCP需要更少的线程去运行，所以建议使用NIO协议
  - 可能对Broker有一个很迟钝的网络传输，NIO比TCP提供更好的性能
- NIO连接的URL：nio://hostname:port?key=value
- 关于Transport协议的可配置参数可以参考：http://activemq.apache.org/configuring-version-5-transports.html

### 8.2.3 AMQP协议

- Advanced Message Queing Protocol
- 一个提供统一消息服务的应用层标准高级消息队列协议，是应用层协议的一个开放标准，为面向消息的中间件设计。
- 基于此协议的客户端与消息中间件可传递消息，并不受客户端/中间件不同产品，不同开发语言等条件的限制。

### 8.2.4 stomp协议

-  Streaming Text Orientated Message Protocol
- 是流文本定向消息协议，是一种为MOM设计的简单文本协议

### 8.2.5 Secure Sockets Layer Protocol(SSL)

- 连接的URL形式：ssl://hostname:port?key=value

- 配置实例

  ![image-20220819143802286](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220819143802286.png)

### 8.2.6 mqtt协议

- Message Queuing Telemetry Transport，消息队列遥测传输
- 是IBM开发的一个即时通讯协议，有可能称为物联网的重要组成部分。
- 该协议支持所有平台，几乎可以把所有联网物品和外部连接起来，被用来当做传感器和致动器（比如通过Twitter让房屋联网）的通讯协议

### 8.2.7 ws协议

### 8.2.8 小结

![image-20220819144125561](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220819144125561.png)

## 8.3 NIO案例

### 8.3.1 修改配置文件activemq.xml

- 修改之前记得先备份！

  ![image-20220819144541872](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220819144541872.png)

### 8.3.2 修改后端程序中的URL即可

## 8.4 NIO增强

- 还存在的问题：URI格式头以“nio”开头，表示这个端口使用以TCP协议为基础的NIO网络IO模型，但是这样的设置方式，只能使这个端口支持Openwire协议，如何让这个端口既支持NIO网络IO模型，又让它支持多个协议呢？

- 解决：

  - 使用auto关键字，使用“+”符号来为端口设置多种特性

    ![image-20220819145819650](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220819145819650.png)

# 第9章 ActiveMQ的消息存储和持久化

## 9.1 介绍

- 为了避免以外宕机以后丢失信息，需要做到重启后可以恢复消息队列，消息系统一般都会采用持久化机制。
- ActiveMQ的消息持久化机制有JDBC、AMQ、KahaDB和LevelDB，无论使用哪一种持久化方式，消息的存储逻辑都是一致的
- 存储逻辑：在发送者将消息发送出去后，消息中心首先将消息存储到本地数据文件、内存数据库或者远程数据库等，再试图将消息发送给接收者，成功则将消息从存储中删除，失败则继续尝试发送。
- 消息中心启动以后首先要检查指定的存储位置，如果有未发送成功的消息，则需要把消息发送出去。

## 9.2 消息存储机制

### 9.2.1 AMQ Message Store

- 基于文件的存储方式，是以前的默认消息存储机制
- AMQ是一种文件存储形式，它具有写入速度快和容易恢复的特点。
- 消息存储在一个个文件中，文件的默认大小为32M，当一个存储文件中的消息已经全部被消费，那么这个文件将被标识为可删除，在下一个清楚阶段，这个文件被删除。
- 适用于ActiveMQ5.3之前的版本

### 9.2.2 KahaDB消息存储(默认)

- 基于日志文件，从ActiveMQ5.4开始默认的持久化插件

- 说明

  - KahaDB是目前默认的存储方式，可用于任何场景，提高了性能和恢复能力。
  - 消息存储使用一个事务日志和仅仅用一个索引文件来存储它所有的地址。
  - KahaDB是一个专门针对消息持久化的解决方案，它对典型的消息使用模式进行了优化。
  - 数据被追加到data logs中。当不再需要log文件中的数据的时候，log文件会被丢弃。

- KahaDB的存储原理

  - KahaDB在消息保存目录中只有4类文件和一个lock，跟ActiveMQ的其他几种文件存储引擎相比这就非常简洁了。

  - db-1.log

    ```
    db-<number>.log
    KahaDB存储消息到预定义大小的数据记录文件中，文件命名为db-<number>.log。当数据文件已满时，一个新的文件会随之创建。number数值也会随之递增，它随着消息数量的增多，如每32M一个文件，文件名按照数字进行编号，如db-1.log、db-2.log、db-3.log等，当不再有引用到数据文件中的任何消息时，文件会被删除或归档。
    ```

    ![image-20220821143359708](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220821143359708.png)

  - db.data

    ```
    该文件包含了持久化的BTree索引，索引了消息数据记录中的消息，它是消息的索引文件，本质上是B-Tree(B树)，使用B-Tree作为索引指向db-<number>.log里面存储的消息
    ```

  - db.free

    ```
    当前db.data文件里哪些页面是空闲的，文件具体内容是所有空闲页的ID
    ```

  - db.redo

    ```
    用来进行消息恢复，如果KahaDB消息存储在强制退出后启动，用于恢复BTree索引。
    ```

  - lock

    ```
    文件锁，表示当前获得KahaDB读写权限的broker
    ```

### 9.2.3 JDBC消息存储

- 添加mysql数据库的驱动包到lib文件夹下，还有连接池的jar包

- 在conf目录下，配置activeMQ.xml文件

  | 修改前KahaDB                                                 | 修改后JDBC PersisttenceAdapter                               |
  | ------------------------------------------------------------ | ------------------------------------------------------------ |
  | \<persistenceAdapter\><br/>            \<kahaDB directory="${activemq.data}/kahadb"/\><br/>\</persistenceAdapter\> | \<persistenceAdapter\><br/>            \<jdbcPersistenceAdapter dataSource="#mysql-ds"/\><br/>\</persistenceAdapter\> |
  |                                                              | dataSource指定将要引用的持久化数据库的bean名称。<br />createTablesOnStartup是否在启动的时候创建数据库表，默认值是true，这样每次启动都会去创建数据表了，一般是第一次启动的时候设置为true之后改成false |

- 数据库连接配置

  - 在\</broker\>标签外，\<import resource="jetty.xml"\>内添加以下设置

    ```
    <bean id="mysql-ds" class="org.apache.commons.dbcp2.BasicDataSource" destroy-method="close">
    	<property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
    	<property name="url" value="jdbc:mysql://localhost:3306/activemq?relaxAutoCommit=true"/>
    	<property name="username" value="root"/>
    	<property name="password" value="jht123456"/>
    	<property name="maxTotal" value="200"/>
    	<property name="poolPreparedStatements" value="true"/>
    </bean>
    ```

- 建仓sql和建表说明

  - 新建名为activemq的数据库

  - 自动新建的三张表

    - ACTIVEMQ_MSGS

      ```
      ID:自增的数据库主键
      CONTAINER：消息的Destination
      MSGID_PROD：消息发送者的主键
      MSG_SEG：发送消息的顺序，MSGID_PROD+MSG_SEQ可以组成JMS的MessageID
      EXPIRATION：消息的过期时间，存储的是从1970-1-1到现在的毫秒数
      MSG：消息本体的java序列化对象的二进制数据
      PRIORITY：优先级，从0-9，数值越大优先级越高
      ```

    - ACTIVEMQ_ACKS

      ```
      activemq_acks用于存储订阅关系，如果是持久化Topic，订阅者和服务器的订阅关系在这个表保存。数据库字段如下：
      CONTAINER：消息的Destination
      SUB_DEST：如果是使用Static集群，这个字段会有集群其他系统的信息
      CLIENT_ID：每个订阅者都必须有一个唯一的客户端ID用以区分
      SUB_NAME：订阅者名称
      SELECTOR：选择器，可以选择只消费满足条件的消息。条件可以用自定义属性实现，可支持多属性AND和OR操作
      LAST_ACKED_ID：记录消费过的消息的ID
      ```

    - ACTIVEMQ_LOCK

      ```
      这个表在集群环境中才有用，只有一个Broker可以获得消息，称为Master Broker，其他的只能作为备份等待Master Broker不可用，才可能称为下一个Master Broker。这个表用于记录哪个Broker是当前的Master Broker。
      ```

- 代码运行验证

  - 点对点
    - 一定要开启持久化messageProducer.setDeliveryMode(DeliveryMode.PERSISTENT)

    - 在点对点类型中

      ```
      当DeliveryMode设置为NON_PERSISTENT时，消息被保存在内存中
      当DeliveryMode设置为PERSISTENT时，消息保存在broker的相应的文件或者数据库中
      而且点对点类型中消息一旦被Consumer消费，就从broker中删除
      ```

  - 发布/订阅类型

    - 一定要开启持久化messageProducer.setDeliveryMode(DeliveryMode.PERSISTENT)

- 总结

  - 如果是Queue，在没有消费者消费的情况下会将消息保存到activemq_msgs表中，只要有任意一个消费者已经消费过了，消费之后这些消息将会立即被删除。
  - 如果是Topic，一般是先启动消费订阅然后再生产的情况下会将消息保存到activemq_acks

- 编程中注意事项

  - 数据库jar包
    - 记得需要将是要到的相关jar文件放着到ActiveMQ安装路径下的lib目录。mysql-jdbc驱动的jar包和对应的数据库连接池jar包
  - createTablesOnStartup属性
    - 在jdbcPersistentAdapter标签中设置createTablesOnStartup属性为true是，在第一次启动ActiveMQ时，ActiveMQ服务节点会自动创建所需要的表。启动完成后可以去掉这个属性，或者更改为createTablesOnStartup属性为false
  - 下划线问题
    - 操作系统的机器名中有"_"符号，解决方法：更改机器名并重启。

### 9.2.4 LevelDB消息存储

- 

### 9.2.5 JDBC Message Store with ActiveMQ Journal

- 简介：

  ```
  这种方式克服了JDBC Store的不足，JDBC每次消息过来，都需要去写库和读库。
  ActiveMQ Journal，使用高速缓存写入技术，大大提高了性能。
  当消费者的消费速度能够及时更上生产者消息的生产速度时，journal文件能够大大减少需要写入到DB中的消息。
  
  举例：
  生产者生产了1000条消息，这1000条消息会保存到journal文件，如果消费者的消费速度很快的情况下，在journal文件还没有同步到DB之前，消费者已经消费了90%的消息，那么这个时候只需要同步剩余的10%的消息到DB。
  
如果消费者的消费速度很慢，这个时候journal文件可以使消息以批量方式写到DB
  ```
  
- 配置

  - 在conf路径下修改activemq.xml配置文件如下：

    | 修改前                                                       | 修改后                                                       |
    | ------------------------------------------------------------ | ------------------------------------------------------------ |
    | \<persistenceAdapter\><br/>            \<jdbcPersistenceAdapter dataSource="#mysql-ds"/\><br/>        \</persistenceAdapter\> | \<persistenceFactory\><br/>            \<journalPersistenceAdapterFactory journalLogFiles="4" journalLogFileSize="32768" useJournal="true" useQuickJournal="true" dataSource="#mysql-ds" dataDirectory="a<br/>ctivemq-data"/\><br/>        \</persistenceFactory\> |

## 9.3 总结

### 9.3.1 持久化机制演变

- 从最初的AMQ Message Store方案到V4版本中推出的High performance journal（高性能事务支持）附件，并且同步推出了关于关系型数据库的存储方案。V5.3版本中又推出了对KahaDB的支持（V5.4版本后成为ActiveMQ默认的持久化方案），后来V5.8版本开始支持LevelDB，到现在，V5.9版本提供了标准的Zookeeper+LevelDB集群化方案。

### 9.3.2 ActiveMQ消息持久化机制

| 机制                     | 功能                                                         |
| ------------------------ | ------------------------------------------------------------ |
| AMQ                      | 基于日志文件                                                 |
| KahaDB                   | 基于日志文件，从5.4版本开始默认的持久化插件                  |
| JDBC                     | 基于第三方数据库                                             |
| LevelDB                  | 基于文件的本地数据库存储，从5.8版本之后推出的LevelDB性能高于KahaDB |
| Replicated LevelDB Store | 从5.9版本提供了基于LevelDB和Zookeeper的数据复制方式，用于Master-slave方式的首选方案。 |

### 9.3.3 持久化逻辑

- 在发送者将消息发送出去后，消息中心首先将消息存储到本地数据文件、内存数据库或者远程数据库等，然后试图将消息发送给接收者，发送成功则将消息从存储中删除，失败则继续尝试。
- 消息中心启动后首先要检查指定的存储位置，如果有未发送成功的消息，则需要把消息重新发送出去。

# 第10章 ActiveMQ多节点集群

- 解决引入消息队列之后该如何保证其高可用性
- 基于Zookeeper和LevelDB搭建ActiveMQ集群。集群仅提供主备方式的高可用集群功能，避免单点故障。

# 第11章 高级特性和重难点

## 11.1 异步投递Async Sends

- 简介

  ![image-20220822144842296](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220822144842296.png)

  ![image-20220822144850131](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220822144850131.png)

- 配置

  ![image-20220822144902207](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220822144902207.png)

- 如何保证消息成功发送

  ![image-20220822144926109](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220822144926109.png)

  - 代码：

    ![image-20220822145037369](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220822145037369.png)

## 11.2 延迟投递和定时投递

- ![img](file:///D:\QQ\1751263383\Image\C2C\EN3H@9G9PSZQW`S1Y[I07I2.png)
- ![image-20220822150601576](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220822150601576.png)
- ![image-20220822150552440](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220822150552440.png)

## 11.3 分发策略

- 

## 11.4 ActiveMQ消费重试机制

- ![image-20220822150616481](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220822150616481.png)
- ![image-20220822150622481](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220822150622481.png)
- ![image-20220822150629095](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220822150629095.png)

## 11.5 死信队列

- ![image-20220822150343461](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220822150343461.png)
- ![image-20220822150407714](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220822150407714.png)
- ![image-20220822150411834](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220822150411834.png)
- ![image-20220822150420178](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220822150420178.png)
- ![image-20220822150424437](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220822150424437.png)
- ![image-20220822150428539](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220822150428539.png)
- ![image-20220822150432710](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220822150432710.png)

## 11.6 如何保证消息不被重复消费

- ![image-20220822150440750](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220822150440750.png)