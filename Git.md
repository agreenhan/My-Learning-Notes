# Git

![image-20220726153523495](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220726153523495.png)

## Git常用命令

| 命令名称                             | 作用           |
| ------------------------------------ | -------------- |
| git config --global user.name 用户名 | 设置用户签名   |
| git config --global user.email 邮箱  | 设置用户签名   |
| git init                             | 初始化本地库   |
| git status                           | 查看本地库状态 |
| git add 文件名                       | 添加到暂存区   |
| git commit -m "日志信息" 文件名      | 提交到本地库   |
| git reflog                           | 查看历史记录   |
| git reset --hard 版本号              | 版本穿梭       |

### 设置用户签名

- git config --global user.name 用户名  设置用户签名
- git config --global user.email 邮箱  设置用户签名
- 说明：签名的作用是区分不同操作者身份。用户的签名信息在每一个版本的提交信息中能够看到，以此确认本次提交是谁做的。Git 首次安装必须设置一下用户签名，否则无法提交代码。
- 注意：这里设置用户签名和将来登录 GitHub（或其他代码托管中心）的账号没有任何关系。
- 用户签名的保存地址在.gitconfig文件下.

### 初始化本地库

- 语法：git init
- 如果在文件目录中没有查看到.git文件，记得打开查看隐藏文件功能

### 查看本地库状态

- 语法：git status
- 红色代表未追踪的文件，即工作区的文件。
- 绿色代表已追踪的文件，即暂存区的文件。

### 添加到暂存区

- 代码：git add 文件名
- 作用：将工作区的文件添加到暂存区

### 提交到本地库

- 语法： git commit -m "日志信息" 文件名

### 查看历史版本

- 查看版本信息：git reflog
- 查看版本详细信息：git log

### 版本穿梭

- 语法：git reset --hard 版本号
- 原理：底层原理是master指针的移动，header始终指向master

## Git分支操作

### 什么是分支

- 在版本控制过程中，同时推进多个任务，为每个任务，我们就可以创建每个任务的单独分支。使用分支意味着程序员可以把自己的工作从开发主线上分离开来，开发自己分支的时候，不会影响主线分支的运行。对于初学者而言，分支可以简单理解为副本，一个分支就是一个单独的副本。（分支底层其实也是指针的引用）

### 分支的好处

- 同时并行推进多个功能开发，提高开发效率。
- 各个分支在开发过程中，如果某一个分支开发失败，不会对其他分支有任何影响。失败的分支删除重新开始即可。

### 分支操作

| 命令名称            | 指令作用                     |
| ------------------- | ---------------------------- |
| git branch 分支名   | 创建分支                     |
| git branch -v       | 查看分支                     |
| git checkout 分支名 | 切换分支                     |
| git merge 分支名    | 把指定的分支合并到当前分支上 |

### 查看分支

- 语法：git branch -v

### 创建分支

- 语法：git branch 分支名

### 切换分支

- 语法：git checkout 分支名

### 把指定的分支合并到当前分支上

- 语法：git merge 分支名

#### 产生冲突情况

- 冲突表现：后面状态为：MERGING
- 冲突产生原因：合并分支时，两个分支在**同一个文件的同一个位置**有两套完全不同的修改。Git 无法替我们决定使用哪一个。必须**人为决定**新代码内容。
- 解决方法：
  - 编辑有冲突的文件，删除特殊符号，决定要使用的内容(特殊符号：<<<<<<< HEAD 当前分支的代码 ======= 合并过来的代码 >>>>>>> hot-fix)
  - 添加到暂存区
  - 执行提交（注意：此时使用 git commit 命令时**不能带文件名**）

#### 未产生冲突情况

- 直接合并

## Git团队协作

### 团队内协作

- 图解

  ![image-20220726161411138](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220726161411138.png)

  

### 跨团队协作

- 图解：

  ![image-20220726161443287](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220726161443287.png)

# GitHub

- 网址：https://github.com/

## 创建远程仓库

![image-20220726175902864](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220726175902864.png)

## 远程仓库操作

| 命令名称                           | 作用                                                     |
| ---------------------------------- | -------------------------------------------------------- |
| git remote -v                      | 查看当前所有远程地址别名                                 |
| git remote add 别名 远程地址       | 给远程地址起别名                                         |
| git push 别名 分支                 | 推送本地分支上的内容到远程仓库                           |
| git clone 远程地址                 | 将远程仓库的内容克隆到本地                               |
| git pull 远程库地址别名 远程分支名 | 将远程仓库对于分支最新内容拉下来后与当前本地分支直接合并 |

