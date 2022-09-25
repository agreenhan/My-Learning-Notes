# 	第1章 Linux开篇

## 1.1 笔记内容

![image-20220810095704175](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220810095704175.png)

![image-20220810095749170](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220810095749170.png)

## 1.2 Linux学习方向

![image-20220810095801982](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220810095801982.png)

## 1.3 Linux的应用领域

### 1.3.1 个人桌面应用领域

- 此领域是传统Linux应用最薄弱的环节，传统Linux由于界面简单、操作复杂、应用软件少的缺点，一直被windows所压制，但近些年来随着ubuntu、fedora等优秀桌面环境的兴起，同时各大硬件厂商对其支持的加大，Linux在个人桌面领域的占有率在逐渐提高。

### 1.3.2 服务器应用领域

- linux在服务器领域的应用是最强的。
- linux免费、稳定、高效等特点在这里得到了很好地体现，近些年来linux服务器市场得到了飞速的提升，尤其在一些高端领域尤为广泛。

### 1.3.3 嵌入式应用领域

- linux运行稳定、对网络的良好支持性、低成本，且可以根据需要进行软件裁剪，内核最小，可以打到几百kb等特点，使其近些年来在嵌入式领域的应用得到非常大的提高。
- 主要应用：机顶盒、数字电视、网络电话、程控交换机、手机、PDA、智能家居、智能硬件等都是其应用领域，以后在物联网中应用会更加广泛。

# 第2章 Linux基础篇

## 2.1 Linux介绍

- Linux是一款操作系统，免费且开源。安全、高效、稳定，处理高并发非常强悍。现在很多企业级的项目都部署到Linux/unix服务器运行。

- Linux创始人是Linus Torvalds，林纳斯

- Linux吉祥物：Tux

  <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220810104857604.png" alt="image-20220810104857604" style="zoom:50%;" />

- Linux的主要发行版

  ![image-20220810105227226](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220810105227226.png)

- 目前主要的操作系统：
  - windows
  - android
  - 车载系统
  - Linux
  - IOS
  - MAC
  - .....

## 2.2 Linux和windows区别

- ![image-20220810144317880](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220810144317880.png)

## 2.3 VM和Linux系统(CentOS)安装

- 学习Linux需要一个环境，我们需要创建一个虚拟机，然后在虚拟机上安装一个CentOS系统来学习。

### 2.3.1 先安装Vm

- 可参考百度

### 2.3.2 再安装Linux(CentOS)

1. 下载地址阿里镜像：http://mirrors.aliyun.com/centos/7/isos/x86_64/
2. 创建虚拟机
   1.  网络适配器：
      1. 虚拟机的网络连接三种形式的说明
         1. 桥连接：Linux可以和其他的系统通信，但是可能造成IP冲突
         2. NAT：网络地址转换方式：linux可以访问外网，不会造成ip冲突
         3. 主机模式：linux是一个独立的主机，不能访问外网
3. 剩下参考百度

### 2.3.3 原理示意图

- ![image-20220810145101598](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220810145101598.png)

## 2.4 Linux文件与目录结构

### 2.4.1 Linux文件

- Linux系统中一切皆文件
- 采用级层式的树状目录结构

### 2.4.2 Linux目录结构

- 默认目录结构

  ![image-20220810185838956](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220810185838956.png)

### 2.4.3 Linux目录简介

- /bin：（/usr/bin、/usr/local/bin)
  - 是Binary的缩写，这个目录存放着最经常使用的命令。
- /sbin：(/usr/sbin、/usr/local/sbin)
  - s就是Super User的意思，这里存放的是系统管理员使用的系统管理程序。
- /home
  - 存放普通用户的主目录，在Linux中每个用户都有一个自己的目录，一般该目录名是以用户的账号命名的。
- /root
  - 该目录为系统管理员，也称作超级权限者的用户主目录。
- /lib
  - 系统开机所需要最基本的动态连接共享库，其作用类似于Windows里的DLL文件。几乎所有的应用程序都需要用到这些共享库。
- /lost+found
  - 这个目录一般情况下是空的，当系统非法关机后，这里就存放了一些文件。
- /etc
  - 所有的系统管理所需要的配置文件和子目录
- /usr
  - 这是一个非常重要的目录，用户的很多用户程序和文件都放在这个目录下，类似于windows下的program files目录。
- /boot
  - 存放的是启动Linux时使用的一些核心文件，包括一些连接文件以及镜像文件。
- /proc
  - 这个目录是一个虚拟的目录，它是系统内存的映射，访问这个目录来获取系统信息。
- /srv
  - service缩写，该目录存放一些服务启动之后需要提取的数据
- /sys
  - 这是linux2.6内核的一个很大的变化，该目录下安装了2.6内核中新出现的一个文件系统sysfs
- /tmp
  - 这个目录是用来存放一些临时文件的。
- /dev
  - 类似于windows的设备管理器，把所有的硬件用文件的形式存储。
- /media
  - linux系统会自动识别一些设备，例如U盘、光驱等等，当识别后，linux会把识别的设备挂载到这个目录下。
  - CentOS7迁移到/run/media
- /mnt
  - 系统提供该目录是为了让用户临时挂在别的文件系统的，我们可以将外部的存储挂载在/mnt上，然后进入该目录就可以查看里面的内容了。
- /opt
  - 这是给主机额外安装软件所摆放的目录。如安装ORACLE数据库就可以放到该目录下，默认为空。
- /usr/local
  - 这是另一个给主机额外安装软件所安装的目录。一般是通过编译源码方式安装的程序。
- /var
  - 这个目录中存放着在不断扩充着的东西，习惯将经常被修改的目录放在这个目录下，包括各种日志文件。
- /selinux
  - SELinux是一种安全子系统，它能控制程序只能访问特定文件

### 2.4.4 Linux目录总结

- linux的目录中有且只有一个根目录
- linux的各个目录存放的内容是规划好的，不能乱放文件
- linux是以文件的形式管理我们的设备的，因为linux系统，一切皆为文件



# 第3章 Linux实操篇

## 3.1 远程登录Linux

### 3.1.1 为什么需要远程登录Linux

1. 实际情况下，linux服务器是开发小组共享的
2. 正式上线的项目是运行在公网的
3. 因此需要远程登录到centos进行项目管理或者开发
4. 画出简单的网络拓扑示意图（帮助理解）
5. 远程登录客户端有Xshell、Xftp，学习使用MobaXterm和Xftp，其他工具大同小异。

### 3.1.2  登录准备

- 下载并安装MobaXterm、Xftp
- 在Linux查看sshd服务是否开放，该服务会监听22号端口。

### 3.1.3 MobaXterm的远程连接

