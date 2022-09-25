# 一、Nginx简介

## 1.1 什么是nginx

- Nginx是一款轻量级的Web 服务器/反向代理服务器及电子邮件（IMAP/POP3）代理服务器，在BSD-like 协议下发行。其特点是占有内存少，并发能力强，事实上nginx的并发能力在同类型的网页服务器中表现较好。
- 在连接高并发的情况下，Nginx是Apache服务不错的替代品：Nginx在美国是做虚拟主机生意的老板们经常选择的软件平台之一。能够支持高达 50,000 个并发连接数的响应，感谢Nginx为我们选择了 epoll and kqueue作为开发模型。

## 1.2 反向代理

### 1.2.1 正向代理

- 一个位于客户端和原始服务器(origin server)之间的服务器，为了从原始服务器取得内容，客户端向代理发送一个请求并指定目标(原始服务器)，然后代理向原始服务器转交请求并将获得的内容返回给客户端。客户端才能使用正向代理。

### 1.2.2 反向代理

- 只需要将请求发送到反向代理服务器，由反向代理服务器去选择目标服务器获取数据后，再返回给客户端，此时反向代理服务器和目标服务器对外就是一个服务器，暴露的是代理服务器地址，隐藏了真实服务器IP地址。

## 1.3 负载均衡

- 单个服务器解决不了，增加服务器数量，然后将请求分发到各个服务器上，将原先请求击中到单个服务器上的情况改为将请求分发到多个服务器上，将负载分发到不同的服务器，也就是所说的负载均衡。

## 1.4 动静分离

- 为了加快网站的解析速度，可以把动态页面和静态页面由不同的服务器来解析，加快解析速度，降低原来单个服务器的压力。

# 二、Nginx的安装与配置

## 2.1 进入Nginx官网

- http://nginx.org/
- 需要的前置依赖：
  - pcre
  - openssl
  - zlib

## 2.2 安装相关依赖

- 一键安装：

  ```
  yum -y install gcc zlib zlib-devel pcre-devel openssl openssl-devel
  ```

## 2.3 安装Nginx

1. 解压缩

2. 进入目录，输入./configure

3. make && make install

4. 安装成功后，在/usr中会多处文件夹local/nginx，在nginx有sbin里有启动脚本

5. 查看开放的端口：firewall-cmd --list-all

6. 设置开放的端口：

   ```
   firewall-cmd --add-service=http --permanent
   sudo firewall-cmd --add-port=80/tcp --permanent
   ```

7. 重启防火墙：firewall-cmd --reload

# 三、Nginx操作的常用命令

- 前提：使用nginx操作命令前提条件：必须进入nginx目录（/usr/local/nginx/sbin）

## 3.1 查看nginx的版本号

- 语法：./nginx -v

## 3.2 启动nginx

- 语法：./nginx

## 3.3 关闭nginx

- 语法：./nginx -s stop

## 3.4 重新加载nginx

- 语法：./nginx -s reload

# 四、Nginx的配置文件

- nginx的配置文件位置：/usr/local/nginx/nginx.conf

## 4.1 全局块

- 从配置文件开始到events块之间的内容，主要会设置一些影响nginx服务器整体运行的配置指令，只要包括配置运行Nginx服务器的用户(组)、允许生成的worker process数，进程PID存放路径、日志存放路径和类型以及配置文件的引入等。
- worker_processes 1
  - 这是Nginx服务器并发处理服务的关键配置，worker_processes越大，可以支持的并发处理量也越多，但是会受到硬件、软件等设备的制约。

## 4.2 events块

- events块涉及的指令主要影响Nginx服务器与用户的网络连接，常用的设置包括：
  - 是否开启对多work process下的网络连接进行序列化，
  - 是否允许同时接收多个网络连接，
  - 选取哪种事件驱动模型来处理连接请求，
  - 每个work process可以同时支持的最大连接数等
- worker_connections表示支持最大连接数为1024

## 4.3 http块

- 这是Nginx服务器配置中最频繁的部分，代理、缓存和日志定义等绝大多数功能和第三方模块的配置都在这里，需要注意的是：http块也可以包括http全局块、server块

### 4.3.1 http全局块

- 包括文件引入、MIME-TYPE定义、日志自定义、连接超时时间、单链接请求数上限等。

### 4.3.2 server块

- 和虚拟主机有密切关系，虚拟主机从用户角度看，和一台独立的硬件主机是完全一样的，该技术的产生是为了节省互联网服务器硬件成本。
- 每个http块可以包括多个server块，而每个server块相当于一个虚拟主机
- 每个server块也分为全局server块以及可以同时包含多个location块

#### 4.3.2.1 全局server块

- 最常见的配置是本虚拟主机的监听配置和本虚拟主机的名称或ip配置

#### 4.3.2.2 location块