### 创建远程仓库别名

- 查看当前所有远程地址别名：git remote -v 
- 起别名：git remote add 别名 远程地址

### 克隆远程仓库到本地

- 语法：git clone 远程地址
- clone 会做如下操作:
  - 1、拉取代码。
  - 2、初始化本地仓库。
  - 3、创建别名

## 邀请加入团队

1、选择邀请合作者

![image-20220726184603170](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220726184603170.png)

2、填入想要合作的人

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220726184637942.png" alt="image-20220726184637942" style="zoom:67%;" />

3、复制地址发送给该用户，内容例子如下：

- **https://github.com/atguiguyueyue/git-shTest/invitations**

4、在受邀账号中的地址栏复制收到邀请的链接，点击接受邀请

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220726184846371.png" alt="image-20220726184846371" style="zoom:67%;" />

5、**成功之后可以在**受邀账号上看到**git-Test**的远程仓库。

![image-20220726184953787](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220726184953787.png)

## 拉取远程库内容

- 语法：git pull 远程库地址别名 远程分支名

## 跨团队协作

- 将远程库的地址复制发给邀请跨团队协作的人

  <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220726185545904.png" alt="image-20220726185545904" style="zoom: 50%;" />

- 在受邀人的账号里的地址栏中复制收到的链接，然后点击Fork将项目叉到自己的本地仓库

  ![image-20220726185717199](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220726185717199.png)

  <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220726185739560.png" alt="image-20220726185739560" style="zoom:50%;" />

## SSH免密登录

- 运行命令生成.ssh 秘钥目录，注意：这里-C 这个参数是大写的 C，然后三次回车

  ```
  ssh-keygen -t rsa -C 1751263383@qq.com
  ```

- 在当前用户的文件夹下，找到.ssh文件，复制 id_rsa.pub 文件内容，登录 GitHub，点击用户头像→Settings→SSH and GPG keys，将内容复制到key中，点击Add SSH key

  ```
  GHirczyMvozXtq4MlHfsmtIbsDYWiqsLCwaupsFbDoE
  ```

# IDEA集成Git

## 配置Git忽略文件

- IDEA特定文件：

  <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220726191958533.png" alt="image-20220726191958533" style="zoom:67%;" />

- **问题** **1**:为什么要忽略他们？

  - 答：与项目的实际功能无关，不参与服务器上部署运行。把它们忽略掉能够屏蔽 IDE 工具之间的差异。

- **问题** **2**：怎么忽略？

  - 创建忽略规则文件 xxxx.ignore（前缀名随便起，建议是 git.ignore）这个文件的存放位置原则上在哪里都可以，为了便于让~/.gitconfig 文件引用，建议也放在用户家目录下，模板如下所示

    ```
    # Compiled class file
    *.class
    # Log file
    *.log
    # BlueJ files
    *.ctxt
    # Mobile Tools for Java (J2ME)
    .mtj.tmp/
    # Package Files #
    *.jar
    *.war
    *.nar
    *.ear
    *.zip
    *.tar.gz
    *.rar
    # virtual machine crash logs, see http://www.java.com/en/download/help/error_hotspot.xml
    hs_err_pid*
    .classpath
    .project
    .settings
    target
    .idea
    *.iml
    ```

  - 在.gitconfig 文件中引用忽略配置文件（此文件在 Windows 的家目录中）

    ```
    [user]
    	name = Layne
    	email = Layne@atguigu.com
    [core]
    	excludesfile = C:/Users/asus/git.ignore
    注意：这里要使用“正斜线（/）”，不要使用“反斜线（\）”
    ```

## 定位Git程序

- 打开IDEA，File->Settings->Version Control->Git，选择Git安装的bin目录下的Git.exe

## 初始化本地库

- VCS->Create Git Repository，选择要创建 Git 本地仓库的工程

  <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220726193049006.png" alt="image-20220726193049006" style="zoom:50%;" />

## 添加到暂存区

- 右键点击项目选择 Git -> Add 将项目添加到暂存区。

## 提交到本地库

- 右键点击项目选择 Git -> Commit Directory 将项目提交到本地仓库

## 添加新分支

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220726193810298.png" alt="image-20220726193810298" style="zoom:50%;" />

或者

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220726194139458.png" alt="image-20220726194139458" style="zoom:50%;" />

