# 1. 介绍

## 1.1 Vue.js是什么

- Vue (读音 /vjuː/，类似于 view) 是一套用于构建用户界面的渐进式框架。

- Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。另一方面，当与现代化的工具链以及各种支持类库结合使用时，Vue 也完全能够为复杂的单页应用提供驱动。

- 官方网站：https://cn.vuejs.org

## 1.2 初试Vue.js

- 创建demo.html

  ```
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>Document</title>
  </head>
  <body>
      <div id="app">
          {{message}}
      </div>
  
      <script type="text/javascript" src="vue.min.js"></script>
      <script>
          // 创建一个vue对象
          new Vue({
              el: '#app',
              data: {
                  message: 'hello Vue!'
              }
          })
      </script>
  </body>
  </html>
  ```

- 这就是声明式渲染：Vue.js 的核心是一个允许采用简洁的模板语法来声明式地将数据渲染进 DOM 的系统

- 这里的核心思想就是没有繁琐的DOM操作，例如jQuery中，我们需要先找到div节点，获取到DOM对象，然后进行一系列的节点操作

- 在vscode中创建代码片段
  - 文件 => 首选项 => 用户代码片段 => 新建全局代码片段/或文件夹代码片段：vue-html.code-snippets
  - 注意：制作代码片段的时候，字符串中如果包含文件中复制过来的“Tab”键的空格，要换成“空格键”的空格

# 2. 基本语法

## 2.1 基本数据渲染和指令

- v-bind 特性被称为指令。指令带有前缀 v-除了使用插值表达式{{}}进行数据渲染，也可以使用 v-bind指令，它的简写的形式就是一个冒号

  ```html
  <body>
      <div id="app">
  	    <!-- 如果要将模型数据绑定在html属性中，则使用 v-bind 指令
  		 此时title中显示的是模型数据
  		-->
          <h1 v-bind:title="message">
              {{content}}
          </h1>
     		<!-- v-bind 指令的简写形式： 冒号（:） -->
          <h2 :title="message">
              {{content}}
          </h2>
      </div>
      <script type="text/javascript" src="vue.min.js"></script>
      <script>
          // 创建一个vue对象
          new Vue({
              el: '#app',
              data: {
                  content: '我是标题',
                  message: '我是信息'
              }
          })
      </script>
  </body>
  </html>
  ```

## 2.2 双向数据绑定

- 使用 v-model 进行双向数据绑定

- 示例

  ```html
  <body>
      <div id="app">
          <!-- v-bind:value只能进行单向的数据渲染 -->
          <input type="text" v-bind:value="content"/>
          <!-- v-model 可以进行双向的数据绑定 -->
          <input type="text" v-model="content"/>
      </div>
      <script type="text/javascript" src="vue.min.js"></script>
      <script>
          // 创建一个vue对象
          new Vue({
              el: '#app',
              data: {
                  content: '我是标题',
                  message: '我是信息'
              }
          })
      </script>
  </body>
  ```

## 2.3 事件

- 使用 v-on 进行数件处理，v-on:click 表示处理鼠标点击事件，事件调用的方法定义在 vue 对象声明的 methods 节点中

- 示例

  ```html
  <body>
      <div id="app">
          <input type="text" v-bind:value="searchMap.keyWord"/>
          <!-- v-on 指令绑定事件，click指定绑定的事件类型，事件发生时调用vue中methods节点中定义的方法 -->
          <input type="button" value="搜索" @click="search()"/>
          <p><a v-bind:href="resultMap.site" target="_blank">{{resultMap.title}}</a></p>
      </div>
      <script type="text/javascript" src="vue.min.js"></script>
      <script>
          // 创建一个vue对象
          new Vue({
              el: '#app',
              data: {
                  searchMap: {
                      keyWord: '关键字'
                  },
                  resultMap: {}
              },
              methods: {
                  search(){
                      console.log('搜索中...')
                      this.resultMap = {
                          "title": "百度",
                          "site": "https://www.baidu.com"
                      }
                  }
              }
          })
      </script>
  </body>
  ```

### 2.3.1 事件修饰符

```
prevent:阻止默认事件
stop:阻止事件冒泡
once:事件只触发一次
capture:使用事件的捕获模式
self:只有event.target是当前操作的元素时才触发事件
passive:事件的默认行为立即执行，无需等待事件回调执行完毕
```

