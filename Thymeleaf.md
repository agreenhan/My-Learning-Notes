# 一、Thymeleaf简介

- Thymeleaf 是一个流行的模板引擎，该模板引擎采用 Java 语言开发
- 模板引擎是一个技术名词，是跨领域跨平台的概念，在 Java 语言体系下有模板引擎，在C#、PHP 语言体系下也有模板引擎，甚至在 JavaScript 中也会用到模板引擎技术，Java 生态下的模板引擎有 Thymeleaf 、Freemaker、Velocity、Beetl（国产） 等。

- Thymeleaf 对网络环境不存在严格的要求，既能用于 Web 环境下，也能用于非 Web 环境下。在非 Web 环境下，他能直接显示模板上的静态数据；在 Web 环境下，它能像 Jsp 一样从后台接收数据并替换掉模板上的静态数据。它是基于 HTML 的，以 HTML 标签为载体，Thymeleaf 要寄托在 HTML 标签下实现。

- Spring Boot 集成了 Thymeleaf 模板技术，并且 Spring Boot 官方也推荐使用 Thymeleaf 来替代 JSP 技术，Thymeleaf 是另外的一种模板技术，它本身并不属于 Spring Boot，Spring Boot只是很好地集成这种模板技术，作为前端页面的数据展示，在过去的 Java Web 开发中，我们往往会选择使用 Jsp 去完成页面的动态渲染，但是 jsp 需要翻译编译运行，效率低

- Thymeleaf 的官方网站：http://www.thymeleaf.org

- Thymeleaf 官方手册：https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html

# 二、构建项目



# 三、表达式

- 表达式是在页面获取数据的一种 thymeleaf 语法。类似 ${key}

## 3.1 标准变量表达式

- th:text="" 是 Thymeleaf 的一个属性，用于文本的显示

- 语法：${key}

  - 说明：标准变量表达式用于访问容器（tomcat）上下文环境中的变量，功能和 EL 中的 ${} 相同。Thymeleaf 中的变量表达式使用 ${变量名} 的方式获取 Controller 中 model 其中的数据。也就是 request 作用域中的数据。

- 示例

  - 后端

    ```java
    @GetMapping("/expression1")
        public String expression1(Model model){
            model.addAttribute("site","www.baidu.com");
            model.addAttribute("user", new User(1, "name", "pwd"));
    
            return "expression1";
        }
    ```

  - 前端

    ```html
    <div>
            <p th:text="${site}"></p>
            <br>
            <p>获取user对象属性值</p>
            <p th:text="${user.id}">id</p>
            <p th:text="${user.username}">用户名</p>
            <p th:text="${user.password}">密码</p>
    </div>
    ```

- 模版文件（html）修改后，可以使用 idea—Build 菜单-Recompile 编译文件。重新 Recompile即可生效。（ctrl+shift+F9）

## 3.2 选择变量表达式

- 语法：*{key}

- 说明

  - 需要配和 th:object 一起使用。选择变量表达式，也叫星号变量表达式，使用 th:object 属性来绑定对象，选择表达式首先使用 th:object 来绑定后台传来的对象，然后使用 * 来代表这个对象，后面 {} 中的值是此对象中的属性。
  - 选择变量表达式 *{...} 是另一种类似于标准变量表达式 ${...} 表示变量的方法选择变量表达式在执行时是在选择的对象上求解，而${...}是在上下文的变量 model 上求解

- 示例：

  - 前端

    ```html
    <!-- 使用*{}时，在父标签中必须指定对象 -->
    <div th:object="${user}">
            <p th:text="*{id}"></p>
            <p th:text="*{username}"></p>
            <p th:text="*{password}"></p>
    </div>
    <!-- 也可以直接调用 -->
    ```
    
  - 后端
  ```java
    @GetMapping("/expression1")
        public String expression1(Model model){
            model.addAttribute("site","www.baidu.com");
            model.addAttribute("user", new User(1, "name", "pwd"));
    
            return "expression1";
        }
  ```

## 3.3 链接表达式(URL表达式)

- 语法：@{链接 url}

- 说明：

  - 主要用于链接、地址的展示

    ```
    可用于<script src="...">、<link href="...">、<a href="...">、<form action="...">、<img src="">等，可以
    在 URL 路径中动态获取数据
    ```

- 示例：

  - 前端：

    ```
    <body>
        <a th:href="@{http://www.baidu.com}">绝对地址</a><br>
        <a th:href="@{/tpl/query01}">相对地址，无传参</a><br>
        <a th:href="@{'/tpl/query01?id=' + ${userId}}">相对地址，有传参</a><br>
        <!-- 推荐使用的传参方式 -->
        <a th:href="@{/tpl/query01(id=${userId})}">推荐传参方式</a><br>
        <a th:href="@{/tpl/query01(name='zhangsan',age=20)}">传递多个参数用逗号分隔</a>
    </body>
    ```

  - 后端：

    ```
        @RequestMapping("/query01")
        @ResponseBody
        public String query01(Integer id){
            return "传参为：" + id;
        }
    ```

