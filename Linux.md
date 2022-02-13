# 1.Linux系统分区

boot1G/swap2G/根分区17G

swap分区：临时充当内存 速度不如内存

linux下隐藏文件是以.开头的

ctrl+c通用退出

路径末尾＋/算文件夹 不加算文件

蓝色代表目录/白色代表普通文件/红色是压缩文件或包文件

/绿色可执行的文件或程序/

ctrl+back 删除空格字符

# 2.网络连接三种模式

1.桥接模式，虚拟机系统可以和外部系统通讯，容易造成ip冲突

2.NAT模式，可以给虚拟机虚拟一个ip与主机通信，当需要与外部通讯时自动转换为主机ip，不占用外部ip，避免外部冲突，又叫网络地址转换模式

对于nat模式，虚拟机可以与外部通讯，但是外部与虚拟机通讯时得先到实体机再到虚拟机

3.主机模式，独立的系统，不和外部联系

# 3.Linux命令

#root权限

$普通用户

## 3.1Linux目录结构

/root 管理员用户主目录

/bin 存放最常使用的命令

/sbin 系统管理员使用的系统管理程序

/home 存放普通用户的主目录

/lib 系统开机所需要的最基本的动态连接共享库

/lost+found 一般是空的，系统非法关机后，存放一些文件

/etc 所有的系统管理所需要的配置文件和子目录

/usr用户安装的程序

/boot存放一些Linux启动核心文件

/proc《不能动  系统内存的映射与/srv /sys 都不能动

/tmp存放临时文件

/dev类似于设备管理器，把所有硬件用文件形式存储

/media 自动识别设备，把u盘光驱这些插入的设备挂载到这个目录下

/mnt 挂载，让用户临时挂载别的文件系统，将外部存储挂载在/mnt上，然后就可以查看内容 

/opt 给主机额外安装软件所存放的目录 默认为空

/usr/local 一般是通过编译源码方式安装的程序

/var 不断扩充的东西，比如各类日志

/selinux 保证系统安全的东西

ifconfig 查看ip

cmd中ping +ip/192.168.70.130 查看两个机器是否能正常通讯



## 3.2关机重启命令

shutdown -h now 立刻关机

shutdown -h 1  一分钟后关机

shutdown -r now 立刻重启

halt 关机

reboot 立刻重启

sync 把内存数据同步到磁盘

## 3.3 su - 用户名进行切换（有空格）

logout注销用户

## 3.4用户管理

添加用户

useradd 用户名 默认该用户的家目录再/home 登录milan自动切换到/milan

设置密码 基本语法 passwd 用户名

给milan指定密码

pwd 显示当前用户所在目录

## 3.5删除用户

userdel

1.保留家目录

userdel 用户名

2.删除用户以及用户主目录

userdel -r 用户名

## 3.6查询用户信息指令

id 用户名

查看当前用户 whoami

## 3.6用户组

用户组：系统可以对有共性的多个用户进行统一管理

新增组：groupadd 组名

删除组：groupdel 组名

增加用户时可以直接加上组名

useradd -g 用户组 用户名

新增用户不加组的话会默认生成一个与用户名相同的组名并把该用户加入这个组内

usermod -g 组名 用户名   更改用户所属的组

相关文件：/etc/passwd文件：记录用户各种信息

每行含义：用户名：口令：用户标识号（uid）：组标识号（gid）:注释性描述：主目录：登录shell

/etc/shadow：口令配置文件

每行含义：登录名：加密口令：最后一次修改时间：最小时间间隔：最大时间间隔：警告时间：不活动时间:失效时间:标志

/etc/group 组的配置文件 记录Linux包含的组的信息

每行含义：组名：口令：组标识号：组内用户列表（被隐藏）

# 4.vim

正常模式 vim

插入模式 按i I O o A a R r 任意一个字母进入编辑模式，按esc退回正常模式

命令行模式 输入esc到正常模式  输入：wq(保存并退出) ：q 退出：q! 强制退出不保存

 vim快捷键