### 2.3.2 键盘事件

```
(1)按键别名：
回车 => enter
删除 => delete (退格和删除)
退出 => esc
空格 => space
换行 => tab (必须配合keydown使用)
上  =>  up
下 => down
左 => left
右 => right
(2)Vue未提供别名的按键，可以使用按键原始的key值去绑定，但注意要转为kebab-case（短横线命名）
(3)系统修饰符（用法特殊)：ctrl、alt、shift、meta
	1、配合keyup使用，按下修饰键的同时，再按下其他键，随后释放其他键，事件才会被触发
	2、配合keydown使用，正常触发按键
(4)也可以使用keyCode去指定具体的按键(不推荐)
(5)Vue.config.keyCodes.自定义键名 = 键码，可以去定制按键别名
```



## 2.4 修饰符

- 修饰符 (Modifiers) 是以半角句号（.）指明的特殊后缀，用于指出一个指令应该以特殊方式绑定。例如，.prevent 修饰符告诉 v-on 指令对于触发的事件调用 event.preventDefault()：即阻止事件原本的默认行为

- 示例

  ```
  <body>
      <div id="app">
          <!-- 修饰符用于指出一个指令应该以特殊方式绑定。这里的 .prevent 修饰符告诉 v-on 指令对于触发的事件调用js的event.preventDefault()：即阻止表单提交的默认行为 -->
          <form action="save" v-on:submit.prevent="onSubmit">
              <label form="username">
                  <input type="text" id="username" v-model="user.username"/>
                  <input type="submit" value="保存"/> 
              </label>
          </form>
      </div>
      <script type="text/javascript" src="vue.min.js"></script>
      <script>
          new Vue({
              el: "#app",
              data: {
                  user: {}
              },
              methods: {
                  onSubmit(){
                      if(this.user.username){
                          console.log('提交表单')
                      } else {
                          alert('请输入用户名')
                      }
                  }
              }
          })
      </script>
  </body>
  ```

## 2.5 条件渲染

- v-if：条件指令

  ```
  <body>
      <div id="app">
          <input type="checkbox" v-model="ok"></input>
          <h1 v-if="ok">显示</h1>
          <h1 v-else>隐藏</h1>
      </div>
      <script type="text/javascript" src="vue.min.js"></script>
      <script>
          new Vue({
              el: '#app',
              data: {
                  ok: false
              }
          })
      </script>
  </body>
  ```

- v-show：条件指令，使用v-show完成和上面相同的功能

  ```
  <body>
      <div id="app">
          <input type="checkbox" v-model="ok"></input>
          <h1 v-show="ok">显示</h1>
          <h1 v-show="!ok">隐藏</h1>
      </div>
      <script type="text/javascript" src="vue.min.js"></script>
      <script>
          new Vue({
              el: '#app',
              data: {
                  ok: false
              }
          })
      </script>
  </body>
  ```

- v-if 是“真正”的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。

- v-if 也是**惰性的**：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。

- 相比之下，v-show 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换。

- 一般来说，v-if 有更高的切换开销，而 v-show 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 v-show 较好；如果在运行时条件很少改变，则使用 v-if 较好。

## 2.6 列表渲染

- v-for：列表循环指令

- 简单的列表渲染

  ```
  <body>
      <div id="app">
          <ul>
              <li v-for="(n,index) in 10">{{n}}----{{index}}</li>
          </ul>
          <ul>
              <li v-for="n in 10">{{n}}</li>
          </ul>
      </div>
      <script type="text/javascript" src="vue.min.js"></script>
      <script>
          new Vue({
              el: '#app',
              data: {}
          })
      </script>
  </body>
  ```

- 遍历数据列表

  ```
  <body>
      <div id="app">
          <table border="1">
              <tr v-for="user in userList">
                  <td>{{user.id}}</td>
                  <td>{{user.username}}</td>
                  <td>{{user.age}}</td>
              </tr>
          </table>
      </div>
      <script type="text/javascript" src="vue.min.js"></script>
      <script>
          new Vue({
              el: '#app',
              data: {
                  userList: [
                      {id: 1, username: '张三', age: 18},
                      {id: 2, username: '李四', age: 19},
                      {id: 3, username: '王五', age: 20}
                  ]
              }
          })
      </script>
  </body>
  ```