# 四、Thymeleaf属性

- 大部分属性和 html 的一样，只不过前面加了一个 th 前缀。 加了 th 前缀的属性，是经过模版引擎处理的。

## 4.1 th:action

- 定义后台控制器的路径，类似<form>标签的 action 属性，主要结合 URL 表达式,获取动态变量

  ```html
  <form id="login" th:action="@{/login}" th:method="post">......</form>
  ```

## 4.2 th:method

- 设置请求方法

  ```html
  <form id="login" th:action="@{/login}" th:method="post">......</form>
  ```

## 4.3 th:href

- 定义超链接，主要结合 URL 表达式,获取动态变量

  ```html
  <a th:href="@{/query/student}">相对地址没有传参数</a>
  ```

## 4.4 th:src

- 用于外部资源引入，比如<script>标签的 src 属性，<img>标签的 src 属性，常与@{}表达式结合使用，在 SpringBoot 项目的静态资源都放到 resources 的 static 目录下，放到 static 路径下的内容，写路径时不需要写上 static

  ```html
  <script type="text/javascript" th:src="@{/js/jquery-3.4.1.js}"></script>
  ```

## 4.5 th:text

- 用于文本的显示，该属性显示的文本在标签体中，如果是文本框，数据会在文本框外显示，要想显示在文本框内，使用 th:value

  ```html
  <input type="text" id="realName" name="reaName" th:text="${realName}">
  ```

## 4.6 th:style

- 设置样式

  ```html
  <a th:onclick="'fun1('+${user.id}+')'" th:style="'color:red'">点击我</a>
  ```

# 五、th:each

- 这个属性非常常用，比如从后台传来一个对象集合那么就可以使用此属性遍历输出，它与JSTL 中的<c: forEach>类似，此属性既可以循环遍历集合，也可以循环遍历数组及 Map。
- 语法说明：th:each="user, iterStat : ${userlist}"中的 ${userList} 是后台传过来的集合
  - user：定义变量，去接收遍历${userList}集合中的一个数据
  - iterStat：${userList} 循环体的信息
  - 其中 user 及 iterStat 自己可以随便取名
  - iterStat 是循环体的信息，通过该变量可以获取如下信息
    - index: 当前迭代对象的 index（从 0 开始计算）
    - count: 当前迭代对象的个数（从 1 开始计算）这两个用的较多
    - size: 被迭代对象的大小
    - current: 当前迭代变量
    - even/odd: 布尔值，当前循环是否是偶数/奇数（从 0 开始计算）
    - first: 布尔值，当前循环是否是第一个
    - last: 布尔值，当前循环是否是最后一个
  - 注意：循环体信息 interStat 也可以不定义，则默认采用迭代变量加上 Stat 后缀，即userStat

## 5.1 List循环

- 前端：

  ```html
          <h3>List循环遍历</h3>
          <!-- 第一种方式： -->
          <div>
              <p th:each="user,state:${userList}" th:text="${user} + ${state.index} + '当前迭代数量' + ${state.count} + '大小：' + ${state.size}"></p>
          </div>
          <!-- 第二种方式： -->
          <div th:each="u:${userList}">
              <p th:text="${u.id}"></p>
              <p th:text="${u.username}"></p>
              <p th:text="${u.password}"></p>
          </div>
  ```

- 后端：

  ```java
          List<User> userList = new ArrayList<>();
          userList.add(0, new User(1, "用户1", "密码1"));
          userList.add(1, new User(2, "用户2", "密码2"));
          userList.add(2, new User(3, "用户3", "密码3"));
          userList.add(3, new User(4, "用户4", "密码4"));
          request.setAttribute("userList", userList);
  ```

## 5.2 Array循环

- 前端：

  ```html
          <h3>数组遍历</h3>
          <div>
              <p th:each="user,state:${users}" th:text="${user}"></p>
          </div>
          <div th:each="user:${users}">
              <p th:text="${user.id}"></p>
              <p th:text="${user.username}"></p>
              <p th:text="${user.password}"></p>
          </div>
  ```

- 后端：

  ```java
  		User[] users = new User[4];
          users[0] = new User(1, "飞飞1", "密码01");
          users[1] = new User(2, "飞飞2", "密码02");
          users[2] = new User(3, "飞飞3", "密码03");
          users[3] = new User(4, "飞飞4", "密码04");
          request.setAttribute("users", users);
  ```

## 5.3 Map循环

