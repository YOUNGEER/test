# Linux

## 介绍

- linux是一款免费，开源，安全，稳定，高效处理处理高并发的一款操作系统
- 基于linux开发的操作系统有redhat，ubantu，红旗等等
- Linux来源于Unix，是Unix的一个分支Minix,的基础上开发出来的内核，是GNU计划的一部分

### Window和VM，CentOs的关系

- 在window操作系统的电脑安装VM
- VM开辟了一个空间后，可以在该空间按照CentOs操作系统
- 安装好系统后，就可以进行Linux相关的开发了



### 命令行

- Ctrl l :清屏/clear
- ctrl+c：退出当前状态
- q：退出当前阅读模式
- pwd：当前目录

- ip addr(centOS 7)
- ipconfig(centOS 6)
- linux严格区分大小写
- tab键可以快递不全敲的命令
- linux中，所有的内容都是以文件的形式保存的，比如
  - 硬盘文件是 /dev/sd[a-p]
  - 光盘文件是/dev/sr0等

### Linux目录作用

- /bin/：存放系统命令的目录，普通用户和超级用户都可以执行。在单用户模型下也可以执行。
- /sbin/：保存和系统环境设置相关的命令。只有超级管理员才可以使用这些命令进行设置改动。有些命令普通用户也可以查看。
- /usr/bin/：存放系统命令的目录，普通用户和超级用户都可以执行。这些命令和系统启动无关，在单用户模式下不可以执行。
- /usr/sbin/：存放根文件系统不必要的的系统管理命令，例如多数服务程序。只有超级管理元才可以执行
- /boot/：系统启动目录，保存系统启动相关的文件。如内核文件和启动引导程序的文件。
- /dev/：设备保存文件位置。Linux中所有内容都以文件形式保存，包括硬件，那么这个目录就是用来保存所有硬件设备文件的。
- /etc/：配置文件保存位置。系统中所有通过默认安装方式（rpm）的服务配置文件都是保存在这里。例如账户和密码，服务的启动脚本，常用的服务配置文件。
- /home/：用户的家目录。每个用户在建立时，都要有个默认登陆位置。所有的普通用户的家目录就是在home在建立一个文件夹。比如用户user1的家目录就是/home/user1
- /lib/：系统调用的函数库位置
- /lost+found/：在系统发生奔溃的情况下，而产生的一些文件碎片就放在这里。一般每个分区中都会有这样一个目录文件。（centOS 7中并没有发现这个目录）
- /media/：挂载目录，系统是建议用来挂载媒体设备的，例如软盘和光盘。
- /mnt/：挂载目录。建议挂载额外设备的。比如U盘，移动硬盘和其他操作系统的分区。
- /misc/：挂载目录。建议挂载nfs服务的共享目录。
- /opt/：安装第三方目录保存的位置。但是一般习惯是安装在/usr/local的目录下。
- /proc/：虚拟文件系统，该目录中的文件并不是保存在硬盘中，而是保存在内存中
- /sys/：虚拟文件系统，和proc类似，都是保存在内存中，主要保存内核相关的信息。
- /root/：超级用户的家目录，普通用户是在home下的
- /srv/：系统在运行的时候的服务数据
- /tmp/：系统的临时文件
- /usr/：系统资源目录，Unix Softwre Resource，
- /var/：动态资源保存目录。比如日志文件以及软件在运行过程中产生的数据目录



## linux命令

>- 命令的格式
>
>  命令 [-选项] [参数]，例如：ls -la /etc
>
>- 说明
>
>  - 个别命令不遵循此格式
>  - 当有多个选项时，可以写在一起
>  - 简化选项和完整选项，比如 -a 等于 --all

### 文件处理命令

#### ls

- 功能描述：显示目录文件

- 英文原意：list

- 所在目录：/usr/bin/ls

- 语法：ls 选项[-ald] [文件或目录]

  - a：表示显示所有的文件 -a时--all的简写，一般会显示隐藏的文件

  - -l：表示显示文件的完整信息，表示list，一般完整信息分为七个部分 
- 所有者权限所在组权限其他人权限.  r读，w写，x执行，比如rw-r-----,表示所有者有读写权限，所在组有读权限，其他人没有权限
  