# 3. 组件

- 组件（Component）是 Vue.js 最强大的功能之一。
- 组件可以扩展 HTML 元素，封装可重用的代码。
- 组件系统让我们可以用独立可复用的小组件来构建大型应用，几乎任意类型的应用的界面都可以抽象为一个组件树：

![image-20220909183738787](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220909183738787.png)

## 3.1 局部组件

- 定义组件

  ```
      <script src="vue.min.js"></script>
      <script>
          var app = new Vue({
              el: '#app',
              // 定义局部组件，这里可以定义多个局部组件
              components: {
                  // 组件的名字
                  'Navbar': {
                      // 组件的内容
                      template: '<ul><li>首页</li><li>学员管理</li></ul>'
                  }
              }
          })
      </script>
  ```

- 使用组件

  ```
  <div id="app">
  	<Navbar></Navbar>
  </div>
  ```

## 3.2 全局组件

- 定义全局组件,位置：components/Navbar.js

  ```
   // 定义全局组件
  Vue.component('Navbar', {
      template: '<ul><li>首页</li><li>学员管理</li><li>讲师管理</li></ul>'
  })
  ```

- 使用组件

  ```
  <body>
      <div id="app">
          <Navbar></Navbar>
      </div>
      <script src="vue.min.js"></script>
      <script src="component/Navbar.js"></script>
      <script>
          var app = new Vue({
              el: '#app'
          })
      </script>
  </body>
  ```

# 4. 实例生命周期

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220909191201803.png" alt="image-20220909191201803" style="zoom: 67%;" />

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220909191213461.png" alt="image-20220909191213461" style="zoom:67%;" />

## 4.1 分析生命周期相关方法的执行时机

```
//===创建时的四个事件
beforeCreate() { // 第一个被执行的钩子方法：实例被创建出来之前执行
console.log(this.message) //undefined
this.show() //TypeError: this.show is not a function
// beforeCreate执行时，data 和 methods 中的 数据都还没有没初始化
},
created() { // 第二个被执行的钩子方法
console.log(this.message) //床前明月光
this.show() //执行show方法
 // created执行时，data 和 methods 都已经被初始化好了！
 // 如果要调用 methods 中的方法，或者操作 data 中的数据，最早，只能在 created 中操作
 },
 beforeMount() { // 第三个被执行的钩子方法
 console.log(document.getElementById('h3').innerText) //{{ message }}
 // beforeMount执行时，模板已经在内存中编辑完成了，尚未被渲染到页面中
 },
 mounted() { // 第四个被执行的钩子方法
 console.log(document.getElementById('h3').innerText) //床前明月光
 // 内存中的模板已经渲染到页面，用户已经可以看见内容
 },


 //===运行中的两个事件
 beforeUpdate() { // 数据更新的前一刻
 console.log('界面显示的内容：' + document.getElementById('h3').innerText)
 console.log('data 中的 message 数据是：' + this.message)
 // beforeUpdate执行时，内存中的数据已更新，但是页面尚未被渲染
 },
 updated() {
 console.log('界面显示的内容：' + document.getElementById('h3').innerText)
 console.log('data 中的 message 数据是：' + this.message)
 // updated执行时，内存中的数据已更新，并且页面已经被渲染
 }
```

# 5. 路由

- Vue.js 路由允许我们通过不同的 URL 访问不同的内容。
- 通过 Vue.js 可以实现多视图的单页Web应用（single page web application，SPA）。
- Vue.js 路由需要载入 vue-router 库

## 5.1 示例

- 引入js

  ```
      <script src="vue.min.js"></script>
      <script src="vue-router.min.js"></script>
  ```

- 编写html

  ```
  <div id="app">
          <h1>Hello Route</h1>
          <p>
              <!-- 使用 router-link 组件来导航. -->
              <!-- 通过传入 `to` 属性指定链接. -->
              <!-- <router-link> 默认会被渲染成一个 `<a>` 标签 -->
              <router-link to="/">首页</router-link>
              <router-link to="/student">会员管理</router-link>
              <router-link to="/teacher">讲师管理</router-link></router-link>
          </p>
          <!-- 路由出口 -->
          <!-- 路由匹配到的组件将渲染在这里 -->
          <router-view></router-view>
      </div>
  ```

  