- 前端

  ```html
  		<h3>Map遍历</h3>
          <div th:each="user:${userMap}">
              <p th:text="${user.key}"></p>
              <p th:text="${user.value.id}"></p>
              <p th:text="${user.value.username}"></p>
              <p th:text="${user.value.password}"></p>
          </div>
  ```

- 后端

  ```java
  		Map<String,User> userMap = new HashMap<>();
          userMap.put("user1",new User(1, "安兹1", "密码001"));
          userMap.put("user2",new User(2, "安兹2", "密码002"));
          userMap.put("user3",new User(3, "安兹3", "密码003"));
          userMap.put("user4",new User(4, "安兹4", "密码004"));
          request.setAttribute("userMap", userMap);
  ```

# 六、条件判断if

- 语法：th:if=”boolean 条件” ， 条件为 true 显示体内容

- th:unless 是 th:if 的一个相反操作

- 空字符串是true

- null是false

- 使用示例：

  - 前端

    ```html
    <body>
            <!-- 可以这样写 -->
        	<p th:if="${sex} == '男'">是男的</p>
            <p th:if="${name} != ''">名字</p>
            <p th:if="${isLogin} == true">登陆了</p>
            <p th:if="${age} == 20">年龄是20</p>
            <p th:unless="${sex} != '男'">男</p>
        
        	<!-- 也可以这样写 -->
            <p th:if="${sex == '男'}">是男的</p>
            <p th:if="${name != ''}">名字</p>
            <p th:if="${isLogin == true}">登陆了</p>
            <p th:if="${age == 20}">年龄是20</p>
            <p th:unless="${sex != '男'}">男</p>
    </body>
    ```

  - 后端

    ```java
        @RequestMapping("/if")
        public String ifTest(HttpServletRequest request){
            request.setAttribute("sex", "男");
            request.setAttribute("isLogin", true);
            request.setAttribute("age", 20);
            request.setAttribute("name", "名字");
            return "if-test";
        }
    ```

# 七、switch，case判断语句

- 语法：类似java中的switch-case

- 说明：一旦某个 case 判断值为 true，剩余的 case 则都当做 false，“*”表示默认的case，前面的 case 都不匹配时候，执行默认的 case

- 示例：

  ```html
          <div th:switch="${sex}">
              <p th:case="男">男</p>
              <p th:case="女">女</p>
              <p th:case="*">保密</p>
          </div>
  ```

# 八、th:inline

- th:inline 有三个取值类型 (text, javascript 和 none)

### 8.1 内联text

- 可以让 Thymeleaf 表达式不依赖于 html 标签，直接使用**内敛表达式**[[表达式]]**即可获取动态数据，要求在父级标签上加 **th:inline = “text”属性

- 示例

  ```html
      <body>
          <div th:inline="text">
              姓名：[[${name}]]<br>
              年龄：[[${age}]]<br>
              登录状态：[[${isLogin}]]<br>
          </div>
      </body>
  
  或者：
  
      <body>
          <div>
              姓名：[[${name}]]<br>
              年龄：[[${age}]]<br>
              登录状态：[[${isLogin}]]<br>
          </div>
      </body>
  ```

### 8.2 内联JavaScript

- 可以在 js 中，获取模版中的数据。

- 示例：

  ```html
  <input type="button" onclick="fun()" value="按钮"/>
  
  <script type="text/javascript" th:src="@{/js/jquery-3.4.1.js}"></script>
  <script type="text/javascript" th:inline="javascript">
              function fun(){
                  var name = [[${name}]];
                  var age = [[${age}]];
                  var isLogin = [[${isLogin}]];
                  alert("姓名：" + name + ",年龄:" + age + ",登录状态：" + isLogin);
              }
  </script>
  ```

  

# 九、字面量

### 9.1 文本字面量

- 用单引号'...'包围的字符串为文本字面量

- 示例：

  ```html
  <p th:text="'省：' + ${province} + '市：' + ${city}"></p>
  ```

### 9.2 数字字面量

- 示例

  ```html
  <p th:if="${age > 10}">age > 10</p>
  ```

### 9.3 boolean字面量

- 示例

  ```html
  <p th:if="${isLogin == true}">已登录</p>
  ```

### 9.4 null字面量

- 示例

  ```html
  <p th:if="${name == null}">name为null</p>
  ```

# 十、字符串连接

- 第一种方式：单引号和'+'拼接

  - 示例：

    ```html
    <p th:text="'省：' + ${province} + '市：' + ${city}"></p>
    ```

- 第二种方式：|中间为拼接内容|

  - 示例：

    ```html
    <p th:text="|${province}省${city}市|"></p>
    ```

# 十一、运算符

- 算术运算符：+，-，*，/，%
- 关系比较：>,<,>=,<=(gt,lt,ge,le)
- 相等判断：==,!=(eq,ne)
- 三目运算符：? :