1.拷贝当前行yy 拷贝当前行向下的5行，5yy 并粘贴【p】

2.删除当前行dd  5dd

3.在文件中查找某个单词 /单词  按回车查找 按n查找下一个

4.设置文件行号 取消文件行号  ：set nu ：set nonu

5.编辑/etc/profile文件 使用快捷键到该文档最末行【G】最首行【gg】

6.输入hello 使用u撤销

7.编辑/etc/profile文件  将光标移动到20行 shift+g

# 5.使用指令

## 5.1运行级别

命令:init+0123

0：关机

1：单用户 ---可以找回丢失密码

2：多用户状态没有网络服务 **很少使用**

3：多用户状态有网络服务   **常用**

4：系统未使用保留给用户

5：图形界面

6：系统重启

**3=multi-user**

**5=graphical**

systemctl get-default 得到当前运行级别

systemctl set-default TARGET.target

需要reboot

# 5.2重置/找回root密码

重启时候 按e 进入找到Linux16开头的那行，在行末尾输入init=/bin/sh 按下ctrl+x进入单用户模式

在光标闪烁处输入： mount -o remount,rw /  注意空格

在新的一行最后输入passwd 

输入新密码，再次确认密码

输入： touch空格/.autorelabel

输入 exec空格/sbin/init等待自动修改密码

## 5.3帮助指令

man获得帮助信息

eg:man ls

ls选项可以组合使用，也可以指定某个目录

ls -al /root

-l 按列表显示

-a包括显示隐藏文件

help 获得shell内置命令的帮助信息

## 5.4文件目录类

pwd显示当前工作目录的绝对路径

绝对路径：从/开始定位

相对路径：从当前开始定位

cd指令：cd 参数 切换到指定目录

cd ~或者cd :回到自己的家目录

cd ..回到当前目录的上一级目录

mkdir指令 用于创建目录 -p 创建多级目录

rmdir 删除空目录 如果需要删除非空目录则使用rm -rf 目录

touch 创建空文件 

cp指令 拷贝文件到指定目录 cp 选项 source dest 常用选项：-r 递归复制整个文件夹 \cp强制覆盖不提示

rm移除文件或目录  rm 选项 删除的文件或目录

-r 递归删除整个文件夹 -f 强制删除不提示

mv指令 移动文件与目录或重命名

mv oldNameFile newNameFile重命名

mv pig.txt /root/cow.txt移动并重命名 无cow.txt则仅移动

移动整个目录 mv bbb/ /home/

cat:查看文件内容（只能查看不能修改 比vim安全）

cat 选项 要查看的文件  -n显示行号 可以加管道命令 |more ：把前面的结果交给下一个命令执行 进行交互

less 分屏查看文件内容类似于more但是更加强大，查看大文件更方便

​		空格翻页，pagedown下翻pageup上翻

​		 /字串，向下查找字串 n下N上

​		？字串，向上查找字串 n上N下

​		q离开less这个程序

echo指令输出内容到控制台： echo 选项 输出内容

head指令 显示文件开头的部分内容 默认显示10行

head -n 5 文件：查看头五行内容

tail是显示文件尾部 -f实时追踪文档所有更新

>1   >指令与>>指令 
>
>前者为输出重定向 后者为追加
>
>》输出重定向eg： echo hello>mydata.txt 相当于覆盖
>
>》》不覆盖，追加
>
>ls -l>文件 列表内容覆盖到文件
>
>ls -al>>文件 追加到文件末尾
>
>cat 文件1>文件2 将1的内容覆盖到文件2
>
>echo "内容">>文件 追加

ln指令：软链接 又称符号链接 类似于windows的快捷方式，存放链接其他文件的路径

ln -s 原文件目录 软链接名（自己定）

案例：在home下创建myroot连接到/root目录：ln -s /root /home/myroot

history 查看已经执行过的历史命令，

> history 10查看最近十条指令
>
> !5执行曾经执行过的第五条指令

## 5.5时间日期类指令

date显示当前日期