- 编写js

  ```
  <script>
          // 1. 定义路由组件
          // 可以从其他文件import进来
          const Welcome = { template: '<div>欢迎</div>'}
          const Student = { template: '<div>student</div>'}
          const Teacher = { template: '<div>teacher</div>'}
          // 2. 定义路由
          // 每个路由应该映射一个组件
          const routes = [
              { path: "/", redirect: "/welcome"},// 设置默认指向的路径
              { path: "/welcome", component: Welcome},
              { path: "/student", component: Student},
              { path: "/teacher", component: Teacher}
          ]
          // 3. 创建router实例，然后传'routes'配置
          const router = new VueRouter({
              routes // 缩写，相当于routes: routes
          })
          // 4. 创建和挂载根实例，从而让整个应用都有路由功能
          const app = new Vue({
              el: '#app',
              router
          })
  </script>
  ```

# 6. axios

- axios是独立于vue的一个项目，基于promise用于浏览器和node.js的http客户端
  - 在浏览器中可以帮助我们完成 ajax请求的发送
  - 在node.js中可以向远程接口发送请求

## 6.1 编写html

```
<body>
    <div id="app">
        <table border="1px">
            <tr>
                <td>姓名</td>
                <td>年龄</td>
            </tr>
            <tr v-for="user in userList">
                <td>{{user.name}}</td>
                <td>{{user.age}}</td>
            </tr>
        </table>
    </div>
    <script src="vue.min.js"></script>
    <script src="axios.min.js"></script>
    <script>
        new Vue({
            el: '#app',
            // 固定结构
            data: { // 在data定义变量和初始值
                userList: []
            },
            created(){ // 页面渲染之前执行
                // 调用定义的方法
                this.getList()
            },
            methods: { // 编写具体的方法
                getList(){
                    // 使用axios发送ajax请求
                    // axios.提交方式("请求接口路径")
                    axios.get("return.json")
                         .then(response =>{
                            //console.log(response)
                            this.userList = response.data.data.items
                        })// 请求成功执行then方法
                         .catch(error =>{

                         })// 请求失败执行catch方法
                }
            }
        })
    </script>
</body>
```

## 6.2 数据文件（.json)

```
{
    "success":true,
    "code":200,
    "message":"成功",
    "data":{
        "items":[
            {"name":"张三","age":20},
            {"name":"李四","age":30},
            {"name":"王五","age":40}
        ]
    }
}
```

# 7. element-ui

- element-ui 是饿了么前端出品的基于 Vue.js的 后台组件库，方便程序员进行页面快速布局和构建
- 官网： http://element-cn.eleme.io/#/zh-CN

## 7.1 引入css

```
<!-- import CSS -->
<link rel="stylesheet" href="element-ui/lib/theme-chalk/index.css">
```

## 7.2 引入js

```
<!-- import Vue before Element -->
<script src="vue.min.js"></script>
<!-- import JavaScript -->
<script src="element-ui/lib/index.js"></script>
```

## 7.3 编写html

```
<div id="app">
	<el-button @click="visible = true">Button</el-button>
	<el-dialog :visible.sync="visible" title="Hello world">
		<p>Try Element</p>
	</el-dialog>
</div>
```

关于.sync的扩展阅读

https://www.jianshu.com/p/d42c508ea9de

## 7.4 编写js

```
<script>
	new Vue({
		el: '#app', 
		data: function () {//定义Vue中data的另一种方式
			return { visible: false }
		}
	})
</script>
```

# 8 Node.js

## 8.1 什么是Node.js

- 简单的说 Node.js 就是运行在服务端的 JavaScript。
- Node.js是一个事件驱动I/O服务端JavaScript环境，基于Google的V8引擎，V8引擎执行Javascript的速度非常快，性能非常好。
- 浏览器的内核包括两部分核心：
  - DOM渲染引擎；
  - js解析器（js引擎）
    - js运行在浏览器中的内核中的js引擎内部
    - Node.js是脱离浏览器环境运行的JavaScript程序，基于V8 引擎（Chrome 的 JavaScript的引擎）

## 8.2 Node.js有什么用