- 基于Nginx服务器接收到的请求字符串（例如server_name/uri-string)，对虚拟主机名称（也可以是ip别名）之外的字符串（例如前面的/uri-string)进行匹配，对特定的请求进行处理。地址定向、数据缓存和应答控制等功能，还有许多第三方木块的配置也在这里进行。

- 指令说明

  - 语法：

    ```
    location [ = | ~ | ~* | ^~ ] uri {
    
    }
    ```

  - 说明

    ```
    =：用于不含正则表达式的uri前，要求请求字符串与uri严格匹配，如果匹配成功，就停止继续向下搜索并立即处理该请求
    ~：用于表示uri包含正则表达式，并且区分大小写
    ~*：用于表示uri包含正则表达式，并且不区分大小写
    ^~：用于不含正则表达式的uri前，要求Nginx服务器找到表示uri和请字符串匹配度最高的location后，立即使用此location处理请求，而不再使用location块中的正则uri和请求字符串做匹配
    注意：如果uri包含正则表达式，则必须要有~或~*标识。
    ```

    

  

# 五、反向代理配置实例

## 5.1 反向代理实例1

- 实现效果：打开浏览器，在浏览器输入地址www.123.com，跳转到linux系统tomcat主页面。

- 效果：

  ![image-20220814200601710](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220814200601710.png)

### 5.1.1 准备工作

1. 在linux系统安装tomcat，使用默认端口8080
2. 对外开放访问端口：firewall-cmd --add-port=8080/tcp --permanent
3. 重新加载防火墙：firewall-cmd --reload
4. 在windows系统中通过浏览器访问服务器

### 5.1.2 配置反向代理

1. 在windows系统的host文件进行域名和ip对于关系的配置

   1. 位置：C盘->windows->System32->drivers->etc

   2. 添加内容如下；

      <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220814200942063.png" alt="image-20220814200942063" style="zoom:25%;" />

2. 在nginx进行请求转发的配置（反向代理配置）

   1. 编辑nginx.conf
      1. <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220814201231528.png" alt="image-20220814201231528" style="zoom:33%;" />

3. 启动nginx，输入www.123.com进行测试

## 5.2 反向代理实例2

### 5.2.1 实现效果

- 使用nginx反向代理，根据访问的路径跳转到不同端口的服务中nginx监听端口为9001
- 访问http://192.168.238.129:9001/edu/直接跳转到127.0.0.1:8081
- 访问http://192.168.238.129:9001/vod/直接跳转到127.0.0.1:8082

### 5.2.2 准备工作

1. 准备两个tomcat服务器，一个8080端口，一个8081端口

2. 配置nginx.conf，进行反向代理配置

   <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220815135157040.png" alt="image-20220815135157040" style="zoom: 67%;" />

3. 开放端口号（9001,8080,8081）

### 5.2.3 测试

- 进入两个网站进行测试。

# 六、负载均衡配置实例

## 6.1 实现效果

- 在地址栏输入地址http://192.168.238.129/edu/a.html，负载均衡效果，平均到8080和8081端口中

## 6.2 准备工作

1. 准备两台tomcat，一台端口8080和一台端口8081
2. 在两台tomcat里面webapps目录中，创建名称是edu文件夹，在edu文件夹中创建页面a.html，用于测试
3. 在nginx的配置文件nginx.conf中进行负载均衡的配置
   - <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220815110555806.png" alt="image-20220815110555806" style="zoom:33%;" />
   - <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220815110615646.png" alt="image-20220815110615646" style="zoom:33%;" />

## 6.3 负载均衡总结

### 6.3.1 Nginx提供了几种分配方式（策略）：

- 轮询（默认）

  ```
  每个请求按时间顺序逐一分配到不同的后端服务器，如果后端服务器宕机，能自动剔除。
  ```

- weight（权）

  ```
  （1）weight代表权，重默认为1，权重越高，被分配的客户端越多。
  （2）指定轮询几率，weight和访问比率成正比，用于后端服务器性能不均的情况
  ```

  - 实现方法

    <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220815111846129.png" alt="image-20220815111846129" style="zoom: 50%;" />

- ip_hash

  ```
  （1）每个请求按访问ip的hash结果分配，这样每个访客固定访问一个后端服务器，可以解决session的问题。
  （2）实现方式直接在upstream中加入语句ip_hash;
  ```

  - 实现方式

    <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220815111907439.png" alt="image-20220815111907439" style="zoom:50%;" />

- fair(第三方)

  ```
  （1）按后端服务器的响应时间来分配请求，响应时间短的优先分配
  ```

  - 实现方法：

    <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220815111805686.png" alt="image-20220815111805686" style="zoom:50%;" />

# 七、动静分离配置实例

## 7.1 注意事项