> date 显示当前时间
>
> date +%Y仅显示年份 m月份 d日
>
> date "+%Y-%m-%d %H:%M:%S"显示年月日时分秒
>
> date -s设置日期 :date -s"2020-11-03 20:02:10"
>
> hwclock -s改回来当前日期

cal指令 显示本月日历

cal 2020显示整年日历

## 5.6搜索查找类

* find 从指定目录向下递归遍历各个子目录，将满足条件的文件或者目录显示在终端

  find +搜索范围 +选项

-name<查询方式>按照指定文件名查找模式查找文件，也可以用匹配符

find /home -name hello.txt

-user<用户名>查找属于指定用户名所有文件

find /opt -user nobody

-size<文件大小>查找指定大小的文件 +n大于 -n小于 n等于

find / -size +200M(单位有k M G)

* locate指令 快速定位文件路径 基于数据库进行查询 所以必须定期更新，使用updatedb指令创建locate数据库

* which指令 查看某个指令在哪个目录下 eg：which ls

* grep指令与管道符号|

grep 过滤查找 管道符| 表示将前一个命令的处理结果输出传递给后面的命令处理

grep选项+查找内容+源文件

-n显示行号

-i忽略字母大小写

eg： cat /home/hello.txt | grep "yes"

grep -n "yes" /home/hello.txt

## 5.7压缩和解压

* gzip/gunzip指令 压缩/解压

gzip 文件 只能压缩为.gz文件

gunzip 文件.gz

* zip/unzip指令更常用一点

zip 选项 xxx.zip压缩文件和目录

unzip 选项 xxx.zip解压缩文件

zip常用选项 -r：递归压缩，即就是压缩目录

zip -r myhome.zip /home/    (压缩包括home本身)

unzip常用选项 -d+<目录>：指定解压后文件的存放目录

unzip -d+目录+ 文件目录

* tar指令 既可以打包也可以解压

  打包后的文件是.tar.gz文件

  tar 选项 xxx.tar.gz 打包内容

  -c产生.tar打包文件  -v显示详细信息 -f 指定压缩后的文件名 

  -z打包同时压缩     -x解包.tar文件

# 6.组管理与权限管理

## 6.1 基本介绍

每个用户必须属于一个组，不能独立于组外 

对于Linux中每个文件 有**所有者/所在组/其他组**的概念

* **1.所有者** 一般为文件的创建者，谁创建了该文件，就自然的成为所有者

ls -ahl查看文件所有者 

其中首列为文件所在用户 第二列为文件所属组

* **改变文件所有者** chown 用户名 文件名

* 组的创建 groupadd 组名

  创建用户放入组 useradd -g 组名 用户名 （-g是添加用户到指定的组）

* **2**.**修改文件所在的组** chgrp 组名 文件名

* **3.其他组** ：除文件的所有者和所在组的用户外，系统的其他用户都是文件的其他组

  改变用户所在的组

  usermod -g 新组名 用户名

  usermod -d 目录名 用户名 改变该用户登录的初始目录/用户得有进入新目录的权限

## 6.2权限的基本介绍

ls -l中显示内容如下

***-rw-------. 1 root root  1883 2月   7 17:45 anaconda-ks.cfg***

***drwxr-xr-x. 2 root root  4096 2月   7 18:04 公共***

**文件类型/文件拥有者权限/同组用户权限/其他用户权限/  r=4，w=2，x=1**

**1/2含义 如果是文件：硬链接数 是目录：子目录数**

**第一个root 是所属用户/第二个root是组** 

**1883是大小（字节）如果是文件夹显示4096**

#### **日期时间是最后修改的时间 最后为文件名**

前面有0-9位

* 第0位确定文件类型（d,-,l,c,b）

l是链接，相当于windows的快捷方式

d是目录，相当于windows的文件夹

c是字符设备文件，鼠标键盘

b是块设备，比如硬盘

-是普通文件

* 1-3位确定所有者拥有该文件的权限 ---User
* 4-6位确定所属组拥有该文件的权限 ---Group
* 7-9位确定其他用户拥有该文件的权限 ---Other

##### **rwx作用到文件**