- 如果你是一个前端程序员，你不懂得像PHP、Python或Ruby等动态编程语言，然后你想创建自己的服务，那么Node.js是一个非常好的选择。
- Node.js 是运行在服务端的 JavaScript，如果你熟悉Javascript，那么你将会很容易的学会Node.js。当然，如果你是后端程序员，想部署一些高性能的服务，那么学习Node.js也是一个非常好的选择。

## 8.3 安装

### 8.3.1 下载

- 官网：https://nodejs.org/en/
- 中文网：http://nodejs.cn/

### 8.3.2 安装

### 8.3.3 查看版本

```
node -v
```

## 8.4 快速入门

### 8.4.1 创建文件夹nodejs

### 8.4.2 控制台程序

```
console.log('Hello Node.js')
```

- 打开命令行终端

- 进入程序所在的目录，输入

  ```
  node 文件名.js
  ```

### 8.4.3 服务器端应用开发

- 创建js文件

  ```html
  const http = require('http');
  http.createServer(function(request, response){
      // 发送HTTP头部
      // HTTP状态值：200：ok
      // 内容类型：text/plain
      response.writeHead(200, {'Content-Type': 'text/plain'});
      // 发送响应数据 "Hello World"
      response.end('Hello World');
  }).listen(8888);
  console.log('Server running at http://127.0.0.1:8888/');
  ```

# 9. NPM

## 9.1 什么是NPM

- NPM全称Node Package Manager，是Node.js包管理工具，是全球最大的模块生态系统，里面所有的模块都是开源免费的；也是Node.js的包管理工具，相当于前端的Maven 。

## 9.2 NPM工具的安装位置

- 我们通过npm 可以很方便地下载js库，管理前端工程。
- Node.js默认安装的npm包和工具的位置：Node.js目录\node_modules
  - 在这个目录下你可以看见 npm目录，npm本身就是被NPM包管理器管理的一个工具，说明Node.js已经集成了npm工具

## 9.3 使用npm管理项目

### 9.3.1 创建文件夹npm

### 9.3.2 项目初始化

```
#建立一个空文件夹，在命令提示符进入该文件夹 执行命令初始化
npm init
#按照提示输入相关信息，如果是用默认值则直接回车即可。
#name: 项目名称
#version: 项目版本号
#description: 项目描述
#keywords: {Array}关键词，便于用户搜索到我们的项目
#最后会生成package.json文件，这个是包的配置文件，相当于maven的pom.xml
#我们之后也可以根据需要进行修改。
#如果想直接生成 package.json 文件，那么可以使用命令
npm init -y
```

### 9.3.3 修改npm镜像

- NPM官方的管理的包都是从 http://npmjs.com下载的，但是这个网站在国内速度很慢。

- 这里推荐使用淘宝 NPM 镜像 http://npm.taobao.org/ ，淘宝 NPM 镜像是一个完整 npmjs.com 镜像，同步频率目前为 10分钟一次，以保证尽量与官方服务同步。

- 设置镜像地址

  ```
  #经过下面的配置，以后所有的 npm install 都会经过淘宝的镜像地址下载
  npm config set registry https://registry.npm.taobao.org
  #查看npm配置信息
  npm config list
  ```

### 9.3.4 npm install命令的使用

```
#使用 npm install 安装依赖包的最新版，
#模块安装的位置：项目目录\node_modules
#安装会自动在项目目录下添加 package-lock.json文件，这个文件帮助锁定安装包的版本
#同时package.json 文件中，依赖包会被添加到dependencies节点下，类似maven中的<dependencies>
npm install jquery
#npm管理的项目在备份和传输的时候一般不携带node_modules文件夹
npm install #根据package.json中的配置下载依赖，初始化项目
#如果安装时想指定特定的版本
npm install jquery@2.1.x
#devDependencies节点：开发时的依赖包，项目打包到生产环境的时候不包含的依赖
#使用 -D参数将依赖添加到devDependencies节点
npm install --save-dev eslint
#或
npm install -D eslint
#全局安装
#Node.js全局安装的npm包和工具的位置：用户目录\AppData\Roaming\npm\node_modules
#一些命令行工具常使用全局安装的方式
npm install -g webpack
```

### 9.3.5 其他命令

```
#更新包（更新到最新版本）
npm update 包名
#全局更新
npm update -g 包名
#卸载包
npm uninstall 包名
#全局卸载
npm uninstall -g 包名
```