- 引用计数
    - 所有者
    - 所在组
    - 文件大小 ，一般显示时byte，用-h可以显示k，m的单位，h表示human，人性化
    - 文件时间 
    - 文件目录名称
    
- -d：directory，表示只显示当前目录本身的信息，而不会显示目录下的文件信息
  
- -i：查询文件/目录的i节点

#### cd

- 功能描述：切换目录
- 英文原意：change directories
- 所在目录：shell内置命令
- 语法：cd  [目录名]
  - cd /tmp/text1 切换到目录text1上
  - cd ..         表示返回到上一级目录

#### mkdir

- 功能描述：创建新目录
- 英文原意：make directories
- 所在目录：/usr/bin/mkdir
- 语法：mkdir -p [目录名]
  - -p：表示如果创建的目录不存在，会进行循环创建
  - 可以一次性创建多个文件目录 ，比如mkdir -p /tmp/text1 /tmp/text2

#### rmdir

- 功能描述：删除**空**目录
- 英文原意：remove empty directories
- 所在目录：/usr/bin/rmdir
- 语法：rmdir  [目录名]
  - rmdir /tmp/text1
  - 删除的目录必须是空的目录

#### cp

- 功能描述：复制文件或目录
- 英文原意：copy
- 所在目录：/usr/bin/cp
- 语法：cp -rp  [原文件或目录，可以是多个文件] [目标目录]
  - -r：复制目录
  - -p：保留文件属性，比如文件的修改时间

#### mv

- 功能描述：剪切文件/改名
- 英文原意：move
- 所在目录：/usr/bin/mv
- 语法：mv  [原文件或目录，可以是多个文件] [目标目录]
  - 剪切：mv /tmp/text1 /root
  - 改名：mv text1 text2

#### rm

- 功能描述：删除文件或目录
- 英文原意：remove
- 所在目录：/usr/bin/rm
- 语法：rm -rf  [文件或目录]
  - -r：删除目录
  - -f：force，强制删除，没有这个选项，一般是会提醒你是否删除，同时linux没有回收站的概念

#### touch

- 功能描述：创建空的文件
- 所在目录：/usr/bin/touch
- 语法：touch [文件名]
  - 如果文件名是带空格的，可以用``包裹起来

#### cat

- 功能描述：查看文件的内容
- 所在目录：/usr/bin/cat
- 语法：cat [文件名]
  - -n：显示行号
  - tac：可以倒着行显示内容

#### more

- 功能描述：分页显示文件内容
- 所在目录：/usr/bin/more
- 语法：more [文件名]
  - 空格/f：翻页
  - enter：换行
  - q/Q：退出

#### less

- 功能描述：分页显示文件内容,可以往上翻页，
- 所在目录：/usr/bin/more
- 语法：more [文件名]
  - pageup/pagedown
  - /key，可以实现对key的搜索

#### head

- 功能描述：显示文件内容的前几行
- 所在目录：/usr/bin/head
- 语法：head -n 行数 [文件名]
  - n后面加行数，默认10行

#### tail

- 功能描述：显示文件内容的后几行
- 所在目录：/usr/bin/tail
- 语法：tail -n 行数 [文件名]
  - n后面加行数
  - -f：可以动态查看文件的尾部数据，比如可以查看请求日志的最新记录，界面会不断刷新最新的日志记录，在调试的时候非常实用，用的也特别多。  tail -n 100  -f

#### ln

- 功能描述：生成链接文件，类似windows的快捷方式
- 所在目录：/usr/bin/ln
- 英文：link
- 语法：ln -s [源文件] [目标文件]
  - -s：创建软链接文件
  - 软连接，文件类型为lrwxrwxrwx,有箭头指向
  - 硬连接，类似文件复制，修改一份文件，另一份也会随着改变

### 权限管理命令

#### chmod