* r代表可读，查看 

* w代表可写，修改，但是不代表可以删除，删除文件的前提是对该文件所在的目录拥有写权限，才能删除文件

* x代表可执行

##### **rwx作用到目录**

* r代表可读 ls查看目录内容
* w可以修改，对目录内创建/删除/重命名目录
* x可执行，进入该目录 比如执行cd

**chmod 修改权限**

使用= + -变更权限

u所有者 g所有组 o其他人 a所有人（u+g+o）

chmod u=rwx ，g=rx ，o=x 文件/目录名

chmod o+w 文件/目录名 附加权限

chmod a-x 文件/目录名 减少权限

也可以用数字变更权限

chmod u=rwx，g=rx，o=x abc相当于chmod 751 abc

**chown修改文件所有者**

chown newowner 文件/目录  改变所有者

chown newowner：newgroup 文件/目录 改变所有者和所在组

如果是目录 则使其下所有子文件或目录递归生效 加-R

**chgrp修改文件所在组**

chgrp newgroup 文件/目录  改变所有组

如果要对文件夹内/目录内的文件操作 先得拥有对该目录的相应权限

# 7任务调度

## 7.1crond任务调度

任务调度：指系统在某个时间执行的特定的命令或程序

任务调度分类：1.系统工作：有些重要工作需要周而复始的执行，比如病毒扫描或者MySQL的备份

基本语法: crontab 选项

-e 编辑crontab定时任务

-l 查询crontab任务

-r 删除当前用户所有的crontab任务

```xml
*/1**** ls -l /etc/>/tmp/to.txt
第一个* 一小时当中的第几分钟 0-59  
第二个* 一天当中的第几小时 0-23
第三个* 一月当中的第几天 1-31
第四个* 一年当中的第几月 1-12
第五个* 一周当中的星期几 0-7  0/7都代表星期日
0 8，12，16 *** 代表每天8，12，16整点执行
0 5 ** 1-6 代表每周一到周六的凌晨五点执行
*/n代表没多久执行一次   */1 每一分钟执行一次
特定时间执行任务案例
45 22 *** 在22：45执行命令
```

crontab -r 终止任务调度

crontab -l列出当调度任务

service crond restart 重启任务调度

## 7.2at定时任务

一次性定时计划任务

每60s检查作业队列，有作业时检查作业运行时间，如果时间与当前吻合，则运行此作业

at是一次性定时计划任务，仅执行一次后不再执行此任务

使用at命令时保证atd进程的启动 使用ps -ef来查看atd是否运行

**命令格式：at 选项+时间 ctrl+d结束at命令输入**

指定时间方法 六种

1.当天hh:mm如果时间已经过去则在第二天执行

2.midnight，noon，teatime（4.00）比较模糊

3.12pm 12am

4.指定日期 04：00 2021-03-1

5.相对计时法 now+count time-units（时间单位） eg：now+5 minutes

6.today tomorrow

选项 

 -m 当指定任务完成后 给用户发送邮件

-I atq别名

-d atrm别名

-v 显示任务将被执行的时间

-c 打印任务内容到标准输出

-V 显示版本信息

-q 队列  使用指定的队列

-f 文件  从指定文件读入任务而不是从标准输入读入

-t 时间参数  以时间参数的形式提交要运行的任务

eg 两天后下午五点执行 /bin/ls /home

atq 查看系统中还没有执行的工作内容

atrm 删除已经设置的任务 atrm+编号

# 8.Linux磁盘分区与挂载

## 8.1新增磁盘与分区与挂载

Linux分ide硬盘/scsi硬盘 目前基本上是scsi盘

scsi硬盘 标识为sdx~ 

ide盘 驱动器标识符为hdx~ 

hd表示分区所在的设备的类型 

x代表盘号 a为基本盘 b为基本从属盘 c为辅助主盘 d为辅助从属盘 

~代表分区 1-4分别表示主分区或扩展分区  5开始就是逻辑分区 

sda代表硬盘

sda1代表第一块硬盘的第一个分区