# 10. Babel

## 10.1 简介

- Babel是一个广泛使用的转码器，可以将ES6代码转为ES5代码，从而在现有环境执行执行。这意味着，你可以现在就用 ES6 编写程序，而不用担心现有环境是否支持。

## 10.2 安装

### 10.2.1 安装命令行转码工具

- Babel提供babel-cli工具，用于命令行转码。它的安装命令如下：

  ```
  npm install --global babel-cli
  #查看是否安装成功
  babel --version
  ```

## 10.3 Babel的使用

### 10.3.1 初始化项目

```
npm init -y
```

### 10.3.2 创建es6代码文件

```
let input = [1,2,3]
input = input.map(item => item + 1)
console.log(input)
```

### 10.3.3 配置.babelrc

- Babel的配置文件是.babelrc，存放在项目的根目录下，该文件用来设置转码规则和插件，基本格式如下。

  ```
  { 
  	"presets": [],
  	"plugins": []
  }
  ```

- presets字段设定转码规则，将es2015规则加入 .babelrc：

  ```
  {
      "presets": ["es2015"],
      "plugins": []
  }
  ```

### 10.3.4 安装转码器

- 在项目中安装

  ```
  npm install --save-dev babel-preset-es2015
  ```

### 10.3.5 转码

```
# 转码结果写入一个文件
# --out-file 或 -o 参数指定输出文件
babel src/example.js --out-file dist1/compiled.js
# 或者
babel src/example.js -o dist1/compiled.js

# 整个目录转码
# --out-dir 或 -d 参数指定输出目录
babel src --out-dir dist2
# 或者
babel src -d dist2
```

# 11. 模块化

## 11.1 模块化产生的背景

- Javascript模块化编程，已经成为一个迫切的需求。理想情况下，开发者只需要实现核心的业务逻辑，其他都可以加载别人已经写好的模块。
- 但是，Javascript不是一种模块化编程语言，它不支持"类"（class），包（package）等概念，更遑论"模 块"（module）了

## 11.2 什么是模块化开发

- 传统非模块化开发有如下的缺点：
  - 命名冲突
  - 文件依赖
- 模块化规范：
  - CommonJS模块化规范
  - ES6模块化规范

## 11.3 CommonJS模块规范

- 每个文件就是一个模块，有自己的作用域。在一个文件里面定义的变量、函数、类，都是私有的，对其他文件不可见。

### 11.3.1 创建文件夹

### 11.3.2 导出模块

- 创建common-js模块化/四则运算.js

  ```
  // 定义成员
  const sum = function(a,b){
      return parseInt(a) + parseInt(b)
  };
  const substract = function(a,b){
      return parseInt(a) - parseInt(b)
  }
  const mulitply = function(a,b){
      return parseInt(a) * parseInt(b)
  }
  const divide = function(a,b){
      return parseInt(a) / parseInt(b)
  }
  ```

- 导出模块中的成员

  ```
  module.exports = {
      sum: sum,
      substract: substract,
      mulitply: mulitply,
      divide: divide
  }
  ```

- 简写

  ```
  module.exports = {
      sum,
      substract,
      mulitply,
      divide
  }
  ```

### 11.3.3 导入模块

- 创建common-js模块化/引入模块.js

  ```
  const m = require('./四则运算.js')
  console.log(m)
  const result1 = m.sum(1,2)
  const result2 = m.mulitply(3,3)
  console.log(result1, result2)
  ```

### 11.3.4 运行程序

- CommonJS使用exports和require来导出和导入模块。

```
node .\引入模块.js
```

## 11.4 ES6模块化规范

### 11.4.1 导出模块

- 创建es6模块化/userApi.js

  ```
  export function getList() {
  	console.log('获取数据列表')
  }
  export function save() {
  	console.log('保存数据')
  }
  ```

### 11.4.2 导入模块

- 创建es6模块化/userComponent.js

  ```
  //只取需要的方法即可，多个方法用逗号分隔
  import { getList, save } from "./userApi.js"
  getList()
  save()
  ```

- **注意：这时的程序无法运行的，因为ES6的模块化无法在Node.js中执行，需要用Babel编辑成ES5后再执行。**

### 11.4.3 运行程序

```
node es6模块化-dist/userComponent.js
```