# 十二、Thymeleaf基本对象

- 模板引擎提供了一组内置的对象，这些内置的对象可以直接在模板中使用，这些对象由#号开始引用，我们比较常用的内置对象

## 12.1 #request

- 表示HttpServletRequest

- 示例：

  ```html
  <p th:text="${#request.getAttribute('requestData')}">request作用域</p>
  <p th:text="${#request.getContextPath()}"></p>
  <p th:text="${#request.getServerName()}"></p>
  <p th:text="${#request.getServerPort()}"></p>
  ```

## 12.2 #session

- 表示HttpSession对象

  ```html
  <p th:text="${#session.getAttribute('sessionData')}">session作用域</p>
  ```

## 12.3 session

- 表示HttpSession对象，是#session的简单表示方式

- 实际上是map，只用于获取session作用域中的值，如果要使用session的方法，只能使用#session

  ```html
  <p th:text="${session.sessionData}"></p>
  ```

# 十三、Thymeleaf内置工具类对象

## 13.1 内置工具类对象

- 模板引擎提供的一组功能性内置对象，可以在模板中直接使用这些对象提供的功能方法

- 工作中常使用的数据类型，如集合，时间，数值，可以使用 Thymeleaf 的提供的功能性对象来处理它们

- 内置功能对象前都需要加#号，内置对象一般都以 s 结尾

- 官方手册：http://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html

  ```
  #dates: java.util.Date 对象的实用方法，<span th:text="${#dates.format(curDate, 'yyyy-MM-dd 
  HH:mm:ss')}"></span>
  #calendars: 和 dates 类似, 但是 java.util.Calendar 对象；
  #numbers: 格式化数字对象的实用方法；
  #strings: 字符串对象的实用方法： contains, startsWith, prepending/appending 等；
  #objects: 对 objects 操作的实用方法；
  #bools: 对布尔值求值的实用方法；
  #arrays: 数组的实用方法；
  #lists: list 的实用方法，比如<span th:text="${#lists.size(datas)}"></span>
  #sets: set 的实用方法；
  #maps: map 的实用方法；
  #aggregates: 对数组或集合创建聚合的实用方法
  ```

### 13.1.1 #dates

- 示例：

  ```html
  <div>
      <p th:text="${#dates.format(myDate,'yyyy-MM-dd')}"></p>
      <p th:text="${#dates.format(myDate,'yyyy-MM-dd HH:mm:ss')}"></p>
      <p th:text="${#dates.year(myDate)}"></p>
      <p th:text="${#dates.month(myDate)}"></p>
      <p th:text="${#dates.day(myDate)}"></p>
      <p th:text="${#dates.createNow()}"></p>
  </div>
  ```

### 13.1.2 #numbers

- 示例：

  ```html
  <div>
      <p th:text="${#numbers.formatCurrency(myNum)}"></p>
      <p th:text="${#numbers.formatDecimal(myNum,5,2)}"></p>
  </div>
  ```

### 13.1.3 #strings

- 示例：

  ```html
  <div>
      <p th:text="${#lists.isEmpty(myList)}"></p>
      <p th:if="!${#lists.isEmpty(myList)}">list非空</p>
      <p th:text="${#lists.size(myList)}"></p>
  </div>
  ```

### 13.1.4 null处理

- 示例：?判断是否为null，如果为null，则不执行

  ```
  <p th:text="${zoo?.dog?.name}"></p>
  ```

# 十四、内容复用

- 自定义模板是复用的行为。可以把一些内容，多次重复使用。

## 14.1 定义模板

- 语法：th:fragment="top" , 定义模板，自定义名称是 top

- 示例：

  ```html
  <div th:fragment="top">
      <p>测试</p>
      <p>www.baidu.com</p>
  </div>
  ```

## 14.2 引用模板

- 语法：~{ templatename :: selector} 或者 templatename :: selector

- 说明：

  - templatename 表示html文件名，selector表示模板的自定义名称
  - include会全部替换原有标签，但是insert会保留父标签

- 示例：

  ```html
  <div th:insert="~{utilObject::top}"></div>
  或者
  <div th:include="utilObject::top"></div>
  ```

## 14.3 整个html文件作为模板

- 第一种方式：

  ```html
  <div th:include="utilObject::html"></div>
  或
  <div th:insert="utilObject::html"></div>
  ```

- 第二种方式：

  ```html
  <div th:include="utilObject"></div>
  或
  <div th:insert="utilObject"></div>
  ```

## 14.4 使用其他目录中的模板

- 第一种方式

  ```
  <div th:insert="common/left::html"></div>
  说明：
  （1）common是目录
  （2）left是html文件名
  ```

- 第二种方式

  ```
  <div th:include="common/left"></div>
  说明：
  （1）common是目录
  （2）left是html文件名
  ```

  