lsblk/lsblk -f查看设备挂载情况

uuid是分区唯一的编码40位  mountpoint是挂载地方

例子：增加一块硬盘

1.虚拟机添加硬盘

2.分区

分区命令 fdisk /dev/sdb   ：dev是设备文件夹

m 显示命令列表

p 显示磁盘分区 同fdisk -l

n 新增分区

d 删除分区

w 写入并退出 如果想不保存退出输入q

3.格式化

分区不格式化并不能用，分区后确定uuid

mkfs -t ext4 /dev/sdb1

ext4是分区类型

4.挂载

注意：用命令行挂载后 再重启会解除挂载

umount 卸载

umount 设备名称或挂载目录

mount 挂载

mount 设备名称 挂载目录

5.设置可以自动挂载

永久挂载 

vim修改/etc/fstab

添加到挂载目录 即可实现挂载

## 8.2 磁盘情况查询

df -h查询系统整体磁盘使用情况

du -h /目录 查询指定目录的磁盘占用情况 不带的话默认查询当前目录

-a含文件

-s指定目录占用大小汇总

--max-depth=1 子目录深度

-c列出明细同时增加汇总值

## 8.3 常用指令

1.统计/opt文件夹下文件的个数

ls -l /opt |grep "^-" | wc -l   -开头的是文件

2.统计目录个数

ls -l /opt |grep "^d" | wc -l  d开头的是目录

3.统计文件的个数 包括子文件夹里的

ls -lR /opt |grep "^-" | wc -l  R代表递归

4.统计文件夹下目录的个数包括子文件夹

ls -lR /opt |grep "^d" | wc -l

5.以树状显示目录结构 

tree 目录名

默认情况是没有安装tree  使用yum install tree安装

# 9.网络配置

## 9.1查看网络ip

# 10进程管理

## 10.1简介

每个执行的程序都称为一个进程，每个进程分配一个id号 pid，进程号

程序执行起来装载在内存中就是一个进程

每个进程有前台和后台两种方式，前台就是占据着屏幕，后台屏幕无法显示但是在运行，系统服务都是后台常驻运行直到关机

pid是进程号 ppid是程序的父进程号

## 10.2显示系统执行的进程

ps 查看当前执行的进程

-a 显示终端所有进程

-u 以用户的格式显示进程信息

-x 显示后台进程运行的参数

ps -ef以全格式显示当前所有进程

-e是所有进程-f是全格式

终止进程

kill 选项 进程号 **通过进程号终止进程**

killall 进程名称 **通过进程名的方式终止**

终止一个进程后其所有子进程也会被终止

选项

-9 表示强迫进程立即停止

kill sshd对应的进程号停止用户登录

**/bin/systemctl start sshd.service恢复**

pstree 查看进程树 -p 显示进程pid -u显示进程用户

## 10.3服务管理

本质上是进程管理，运行在后台，通常会监听某个端口，等待其他进程的请求

service 管理指令

service 服务名 start|stop|restart|reload-重载|status-查看状态

centos7.0后大部分不再使用service 二十systemctl，被service管理的可以在 /etc/init.d中查看

查看服务名

1.setup 在界面选择系统服务 就可以看到全部 有*的是打开Linux自启动的

2./etc/init.d 看service管理的服务

服务的运行级别有七种 常用3和5

**使用init +数字设置**

0.停机状态

1.单用户工作状态 root权限，用于系统维护，禁止远程登陆

2.多用户状态，无网络

3.多用户有网络

4.系统没用，保留给用户

5.x11控制台，图形gui模式

6.正常关闭并重启

Linux开机流程

开机->BIOS->/boot->systemd进程1->运行级别->运行级别对应的服务



chkconfig指令

可以给服务的各个运行级别设置自 启动/关闭

在/etc/init.d查看该指令管理的服务

现在很多服务使用systemctl管理

基本语法

查看服务 chkconfig --list |grep xxx

chkconfig 服务名 --list

chkconfig --level 5 服务名 on/off 设置自启动或关闭

eg：chkconfig --level 5 network off/on  重启时才会生效