## 11.5 ES6模块化的另一种写法

### 11.5.1 导出模块

```
export default {

	getList() {
	console.log('获取数据列表2') 
	},
	
	save() {
		console.log('保存数据2')
    }
}
```

### 11.5.2 导入模块

```
import user from "./userApi2.js"
user.getList()
user.save()
```

# 12. Webpack

- Webpack 是一个前端资源加载/打包工具。它将根据模块的依赖关系进行静态分析，然后将这些模块按照指定的规则生成对应的静态资源。

- 从图中我们可以看出，Webpack 可以将多种静态资源 js、css、less 转换成一个静态文件，减少了页面的请求。

  ![image-20220911093743070](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220911093743070.png)

## 12.1 全局安装

```
npm install -g webpack webpack-cli
```

## 12.2 安装后查看版本号

```
webpack -v
```

## 12.3 初始化项目

### 12.3.1 创建webpack文件夹

- 进入webpack目录，执行命令

  ```
  npm init -y
  ```

### 12.3.2 创建src文件夹

### 12.3.3 src下创建common.js

```
exports.info = function(str){
    document.write(str);
}
```

### 12.3.4 src下创建utis.js

```
exports.add = function(a,b){
    return parseInt(a) + parseInt(b)
}
```

### 12.3.5 src下创建main.js

```
const common = require('./common.js')
const utils = require('./utils.js')

common.info('Hello webpack!' + utils.add(111,555))
```

## 12.4 JS打包

### 12.4.1 webpack目录下创建配置文件webpack.config.js

- 以下配置的意思是：读取当前项目目录下src文件夹中的main.js（入口文件）内容，分析资源依赖，把相关的js文件打包，打包后的文件放入当前目录的dist文件夹下，打包后的js文件名为bundle.js

  ```
  const path = require('path')// Node.js内置模块
  module.exports = {
      entry: './src/main.js', // 配置入口文件
      output: {
          path: path.resolve(__dirname, './dist'), // 输出路径，__dirname：当前文件所在路径
          filename: 'bundle.js' // 输出文件
      }
  }
  ```

### 12.4.2 命令行执行编译命令

- 第一种方式：

  ```
  webpack #有黄色警告
  webpack --mode=development #没有警告
  #执行后查看bundle.js 里面包含了上面两个js文件的内容并惊醒了代码压缩
  ```

- 第二种方式

  - 配置项目的npm运行命令，修改package.json文件

    ```
    "scripts": {
    	//...,	
    	"dev": "webpack --mode=development"
    }
    ```

  - 运行npm命令执行打包

    ```
    npm run dev
    ```

### 12.4.3 webpack目录下创建index.html

```
<body>
    <script src="dist/bundle.js"></script>
</body>
```

### 12.4.4 浏览器中查看index.html

![image-20220911095521582](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220911095521582.png)

## 12.5 CSS打包

### 12.5.1 安装style-loader和 css-loader

- Webpack 本身只能处理 JavaScript 模块，如果要处理其他类型的文件，就需要使用 loader 进行转换。Loader 可以理解为是模块和资源的转换器。

- 首先我们需要安装相关Loader插件，css-loader 是将 css 装载到 javascript；style-loader 是让 javascript认识css

  ```
  npm install --save-dev style-loader css-loader
  ```

### 12.5.2 修改webpack.config.js

```
const path = require('path')// Node.js内置模块
module.exports = {
    entry: './src/main.js', // 配置入口文件
    output: {
        path: path.resolve(__dirname, './dist'), // 输出路径，__dirname：当前文件所在路径
        filename: 'bundle.js' // 输出文件
    },
    module: {
        rules: [
            {
                test: /\.css$/, // 打包规则应用到以css结尾的文件上
                use: ['style-loader', 'css-loader']
            }
        ]
    }
}
```

### 12.5.3 在src文件夹创建style.css

```
body{
    background-color: pink;
}
```

### 12.5.4 修改main.js

- 在第一行引入style.css

```
require('./style.css')
```

### 12.5.5 浏览器中查看index.html

![image-20220911100549745](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220911100549745.png)

# 13. vue-element-admin

## 13.1 简介