## 切换版本

- 在 IDEA 的左下角，点击 Git，然后点击 Log 查看版本

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220726193659459.png" alt="image-20220726193659459" style="zoom:50%;" />

- 切换版本
- <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220726195323197.png" alt="image-20220726195323197" style="zoom:50%;" />

## 切换分支

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220726195355656.png" alt="image-20220726195355656" style="zoom:50%;" />

## 合并分支

- 如果代码没有冲突，分支直接合并成功，分支合并成功以后，代码自动提交，无需手动提交本地库。
- <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220726195721240.png" alt="image-20220726195721240" style="zoom: 80%;" />

## 解决冲突

- 如果 master 分支和 hot-fix 分支都修改了代码，在合并分支的时候就会发生冲突
- 点击 Conflicts 框里的 Merge 按钮，进行手动合并代码。
- 代码冲突解决，自动提交本地库。

# IDEA集成GitHub

## 设置GitHub账号

- File->Settings->Version Control->GitHub，添加GitHub账号
- ![image-20220726200235258](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220726200235258.png)
- 如果网速慢，可以通过Token，获取Token方式：
  - <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220726200319703.png" alt="image-20220726200319703" style="zoom:50%;" />
  - ![image-20220726200343477](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220726200343477.png)
  - 然后设置相关权限，获取Token

## 分享工程到GitHub

- <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220726200650950.png" alt="image-20220726200650950" style="zoom:50%;" />

## push推送本地库到远程库

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220726200833024.png" alt="image-20220726200833024" style="zoom:50%;" />

![image-20220726200853822](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220726200853822.png)

- 注意：push 是将本地库代码推送到远程库，如果本地库代码跟远程库代码版本不一致，push 的操作是会被拒绝的。也就是说，要想push 成功，一定要保证本地库的版本要比远程库的版本高！因此一个成熟的程序员在动手改本地代码之前，一定会先检查下远程库跟本地代码的区别！如果本地的代码版本已经落后，切记要先 pull 拉取一下远程库的代码，将本地代码更新到最新以后，然后再修改，提交，推送！

## pull拉取远程库到本地库

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220726200959639.png" alt="image-20220726200959639" style="zoom:50%;" />

- 注意：pull 是拉取远端仓库代码到本地，如果远程库代码和本地库代码不一致，会自动合并，如果自动合并失败，还会涉及到手动解决冲突的问题。

## clone克隆远程库到本地

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220726201103137.png" alt="image-20220726201103137" style="zoom:50%;" />

![image-20220726201122809](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220726201122809.png)



# Gitee码云

码云是开源中国推出的基于 Git 的代码托管服务中心，网址是 https://gitee.com/ ，使用方式跟 GitHub 一样，而且它还是一个中文网站，如果你英文不是很好它是最好的选择。

# GitLab

## 简介

- GitLab 是由 GitLabInc.开发，使用 MIT 许可证的基于网络的 Git 仓库管理工具，且具有wiki 和 issue 跟踪功能。使用 Git 作为代码管理工具，并在此基础上搭建起来的 web 服务。GitLab 由乌克兰程序员 DmitriyZaporozhets 和 ValerySizov 开发，它使用 Ruby 语言写成。后来，一些部分用 Go 语言重写。截止 2018 年 5 月，该公司约有 290 名团队成员，以及 2000 多名开源贡献者。GitLab 被 IBM，Sony，JülichResearchCenter，NASA，Alibaba，Invincea，O’ReillyMedia，Leibniz-Rechenzentrum(LRZ)，CERN，SpaceX 等组织使用。

## 官网地址

- 官网地址：https://about.gitlab.com/
- 安装说明：https://about.gitlab.com/installation/

## GitLab安装

- Yum 在线安装 gitlab- ce 时，需要下载几百 M 的安装文件，非常耗时，所以最好提前把所需 RPM 包下载到本地，然后使用离线 rpm 的方式安装。

- 下载地址：https://packages.gitlab.com/gitlab/gitlab-ce/packages/el/7/gitlab-ce-13.10.2-ce.0.el7.x86_64.rpm

## 编写安装脚本