- 创建新的会话（session）

  ![FobGavin: MobaXterm详细使用教程（一）](https://img-blog.csdnimg.cn/img_convert/cc1e3d9a6e3c5a89a81183996a9ff7eb.png)

- ip地址可以通过ifconfig查看

### 3.1.4 MobaXterm的文件上传与下载

#### 3.1.4.1 上传

- 在地址栏输入，要上传到的目录位置。

  ![image-20220810201353357](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220810201353357.png)

- 点击Upload to current folder

  ![image-20220810201413534](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220810201413534.png)

- 选择文件上传即可

#### 3.1.4.2 下载

- 在服务器点击选择要下载的文件或目录。点击下载按钮，或右键点击文件/目录，选择“download”

  <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220810201535151.png" alt="image-20220810201535151" style="zoom: 67%;" />

## 3.2 vi和vim编辑器

### 3.2.1 基本介绍

- 所有的Linux系统都会内建vi文本编辑器。
- vim具有程序编辑的能力，可以看做是vi的增强版本，可以主动的以字体颜色辨别语法的正确性，方便程序设计。代码补充、编译及错误跳转等方便编程的功能特别丰富，在程序员中被广泛使用。

### 3.2.2 vi和vim的三种常见模式

#### 3.2.2.1 一般模式

- 以vim打开一个档案就直接进入一般模式（默认模式）
- 可以使用[上下左右]按键移动光标
- 可以使用[删除字符]或[删除整行]来处理文档内容
- 也可以使用[复制、粘贴]来处理文件数据。

#### 3.2.2.2 编辑模式

- 按下i,I,o,O,a,A,r,R中任何一个字母后才回进入编辑模式，一般来说按i即可。

#### 3.2.2.3 命令行模式

- 在这个模式中，可以提供相关指令，完成读取、存盘、替换、离开vim、显示行号等的动作则是在此模式中达成的。

#### 3.2.2.4 vi和vim模式的相互切换

- ![image-20220810203522280](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220810203522280.png)
- :wq表示保存并退出
- :q表示退出
- :q!表示不保存退出

### 3.2.3 vi和vim的快捷键使用

#### 3.2.3.1 一般模式

| 语法     | 功能描述                       |
| -------- | ------------------------------ |
| yy       | 复制光标当前一行               |
| y 数字 y | 复制一段（从第几行到第几行）   |
| p        | 箭头移动到目的行粘贴           |
| u        | 撤销上一步                     |
| dd       | 删除光标当前行                 |
| d 数字 d | 删除光标（含）后多少行         |
| x        | 剪切一个字母，相当于 del       |
| X        | 剪切一个字母，相当于 Backspace |
| yw       | 复制一个词                     |
| dw       | 删除一个词                     |
| gg       | 移动到行头                     |
| G        | 移动到行尾                     |
| 1+G      | 移动到页头，数字               |
| G        | 移动到页尾                     |
| 数字+G   | 移动到目标行                   |

#### 3.2.3.2 编辑模式

| 按键 | 功能               |
| ---- | ------------------ |
| i    | 当前光标前         |
| a    | 当前光标后         |
| o    | 当前光标行的下一行 |
| I    | 光标所在行最前     |
| A    | 光标所在行最后     |
| O    | 当前光标行的上一行 |

#### 3.2.3.3 命令行模式

| 命令          | 功能                             |
| ------------- | -------------------------------- |
| :w            | 保存                             |
| :q            | 退出                             |
| :!            | 强制退出                         |
| /要查找的词   | n 查找下一个，N 往上查找         |
| :noh          | 取消高亮显示                     |
| :set nu       | 显示行号                         |
| :set nonu     | 关闭行号                         |
| :%s/old/new/g | 替换内容 /g 替换匹配到的所有内容 |

## 3.3 开机、重启和用户登录注销

### 3.3.1 关机&重启指令

- shutdown

  ```
  shutdown -h now:表示立即关机
  shutdown -h 1:表示1分钟后关机
  shutdown -r now:立即重启
  ```

- halt：直接使用，效果等价于关机

- reboot：现在重新启动计算机

- syn：把内存的数据同步到磁盘 

### 3.3.2 注意细节

- 当关机或者重启时，都应先执行sync指令，把内存的数据写入磁盘，防止数据丢失。

### 3.3.3 用户登录和注销

- 登录时尽量少用root账号登录，避免操作失误，可以利用普通用户登录，登录后再用"su -用户名"命令来切换成系统管理员身份
- 在提示符下输入logout即可注销用户
  - logout注销指令在图形运行级别无效，在运行级别3下有效

## 3.4 用户管理

- 规则示意图

  ![image-20220812135028872](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220812135028872.png)

- linux是一个多用户多任务的操作系统，任何一个要使用系统资源的用户，都必须首先向系统管理员申请一个账号，然后以这个账号的身份进入系统

- Linux的用户需要至少要属于一个组

### 3.4.1 添加用户

- 基本语法：useradd [选项] 用户名

- 示例：

  ```
  (1)useradd 用户名
  说明：用户家目录默认加入到/home/目录下
  (2)useradd -d 目录路径 用户名
  说明：用户家目录加入到指定目录下(目录需要不存在)
  (3)useradd -g 用户组 用户名
  说明：增加用户时直接加上组
  ```

### 3.4.2 指定/修改密码

- 基本语法：passwd 用户名

### 3.4.3 删除用户

- 基本语法：userdel 用户名

- 示例：

  ```
  (1)userdel 用户名
  说明：删除用户，但保留家目录
  (2)userdel -r 用户名
  说明：删除用户，不保留家目录
  ```

- 注意：是否保留家目录？

  - 除非要求删，否则尽量保留家目录。

### 3.4.4 查询用户信息

- 语法：id 用户名
- 说明：如果用户名不存在，则显示“无此用户”

### 3.4.5 切换用户

- 语法：su - 用户名
- 说明：
  - 从高权限转到低权限用户时，无需输入密码
  - 从低权限转到高权限用户时，需要输入密码
  - 当需要返回原来用户时，使用exit指令

### 3.4.6 查看当前用户/登录用户

- 语法：whoami/ who am i
- 用户组
  - 类似于角色，系统可以对有共性的多个用户进行统一的管理。
  - 新增组：groupadd 组名 
  - 删除组：groupdel 组名
  - 增加用户时直接加上组：useradd -g 用户组 用户名
  - 修改用户组：usermod -g 用户组 用户名

### 3.4.7 用户和组的相关文件

#### 3.4.7.1 /etc/passwd文件

- 用户（user）的配置文件，记录用户的各种信息

- 每行的含义：

  ```
  用户名:口令:用户标识号:组标识号:注释性解释:主目录:登录Shell
  ```

  

#### 3.4.7.2 /etc/shadow文件

- 口令的配置文件

- 每行的含义：

  ```
  登录名:加密口令:最后一次修改时间:最小时间间隔:最大时间间隔:警告时间:不活动时间:失效时间:标志
  ```

  

#### 3.4.7.3 /etc/group文件

- 组的配置文件，记录Linux包含的组的信息

- 每行的含义：

  ```
  组名:口令:组标识号:组内用户列表
  ```

## 3.5 实用指令

### 3.5.1 指定运行级别

- 运行级别说明：

  ```
  0：关机
  1：单用户【找回丢失密码】
  2：多用户状态没有网络服务
  3：多用户状态有网络服务
  4：系统未使用保留给用户
  5：图形界面
  6：系统重启
  ```

- 常用的运行级别是3和5，要修改默认的运行级别可改文件/etc/inittab的id:5:initdefault:这一行中的数字

- 运行级别示意图：

  ![image-20220812151938347](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220812151938347.png)

### 3.5.2 切换到指定运行级别的指令

- 语法：init [0123456]

- 例子：从5切换到3

  ```
  inti 3
  ```

- 如何找回root密码：

  ```
  进入单用户模式，然后修改root密码，因为进入单用户模式，root不需要密码就可以登录。
  ```

## 3.6 帮助指令

### 3.6.1 介绍

- 对我们对某个指令不熟悉时，可以使用Linux提供的帮助指令来了解这个指令的使用方法

### 3.6.2 man获得帮助信息

- 语法：man [命令或配置文件]

- 说明：获得帮助信息

- 示例：查看ls指令如何使用

  ```
  man ls
  ```

### 3.6.3 help指令

- 语法：help 命令

- 说明：获得shell内置命令的帮助信息

- 示例：查看cd命令的帮助信息

  ```
  help cd
  ```

## 3.7 文件目录类命令

### 3.7.1 pwd指令

- 基本语法：pwd

- 功能：显示当前工作目录的绝对路径

- 示例：

  ```
  pwd
  ```

### 3.7.2 ls指令

- 基本语法：ls [选项] [目录或文件]

- 常用选项：

  ```
  -a:显示当前目录所有的文件和目录，包括隐藏的
  -l:以列表的方式显示信息
  ```

- 示例：查看当前目录的所有内容信息

  ```
  ls -al
  ```

### 3.7.3 cd指令

- 语法：cd [参数]

- 功能：切换到指定目录

- 常用参数

  ```
  绝对路径和相对路径
  cd ~或者cd  回到自己的家目录
  cd ..	   回到当前目录的上一级目录
  ```

- 示例

  ```
  1、使用绝对路径切换root目录
  cd root
  2、使用相对路径到/root目录(当前用户在home目录下)
  cd ../root
  3、表示回到当前目录的上一级目录
  cd ..
  4、回到家目录
  cd或者cd ~
  ```

### 3.7.4 mkdir命令

- 语法：mkdir [选项] 要创建的目录

- 说明：用于创建目录

- 常用选项：

  - -p 创建多级目录

- 示例

  ```
  1、创建一个目录/home/dog
  mkdir /home/dog
  2、创建多级目录/home/animal/dog
  mkdir -p /home/animal/dog
  ```

### 3.7.5 rmdir命令

- 语法：rmdir [选项] 要删除的空目录
- 说明：用于删除空目录
- 注意：
  - rmdir删除的是空目录，如果目录下有内容时无法删除
  - 如果需要删除非空目录，需要使用rm -rf 要删除的目录

### 3.7.6 touch指令

- 语法：touch 文件名称
- 说明：创建空文件
- 注意：
  - 可以一次性创建多个文件

### 3.7.7 cp指令

- 语法：cp [选项] source dest

- 说明：拷贝文件到指定目录

- 常用选项：

  - -r：递归复制整个文件夹

- 示例：

  ```
  1、将/home/aaa.txt拷贝到/home/bbb目录下
  cp aaa.txt bbb/
  2、递归赋值整个文件夹，将/home/test整个目录拷贝到/home/zwj目录
  cp -r test/ zwj/
  强制覆盖不提示的方法：
  \cp -r test/ zwj/
  ```

### 3.7.8 rm指令

- 语法：rm [选项] 要删除的文件或目录

- 说明：移除文件或目录

- 常用选项

  - -r:递归删除整个文件夹
  - -f:强制删除不提示

- 示例：

  ```
  1、将/home/aaa.txt删除
  rm /home/aaa.txt
  2、递归删除整个文件夹/home/bbb
  rm -r /home/bbb
  强制删除不提示：
  rm -rf /home/bbb
  ```

### 3.7.9 mv指令

- 语法：

  - mv oldNameFile newNameFile
  - mv /temp/movefile/targetFolder

- 功能：

  - 重命名
  - 移动文件与目录

- 示例

  ```
  1、将/home/aaa.txt文件重命名为pig.txt
  mv aaa.txt pig.txt
  2、将/home/pig.txt文件移动到/root目录下
  mv pig.txt /root/
  ```

### 3.7.10 cat指令

- 基本语法：cat [选项] 要查看的文件

- 常用选项

  - -n：显示行号

- 示例

  ```
  /ect/profile文件内容，并显示行号
  cat -n /etc/profile | more
  使用细节：
  cat只能浏览文件，而不能修改文件，为了浏览方便，一般会带上管道命令|more(分页显示)
  ```

### 3.7.11 more指令

- 语法：more 要查看的文件

- 说明：more指令是一个基于VI编辑器的文本过滤器，它以全屏幕的方式按页显示文本文件的内容。

- more指令中内置了若干快捷键，详见下表

  | 操作     | 功能                             |
  | -------- | -------------------------------- |
  | space    | 向下翻一页                       |
  | Enter    | 向下翻一行                       |
  | q        | 立即离开more，不再显示该文件内容 |
  | Ctrl + F | 向下滚动一屏                     |
  | Ctrl + B | 返回上一屏                       |
  | =        | 输出当前行的行号                 |
  | :f       | 输出文件名和当前行的行号         |

- 示例：

  ```
  采用more查看文件
  more /etc/profile
  ```

### 3.7.12 less指令

- 语法：less 要查看的文件

- 说明：

  - less指令用来分屏查看文件内容，它的功能与more指令类似，但是比more指令更加强大，支持各种显示终端。
  - less指令在显示文件内容时，并不是一次将整个文件加载之后才显示，而是根据显示需要加载内容，对于显示大型文件具有较高的效率。

- 操作快捷键

  | 操作       | 功能                                                     |
  | ---------- | -------------------------------------------------------- |
  | space      | 向下翻动一页                                             |
  | [pagedown] | 向下翻动一页                                             |
  | [pageup]   | 向上翻动一页                                             |
  | /字符串    | 向下搜寻【字符串】的功能：<br />n：向下查找，N：向上查找 |
  | ?字符串    | 向上搜寻【字符串】的功能：<br />n：向上查找，N：向下查找 |
  | q          | 离开less这个程序                                         |

### 3.7.13 >指令和>>指令

- 基本语法

  - ls -l > 文件
    - 功能：列表的内容写入文件a.txt中（覆盖写）
    - 如果文件不存在，则新建文件
  - ls -al >> 文件
    - 功能：列表的内容追加到文件aa.txt的末尾
    - 如果文件不存在，则新建文件
  - cat 文件1 > 文件2
    - 功能：将文件1的内容覆盖到文件2
  - echo "内容" >> 文件
    - 功能：列表的内容追加到文件aa.txt的末尾

- 说明：

  - \>输出重定向：会将原来的文件内容覆盖
  - \>\>追加：不会覆盖原来文件的内容，而是追加到文件的尾部。

- 示例

  ```
  1、将/home目录下的文件列表写入到/home/info.txt中
  ls -l /home/ > /home/info.txt
  2、将当前日历信息追加到/home/mycal文件中[提示cal]
  cal >> /home/mycal
  ```

### 3.7.14 echo、head、tail命令

#### 3.7.14.1 echo

- 语法：echo [选项] [输出内容]

#### 3.7.14.2 head

- 语法
  - head 文件
    - 查看文件头10行内容
  - head -n 5 文件
    - 查看文件头5行内容，5可以修改

#### 3.7.14.3 tail

- 语法
  - tail 文件
    - 查看文件尾10行内容
  - tail -n 5 文件
    - 查看文件尾5行内容，5可以修改
  - tail -f 文件
    - 实时追踪该文档的所有更新

### 3.7.15 ln指令

- 语法：ln -s [原文件或目录] [软链接名]
- 功能：
  - 给源文件创建一个软连接
  - 软连接也叫符号链接，类似windows里的快捷方式，主要存放了链接其他文件的路径。
-  删除软链接
  - rm -rf 软链接名
- 细节说明：
  - 当使用pwd指令查看目录时，仍然看到的是软链接所在目录

### 3.7.16 history指令

- 查看已经执行过历史命令，也可以执行历史指令
- 语法：history

## 3.8 时间日期类

### 3.8.1 date指令-显示当前日期

- 基本语法

  | 基本语法                  | 功能描述         |
  | ------------------------- | ---------------- |
  | date                      | 显示当前时间     |
  | date +%Y                  | 显示当前年份     |
  | date +%m                  | 显示当前月份     |
  | date +%d                  | 显示当前是哪一天 |
  | date "+%Y-%m-%d %H:%M:%S" | 显示年月日时分秒 |

### 3.8.2 date指令-设置日期

- 基本语法：date -s 字符串时间
- 字符串格式：yyyy-MM-dd HH:mm:ss

### 3.8.3 cal指令

- 基本语法：cal [选项]
  - 不加选项，则显示本月日历

## 3.9 搜索查找类

### 3.9.1 find指令

- 语法：find [搜索范围] [选项]

- 选项说明

  | 选项            | 功能                             |
  | --------------- | -------------------------------- |
  | -name<查询方式> | 按照指定的文件名查找模式查找文件 |
  | -user<用户名>   | 查找属于指定用户名所有文件       |
  | -size<文件大小> | 按照指定的文件大小查找文件       |

- 说明：find指令将从指定目录向下递归地遍历其各个子目录，将满足条件的文件或者目录显示在终端。

- 示例：

  ```
  1、按文件名：根据名称查找/home目录下的hello.txt文件
  find /home -name hello.txt
  2、按拥有者：查找/opt目录下，用户名称为nobody的文件
  find /opt -user nobody
  3、查找整个linux系统下大于20m的文件(+n大于 -n小于 n等于)
  find / -size +20M
  ```

### 3.9.2 locate指令

- 说明：

  - locate指令可以快速定位文件路径。locate指令利用事先建立的系统中所有文件名称及路径的locate数据库实现快速定位给定的文件。
  - Locate指令无需遍历整个文件系统，查询速度较快，为了保证查询结果的准确度，管理员必须定期更新locate时刻。

- 语法：locate 搜索文件

- 注意：由于locate指令基于数据库进行查询，所以第一次运行前，必须使用updatedb指令创建locate数据库

- 示例：

  ```
  使用locate指令快速定位hello.txt文件所在目录
  updatedb
  locate hello.txt
  ```

### 3.9.3 grep指令和管道符号|

- 说明：grep过滤查找，管道符”|“，表示将前一个命令的处理结果输出传递给后面的命令处理

- 语法：grep [选项] 查找内容 源文件

- 常用选项

  - -n：显示匹配喊及行号
  - -i：忽略字母大小写

- 示例：

  ```
  请在hello.txt文件中，查找"yes"所在行，并显示行号
  cat hello.txt | grep -n yes
  ```

## 3.10 压缩和解压类

### 3.10.1 gzip/gunzip指令

- 语法：

  - gzip 文件
    - 压缩文件，只能将文件压缩为*.gz文件，原文件变成压缩文件
  - gunzip 文件.gz
    - 解压缩文件命令，将压缩文件变成原文件

- 示例

  ```
  1、gzip压缩，将/home下的hello.txt文件进行压缩
  gzip hello.txt
  2、gunzip压缩，将/home下的hello.txt.gz文件进行解压缩
  gunzip hello.txt
  ```

### 3.10.2 zip/unzip指令

- 说明：zip用于压缩文件，unzip用于解压文件，在项目打包发布中很有用

- 语法

  - zip [选项] xxx.zip 将要压缩的内容
    - 压缩文件和目录的命令
  - unzip [选项] xxx.zip
    - 解压缩文件

- zip常用选项

  - -r：递归压缩，即压缩目录

- unzip常用选项

  - -d<目录>：指定解压后文件的存放目录

- 示例

  ```
  1、将/home下的所有文件进行压缩成mypackage.zip
  zip -r mypackage.zip /home/
  2、mypackage.zip解压到/opt/tmp目录下
  unzip -d /opt/tmp/ mypackage.zip
  ```

### 3.10.3 tar指令

- 说明：tar指令是打包指令，最后打包后的文件是.tar.gz文件

- 语法：tar [选项] xxx.tar.gz打包的内容

  - 选项说明

    | 选项 | 功能               |
    | ---- | ------------------ |
    | -c   | 产生.tar打包文件   |
    | -v   | 显示详细信息       |
    | -f   | 指定压缩后的文件名 |
    | -z   | 打包同时压缩       |
    | -x   | 解包.tar文件       |

- 示例

  ```
  1、压缩多个文件，将/home/a1.txt和/home/a2.txt压缩成a.tar.gz
  tar -zcvf a.tar.gz a1.txt a2.txt
  2、将/home的文件夹压缩成myhome.tar.gz
  tar -zcvf myhome.tar.gz /home/
  3、将a.tar.gz解压到当前目录
  tar -zxvf a.tar.gz
  4、将myhome.tar.gz解压到/opt/tmp目录下
  tar -zxvf myhome.tar.gz -C /opt/tmp/
  ```


## 3.11 组管理

### 3.11.1 Linux组的基本介绍

- 在linux中的每个用户必须属于一个组，不能独立于组外。
- 在linux中每个文件的所有者、所在组、其他组的概念。

### 3.11.2 文件/目录的所有者

- 一般为文件的创建者，谁创建了该文件，就自然的成为改文件的所有者。

- 查看文件的所有者

  - 指令：ls -ahl

  - 示例：

    ```
    创建一个组police，再创建一个用户tom，将tom放在police组，然后使用tom来创建一个文件
    groupadd polic
    useradd -g police tom
    passwd tom
    su tom
    touch test.txt
    ls -ahl
    ```

  - ![image-20220813091023455](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220813091023455.png)

- 修改文件的所有者

  - 指令：chown 用户名 文件名

  - 示例：

    ```
    使用root创建一个文件apple.txt，然后将其所有者修改成tom
    touch apple.txt
    chown tom apple.txt
    ```

### 3.11.3 组的创建

- 指令：groupadd 组名

### 3.11.4 文件/目录的所在组

- 当某个用户创建了一个文件后，默认这个文件的所在组就是该用户所在的组。

- 查看文件/目录所在组

  - 指令：ls -ahl

- 修改文件所在的组

  - 指令：chgrp 组名 文件名

  - 示例：

    ```
    使用root用户创建文件orange.txt，看看当前这个文件属于哪个组，然后将这个文件所在组修改到police组
    touch orange.txt
    ls -ahl
    chgrp police orange.txt
    ```

### 3.11.5 其他组

- 除文件的所有者和所在组的用户外，系统的其他用户都是文件的其他组

- 改变用户所在组

  - 在添加用户时，可以指定将该用户添加到哪个组中，同样用root的管理权限可以改变某个用户所在的组

  - 语法：

    - usermod -g 组名 用户名
    - usermod -d 目录名 用户名 改变该用户登录的初始目录

  - 示例：

    ```
    创建一个土匪组（bandit）将tom这个用户从原来所在的police组，修改到bandit组
    usermod -g bandit police
    ```

## 3.12 权限管理

### 3.12.1 权限的基本介绍

- ls -l 中显示的内容如下：

  ```
  rwx rw- r-- 1 root root 1213 Feb 2 09:39 abc
  ```

- 第0-9位说明：

  - 第0位确定文件类型(d,-,l,c,b)
  - 第1-3位确定所有者拥有该文件的权限
  - 第4-6位确定所在组拥有该文件的权限
  - 第7-9位确定其他用户拥有该文件的权限

- ![image-20220813095022620](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220813095022620.png)

- rwx权限详解

  - rwx作用到文件

    ```
    [r]代表可读(read):可以读取、查看
    [w]代表可写(write):可以修改，但是不代表可以删除该文件，删除一个文件的前提是对改文件所在的目录有写权限，才能删除改文件
    [x]代表可执行(execute):可以被执行
    ```

  - rwx作用到目录

    ```
    [r]代表可读(read):可以读取，ls查看目录内容
    [w]代表可写(write):可以修改，目录内创建、删除、重命名目录
    [x]代表可执行(execute):可以进入该目录
    ```

- 实际案例

  ![image-20220813100019714](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220813100019714.png)

### 3.12.2 修改权限-chmod

- 说明：通过chmod，可以修改文件或者目录的权限

#### 3.12.2.1 第一种方式：+、-、=变更权限

- u：所有者，g：所有组，o：其他人，a：所有人

- 命令:

  ```
  chmod u=rwx,g=rx,o=x 文件/目录名
  chmod o+w 文件/目录名
  chmod a-x 文件/目录名
  ```

- 示例

  ```
  1、给abc文件的所有者读写执行的权限，给所有组读执行权限，给其他组读执行权限
  chmod u=rwx,g=rx,o=rx abc
  2、给abc文件的所有者除去执行的权限，增加组写的权限
  chmod u-x,g+w abc
  3、给abc文件的所有用户添加读的权限
  chmod a+r abc
  ```

#### 3.12.2.2 第二种方式:通过数字变更权限

- 规则：r=4,w=2,x=1

- chmod u=rwx,g=rx,o=x 文件目录名

- chmod 751 文件目录名

- 示例：

  ```
  将/home/abc.txt文件的权限修改成rwxr-xr-x使用数字变更方式实现
  chmod 755 /home/abc.txt
  ```

### 3.12.3 修改文件所有者-chown

- 命令

  ```
  chown newowner file		改变文件的所有者
  chown newowner:newgroup file		改变用户的所有者和所有组
  
  -R	如果是目录，则使其所有子文件或目录递归生效
  ```

- 示例：

  ```
  1、请将/home/abc.txt文件的所有者修改成tom
  chown tom /home/abc.txt
  2、请将/home/kkk目录下所有的文件和目录的所有者都修改成tom
  chown -R tom /home/kkk
  ```

### 3.12.4 修改文件所在组-chgrp

- 指令：chgrp newgroup file 改变文件的所有组

- 示例

  ```
  1、请将/home/abc.txt文件的所在组修改成bandit
  chgrp bandit /home/abc.txt
  2、请将/home/kkk目录下所有的文件和目录的所在组都修改成bandit
  chgrp -R bandit /home/abc.txt
  ```

## 3.13 crond任务调度

- crontab进行定时任务的设置
- 任务调度：是指系统在某个时间执行的特定的命令或程序
- 任务调度分类
  - 系统工作：有些重要的工作必须周而复始地执行，如病毒扫描等
  - 个别用户工作：个别用户可能希望执行某些程序，比如对mysql数据库的备份
- 语法：
  - crontab [选项]
- 常用选项
  - -e：编辑crontab定时任务
  - -l：查询crontab任务
  - -r：删除当前用户所在的crontab任务

### 3.13.1 快速入门

- 快速入门

  ```
  设置任务调度文件:/etc/crontab
  设置个人任务调度。执行crontab -e命令
  接着输入任务到调度文件，如：*/1 * * * * ls -l /etc/ > /tmp/to.txt
  意思是每小时的每分钟执行ls -l /etc/ > /tmp/to.txt命令
  ```

- 参数细节说明：

  - 5个占位符的说明

    | 项目      | 含义                 | 范围                    |
    | --------- | -------------------- | ----------------------- |
    | 第一个"*" | 一小时当中的第几分钟 | 0-59                    |
    | 第二个"*" | 一天当中的第几个小时 | 0-23                    |
    | 第三个"*" | 一个月当中的第几天   | 1-31                    |
    | 第四个*"  | 一年当中的第几个月   | 1-12                    |
    | 第五个"*" | 一周当中的星期几     | 0-7（0和7）都代表星期日 |

  - 特殊符号说明

    | 特殊符号 | 含义                                                         |
    | -------- | ------------------------------------------------------------ |
    | *        | 代表任何时间。比如第一个"*"就代表一小时中每分钟都执行一次的意思 |
    | ,        | 代表不连续的时间。比如"0 8,12,16 * * * 命令"<br />就代表在每天的8:00、12:00、16:00都执行一次命令 |
    | -        | 代表连续的时间范围。比如"0 5 * * 1-6 命令"，<br />代表在周一到周六的凌晨5::00执行命令 |
    | */n      | 代表每隔多久执行一次。例如"*/10 * * * * 命令"，代表每隔10分钟就执行一遍命令 |

  - 特定时间执行任务案例

    | 时间              | 含义                                                         |
    | ----------------- | ------------------------------------------------------------ |
    | 45 22 * * * 命令  | 在22点45分执行命令                                           |
    | 0 17 * * 1 命令   | 每周1的17点执行命令                                          |
    | 0 5 1,15 * * 命令 | 每周1号和15号的凌晨5点执行命令                               |
    | 40 4 * * 1-5 命令 | 每周一到周五的凌晨4.40执行命令                               |
    | */10 4 * * * 命令 | 每天的凌晨4点，每隔10分钟执行一次命令                        |
    | 0 0 1,15 * 1 命令 | 每月1号和15号，每周一的0点都会执行命令。<br />注意：星期几和几号最好不要同时出现，因为定义的都是天，容易让管理员混乱 |

### 3.13.2 应用实例

```
1、每隔1分钟，就将当前的日期信息，追加到/tmp/mydate文件中
(1)先编写文件/home/mytask1.sh
data >> /tmp/mydate
(2)给mytask1.sh可执行权限
chmod 744 mytask1.sh
(3)crontab -e
*/1 * * * * /home/mytask1.sh
2、每隔1分钟，就将当前日期和日历都追加到/home/mycal文件中
(1)先编写文件/home/mytask2.sh
date >> /home/mycal
cal >> /home/mycal
(2)给mytask1.sh可执行权限
chmod 744 mytask2.sh
(3)crontab -e
*/1 * * * * /home/mytask1.sh
3、每天凌晨2:00将mysql数据库testdb，备份到文件中
(1)先编写文件/home/mytask3.sh
/usr/local/mysql/bin/mysqldump -u root -proot testdb > /temp/mydb.bak
(2)给mytask1.sh可执行权限
chmod 744 mytask3.sh
(3)crontab -e
0 2 * * * /home/mytask3.sh
```

### 3.13.3 crond相关命令

- crontab -r :终止任务调度
- crontab：列出当前有哪些任务调度
- service crond restart ：重启任务调度

## 3.14 Linux磁盘分区、挂载

### 3.14.1 分区基础知识

#### 3.14.1.1 分区的方式

- mbr分区
  - 最多支持四个主分区
  - 系统只能安装在主分区
  - 扩展分区要占一个主分区
  - MBR最大只支持2TB，但拥有最好的兼容性
- gtp分区
  - 支持无线多个主分区（但操作系统可能限制，比如windows下最多128个分区）
  - 最大支持18EB的大容量（EB=1014PB，PB=1024TB）
  - windows7 64位以后支持gtp

#### 3.14.1.2 windows下的磁盘分区

![image-20220813145757727](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220813145757727.png)

#### 3.14.1.3 Linux分区

- linux来说无论有几个分区，分给哪一个目录使用，它归根结底就只有一个根目录，一个独立且唯一的文件结构，Linux中每个分区都是用来组成整个文件系统的一部分。

- linux采用了一种叫"载入"的处理方法，他的整个文件系统中包含了一整套的文件和目录，且将一个分区和一个目录联系起来，这时要载入的一个分区将使它的存储空间在一个目录下获得

- 示意图

  ![image-20220813150235216](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220813150235216.png)

- 硬盘说明：

  ```
  1、linux硬盘分为IDE硬盘和SCSI硬盘，目前基本上是SCSI硬盘
  2、对于IDE硬盘，驱动器标识符为"hdx~",其中"hd"表明分区所在设备的类型，这里是指IDE硬盘了。"x"为盘号(a为基本盘，b为基本从属盘,c为辅助主盘,d为辅助从属盘),"~"代表分区，前四个分区用数字1到4表示，他们是主分区或拓展分区，从5开始就是逻辑分区。例入，hda3表示为第一个IDE硬盘上的第三个主分区或拓展分区，hdb2表示为第二个IDE硬盘上的第二个主分区或拓展分区。
  3、对于SCSI硬盘则标识为"sdx~"，SCSI硬盘是用"sd"来表示分区所在设备的类型，其他则和IDE硬盘的表示方法一样。
  ```

- 查看所有设备挂载情况

  - lsblk或lsblk -f

- 图解：

  ![image-20220813151055642](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220813151055642.png)

#### 3.14.1.4 挂载的经典案例

- 需求：给linux系统增加一个新的硬盘，并且挂载到/home/newdisk

- 步骤：

  1. 虚拟机添加硬盘

  2. 分区       fdisk/dev/sdb

  3. 格式化          mkfs -t ext4 /dev/sdb1

  4. 挂载    先创建newdisk目录，挂载mount /dev/sdb1 /home/newdisk

  5. 设置可以自动挂载(永久挂载，即重启系统，仍能挂载)

     1. 编辑vim /etc/fstab

     2. 添加如下如所示

        ![image-20220813152730869](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220813152730869.png)

     3. 输入mount -a开启自动挂载

### 3.14.2 磁盘情况查询

#### 3.14.2.1 查询系统整体磁盘使用情况

- 语法：df -h

#### 3.14.2.2 查询指定目录的磁盘占用情况

- 语法：du -h /目录

- 说明：查询指定目录的磁盘占用情况，默认为当前目录

- 选项：

  - -s：指定目录占用大小汇总
  - -h：带计量单位
  - -a：含文件
  - --max-depth=1：子目录深度
  - -c：列出明细的同时，增加汇总量

- 示例

  ```
  查询/opt目录的磁盘占用情况，深度为1
  du -ach --max-depth=1 /opt
  ```

#### 3.14.2.3 常用使用指令

```
1、统计/home文件夹下文件的个数
ls -l /home | grep "^-" | wc -l
2、统计/home文件夹下目录的个数
ls -l /home | grep "^d" | wc -l
3、统计/home文件夹下文件的个数，包括子文件夹里的
ls -lR /home | grep "^-" | wc -l
4、统计文件夹下目录的个数，包括子文件夹里的
ls -lR /home | grep "^d" | wc -l
5、以树状显示目录结构
tree
```

## 3.15 网络配置

### 3.15.1 Linux网络配置原理图（含虚拟机）

![image-20220813155143877](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220813155143877.png)

### 3.15.2 查看网络IP和网关

#### 3.15.2.1 查看虚拟网络编辑器

vm中点击编辑->虚拟网络编辑器

#### 3.15.2.2 修改ip地址（虚拟网络的IP）

在vm中点击编辑->虚拟网络编辑器中修改

#### 3.15.2.3 查看网关

在vm中点击编辑->虚拟网络编辑器->NAT设置

#### 3.15.2.4 查看windows环境中的VMnet8网络配置

- 使用ipconfig查看
- 界面查看

#### 3.15.2.5 ping测试主机之间的网络连通性

- 语法：ping ip地址

### 3.15.3 linux网络环境配置

#### 3.15.3.1 自动获取

- 系统->首选项，选择网卡，点击编辑，勾上自动连接，点应用
- 特点：linux启动后会自动获取ip，缺点是每次自动获取的ip地址可能不一样（不适用于服务器）

#### 3.15.3.2 指定固定的ip

- 说明：直接修改配置文件来指定IP，并可以连接到外网

- 步骤

  - 编辑vi /etc/sysconfig/network-scripts/ifcfg-ens33

  - 将ip地址配置为静态的如下图

    ![image-20220813160551345](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220813160551345.png)

  - 修改后一定要重启服务service network restrat或reboot

- ifcfg-ens33文件说明

  ![image-20220813160758580](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220813160758580.png)

## 3.16 进程管理

### 3.16.1 基本介绍

- 在linux中，每个执行的程序都称为一个进程。每一个进程都分配一个ID号
- 每一个进程，都会对应一个父进程，而这个父进程可以复制多个子进程。例如www服务器
- 每个进程都可能以两种方式存在。前台和后台，所谓前台就成就是用户目前的屏幕上可以进行操作的。后台进程则是实际在操作，但由于屏幕上无法看到的进程，通常使用后台方式执行
- 一般系统的服务都是以后台进程的方式存在，而且都会常驻在系统中，直到关机才结束。

### 3.16.2 显示系统执行的进程

- 指令：ps

- 说明：ps命令时用来查看目前系统中，有哪些正在执行，以及它们的执行情况，可以不加任何参数

- 参数：

  - -a:显示当前终端的所有进程信息
  - -u：以用户的格式显示进程信息
  - -x：显示后台进程运行的参数

- ps显示的信息选项：

  | 字段 | 说明                   |
  | ---- | ---------------------- |
  | PID  | 进程识别号             |
  | TTY  | 终端机号               |
  | TIME | 此进程所消CPU时间      |
  | CMD  | 正在执行的命令或进程名 |

- 参数详解

  - ```
    System V展示风格
    USER：用户名称
    PID：进程号
    %CPU：进程占用CPU的百分比
    %MEM：进程占用物理内存的百分比
    VSZ：进程占用的虚拟内存大小（单位：KB）
    RSS：进程占用的物理内存大小（单位：KB）
    TT：终端名称，缩写
    STAT：进程状态，其中S-睡眠，s-表示该进程是会话的先导进程，N-表示进程拥有比普通优先级更低的优先级，R-正在运行，D-短期等待，Z-僵死进程，T-被跟踪或者被停止等等
    STARTED：进程的启动时间
    TIME：CPU时间，即进程使用CPU的总时间
    COMMAND：启动进程所用的命令和参数，如果过长会被截断显示。
    ```

  - 图解

  ![image-20220813182555177](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220813182555177.png)

  - 指令示例：ps -aux | grep xxx

- 以全格式显示当前所有的进程，查看进程的父进程

  - ps -ef是以全格式显示当前所有的进程

    - -e显示所有进程
    - -f全格式

  - 详解

    ```
    是BSD风格
    UID：用户ID
    PID：进程ID
    PPID：父进程ID
    C：CPU用于计算执行优先级的因子。数值越大，表名进程是CPU密集型运算，执行优先级会降低；数值越小，表名进程是I/O密集型运算，执行优先级会提高
    STIME：进程启动的时间
    TTY：完整的终端名称
    TIME：CPU时间
    CMD：启动进程所用的命令和参数
    ```

### 3.16.3 终止进程kill和killall

#### 3.16.3.1 介绍

- 若是某个进程执行一般需要停止时，或是已消了很大的系统资源时，此时可以考虑停止该进程。使用kill命令来完成此项任务。

#### 3.16.3.2 基本语法

- kill [选项] 进程号
  - 通过进程号杀死进程
- killall 进程名称
  - 通过进程名称杀死进程，也支持通配符，这在系统因负载过大而变得很慢时很有用
- 常用选项
  - -9：表示强迫进程立即停止

#### 3.16.3.3 示例

```
1、踢掉某个非法登录用户
(1)先搜索出进程号
ps -aux | grep sshd
(2)找到进程号，并杀掉
kill 4010
2、终止远程登录服务sshd，在适当时候再次重启sshd服务
(1)先搜索出进程号
ps -aux | grep sshd
(2)找到进程号，并杀掉
kill 3908
(3)重启sshd
service sshd restart
3、终止多个gedit编辑器
killall gedit
4、强制杀掉一个终端
(1)先搜索出进程号
ps -aux | grep bash
(2)找到进程号，并杀掉
kill -9 4090 
```

### 3.16.4 查看进程树pstree

- 语法：pstree [选项]

- 说明：可以更加直观地看进程信息

- 常用选项：

  - -p：显示进程的PID
  - -u：显示进程的所属用户

- 示例

  ```
  1、请以树状的形式显示进程的pid
  pstree -p
  2、请以树状的形式显示进程的用户id
  pstree -u
  ```

### 3.16.5 服务(service)管理

- 服务本质就是进程，但是是运行在后台的，通常都会监听某个端口，等待其他程序的请求（例如mysql、sshd、防火墙等），因此又称为守护进程

#### 3.16.5.1 service管理指令

- 指令：service 服务名 [strat|stop|restart|reload|status]

  - 注：在CentOS7.0后，不再使用service，而是systemctl

- 示例

  ```
  查看当前防火墙的状况，关闭防火墙和重启防火墙
  开启防火墙：service iptables restart
  查看防火墙状态：service iptables status
  关闭防火墙：service iptables stop
  重启防火墙：service iptables restart
  ```

- 注意

  - 关闭或者启用防火墙后，立即生效。[可以在dos窗口输入telnet ip 端口测试]
  - 这种方式只是临时生效，当重启系统后，还是会回归以前对服务的设置
  - 如果希望设置某个服务自启动或关闭永久生效，要使用chkconfig指令

#### 3.16.5.2 查看服务名

- 第一种方式：使用setup ->系统服务可以看到
- 第二种方式：/etc/init.d/服务名称
  - 列出系统有哪些服务：ls -l /etc/init.d

#### 3.16.5.3 服务的运行级别(runlevel)

- 查看或者修改默认级别：vi /etc/inittab

- Linux系统有7种运行级别(runlevel)：常用的是级别3和级别5

  ```
  运行级别0：系统停机状态，系统默认运行级别不能设为0，否则不能正常启动
  运行级别1：单用户工作状态，root权限，用于系统维护，进制远程登录
  运行级别2：多用户状态（没有NFS），不支持网络
  运行级别3：完全的多用户状态（有NFS），登录后进入控制台命令行模式
  运行级别4：系统未使用，保留
  运行级别5：X11控制台，登录后进入图形GUI模式
  运行级别6：系统正常关闭并重启，默认运行级别不能设为6，否则不能正常启动
  ```

- 开机的流程说明：

  - 开机->BIOS->/boot->init进程->运行级别->运行级对应的服务

#### 3.16.5.4 chkconfig指令

- 说明：通过chkconfig命令可以给各个运行级别设置自启动/关闭
- 查看服务：chkconfig --list | grep xxx
- 查看服务：chkconfig 服务名 --list
- 设置某个服务在某个级别自启动/关闭：chkconfig --level 5 服务器 on/off

- 示例：

  ```
  1、显示当前系统所有服务的各个运行级别的运行状态
  chkconfig --list
  2、查看sshd服务的运行状态
  service sshd status
  3、将sshd服务在运行级别5下设置为不自动启动
  chkconfig --level 5 sshd off
  4、当运行级别为5时，关闭防火墙
  chkconfig --level 5 iptables off
  5、在所有运行级别下，开启防火墙
  chkconfig iptables on
  6、在所有运行级别下，关闭防火墙
  chkconfig iptables off
  ```

- 注意：chkconfig设置后，需要重新启动机器reboot才能生效

### 3.16.6 动态监控进程

- 说明：top与ps命令很相似，它们都用来显示正在执行的进程。top和ps最大的不同之处，在于top在执行一段时间可以更新正在运行的进程

- 语法：top [选项]

- 常用选项：

  | 选项    | 功能                                                         |
  | ------- | ------------------------------------------------------------ |
  | -d 秒数 | 指定top命令每隔几秒更新。默认是3秒。在top命令的交互模式当中可以执行的命令 |
  | -i      | 使top不显示任何闲置或者僵死进程                              |
  | -p      | 通过指定检控进程ID来仅仅检控某个进程的状态                   |

- 交互操作说明：

  | 操作 | 功能                          |
  | ---- | ----------------------------- |
  | P    | 以CPU使用率排序，默认就是此项 |
  | M    | 以内存的使用率排序            |
  | N    | 以PID排序                     |
  | q    | 退出top                       |

- 示例

  ```
  1、监视特定用户
  (1)先输入top，查看执行的进程
  (2)然后输入u回车，再输入用户名即可
  2、终止指定的进程
  (1)先输入top
  (2)然后输入k回车，在输入要结束的进程ID号
  3、指定系统状态更新的时间
  top -d 10
  ```

### 3.16.7 监控网络状态

- 查看系统网络情况netstat

  - 语法：netstat [选项]

  - 选项说明：

    - -an：按一定顺序排列输出
    - -p：显示哪个进程在调用

  - 示例

    ```
    查看服务名为sshd的服务的信息
    netstat -anp | grep sshd
    查看所有服务的信息
    netstat -anp
    ```

## 3.17 RPM 和 YUM

### 3.17.1 rpm包的管理

- 介绍：
  - 一种用于互联网下载包的打包及安装工具，它包含在某些Linux分发版中。它生成具有.RPM扩展名的文件。
  - RPM是RedHat Package Manager（RedHat软件包管理工具）的缩写，类似windows的setup.exe，这一文件格式名称虽然打上了RedHat的标志，但是理念是通用的
  - linux的分发版本都有采用（suse，RedHat，centos等等），可以算是公认的行业标准了。

#### 3.17.1.1 rpm包的简单查询指令

- 查询已安装的rpm列表：rpm -qa | grep xxx

#### 3.17.1.2 rpm包名基本格式

- 一个rpm包名：firefox-45.0.1-1.el6.centos.x86_64.rpm
  - 名称：firefox
  - 版本号：45.0.1-1
  - 适用操作系统：el6.centos.x86_64
    - 表示centos6.x的64位系统
    - 如果是i686、i386表示32位系统，noarch表示通用

#### 3.17.1.3 rpm包的其他查询指令

- rpm -qa：查询所安装的所有rpm软件包
  - rpm -qa | more
  - rpm -qa | grep X
- rpm -q 软件包名：查询软件包是否安装
- rpm -qi 软件包名：查询软件包信息
- rpm -ql 软件包名：查询软件包中的文件
- rpm -qf 文件全路径名：查询文件所属的软件包

#### 3.17.1.4 卸载rpm包

- 语法：rpm -e RPM包的名称

- 示例：

  ```
  删除firefox软件包
  
  ```

- 注意：

  - 如果其他软件包依赖于要卸载的软件包，卸载时则会产生错误信息
  - 如果就是要删除，可以增加参数--nodeps，就可以强制删除，但是一般不推荐，因为依赖于该软件包的程序可能无法运行

#### 3.17.1.5 安装rpm包

- 语法：rpm -ivh RPM包全路径名称
- 参数说明：
  - i=install安装
  - v=verbose提示
  - h=hash进度条

### 3.17.2 yum

- 介绍：
  - yum是一个Shell前端软件包管理器。
  - 基于RPM包管理，能够从指定的服务器自动下载RPM包并且安装，可以自动处理依赖性关系，并且一次安装所有依赖的软件包

#### 3.17.2.1 yum的基本指令

- 查询yum服务器是否有需要安装的软件：
  - yum list | grep xx
- 安装指定的yum包
  - yum install xxx

# 第4章 Linux之JavaEE

## 4.1 概述

- 如果需要在Linux下进行JavaEE开发，需要安装mysql、jdk、java开发工具、tomcat

## 4.2 JDK的安装和配置

1. 先将软件上传到/opt下
2. 解压缩到/opt
   1. tar -zxvf 待解压文件
3. 配置环境变量的配置文件vim /etc/profile
   1. JAVA_HOME=/opt/jdk1.8
   2. PATH=/opt/jdk1.8/bin:$PATH
   3. export JAVA_HOME PATH
   4. 注销环境用户，环境变量才能生效。
      1. 如果在3运行级别，直接logout
      2. 如果在5运行级别，可视化注销
4. 测试是否安装成功
   1. 编写简单的java程序，通过javac和java测试

## 4.3 tomcat的安装

1. 下载tomcat的linux环境安装包
2. 解压缩到/opt
3. 进入tomcat的bin目录，启动tomcat 
   - ./startup.sh
4. 开放端口
   1. vim /etc/sysconfig/iptables
   2. ![image-20220814102307021](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220814102307021.png)
   3. 重启防火墙，才能生效
5. 测试是否成功
   1. 进入http://LinuxIp:8080

## 4.4 idea的安装

1. 解压缩到/opt
2. 启动idea，配置jre和server
3. 编写Hello world程序并测试

## 4.5 mysql8的安装

### 4.5.1 安装

1. 卸载旧版本

   1. 使用rpm -qa | grep mysql检查是否安装有MySQL Server
   2. 如果有旧卸载
      1. 普通删除：rpm -e mysql_libs
      2. 强力删除：rpm -e --nodeps mysql_libs

2. 安装编译代码所需要的包

   1. yum -y install make gcc-c++ cmake bison-devel ncurses-devel

3. 下载MySQL

4. 解压到到/opt

   1. tar xvf mysql-***.tar.gz

5. 进入解压好的mysql目录中

   1. cd mysql-***

6. 编译安装

   ```
   cmake -DCMAKE_INSTALL_PREFIX=/usr/local/mysql
   -DMYSQL_DATADIR=/usr/local/mysql/data -DSYSCONFDIR=/etc
   -DWITH_MYISAM_STORAGE_ENGINE=1 -DWITH_INNOBASE_STORAGE_ENGINE=1
   -DWITH_MEMORY_STORAGE_ENGINE=1 -DWITH_READLINE=1
   -DMYSQL_UNIX_ADDR=/var/lib/mysql/mysql.sock -DMYSQL_TCP_PORT=3306
   -DENABLED_LOCAL_INFILE=1 -DWITH_PARTITION_STORAGE_ENHINE=1
   -DEXTRA_CHARSETS=all -DDEFAULT_CHARSET=utf8
   -DDEFAULT_COLLATION=utf8_general_cl
   ```

7. 编译并安装

   1. make && make install

### 4.5.2 配置mysql

1. 设置权限，查看是否有mysql用户及用户组

   1. cat /etc/passwd ：查看用户列表
   2. cat /etc/group ：查看用户组列表
   3. 如果没有就创建

2. 创建用户及用户组

   1. groupadd mysql
   2. useradd -g mysql mysql

3. 修改/usr/local/mysql权限

   1. chown -R mysql:mysql /usr/local/mysql

4. 初始化配置，进入安装路径（在执行下面的指令），执行初始化配置脚本，创建系统自带的系统库和系统表

   1. cd /usr/local/mysql

   2. ```
      scripts/mysql_install_db --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data --user=mysql
      ```

5. 启动MySQL服务

   1. 注意：会按照一定次序搜索my.cnf，现在/etc目录下找，找不到则会搜索"$basedir/my.cnf"

   2. 有些版本的Linux操作系统中，会在/etc目录下存在一个my.cnf，需要将其改为其他名字，否则该文件会干扰源码安装的Mysql的正确配置，造成无法启动

   3. 添加服务，拷贝服务脚本到init.d目录，并设置开机启动

   4. 在/usr/local/mysql下执行

      ```
      cp suppot -files /mysql.server /etc/init.d/mysql
      chkconfig mysql on
      service mysql start
      ```

   5. 修改root密码

      ```
      cd /usr/local/mysql/bin
      ./mysql -u root -p
      mysql>set password = password('密码')
      ```

   6. 配置mysql环境变量

      1. 打开/etc/profile:vim /etc/profile

      2. 修改如下：

         ```
         PATH=/usr/local/mysql/bin:$PATH
         export PATH
         ```

      3. 刷新profile：source /etc/profile


# 第5章 Shell编程

- Shell是一个命令行解释器，它为用户提供了一个向Linux内核发送请求以便运行程序的界面系统级程序，用户可以用Shell来启动、挂起、停止甚至是编写一些程序。

## 5.1 Shell脚本的执行方式

### 5.1.1 脚本格式要求

- 脚本以#!/bin/bash开头
- 脚本需要有可执行权限

### 5.1.2 编写第一个Shell脚本

- 创建一个Shell脚本

  ```
  echo "hello,shell"
  ```

### 5.1.3 脚本的常用执行方式

- 方式一

  1. 首先赋予shell脚本的+x权限

     chmod 744 myShell.sh

  2. 执行脚本

     ```
     相对路径或绝对路径
     ./myShell.sh
     或
     /root/shell/myShell.sh
     ```

- 方式二

  1. sh ./myShell.sh
  2. sh /root/shell/myShell.sh

## 5.2 Shell的变量

### 5.2.1 介绍

- Linux的Shell中的变量分为：系统变量和用户自定义变量
- 系统变量：$HOME、$PWD、$SHELL、$USER等等
- 显示当前shell中所有变量：set

### 5.2.2 shell变量的定义

- 语法：

  - 定义变量：变量=值
  - 撤销变量：unset 变量
  - 声明静态变量：readonly 变量，注意：不能unset

- 示例

  ```
  定义变量A
  A=100
  撤销变量A
  unset A
  声明静态的变量B=2
  readonly B=2
  可把变量提升为全局环境变量，可供其他shell程序使用
  
  ```

- 定义变量的规则：

  - 变量名称可以由字母、数字和下划线组成，但是不能以数字开头
  - 等号两侧不能有空格
  - 变量名称一般习惯为大写

- 将命令的返回值赋给变量（反引号）

  ```
  A=`ls-la`，运行里面的命令，并把结果返回给变量A
  等价于A=$(ls-la)
  ```

### 5.2.3 设置环境变量

- 语法：

  - export 变量名=变量值
    - 将shell变量输出为环境变量
  - source 配置文件
    - 让修改后的配置信息立即生效
  - echo $变量名
    - 查询环境变量的值

- 示例

  ```
  1、在/etc/profile文件中定义TOMCAT_HOME环境变量
  TOMCAT_HOME=/opt/tomcat
  export TOMCAT_HOME
  
  source /etc/profile
  2、查看环境变量TOMCAT_HOME的值
  echo $TOMCAT_HOME
  3、在另外一个shell程序中使用TOMCAT_HOME
  PATH=$TOMCAT_HOME
  ```

### 5.2.4 位置参数变量

- 说明：当执行一个shell脚本时，如果希望获取到命令行的参数信息，就可以使用到位置参数变量。

- 语法：

  | 语法 | 功能                                                         |
  | ---- | ------------------------------------------------------------ |
  | $n   | n为数字，$0代表命令本身，$1-$9代表第一到第九个参数，十以上的参数需要用大括号包含，如${10} |
  | $*   | 这个变量代表命令行中所有的参数，把所有的参数看成一个整体     |
  | $@   | 代表命令行中的所有参数，不过把每个参数区分对待               |
  | $#   | 代表命令行中所有参数的个数                                   |

### 5.2.5 预定义变量

- 说明：shell设计者事先已经定义好的变量，可以直接在shell脚本中使用

- 语法

  | 语法 | 功能                                                         |
  | ---- | ------------------------------------------------------------ |
  | $$   | 当前进程的进程号(PID)                                        |
  | $!   | 后台运行的最后一个进程的进程号(PID)                          |
  | $?   | 最后一次执行的命令的返回状态，如果这个变量的值为0，证明上一个命令正确执行，如果这个变量的值为非0，则证明上一个命令执行不正确 |

## 5.3 Shell运算符

- 语法

  - "$((运算式))"或"$[运算式]"
  - expr m + n
  - expr m - n
  - expr \\*,/,% 乘，除，取余

- 示例

  ```
  1、计算(2+3)*4
  (1)$(((2+3))*4))
  (2)$[(2+3)*4]
  (3)
  TEMP=`expr 2 + 3`
  RESULT=`expr $TEMP \* 4`
  2、求出命令行的两个参数[整数]的和
  RESULT=$[$1+$2]
  ```

## 5.4 条件判断

- 语法

  - [ condition ]
    - 注意condition前后要有空格
    - #非空返回true，可使用$?验证（0为true，>1为false）

- 示例

  ```
  [] 返回false
  [ x ] 返回true
  [ condition ]$$echo OK || echo notok
  ```

- 常用判断条件

  - 两个整数比较

    | 语法 | 功能       |
    | ---- | ---------- |
    | =    | 字符串比较 |
    | -lt  | 小于       |
    | -le  | 小于等于   |
    | -eq  | 等于       |
    | -gt  | 大于       |
    | -ge  | 大于等于   |
    | -ne  | 不等于     |

  - 按照文件权限进行判断

    | 语法 | 功能         |
    | ---- | ------------ |
    | -r   | 有读的权限   |
    | -w   | 有写的权限   |
    | -x   | 有执行的权限 |

  - 按照文件类型进行判断

    | 语法 | 功能                         |
    | ---- | ---------------------------- |
    | -f   | 文件存在并且是一个常规的文件 |
    | -e   | 文件存在                     |
    | -d   | 文件存在并且是一个目录       |

- 示例

  ```
  1、"ok"是否等于"ok"
  [ "ok" = "ok" ]
  2、23是否大于等于22
  [ 23 -gt 22 ]
  3、/root/install.log目录中的文件是否存在
  [ -e /root/install.log ]
  ```

## 5.5 流程控制

### 5.5.1 if判断

- 基本语法

  - 第一种

    ```
    if[ 条件判断式 ];then
    	程序
    fi
    ```

  - 第二种（推荐）

    ```
    if[ 条件判断式 ]
    	then
    		程序
    		elif[ 条件判断式 ]
    		then
    			程序
    fi
    ```

- 示例

  ```
  输入的参数大于等于60，则输出"及格了",如果小于60,则输出"不及格"
  #!/bin/bash
  if[ $1 -ge 60 ]
  then
  	echo "及格了"
  elif [ $1 -lt 60 ]
  then
  	echo "不及格"
  fi
  ```

### 5.5.2 case语句

- 基本语法

  ```
  case $变量名 in
  "值1")
  程序1
  ;;
  "值2")
  程序2
  ;;
  ...省略其他分支...
  *)
  程序3
  ;;
  esac
  ```

- 示例

  ```
  当命令行参数是1时，输出"周一"，是2时，就输出"周二"，其他情况输出"other"
  #!/bin/bash
  case $1 in
  "1")
  echo "周一"
  ;;
  "2")
  echo "周二"
  ;;
  *)
  echo "other"
  ;;
  esac
  ```

### 5.5.3 for循环

- 语法1：

  ```
  for 变量 in 值1 值2 值3 ....
  	do
  		程序
  	done
  ```

- 示例1

  ```
  打印命令行输入的参数
  #!/bin/bash
  for i in "$*"
  do
  	echo "the num is $i"
  done
  ```

- 语法2

  ```
  for((初始值;循环控制条件;变量变化))
  	do
  		程序
  	done
  ```

- 示例2

  ```
  从1加到100的值输出显示
  #!/bin/bash
  SUM=0
  for((i=1;i<=100;i++))
  do
  	SUM=$[$SUM+$i]
  done
  ```

### 5.5.4 while循环

- 语法

  ```
  while [ 条件判断式 ]
  	do
  		程序
  	done
  ```

- 示例

  ```
  从命令行输入一个数n，统计从1+2...+n的值是多少
  #!/bin/bash
  SUM=0
  i=0
  while [ $i -le $1 ]
  do
  	SUM=$[$SUM+$i]
  	i=$[$i+1]
  done
  ```

## 5.5 read读取控制台输入

- 语法：read(选项)(参数)

- 选项

  - -p:指定读取值时的提示符；
  - -t:指定读取值时等待的时间（秒），如果没有在指定的时间内输入，就不再等待了

- 参数

  - 变量：指定读取值的变量名

- 示例

  ```
  1、读取控制台输入一个num值
  #!/bin/bash
  read -p "请输入一个数num1" NUM1
  echo "输入为$NUM1"
  2、读取控制台输入一个num值，在10秒内输入
  read -t 10 -p "请输入一个数num2" NUM2
  ```

## 5.6 函数

### 5.6.1 系统函数

#### 5.6.1.1 basename

- 语法

  ```
  basename [pathname] [suffix] :返回完整路径最后/的部分，常用于获取文件名
  basename [string] [suffix] ：删掉所有的前缀包括最后一个（'/'）字符，然后将字符串显示出来
  ```

- 选项

  - suffix为后缀，如果suffix被指定了，basename会将pathname或string中的suffix去掉

- 示例

  ```
  返回/home/aaa/test.txt中的"test.txt"部分
  basename /home/aaa/test.txt
  ```

#### 5.6.1.2 dirname

- 语法：dirname 文件绝对路径

- 功能：返回完整路径最后/的前面的部分，常用语返回路径部分

- 示例

  ```
  返回/home/aaa/test.txt的/home/aaa
  dirname /home/aaa/test.txt
  ```

### 5.6.2 自定义函数

- 语法

  ```
  [ function ] funname[()]
  {
  	Action;
  	[return int;]
  }
  
  调用直接写函数名：funname [值]
  ```

- 示例

  ```shell
  计算输入两个参数的和
  #!/bin/bash
  function getSum(){
  	SUM=$[$1+$2]
  	echo "和是$SUM"
  }
  read -p "输入第一个数" n1
  read -p "输入第一个数" n2
  
  #调用
  getSum $n1 $n2
  ```

## 5.7 Shell编程综合案例

### 5.7.1 需求分析

1. 每天凌晨2:10备份数据库db到/data/backup/db
2. 备份开始和备份结束能够给出相应的提示信息
3. 备份后的文件要求以备份时间为文件名，并打包成.tar.gz的形式
4. 在备份的同时，检查是否有10天前备份的数据库文件，如果有就将其删除

### 5.7.2 思路分析

![image-20220814180619103](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220814180619103.png)

### 5.7.3 代码实现

- mysql_db_backup.sh脚本

  ```
  #!/bin/bash
  
  #完成数据库定时备份
  #备份的路径
  BACKUP=/data/backup/db
  #当前时间作为文件名
  DATETIME=$(data +%Y_%m_%d_%H%M%S)
  #给出提示信息
  echo "开始备份...."
  echo "备份路径为 $BACKUP/$DATETIME.tar.gz"
  
  #主机
  HOST=localhost
  #用户名
  DB_USER=root
  #密码
  DB_PWD=123
  #备份数据库名
  DATABASE=db
  
  #创建备份的路径，如果存在就使用，否则就创建
  [ ! -d "$BACKUP/$DATETIME" ] && mkdir -p "$BACKUP/$DATETIME" 
  
  #执行mysql的备份数据库指令
  mysqldump -u$DB_USER -p$DB_PWD --host=$HOST $DATABASE | gzip > $BACKUP/$DATETIME/$DATETIME.sql.gz
  #打包备份文件
  cd $BACKUP
  tar -zcvf $DATETIME.tar.gz $DATETIME
  #删除临时目录
  rm -rf $BACKUP/$DATETIME
  
  #删除10天前的备份文件
  find $BACKUP -mtime +10 -name "*.tar.gz" -exec rm -rf {} \;
  echo "备份成功！！！"
  ```

- 将脚本交给crond执行：crontab -b

  - 10 2 * * * /usr/sinb/mysql_db_backup.sh