- 而vue-element-admin是基于element-ui 的一套后台管理系统集成方案。
- **功能：**https://panjiachen.github.io/vue-element-admin-site/zh/guide/#功能
- **GitHub**地址：https://github.com/PanJiaChen/vue-element-admin
- 项目在线预览：https://panjiachen.gitee.io/vue-element-admin

## 13.2 安装

```
# 解压压缩包
# 进入目录
cd vue-element-admin-master
# 安装依赖
npm install
# 启动。执行后，浏览器自动弹出并访问http://localhost:9527/
npm run dev
```

# 14. vue-admin-template

## 14.1 简介

- vueAdmin-template是基于vue-element-admin的一套后台管理系统基础模板（最少精简版），可作为模板进行二次开发。
- **GitHub**地址：https://github.com/PanJiaChen/vue-admin-template
- **建议：**你可以在 vue-admin-template 的基础上进行二次开发，把 vue-element-admin当做工具箱，想要什么功能或者组件就去 vue-element-admin 那里复制过来。

## 14.2 安装

```
# 解压压缩包
# 进入目录
cd vue-admin-template-master
# 安装依赖
npm install
# 启动。执行后，浏览器自动弹出并访问http://localhost:9528/
npm run dev
```

## 14.3 项目结构

### 14.3.1 修改项目信息

- package.json

### 14.3.2 修改端口号

- config/index.js中修改

  ```
  prot:9528
  ```

### 14.3.3 项目的目录结构

```
. 
├── build // 构建脚本
├── config // 全局配置
├── node_modules // 项目依赖模块
├── src //项目源代码
├── static // 静态资源
└── package.jspon // 项目信息和依赖配置


src
├── api // 各种接口
├── assets // 图片等资源
├── components // 各种公共组件，非公共组件在各自view下维护
├── icons //svg icon
├── router // 路由表
├── store // 存储
├── styles // 各种样式
├── utils // 公共工具，非公共工具，在各自view下维护
├── views // 各种layout
├── App.vue //***项目顶层组件***
├── main.js //***项目入口文件***
└── permission.js //认证入口
```

### 14.3.4 运行项目

```
npm run dev
```

### 14.3.5 登录页修改

- src/views/login/index.vue(登录组件)

### 14.3.6 页面零星修改

- 标题
  - index.html（项目的html入口)
  - 修改后热部署功能，浏览器自动刷新
- 国际化设置
  - 打开 src/main.js（项目的js入口），第7行，修改语言为 zh-CN，使用中文语言环境，例如：日期时间组件
- icon
  - 复制 favicon.ico 到根目录
- 导航栏文字
  - src/views/layout/components（当前项目的布局组件）
  - src/views/layout/components/Navbar.vue
- 面包屑文字
  - src/components（可以在很多项目中复用的通用组件）
  - src/components/Breadcrumb/index.vue

# 15. Eslint语法规范型检查

## 15.1 简介

- JavaScript 是一个动态的弱类型语言，在开发中比较容易出错。因为没有编译程序，为了寻找JavaScript 代码错误通常需要在执行过程中不断调适。
- ESLint 是一个语法规则和代码风格的检查工具，可以用来保证写出语法正确、风格统一的代码。让程序员在编码的过程中发现问题而不是在执行的过程中。

## 15.2 语法规则

- ESLint 内置了一些规则，也可以在使用过程中自定义规则。
- 本项目的语法规则包括：两个字符缩进，必须使用单引号，不能使用双引号，语句后不可以写分号，代码段之间必须有一个空行等。

## 15.3 确认开启语法检查

- 打开 config/index.js，配置是否开启语法检查

  ```
  useEslint: true,
  ```

- 可以关闭语法检查，建议开启，养成良好的编程习惯。

## 15.4 插件安装

- vs code的ESLint插件，能帮助我们自动整理代码格式

## 15.5 插件的扩展设置

- 选择vs code左下角的“设置”， 打开 VSCode 配置文件,添加如下配置

  ![image-20220911104736569](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220911104736569.png)

  ![image-20220911104757302](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220911104757302.png)

  ```
  "files.autoSave": "off",
  "eslint.validate": [
  	"javascript",
  	"javascriptreact",
  	"vue-html", 
  	{
  		"language": "vue",
  		"autoFix": true
  	}
  ],
  "eslint.run": "onSave",
  "eslint.autoFixOnSave": true
  ```

  