- 功能描述：改变文件或目录权限
- 所在目录：/usr/bin/chmod
- 英文：change the permission of a file
- 语法：chmod ugoa +-=  rwx[文件，目录]
  - ugoa：表示user,group,other,all,文件的所得者，组，其他和所有人
  - +-=：+表示添加权限，-表示移除权限，=更改成此权限
  - rwx：表示读，写和执行权限
  - 文件的权限是对文件的内容权限，文件夹的权限是对文件夹里面的文件权限
  - 权限也可以用数字来表示，rwx分别为421，所有1-7表示3个权限的混合
  - 用数字表示可以为：chmod 640 text.txt 

#### chown

- 功能描述：改变文件或目录的所有者权限
- 所在目录：/usr/bin/chown
- 英文：change file ownership
- 语法：chmod [用户] [文件/目录]
  - 只有root用户才有权限修改文件权限

#### chgrp

- 功能描述：改变文件或目录的所属组
- 所在目录：/usr/bin/chgrp
- 英文：change file group ownership
- 语法：chmod [用户] [文件/目录]
  - 只有root用户才有权限修改文件权限

#### umask

- 功能描述：显示，设置文件的缺省权限
- 所在目录：/usr/bin/umask
- 英文：change file group ownership
- 语法：umask -S
  - -S：以rwx形式显示权限
  - umask 077：将077和777进行相减，得到的数值就是文件夹的默认权限，每个权限去掉一个x，就是默认文件的权限

### 文件搜索命令

#### find

- 功能描述：显示，设置文件的缺省权限
- 所在目录：/usr/bin/find
- 语法：find [搜索范围] [匹配条件]
  - find /home -name init：在home中查找文件名为init的文件
  - -iname：表示不区分大小写查找
  - init*：  * 表示通配符任意个任意字符
  - init？：？表示通配符一个任意字符
  - find /home -size +2048000 ：查找大于100M的文件
  - find /home -user wy：查找所有者是wy的文件
  - find /home -amin -6：6分钟内进入过的文件
  - find /home -cmin -7：7分钟内修改过属性的文件
  - find /home -mmin -8：8分钟内修改过内容的文件
  - find /home -amin -6 a -cmin -7：a表示两个条件都需要满足，同理，o表示两个条件满足一个即可
  - find /home -name wy -a type f/d/l ：f：文件，d：目录，l：软链接
- find /home -name wy -exec ls -l {} \;前面表示所有，后面的固定写法表示对结果进行操作

#### Locate

- 功能描述：在文件资料库中查找文件
- 所在目录：/usr/bin/locate
- 语法：locate 文件
  - 通过一个文件资料数据库来实现快速查找的
  - tmp文件下的文件是无法找到的

#### Which

- 功能描述：查找命令所在的文件位置及命令的别名
- 所在目录：/usr/bin/which
- 语法：which 命令
  - 比如 which ls

#### whereis

- 功能描述：查找命令所在的文件位置及帮助文档的位置
- 所在目录：/usr/bin/whereis
- 语法：whereis 命令
  - 比如 whereis  ls

#### grep

- 功能描述：在文件中搜索字符串匹配的行并输出
- 所在目录：/usr/bin/grep
- 语法：grep -iv [指定字符串] [文件]
  - -i：不区分大小写
  - -v：排除指定字符串，反向查找

#### man

- 功能描述：获取帮助信息
- 所在目录：/usr/bin/man
- 英文原意：manual
- 语法：man 命令或配置文件

#### whatis

- 功能描述：获取命令的简短信息

#### apropos

- 功能描述：获取配置的相关信息

#### --help

- 功能描述：获取命令的选项
- 使用:命令 --help

#### help

- 功能描述：获取shell内置命令的帮助，一般man没有相关文档
- 使用:help 命令
  - 比如：help cd

#### type

- 功能描述：获取命令是否是shell命令还是普通命令
- 比如：type cd

#### useradd

- 功能描述：添加新用户
- 所在目录：/usr/bin/useradd
- 语法：useradd 用户名

#### passed

- 功能描述：更改用户密码
- 所在目录：/usr/bin/passwd
- 语法：passwd 用户名

#### who

- 功能描述：查看用户登陆信息
- 所在目录：/usr/bin/who
- 语法：who

#### w

- 功能描述：查看用户登陆相信信息
- 所在目录：/usr/bin/w
- 语法：w

#### gzip