**systemctl管理指令**

基本语法 systemctl start|stop|restart|status 服务名

在/usr/lib/systemd/system中查看管理的指令

systemctl设置服务自启动状态

systemctl list-unit-files |grep 服务名 查看该服务开机启动状态 grep是过滤

systemctl enable 服务名 设置服务开机启动

systemctl disable 服务名 关闭服务开机启动

systemctl is-enabled 服务名 查询某个服务是否是自启动的

防火墙设置后不用重启 立即生效

stop/start设置的只是临时生效，重启后还会回归以前设置

要永久生效需要使用 enable/disable







打开或者关闭指定端口

打开端口 firewall-cmd   --permanent   --add-port=端口号/协议

关闭端口 firewall-cmd   --permanent   --remove-port=端口号/协议

重新载入才能生效， firewall-cmd --reload

查询端口是否开放 firewall-cmd --query-port=端口/协议





动态监控进程

top/ps都能用来显示正在执行的进程，区别在于top是动态监控的 能更新正在运行的进程

top -d 秒数  top -d 5 每隔五秒刷新当前进程 默认为3秒

-i使top不显示任何闲置或者僵死的进程

-p 通过指定监控进程id来仅仅监控某个进程的状态

PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ 

进程号 用户 

交互操作 P以cpu使用率排序 默认为此项 M为内存 N pid排序  q退出top

监控指定用户 在top中按 u 然后输入用户名tom

终止指定进程 top中按k 输入指定pid

监控网络状态 netstat

选项 -an 按一定顺序排列输出  -p显示哪个进程在调用

ping检测两个机器之间通信是否正常

# 11rpm包的管理

rpm用于下载包以及安装

简单查询指令 rpm -qa|grep xx

firefox-68.10.0-1.el7.centos.x86_64

名称 版本号 .后代表使用的操作系统 i686 i386表示32位 noarch表示通用

rpm -qa查询安装的所有rpm软件包

rpm -q 软件包名 查询软件包是否安装

-qi 软件包名 查询软件包信息

-ql 查询软件包中文件

-qf文件全路径名  查询文件所属软件包

比如 rpm -qf /etc/passwd   rpm -qf /root/install.log

rpm卸载  rpm -e rpm包名称 rpm -e firefox

删除软件可能会被提示有其他软件运行需要依赖此软件  使用--nodeps可以强制删除 但是一般不推荐

rpm --nodeps foo

安装rpm包 rpm -ivh rpm包全路径名称 安装需要下载好安装包

i install  v 提示 h 进度条





yum 更强大的安装工具

yum list|grep xx软件列表 查询yum服务器是否有需要安装的软件

yum install xx 下载安装

并且可以自动处理依赖关系

wget http://dev.mysql.com/get/mysql-5.7.26-1.el7.x86_64.rpm-bundle.tar

mysql密码 zzj123456

# 12.Shell脚本的执行方式

脚本需要以#!/bin/bash开头

需要有可执行权限

可以使用sh+脚本 执行 此类方法不需要x权限  比如 sh hello.sh

shell变量分为系统变量 用户自定义变量  使用set查看当前shell所有变量

shell变量的定义  不打空格

变量名=值

A=100

echo $A 输出变量的时候需要加$

撤销变量： unset 变量

声明静态变量：readonly变量 注意：不能unset

变量定义规则：可以由字母数字下划线组成，不能以数字开头

等号两侧不能有空格 变量名称一般习惯为大写

将命令的返回值赋给变量

A='date'  反引号就相当于是读取命令 无反引号就认为是给A赋date字符串

等价于A=$(date)



设置环境变量

基本语法 export 变量名=变量值  将shell变量输出为环境变量/可以理解为全局变量

source 配置文件 将修改后的配置信息立即生效

echo $变量名 查询环境变量的值

shell脚本的多行注释

：<< ! 内容!

前面的和最后一个感叹号需要单独在一行





位置参数变量

执行一个shell脚本时如果希望获取到命令行的参数信息，就可以使用到位置参数变量