- Nginx动静分离简单来说就是把动态跟静态请求分开，不能理解成只是单纯的把动态页面和静态页面物理分离。严格意义上说应该是动态请求跟静态请求分开，可以理解成使用Nginx处理静态页面，Tomcat处理动态页面。
- 动静分离从目前实现角度大致分为两种
  - 第一种是纯粹把静态文件独立成单独的域名，放在独立的服务器上，也是主流的方案。
  - 另一种是动态跟静态文件混合在一起发布，通过nginx分开。
- 通过location指定不同的后缀名实现不同的请求转发
  - 通过expires参数设置，可以使浏览器缓存过期时间，减少与服务器之间的请求和流量。
  - 具体Expires定义：是给一个资源设定一个过期时间，也就是说无需去服务端验证，直接通过浏览器自身确认是否过期即可，所以不会产生额外的流量。此种方法非常适合不经常变动的资源。（如果经常更新的文件，不建议使用Expires来缓存），我这里设置3d，表示在这3天之内访问这个URL，发送一个请求，对比服务器该文件最后更新时间没有变化，则不会从服务器抓取，返回状态码304，如果有修改，则直接从服务器重新下载，返回状态码200.

## 7.2 准备工作

1. 在Linux系统中准备静态资源，用于进行访问

2. 在nginx的配置文件中进行配置

   <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220815144005630.png" alt="image-20220815144005630" style="zoom: 50%;" />

3. 启动nginx

# 八、配置高可用的集群

## 8.1 什么是高可用

- ![image-20220815150757437](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220815150757437.png)
- 需要多台Nginx
- 需要keepalived
- 需要虚拟ip

## 8.2 配置高可用的准备工作

1. 需要两台服务器

2. 在两台服务器安装nginx（参考二）

3. 在两台服务器安装keepalived

   ```
   yum install keepalived -y
   查看是否已安装：rpm -q -a keepalived
   配置文件目录:/etc/keepalived/keepalived.conf
   ```

4. 完成高可用配置（主从配置）

   ```
   global_defs{#全局配置
   smtp_server 主机ip
   router_id LVS_DEVELBACK #访问主机，可访问/etc/hosts中添加
   }
   vrrp_script chk_http_port{#脚本配置
   	script "脚本路径"
   	interval 2 #检测脚本执行的间隔，单位秒
   	weight 2 #权重，设置权重
   }
   vrrp_instance VI_1{# 虚ip配置
   	state BACKUP # 设置主服务器或者从服务器，主为MASTER，从为BACKUP
   	interface ens33 # 绑定的网卡,通过ifconfig查看
   	virtual_router_id 51 #主、备机的virtual_router_id必须相同
   	priority 90 #主、备机取不同的优先级，主机值较大，备份机值较小
   	advert_int 1 #每隔1秒发送一次心跳，检测主机是否宕机
   	authentication {
   		auth_type PASS
   		auth_pass 1111
   	}			# 权限校验的方式
   	virtual_ipaddress {
   		ip地址  #可以绑定多个虚拟ip，指定虚拟ip
   	}
   }
   ```

# 九、Nginx原理

## 9.1 master和worker

- ![image-20220815154059288](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220815154059288.png)

- 如何工作：

  - ![image-20220815154235867](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220815154235867.png)

- 这种机制的好处

  ```
  （1）首先，对于每隔worker进程来说，独立的进程，不需要加锁，所以省掉了锁带来的开销，同时在编程以及问题查找时，也会方便很多。其次，采用独立的进程，可以让互相之间不会影响，一个进程退出后，其他进程还在工作，服务不会终端，master进程则很快启动新的worker进程。当然，worker进程的异常退出，肯定是程序有bug了，异常退出，会导致当前worker上的所有请求失败，不过不会影响到所有请求，降低了风险。
  （2）可以使用nginx -s reload 热部署
  ```

- 需要设置多少个worker

  - Nginx通redis类似都采用了io多路复用机制，每个worker都是一个独立的进程，但每个进程里只有一个主线程，通过异步非阻塞的方式来处理请求，即使是成千上万个请求也不再话下。
  - 每个worker的线程可以把一个cpu的性能发挥到机制，所以worker数和服务器的cpu数相等是最为适宜的。设少了会浪费cpu，射多了会造成cpu频繁切换上下文带来的损耗。

### 9.1.1 连接数worker_connection

- 这个值表示每个worker进程所能建立连接的最大值。
- 所以一个nginx能建议的最大连接数，应该是worker_connections * worker_processes。当然这里说的是最大连接数，对于HTTP请求本地资源来说，能够支持的最大并发数量是worker_connections * worker_processes，如果是是支持http1.1的浏览器每次访问要占两个连接，所以普通的静态访问最大并发数是worker_connections * worker_processes/2，而如果是HTTP作为反向代理来说，最大并发数量应该是worker_connections * worker_processes/4。因为作为反向代理服务器，每个并发会建立与客户端的链接和与后端服务器的链接，会占用两个连接。