- 功能描述：压缩文件，压缩后的格式是.gz
- 所在目录：/usr/bin/gzip
- 语法：gzip 文件

#### gunzip

- 功能描述：解压缩文件
- 所在目录：/usr/bin/gunzip
- 语法：gunzip 文件

#### tar

- 功能描述：压缩/解文件
- 所在目录：/usr/bin/tar
- 语法：tar [选项-zcf] [压缩后文件名] [文件]；参数f要放在最后面
  - -c：打包
  - -v：显示详细信息
  - -f：指定文件名
  - -z：打包同时压缩
  - -x：解包
  - 例如：tar -zcf name.tar.gz file1
  - 例如：tar -zxvf name.tar.gz 

#### zip

- 功能描述：压缩文件或目录
- 所在目录：/usr/bin/zip
- 语法：zip -r [压缩后文件名] [文件]
  - -r：表示压缩目录
  - 压缩后文件.zip

#### unzip

- 功能描述：解压缩文件或目录
- 所在目录：/usr/bin/unzip
- 语法：unzip  [压缩后文件名]

#### bzip2

- 功能描述：压缩文件或目录，就是zip的升级版
- 所在目录：/usr/bin/bzip2
- 语法：bzip2 -k [压缩后文件名] [文件]
  - -k：表示在压缩的同时，保留原文件
  - 压缩后的格式.bz2
  - 解压缩：unbzip2



### 网络命令

#### ping

- 功能描述：测试网络连通性
- 所在目录：/usr/bin/ping
- 语法：ping 选项 ip地址
  - 比如：ping 192.168.0.128
  - -c：ping的次数，ping -c 3 192.168.0.128

#### ifconfig

- 功能描述：查看和设置网络信息
- 所在目录：/sbin/ifconfig
- 语法：ifconfig 网卡名称 ip地址



#### last

- 功能描述：列出目前和过去登陆过系统的人信息
- 所在目录：/bin/last
- 语法：last



#### lastlog

- 功能描述：检查某特定用户登陆过系统的信息
- 所在目录：/bin/lastlog
- 语法：lastlog -u 用户



#### traceroute

- 功能描述：显示数据包到主机之间的路径
- 所在目录：/bin/traceroute
- 语法：traceroute 主机名
  - 比如：traceroute www.baidu.com

#### netstat

- 功能描述：显示网络相关信息
- 所在目录：/bin/traceroute
- 语法：netstat [选项]
  - -u：UDP协议
  - -t：TCP协议
  - -l：监听
  - -r：路由
  - -n：显示IP地址和端口号

#### Setup

- 功能描述：配置网络信息
- 所在目录：/bin/setup
- 语法：setup

### 关机重启命令

#### shutdown

- 功能描述：开关机命令
- 所在目录：/bin/shutdown
- 语法：shutdown 选项
  - -c：取消前一个命令
  - -h：关机
  - -r：重启

#### reboot

- 功能描述：关机命令
- 所在目录：/bin/reboot
- 语法： reboot



## 文本编辑Vim

>Vim是一个功能强大的全屏幕文本编辑器，是Linxu/Unix上最常用的文本编辑器，它的作用是建立，编辑，显示文本
>
>Vim没有界面，只有命令
>
>打开一个文件 
>
>- vi 文件路径
>- 默认进去后是命令模式



### 编辑模式

>按：可以进入编辑模式

- :set nu   ：设置显示行号
- :set nonu   ：设置取消行号
- :w：保存
- :w newfile：保存指定文件
- :wq：保存退出，快捷键ZZ
- :q!：不保存退出
- :%s/old/new/g：全局替换，old老字符串，new新字符串
- :n1,n2s/old/new/g：指定行进行替换

### 插入模式

>以下可以用命令模式进入到插入模式

- a：在光标所在字符后插入
- A：在光标所在行尾插入
- i：在光标所在字符前插入
- I：在光标所在行首插入
- o：在光标上行插入
- O：在光标下行插入

### 命令模式

- 按esc可以从编辑模式进入命令模式

>定位

- gg：定位到第一行
- G：定位到最后一行
- nG：定位到第n行
- :n.   ：定位到第n行