- 安装 gitlab 步骤比较繁琐，因此我们可以参考官网编写 gitlab 的安装脚本。

  ```
  [root@gitlab-server module]# vim gitlab-install.sh
  sudo rpm -ivh /opt/module/gitlab-ce-13.10.2-ce.0.el7.x86_64.rpm
  sudo yum install -y curl policycoreutils-python openssh-server cronie
  sudo lokkit -s http -s ssh
  sudo yum install -y postfix
  sudo service postfix start
  sudo chkconfig postfix on
  curl https://packages.gitlab.com/install/repositories/gitlab/gitlabce/script.rpm.sh | sudo bash
  sudo EXTERNAL_URL="http://gitlab.example.com" yum -y install gitlabce
  ```

- 给脚本增加执行权限

  ```
  [root@gitlab-server module]# chmod +x gitlab-install.sh
  [root@gitlab-server module]# ll
  总用量 403104
  -rw-r--r--. 1 root root 412774002 4 月 7 15:47 gitlab-ce-13.10.2-
  ce.0.el7.x86_64.rpm
  -rwxr-xr-x. 1 root root 416 4 月 7 15:49 gitlab-install.sh
  ```

- 执行该脚本，开始安装gitlab-ce【需要服务器能够上网】

  ```
  [root@gitlab-server module]# ./gitlab-install.sh 
  警告：/opt/module/gitlab-ce-13.10.2-ce.0.el7.x86_64.rpm: 头 V4 
  RSA/SHA1 Signature, 密钥 ID f27eab47: NOKEY
  准备中... ################################# 
  [100%]
  正在升级/安装...
   1:gitlab-ce-13.10.2-ce.0.el7 
  ################################# [100%]
  。 。 。 。 。 。
  ```

## 初始化GitLab服务

- 执行以下命令初始化 GitLab 服务，过程大概需要几分钟，耐心等待…

  ```
  [root@gitlab-server module]# gitlab-ctl reconfigure
  。 。 。 。 。 。
  Running handlers:
  Running handlers complete
  Chef Client finished, 425/608 resources updated in 03 minutes 08 
  seconds
  gitlab Reconfigured!
  ```

## 启动GitLab服务

- 执行以下命令启动 GitLab 服务，如需停止，执行 gitlab-ctl stop

  ```
  [root@gitlab-server module]# gitlab-ctl start
  ok: run: alertmanager: (pid 6812) 134s
  ok: run: gitaly: (pid 6740) 135s
  ok: run: gitlab-monitor: (pid 6765) 135s
  ok: run: gitlab-workhorse: (pid 6722) 136s
  ok: run: logrotate: (pid 5994) 197s
  ok: run: nginx: (pid 5930) 203s
  ok: run: node-exporter: (pid 6234) 185s
  ok: run: postgres-exporter: (pid 6834) 133s
  ok: run: postgresql: (pid 5456) 257s
  ok: run: prometheus: (pid 6777) 134s
  ok: run: redis: (pid 5327) 263s
  ok: run: redis-exporter: (pid 6391) 173s
  ok: run: sidekiq: (pid 5797) 215s
  ok: run: unicorn: (pid 5728) 221s
  ```

## **使用浏览器访问** **GitLab**

- 使用主机名或者 IP 地址即可访问 GitLab 服务。需要提前配一下 windows 的 hosts 文件。
- ![image-20220728160702743](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220728160702743.png)
- 首次登陆之前，需要修改下 GitLab 提供的 root 账户的密码，要求 8 位以上，包含大小写子母和特殊符号。

## GitLab创建远程仓库

- ![image-20220728161254031](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220728161254031.png)
- ![image-20220728161259045](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220728161259045.png)
- ![image-20220728161304916](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220728161304916.png)

## IDEA集成GitLab

- 安装GitLab插件
  - ![image-20220728161333256](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220728161333256.png)
- 设置GitLab插件
  - ![image-20220728161348307](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220728161348307.png)
  - ![image-20220728161359791](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220728161359791.png)
  - ![image-20220728161404904](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220728161404904.png)
- push本地代码到GitLab远程库
  - ![image-20220728161446075](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220728161446075.png)
- 自定义远程连接
  - ![image-20220728161559260](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220728161559260.png)
  - 注意：gitlab 网页上复制过来的连接是：http://gitlab.example.com/root/git-test.git，需要手动修改为：http://gitlab-server/root/git-test.git，选择 gitlab 远程连接，进行 push。
    - ![image-20220728161731662](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220728161731662.png)
  - 首次向连接 gitlab，需要登录帐号和密码，用 root 帐号和修改的密码登录即可。
  - 只要 GitLab 的远程库连接定义好以后，对 GitLab 远程库进行 pull 和 clone 的操作和Github 和码云一致，此处不再赘述。