基本语法:

$n  n为数字 $0代表命令本身，$1-$9代表第一到第九个参数 10以上的参数需要使用大括号包含 比如${10}

$* 代表命令行所有参数 把所有参数看成一个整体

$@代表命令行中所有参数 不过这个把每个参数区分对待

$#代表命令行中所有参数的个数





预定义变量  可以之间在shell脚本直接使用

基本语法  $$  当前进程的进程号（pid）

$! 后台运行的最后一个进程的进程号（pid）

$? 最后一次执行的命令的返回状态。如果这个变量的值为0，证明上一个命令正确执行；如果这个变量的值非0 证明上一个命令执行不正确

/root/shcode/myshell.sh  &以后台方式运行一个脚本

 

运算符

基本语法：

```shell
"$((运算式))"或 "$[运算式]" 或expr m + n

注意expr运算符间隔要有空格 如果希望将expr结果赋给某个变量 可以反引号反引

expr m - n

expr  \*，/，/% 乘除取余 注意乘法前面有一个反斜杠
```



条件判断

~~~shell
基本语法
if [ condition ] #注意condition前后要有空格
then 

fi #fi表示整个if结束了
#非空返回true 可以使用$?验证 0为true 非0为false
常用的判断语句
字符串比较 =
两个整数的比较
-lt 小于 -le 小于等于 -eq等于 -gt大于 -ge大于等于 -ne不等于
按照文件权限进行判断
-r 读 -w写 -x有执行的权限
按照文件类型判断
-f 常规文件 -e文件存在 -d文件存在并且是个目录

应用实例
ok是否等于ok
23是否大于等于22
/root/shcode/aaa.txt 目录中的文件是否存在
~~~



~~~shell
流程控制
多分支
if [  ]
then
	echo
elif [  ]
then
	echo
fi

case语句
case $变量名 in 
"值1")
;;
"值2")
;;
"*")
;;
esac


for循环
for 变量 in 值1 值2 值3.....
do  
程序/代码
done

for(( 初始值 ; 循环控制条件 ; 变量变换 ))
do
程序
done


while循环
while [ 条件判断 ]
do
程序
done

read读取控制台输入
read 选项 参数
-p 指定读取值时的提示符  类似于printf（）
-t 指定读取值时等待的时间（秒）如果没有在指定的时间内输入，就不再等待了
参数：指定读取值的变量名

函数
系统函数/自定义函数
basename常用于返回完整路径最后/的部分，常用于获取文件名
basename [pathname] [suffix] 删除掉所有的前缀包括最后一个“/”字符 然后将字符串显示出来
选项
suffix为后缀，如果suffix被指定了 basename会将pathname或string中的suffix去掉
basename /home/aaa/test.txt 
返回test.txt
dirname 返回路径/前面的部分
与basename刚好相反
返回/home/aaa


自定义函数
基本语法
function funname (){
	Action;
	return int;
}
调用的时候直接写函数名：funname 值

~~~



~~~shell
shell综合编程案例
需求：每天凌晨2：30备份数据库 hspedudb到 /data/backup/db
备份开始和备份结束给出相应的提示信息
备份后的文件要求以备份时间为文件名，并打包成.tar.gz文件格式 
eg：2021-03-12_230201.tar.gz
备份的同时检查是否有10天前备份的数据库文件，如果有就将其删除
~~~

# 13.日志管理

/var/log/ 系统日志文件的保存位置

常用日志

var/log/boot.log 系统启动日志

var/log/cron 记录与系统定时任务相关的日志

var/log/lasllog  记录系统中所有用户最后一次登录时间的日志，二进制文件使用lastlog命令查看

var/log/mailog 记录邮件信息日志

var/log/message 记录系统重要消息的日志，记录Linux系统绝大多数重要信息

var/log/secure 记录验证与授权方面的信息，涉及账户和密码的程序都会记录 比如系统的登录 ssh登录 su切换用户 sudo授权

var/log/ulmp  记录当前登录用户的信息，只记录当前登录的用户 使用w，who，users等命令查看