- $：移到行尾
- 0：移到行首

>删除

- x：删除光标所在的字符
- nx：删除光标所在后n个字符
- dd：删除光标所在行，ndd，删除n行
- dG：删除从光标到文件末尾的内容
- D：删除光标所在字符到行末尾的内容
- :n1,n2d,删除指定范围行

>复制和剪切，需要进行组合

- yy：复制光标当前行，nyy：复制当前光标n行
- dd：剪切光标当前行，ndd：剪切n行
- p：小写，结合yy和dd，复制或者剪切到当前光标行的下面
- P：大写，结合yy和dd，复制或者剪切到当前光标行的上面

>取消和替换

- r：替换当前字符
- R：替换当前n个字符，按esc退出
- u：取消上一步操作，回退
- /string：字符串查找，string表示要查找的字符串
- n：结合查找到的字符串，n表示下一个位置

### 使用技巧

- :r !命令：可以导入该命令执行结果



## 软件包管理

>- 源码包
>  - 安装慢，能看到源码，容易报错
>- 二进制包/rmp
>  - 安装更加迅速，看不到源码

### RPM命令管理

- 命令规则
- 包的依赖性。 依赖网站查询 www.rpmfind.com

- 安装：rpm -ivh 包全名
  - -i：install安装
  - -v：verbose显示详细信息
  - -h：hash显示进度
  - --nodeps：不检测依赖性，不建议使用，安装可能会有问题
- 升级：rpm -Uvh 包全名
- 卸载：rpm -e 包名
- 查询：rpm -q 包名
  - -qa：查询所有的安装包
  - -qi：包的详细信息
  - -qp：查询未安装包
  - -qf：查询系统文件属于哪个安装包
- 校验：rpm -V

### YUM在线管理

- 显示：yum -list
- 安装：yum -y install 包名 ，-y表示自动安装
- 升级：yum -y update 包名，一定要跟上包名，否则其他的也会升级，容易出问题
- 卸载：yum -y remove 包名，卸载的时候，关联包也容易卸载出现问题，不建议卸载
- 软件包组：yum groupinstall/update/remove 包组



## shell

>- linux交互界面，命令解释器
>- 编程语言，可以直接调用linux的命令
>- 分类bs，cs，bash

#### echo命令

- 输入命令
- echo [选项] [输出内容]
  - -e：支持反斜线控制的字符转换，比如echo -e "abc\nd"=>abc 换行 d

#### 执行脚本

- 给权限chmod 755 ./name.sh

  /.name.sh 老运行

- 直接运行 Bash ./name.sh

### Bash的基本命令

>历史命令

- history [选项] [历史命令保存文件]
  - -c：情况历史命令
  - -w：把内存缓存中的历史命令刷到对应的历史命令文件中，文件名是~./bash_history
  - !n：快速执行第n行命令
  - !!：重复执行上一条命令
  - !字符：重复执行上一条以该字符开头的命令

>命令的别名

- alias 别名='命令'
- root/.bashrc文件中存放着该用户的永久保存的别名
- unalias 删除别名

>Bash的快捷键

- ctrl+a：把光标移到命令行开头
- ctrl+c：强制终止当前命令
- ctrl+e：把光标移到命令结尾
- ctrl+l：清屏
- ctrl+u：删除光标左边的所有命令
- ctrl+k：删除光标右边的所有命令
- ctrl+y：粘贴删除的命令
- ctrl+d：退出当前终端

>输入，输出重定向

- 命令 > 文件，命令正确执行，输出保存到文件中，命令 >> 文件,命令正确执行，输出追加保存到文件中
- 命令 2> 文件，命令错误执行，输出保存到文件中，命令 2>> 文件,命令错误确执行，输出追加保存到文件中
- 命令 &> 文件，命令无论正确还是错误执行，输出保存到文件中，命令 &>> 文件,命令无论正确还是错误执行，输出追加保存到文件中
- 命令 >> 文件1 2>> 文件2，把正确执行的保存在文件1中，把错误执行的保存在文件2中

>多命令执行顺序

- 命令1 ；命令2，分号，表示两个命令无论正确与否，都会执行
- 命令 &&命令2，表示第一个命令正确执行了，第二个命令才会执行，如果第一个未正确执行，第二个不会执行

- 命令1 || 命令2，表示第一个命令未正确执行，第二个命令才会执行,如果第一个正确执行了，第二个不会执行

>管道符 |

- 命令1 | 命令2，命令1正确输出的内容作为命令2的执行对象，比如ls | grep wy

>通配符

- ？：匹配任意一个（0个不行）字符
- *：匹配0到n个任意字符
- [abc]：匹配abc中的一个
- [a-z]：匹配a到z中的一个
- [^a-z]：匹配一个不是a到z中的字符

>Bash中的特殊符号

- ‘’：单引号，会正确输出单引号里面的内容，无论是空格是$,转义符号等
- ""：双银号，会执行特殊符号的意义
- ``：反银号，作用和$()一样，可以执行shell相关的函数,推荐使用$(),比如$(date)
- $：用于调用变量的值
- \：转义符号

>系统环境变量 ，路径未$PATH，系统默认的环境变量名都是大写的

- export 变量=值
- evn 查看变量
- unset 删除变量

>位置参数变量

- $0,$1.....${10}...，表示参数，$0表示文件名，$1至${10}表示shell的参数1到10，比如 bash.sh 2 4
- $*：表示把参数当作一个整体，比如for循环中只会循环一次
- $@：表示把参数都看成独立的个体，

>预定义变量

- $?：表示最后一次执行命令的结果，如果这个变量值是0，表示正确执行，如果非0，表示未正确执行

- $$：当前进程的进程号PID
- $!：表示后台运行的最好一个进程号PID

>接收键盘输入

- read [选项] [变量名]

  - -p：提示信息

  - -t：等待秒数

  - -n 字符数：read接收指定的字符数，就会执行

  - -s：隐藏输入的内容

  - 比如 read -t 30 -p "please input your name : " name

    Echo $name

>数值运算，shell中，变量都是字符串，所以需要 双括号,或者中括号来进行数值计算

```shell
a=10
b=10
echo $(($a+$b))   =>20
```

### 环境变量的配置

### 正则表达式

### 字符串命令

#### 截取cut

- cut [选项] 文件名

  - 结合grep使用

  - -f 列号：提取第几列
  - -d 分割符：按照指定分割符分割列
  - 比如 cut -d ":"  -f 1,3 /etc/password

#### 格式化输出printf

- Printf “输出类型和格式” 输出内容

  - %ns：输出字符串，n是数字，表示输出几个字符串
- %ni：输出数字，n是数字，表示输出几个数字
  - %m.nf：输出浮点数,m表示输出的位数，n表示输出的小数，比如%8.2，2位小数，6位整数
  - \n：换行
  - \t：水平输出tab
- \v：竖直输出tab

#### awk命令

- 文件输出命令

- 语法：awk '条件1 {动作1} 条件2 {动作2} ...' 文件名
  - 条件是返回boolean类型，true就执行动作
  - 动作中：print 输出，默认加换行\n，printf输出，默认不加换行，$1...$6表示地几列
  - 比如 awk ''{print $1 "\t" $6}'' student.txx

#### sed

### 字符串处理

#### sort 排序

- 语法：sort 选项 文件夹

  - -f：忽略大小写

  - -n：数值进行排序，默认使用字符串排序

  - -r：反向排序

    

### if

```linux
// 单分支

if [条件]；then
  	程序
fi
//或者
if [条件]
	then
		程序
fi

//比如：
if [a>b]; then
	echo 'a 大于 b'
fi

//if -else
if [条件]
	then
		程序
	else
		程序
fi
```

### case

```linux
case 变量 in
	"值1")
				程序
				::
	"值2")
				程序
				::
esac				
```

### for

```linux
//语法1
for 变量 in 数1 数2 ...
		do
			程序
		done
		
//语法2
for (( 初始值；循环条件；变量变化 ))
		do
			程序
		done
```

### while---until

```linux
//条件成立就执行
while [条件判断]
		do
			程序
		done
//知道条件成立才执行		
until [条件判断]
		do
			程序
		done		
```



