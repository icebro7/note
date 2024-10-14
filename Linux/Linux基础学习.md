











# Linux基础学习

### 一.常用命令：

1.init 5 ：命令行界面跳转图形化界面            init 3 、 、 ctrl + alt + F2 ：图形化界面跳转命令行界面

2.ctrl + c ：停止运行          ctrl + d ：键盘输入结束，以及取代exit功能

3.shift + page up ：向上翻页               shift + page down ： 向下翻页

4.（任意字母） + Tab Tab  ：查找该字母为开头的命令



### 二.常用软件：

##### 				命令查询： man 以及help



​						（命令） + （--help）：查询语法     （man） + （命令） ：查询语法   （q）退出



##### 										命令中的快捷键：

![2021-05-28 (2)](C:\Users\hasee\OneDrive\图片\屏幕快照\2021-05-28 (2).png)



##### 									命令中【】中数字含义：

```
代号	代表内容
1	 使用者在shell环境中可以操作的指令或可可执行文件
2	 系统核心可调用的函数与工具等
3	 一些常用的函数（function）与函数库（library），大部分为C的函数库（libc）
4	 设备文件的说明，通常在/dev下的文件
5	 配置文件或者是某些文件的格式
6	 游戏（games）
7	 惯例与协定等，例如Linux文件系统、网络协定、ASCII code等等的说明
8	 系统管理员可用的管理指令
9	 跟kernel有关的文件
```



##### 									查询的提醒字：

```
代号	       内容说明
NAME	    简短的指令、数据名称说明
SYNOPSIS	简短的指令下达语法（syntax）简介
DESCRIPTION	较为完整的说明，这部分最好仔细看看！
OPTIONS	    针对 SYNOPSIS 部分中，有列举的所有可用的选项说明
COMMANDS	当这个程序（软件）在执行的时候，可以在此程序（软件）中下达的指令
FILES	    这个程序或数据所使用或参考或链接到的某些文件
SEE ALSO	可以参考的，跟这个指令或数据有相关的其他说明！
EXAMPLE   	一些可以参考的范例
```





##### 				1.文书编辑器： nano：

​						基础操作： nano +文件名 ：打开一个旧文件或新文件



​						快捷键：

```
[ctrl]-G：  取得线上说明（help），很有用的！
[ctrl]-X：  离开naon软件，若有修改过文件会提示是否需要储存喔！
[ctrl]-O：  储存盘案，若你有权限的话就能够储存盘案了；
[ctrl]-R：  从其他文件读入数据，可以将某个文件的内容贴在本文件中；
[ctrl]-W：  搜寻字串，这个也是很有帮助的指令喔！
[ctrl]-C：  说明目前光标所在处的行数与列数等信息；
[ctrl]-_：  可以直接输入行号，让光标快速移动到该行；
[alt]-Y：   校正语法功能打开或关闭（按一下开、再按一下关）
[alt]-M：   可以支持鼠标来移动光标的功能
```





##### 				2.开关机：

​								命令：

```
将数据同步写入硬盘中的指令： sync
惯用的关机指令： shutdown
重新开机，关机： reboot, halt, poweroff
```

​	

​							具体参数：

​	

​									shutdowm：

```
[root@study ~]# /sbin/shutdown [-krhc] [时间] [警告讯息]
选项与参数：
-k     ： 不要真的关机，只是发送警告讯息出去！
-r     ： 在将系统的服务停掉之后就重新开机（常用）
-h     ： 将系统的服务停掉后，立即关机。 （常用）
-c     ： 取消已经在进行的 shutdown 指令内容。
时间   ： 指定系统关机的时间！时间的范例下面会说明。若没有这个项目，则默认 1 分钟后自动进行。
范例：
[root@study ~]# /sbin/shutdown -h 10 'I will shutdown after 10 mins'
Broadcast message from root@study.centos.vbird （Tue 2015-06-02 10:51:34 CST）:
I will shutdown after 10 mins
The system is going down for power-off at Tue 2015-06-02 11:01:34 CST!
```

```
[root@study ~]# shutdown -h now
立刻关机，其中 now 相当于时间为 0 的状态
[root@study ~]# shutdown -h 20:25
系统在今天的 20:25 分会关机，若在21:25才下达此指令，则隔天才关机
[root@study ~]# shutdown -h +10
系统再过十分钟后自动关机
[root@study ~]# shutdown -r now
系统立刻重新开机
[root@study ~]# shutdown -r +30 'The system will reboot' 
再过三十分钟系统会重新开机，并显示后面的讯息给所有在线上的使用者
[root@study ~]# shutdown -k now 'This system will reboot' 
仅发出警告信件的参数！系统并不会关机啦！吓唬人！
```



​								systemctl：所以关机指令都是调用这个

```
[root@study ~]# systemctl [指令]
指令项目包括如下：
halt       进入系统停止的模式，屏幕可能会保留一些讯息，这与你的电源管理模式有关
poweroff   进入系统关机模式，直接关机没有提供电力喔！
reboot     直接重新开机
suspend    进入休眠模式
[root@study ~]# systemctl reboot    # 系统重新开机
[root@study ~]# systemctl poweroff  # 系统关机
```



##### 				3.文件权限概念



​						文档字段含义：

![image-20210608192006600](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\image-20210608192006600.png)



​						

​						文件属性：



![image-20210608192049668](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\image-20210608192049668.png)

```
第一个字符代表这个文件是“目录、文件或链接文件等等”：

当为[ d ]则是目录，例如上表文件名为“.config”的那一行；
当为[ - ]则是文件，例如上表文件名为“initial-setup-ks.cfg”那一行；
若是[ l ]则表示为链接文件（link file）；
若是[ b ]则表示为设备文件里面的可供储存的周边设备（可随机存取设备）；
若是[ c ]则表示为设备文件里面的序列埠设备，例如键盘、鼠标（一次性读取设备）。
接下来的字符中，以三个为一组，且均为“rwx” 的三个参数的组合。其中，[ r ]代表可读（read）、[ w ]代表可写（write）、[ x ]代表可执行（execute）。 要注意的是，这三个权限的位置不会改变，如果没有权限，就会出现减号[ - ]而已。

第一组为“文件拥有者可具备的权限”，以“initial-setup-ks.cfg”那个文件为例， 该文件的拥有者可以读写，但不可执行；
第二组为“加入此群组之帐号的权限”；
第三组为“非本人且没有加入本群组之其他帐号的权限”。
```





​						改变文件属性与权限：

```
chgrp ：改变文件所属群组
chown ：改变文件拥有者
chmod ：改变文件的权限, SUID, SGID, SBIT等等的特性

改变所属群组, chgrp
```



​						chmod：

​								数字类型：

​										r:4 > w:2 > x:1



​								符号类型：		

```
user （u）：具有可读、可写、可执行的权限；
group 与 others （g/o）：具有可读与执行的权限。所以就是：
[root@study ~]# chmod u=rwx,go=rx .bashrc
```

​						

​								案例：

- 使用者操作功能与权限
  刚刚讲这样如果你还是搞不懂～没关系，我们来处理个特殊的案例！假设两个文件名，分别是下面这样：
- /dir1/file1
- /dir2
  假设你现在在系统使用 dmtsai 这个帐号，那么这个帐号针对 /dir1, /dir1/file1, /dir2 这三个文件名来说，分别需要“哪些最小的权限”才能达成各项任务？ 鸟哥汇整如下，如果你看得懂，恭喜你，如果你看不懂～没关系～未来再来继续学！

| 操作动作              | /dir1 | /dir1/file1 | /dir2 | 重点                                      |
| :-------------------- | :---- | :---------- | :---- | :---------------------------------------- |
| 读取 file1 内容       | x     | r           | -     | 要能够进入 /dir1 才能读到里面的文件数据！ |
| 修改 file1 内容       | x     | rw          | -     | 能够进入 /dir1 且修改 file1 才行！        |
| 执行 file1 内容       | x     | rx          | -     | 能够进入 /dir1 且 file1 能运行才行！      |
| 删除 file1 文件       | wx    | -           | -     | 能够进入 /dir1 具有目录修改的权限即可！   |
| 将 file1 复制到 /dir2 | x     | r           | wx    | 要能够读 file1 且能够修改 /dir2 内的数据  |

​			



##### 					4.文件类型：

​							

​						正规文件：  属性第一个字符为（ - ）



- 纯文本文件（ASCII）：这是Linux系统中最多的一种文件类型啰， 称为纯文本文件是因为内容为我们人类可以直接读到的数据，例如数字、字母等等。 几乎只要我们可以用来做为设置的文件都属于这一种文件类型。 举例来说，你可以下达“ cat ~/.bashrc ”就可以看到该文件的内容。 （cat 是将一个文件内容读出来的指令）
- 二进制档（binary）：还记得我们在“ [第零章、计算机概论](https://wizardforcel.gitbooks.io/vbird-linux-basic-4e/Text/index.html) ”里面的[软件程序的运行](https://wizardforcel.gitbooks.io/vbird-linux-basic-4e/Text/index.html#program)中提过， 我们的系统其实仅认识且可以执行二进制文件（binary file）吧？没错～ 你的Linux当中的可可执行文件（scripts, 文字体批处理文件不算）就是这种格式的啦～ 举例来说，刚刚下达的指令cat就是一个binary file。
- 数据格式文件（data）： 有些程序在运行的过程当中会读取某些特定格式的文件，那些特定格式的文件可以被称为数据文件 （data file）。



​						目录：  属性第一个字符为（ d ）



​						链接文件：  属性第一个字符为  （ l ）



​						区块：  属性第一个字符为  （ b ）

​									一些储存数据， 以提供系统随机存取的周边设备



​						字符：   属性第一个字符为  （ c ）

​									一些序列埠的周边设备， 例如键盘、鼠标等等



​						数据接口文件：  属性第一个字符为  （ s ）

​									在网络上的数据承接



​						数据输送档：  属性第一个字符为  （ p ）





​			拓展名：

​					

```
*.sh ： 脚本或批处理文件 （scripts），因为批处理文件为使用shell写成的，所以扩展名就编成 .sh 啰；

Z, .tar, .tar.gz, .zip, *.tgz： 经过打包的压缩文件。这是因为压缩软件分别为 gunzip, tar 等等的，由于不同的压缩软件，而取其相关的扩展名啰！

.html, .php：网页相关文件，分别代表 HTML 语法与 PHP 语法的网页文件啰！ .html 的文件可使用网页浏览器来直接打开，至于 .php 的文件， 则可以通过 client 端的浏览器来 server 端浏览，以得到运算后的网页结果
```



##### 				5.目录意义内容：



| 目录                                   | 应放置文件内容                                               |
| :------------------------------------- | :----------------------------------------------------------- |
| 第一部份：FHS 要求**必须要存在的目录** |                                                              |
| /bin                                   | 系统有很多放置可执行文件的目录，但/bin比较特殊。因为/bin放置的是在单人维护模式下还能够被操作的指令。 在/bin下面的指令可以被root与一般帐号所使用，主要有：cat, chmod, chown, date, mv, mkdir, cp, bash等等常用的指令。 |
| /boot                                  | 这个目录主要在放置开机会使用到的文件，包括Linux核心文件以及开机菜单与开机所需配置文件等等。 Linux kernel常用的文件名为：vmlinuz，如果使用的是grub2这个开机管理程序， 则还会存在/boot/grub2/这个目录喔！ |
| /dev                                   | 在Linux系统上，任何设备与周边设备都是以文件的型态存在于这个目录当中的。 你只要通过存取这个目录下面的某个文件，就等于存取某个设备啰～ 比要重要的文件有/dev/null, /dev/zero, /dev/tty, /dev/loop*, /dev/sd*等等 |
| /etc                                   | 系统主要的配置文件几乎都放置在这个目录内，例如人员的帐号密码档、 各种服务的启始档等等。一般来说，这个目录下的各文件属性是可以让一般使用者查阅的， 但是只有root有权力修改。FHS建议不要放置可可执行文件（binary）在这个目录中喔。比较重要的文件有： /etc/modprobe.d/, /etc/passwd, /etc/fstab, /etc/issue 等等。另外 FHS 还规范几个重要的目录最好要存在 /etc/ 目录下喔：/etc/opt（必要）：这个目录在放置第三方协力软件 /opt 的相关配置文件 /etc/X11/（建议）：与 X Window 有关的各种配置文件都在这里，尤其是 xorg.conf 这个 X Server 的配置文件。 /etc/sgml/（建议）：与 SGML 格式有关的各项配置文件 /etc/xml/（建议）：与 XML 格式有关的各项配置文件 |
| /lib                                   | 系统的函数库非常的多，而/lib放置的则是在开机时会用到的函数库， 以及在/bin或/sbin下面的指令会调用的函数库而已。 什么是函数库呢？你可以将他想成是“外挂”，某些指令必须要有这些“外挂”才能够顺利完成程序的执行之意。 另外 FSH 还要求下面的目录必须要存在：/lib/modules/：这个目录主要放置可抽换式的核心相关模块（驱动程序）喔！ |
| /media                                 | media是“媒体”的英文，顾名思义，这个/media下面放置的就是可移除的设备啦！ 包括软盘、光盘、DVD等等设备都暂时挂载于此。常见的文件名有：/media/floppy, /media/cdrom等等。 |
| /mnt                                   | 如果你想要暂时挂载某些额外的设备，一般建议你可以放置到这个目录中。 在古早时候，这个目录的用途与/media相同啦！只是有了/media之后，这个目录就用来暂时挂载用了。 |
| /opt                                   | 这个是给第三方协力软件放置的目录。什么是第三方协力软件啊？ 举例来说，KDE这个桌面管理系统是一个独立的计划，不过他可以安装到Linux系统中，因此KDE的软件就建议放置到此目录下了。 另外，如果你想要自行安装额外的软件（非原本的distribution提供的），那么也能够将你的软件安装到这里来。 不过，以前的Linux系统中，我们还是习惯放置在/usr/local目录下呢！ |
| /run                                   | 早期的 FHS 规定系统开机后所产生的各项信息应该要放置到 /var/run 目录下，新版的 FHS 则规范到 /run 下面。 由于 /run 可以使用内存来仿真，因此性能上会好很多！ |
| /sbin                                  | Linux有非常多指令是用来设置系统环境的，这些指令只有root才能够利用来“设置”系统，其他使用者最多只能用来“查询”而已。 放在/sbin下面的为开机过程中所需要的，里面包括了开机、修复、还原系统所需要的指令。 至于某些服务器软件程序，一般则放置到/usr/sbin/当中。至于本机自行安装的软件所产生的系统可执行文件（system binary）， 则放置到/usr/local/sbin/当中了。常见的指令包括：fdisk, fsck, ifconfig, mkfs等等。 |
| /srv                                   | srv可以视为“service”的缩写，是一些网络服务启动之后，这些服务所需要取用的数据目录。 常见的服务例如WWW, FTP等等。举例来说，WWW服务器需要的网页数据就可以放置在/srv/www/里面。 不过，系统的服务数据如果尚未要提供给网际网络任何人浏览的话，默认还是建议放置到 /var/lib 下面即可。 |
| /tmp                                   | 这是让一般使用者或者是正在执行的程序暂时放置文件的地方。 这个目录是任何人都能够存取的，所以你需要定期的清理一下。当然，重要数据不可放置在此目录啊！ 因为FHS甚至建议在开机时，应该要将/tmp下的数据都删除唷！ |
| /usr                                   | 第二层 FHS 设置，后续介绍                                    |
| /var                                   | 第二曾 FHS 设置，主要为放置变动性的数据，后续介绍            |
| 第二部份：FHS **建议可以存在的目录**   |                                                              |
| /home                                  | 这是系统默认的使用者主文件夹（home directory）。在你新增一个一般使用者帐号时， 默认的使用者主文件夹都会规范到这里来。比较重要的是，主文件夹有两种代号喔：~：代表目前这个使用者的主文件夹 ~dmtsai ：则代表 dmtsai 的主文件夹！ |
| /lib<qual>                             | 用来存放与 /lib 不同的格式的二进制函数库，例如支持 64 位的 /lib64 函数库等 |
| /root                                  | 系统管理员（root）的主文件夹。之所以放在这里，是因为如果进入单人维护模式而仅挂载根目录时， 该目录就能够拥有root的主文件夹，所以我们会希望root的主文件夹与根目录放置在同一个分区中。 |

Linux当是非常重要的目录：

| 目录        | 应放置文件内容                                               |
| :---------- | :----------------------------------------------------------- |
| /lost+found | 这个目录是使用标准的ext2/ext3/ext4文件系统格式才会产生的一个目录，目的在于当文件系统发生错误时， 将一些遗失的片段放置到这个目录下。不过如果使用的是 xfs 文件系统的话，就不会存在这个目录了！ |
| /proc       | 这个目录本身是一个“虚拟文件系统（virtual filesystem）”喔！他放置的数据都是在内存当中， 例如系统核心、行程信息（process）、周边设备的状态及网络状态等等。因为这个目录下的数据都是在内存当中， 所以本身不占任何硬盘空间啊！比较重要的文件例如：/proc/cpuinfo, /proc/dma, /proc/interrupts, /proc/ioports, /proc/net/* 等等。 |
| /sys        | 这个目录其实跟/proc非常类似，也是一个虚拟的文件系统，主要也是记录核心与系统硬件信息较相关的信息。 包括目前已载入的核心模块与核心侦测到的硬件设备信息等等。这个目录同样不占硬盘容量喔！ |



​						**/usr目录**：



- /usr 的意义与内容：
  依据FHS的基本定义，/usr里面放置的数据属于可分享的与不可变动的（shareable, static）， 如果你知道如何通过网络进行分区的挂载（例如在服务器篇会谈到的[NFS服务器](http://linux.vbird.org/linux_server/0330nfs.php)），那么/usr确实可以分享给区域网络内的其他主机来使用喔！

| 目录                                   | 应放置文件内容                                               |
| :------------------------------------- | :----------------------------------------------------------- |
| 第一部份：FHS 要求**必须要存在的目录** |                                                              |
| /usr/bin/                              | 所有一般用户能够使用的指令都放在这里！目前新的 CentOS 7 已经将全部的使用者指令放置于此，而使用链接文件的方式将 /bin 链接至此！ 也就是说， /usr/bin 与 /bin 是一模一样了！另外，FHS 要求在此目录下不应该有子目录！ |
| /usr/lib/                              | 基本上，与 /lib 功能相同，所以 /lib 就是链接到此目录中的！   |
| /usr/local/                            | 系统管理员在本机自行安装自己下载的软件（非distribution默认提供者），建议安装到此目录， 这样会比较便于管理。举例来说，你的distribution提供的软件较旧，你想安装较新的软件但又不想移除旧版， 此时你可以将新版软件安装于/usr/local/目录下，可与原先的旧版软件有分别啦！ 你可以自行到/usr/local去看看，该目录下也是具有bin, etc, include, lib…的次目录喔！ |
| /usr/sbin/                             | 非系统正常运行所需要的系统指令。最常见的就是某些网络服务器软件的服务指令（daemon）啰！不过基本功能与 /sbin 也差不多， 因此目前 /sbin 就是链接到此目录中的。 |
| /usr/share/                            | 主要放置只读架构的数据文件，当然也包括共享文件。在这个目录下放置的数据几乎是不分硬件架构均可读取的数据， 因为几乎都是文字文件嘛！在此目录下常见的还有这些次目录：/usr/share/man：线上说明文档 /usr/share/doc：软件杂项的文件说明 /usr/share/zoneinfo：与时区有关的时区文件 |
| 第二部份：FHS **建议可以存在的目录**   |                                                              |
| /usr/games/                            | 与游戏比较相关的数据放置处                                   |
| /usr/include/                          | c/c++等程序语言的文件开始（header）与包含档（include）放置处，当我们以tarball方式 （*.tar.gz 的方式安装软件）安装某些数据时，会使用到里头的许多包含档喔！ |
| /usr/libexec/                          | 某些不被一般使用者惯用的可执行文件或脚本（script）等等，都会放置在此目录中。例如大部分的 X 窗口下面的操作指令， 很多都是放在此目录下的。 |
| /usr/lib<qual>/                        | 与 /lib<qual>/功能相同，因此目前 /lib<qual> 就是链接到此目录中 |
| /usr/src/                              | 一般源代码建议放置到这里，src有source的意思。至于核心源代码则建议放置到/usr/src/linux/目录下。 |

 	



##### 	/var目录：

- /var 的意义与内容：
  如果/usr是安装时会占用较大硬盘容量的目录，那么/var就是在系统运行后才会渐渐占用硬盘容量的目录。 因为/var目录主要针对常态性变动的文件，包括高速缓存（cache）、登录文件（log file）以及某些软件运行所产生的文件， 包括程序文件（lock file, run file），或者例如MySQL数据库的文件等等。常见的次目录有：



| 目录                                   | 应放置文件内容                                               |
| :------------------------------------- | :----------------------------------------------------------- |
| 第一部份：FHS **要求必须要存在的目录** |                                                              |
| /var/cache/                            | 应用程序本身运行过程中会产生的一些暂存盘；                   |
| /var/lib/                              | 程序本身执行的过程中，需要使用到的数据文件放置的目录。在此目录下各自的软件应该要有各自的目录。 举例来说，MySQL的数据库放置到/var/lib/mysql/而rpm的数据库则放到/var/lib/rpm去！ |
| /var/lock/                             | 某些设备或者是文件资源一次只能被一个应用程序所使用，如果同时有两个程序使用该设备时， 就可能产生一些错误的状况，因此就得要将该设备上锁（lock），以确保该设备只会给单一软件所使用。 举例来说，烧录机正在烧录一块光盘，你想一下，会不会有两个人同时在使用一个烧录机烧片？ 如果两个人同时烧录，那片子写入的是谁的数据？所以当第一个人在烧录时该烧录机就会被上锁， 第二个人就得要该设备被解除锁定（就是前一个人用完了）才能够继续使用啰。目前此目录也已经挪到 /run/lock 中！ |
| /var/log/                              | 重要到不行！这是登录文件放置的目录！里面比较重要的文件如/var/log/messages, /var/log/wtmp（记录登陆者的信息）等。 |
| /var/mail/                             | 放置个人电子邮件信箱的目录，不过这个目录也被放置到/var/spool/mail/目录中！ 通常这两个目录是互为链接文件啦！ |
| /var/run/                              | 某些程序或者是服务启动后，会将他们的PID放置在这个目录下喔！至于PID的意义我们会在后续章节提到的。 与 /run 相同，这个目录链接到 /run 去了！ |
| /var/spool/                            | 这个目录通常放置一些伫列数据，所谓的“伫列”就是排队等待其他程序使用的数据啦！ 这些数据被使用后通常都会被删除。举例来说，系统收到新信会放置到/var/spool/mail/中， 但使用者收下该信件后该封信原则上就会被删除。信件如果暂时寄不出去会被放到/var/spool/mqueue/中， 等到被送出后就被删除。如果是工作调度数据（crontab），就会被放置到/var/spool/cron/目录中！ |





##### 	6.目录的相关操作：



​									特殊目录：

```
.         代表此层目录
..        代表上一层目录
-         代表前一个工作目录
~         代表“目前使用者身份”所在的主文件夹
~account  代表 account 这个使用者的主文件夹（account是个帐号名称）
```



​									处理目录指令：



- cd：变换目录
- pwd：显示目前的目录
- mkdir：创建一个新的目录
- rmdir：删除一个空的目录
- cd （change directory, 变换目录）



​									复制：cp

​									cp -a：完整复制文件权限

​									cp -d：完整复制文件权限

​									cp -l：创建实体链接文件

​									cp -s：创建符号链接文件

​									cp -u：目标文件与来源文件有差异时，才会复制，常用于备份

​									cp -r：可以复制目录，但是权限有可能会被改变

​									cp -i：复制前询问，常用



​									移除：rm

​									rm -f：忽略不存在的文件，不会出现警告

​									rm -i：删除前询问

​									rm -r：递归删除，常用于目录的删除



​									移动：mv

​									mv -f：强制，以及存在也会覆盖

​									mv -i：如果目标文件已存在，会询问是否覆盖

​									mv -u：若目标文件已经存在，且 source 比较新，才会更新 （update）



##### 7.文件内容查阅：



- cat 由第一行开始显示文件内容
- tac 从最后一行开始显示，可以看出 tac 是 cat 的倒着写！
- nl 显示的时候，顺道输出行号！
- more 一页一页的显示文件内容
- less 与 more 类似，但是比 more 更好的是，他可以往前翻页！
- head 只看头几行
- tail 只看尾巴几行
- od 以二进制的方式读取文件内容！

​				



file：查看文件类型

![image-20210602155120250](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\image-20210602155120250.png)



​				cat：从上至下打印

​						tac：从下至上打印					

```
[root@study ~]# cat [-AbEnTv]
选项与参数：
-A  ：相当于 -vET 的整合选项，可列出一些特殊字符而不是空白而已；
-b  ：列出行号，仅针对非空白行做行号显示，空白行不标行号！
-E  ：将结尾的断行字符 $ 显示出来；
-n  ：打印出行号，连同空白行也会有行号，与 -b 的选项不同；
-T  ：将 [tab] 按键以 ^I 显示出来；
-v  ：列出一些看不出来的特殊字符
```



​				nl：添加行号打印：

```
[root@study ~]# nl [-bnw] 文件
选项与参数：
-b  ：  指定行号指定的方式，主要有两种：
-b a ： 表示不论是否为空行，也同样列出行号（类似 cat -n）；
-b t ： 如果有空行，空的那一行不要列出行号（默认值）；
-n  ：  列出行号表示的方法，主要有三种：
-n ln ：行号在屏幕的最左方显示；
-n rn ：行号在自己字段的最右方显示，且不加 0 ；
-n rz ：行号在自己字段的最右方显示，且加 0 ；
-w  ：  行号字段的占用的字符数。
```



​				more：可翻页检视



- 空白键 （space）：代表向下翻一页；
- Enter ：代表向下翻“一行”；
- /字串 ：代表在这个显示的内容当中，向下搜寻“字串”这个关键字；
- :f ：立刻显示出文件名以及目前显示的行数；
- q ：代表立刻离开 more ，不再显示该文件内容。
- b 或 [ctrl]-b ：代表往回翻页，不过这动作只对文件有用，对管线无用。







​			less：前后翻页搜寻



- 空白键 ：向下翻动一页；
- [pagedown]：向下翻动一页；
- [pageup] ：向上翻动一页；
- /字串 ：向下搜寻“字串”的功能；
- ?字串 ：向上搜寻“字串”的功能；
- n ：重复前一个搜寻 （与 / 或 ? 有关！）
- N ：反向的重复前一个搜寻 （与 / 或 ? 有关！）
- g ：前进到这个数据的第一行去；
- G ：前进到这个数据的最后一行去 （注意大小写）；
- q ：离开 less 这个程序；



​			head：头部撷取



```
[root@study ~]# head [-n number] 文件
选项与参数：
-n  ：后面接数字，代表显示几行的意思
```







​			tail：尾部撷取



```
[root@study ~]# tail [-n number] 文件
选项与参数：
-n  ：后面接数字，代表显示几行的意思
-f  ：表示持续侦测后面所接的文件名，要等到按下[ctrl]-c才会结束tail的侦测
```



​			od：查阅非纯文本文件



```
[root@study ~]# od [-t TYPE] 文件
选项或参数：
-t  ：后面可以接各种“类型 （TYPE）”的输出，例如：
      a       ：利用默认的字符来输出；
      c       ：使用 ASCII 字符来输出
      d[size] ：利用十进制（decimal）来输出数据，每个整数占用 size Bytes ；
      f[size] ：利用浮点数值（floating）来输出数据，每个数占用 size Bytes ；
      o[size] ：利用八进位（octal）来输出数据，每个整数占用 size Bytes ；
      x[size] ：利用十六进制（hexadecimal）来输出数据，每个整数占用 size Bytes ；
```





##### 8.修改文件时间或创建新文件:



​			touch:



- **modification time （mtime）**：当该文件的“内容数据”变更时，就会更新这个时间！内容数据指的是文件的内容，而不是文件的属性或权限喔！
- **status time （ctime）**：当该文件的“状态 （status）”改变时，就会更新这个时间，举例来说，像是权限与属性被更改了，都会更新这个时间啊。
- **access time （atime）**：当“该文件的内容被取用”时，就会更新这个读取时间 （access）。举例来说，我们使用 cat 去读取 /etc/man_db.conf ， 就会更新该文件的 atime 了。



```
[root@study ~]# touch [-acdmt] 文件
选项与参数：
-a  ：仅修订 access time；
-c  ：仅修改文件的时间，若该文件不存在则不创建新文件；
-d  ：后面可以接欲修订的日期而不用目前的日期，也可以使用 --date="日期或时间"
-m  ：仅修改 mtime ；
-t  ：后面可以接欲修订的时间而不用目前的时间，格式为[YYYYMMDDhhmm]
```



##### 9.文件默认权限：umask

```
[root@study ~]# umask
0022             &lt;==与一般权限有关的是后面三个数字！
[root@study ~]# umask -S
u=rwx,g=rx,o=rx
```



##### 10.文件隐藏属性：



​				chattr：设置文件隐藏属性

```
[root@study ~]# chattr [+-=][ASacdistu] 文件或目录名称
选项与参数：
```

- ：增加某一个特殊参数，其他原本存在参数则不动。

- ：移除某一个特殊参数，其他原本存在参数则不动。
  = ：设置一定，且仅有后面接的参数

A ：当设置了 A 这个属性时，若你有存取此文件（或目录）时，他的存取时间 atime 将不会被修改，
可避免 I/O 较慢的机器过度的存取磁盘。（目前建议使用文件系统挂载参数处理这个项目）
S ：一般文件是非同步写入磁盘的（原理请参考[前一章sync](https://www.bookstack.cn/read/Text/index.html#sync)的说明），如果加上 S 这个属性时，
当你进行任何文件的修改，该更动会“同步”写入磁盘中。
a ：当设置 a 之后，这个文件将只能增加数据，而不能删除也不能修改数据，只有root 才能设置这属性
c ：这个属性设置之后，将会自动的将此文件“压缩”，在读取的时候将会自动解压缩，
但是在储存的时候，将会先进行压缩后再储存（看来对于大文件似乎蛮有用的！）
d ：当 dump 程序被执行的时候，设置 d 属性将可使该文件（或目录）不会被 dump 备份
i ：这个 i 可就很厉害了！他可以让一个文件“不能被删除、改名、设置链接也无法写入或新增数据！”
对于系统安全性有相当大的助益！只有 root 能设置此属性
s ：当文件设置了 s 属性时，如果这个文件被删除，他将会被完全的移除出这个硬盘空间，
所以如果误删了，完全无法救回来了喔！
u ：与 s 相反的，当使用 u 来设置文件时，如果该文件被删除了，则数据内容其实还存在磁盘中，
可以使用来救援该文件喔！
注意1：属性设置常见的是 a 与 i 的设置值，而且很多设置值必须要身为 root 才能设置
注意2：xfs 文件系统仅支持 AadiS 而已



​				lsattr：显示wenjian隐藏属性

```
[root@study ~]# lsattr [-adR] 文件或目录
选项与参数：
-a ：将隐藏文件的属性也秀出来；
-d ：如果接的是目录，仅列出目录本身的属性而非目录内的文件名；
-R ：连同子目录的数据也一并列出来！
```



##### 11.文件特殊权限：

​				数字法设置权限：

- 4 为 SUID

- 2 为 SGID

- 1 为 SBIT

  

​				符号法增加权限：

​                             SUID 为 u+s ，而 SGID 为 g+s ，SBIT 则是 o+t 





​				SUID权限：仅对二进制文件有效



- Set UID
  当 s 这个标志出现在文件拥有者的 x 权限上时，例如刚刚提到的 /usr/bin/passwd 这个文件的权限状态：“-rw**s**r-xr-x”，此时就被称为 Set UID，简称为 SUID 的特殊权限。 那么SUID的权限对于一个文件的特殊功能是什么呢？基本上SUID有这样的限制与功能：
- SUID 权限仅对二进制程序（binary program）有效；
- 执行者对于该程序需要具有 x 的可执行权限；
- 本权限仅在执行该程序的过程中有效 （run-time）；
- 执行者将具有该程序拥有者 （owner） 的权限。



​				SGID权限：与SGID作用类似，但可针对文件或目录设置



​										对于文件：



- SGID 对二进制程序有用；
- 程序执行者对于该程序来说，需具备 x 的权限；
- 执行者在执行的过程中将会获得该程序群组的支持！



​										对于目录：



- 使用者若对于此目录具有 r 与 x 的权限时，该使用者能够进入此目录；
- 使用者在此目录下的有效群组（effective group）将会变成该目录的群组；
- 用途：若使用者在此目录下具有 w 的权限（可以新建文件），则使用者所创建的新文件，该新文件的群组与此目录的群组相同。



​				SBIT权限：只针对目录有效



- 当使用者对于此目录具有 w, x 权限，亦即具有写入的权限时；
- 当使用者在该目录下创建文件或目录时，仅有自己与 root 才有权力删除该文件



##### 12.指令与文件的搜寻

​				which：指令文件名的搜索

```
[root@study ~]# which [-a] command
选项或参数：
-a ：将所有由 PATH 目录中可以找到的指令均列出，而不止第一个被找到的指令名称
```



​				whereis：特定目录内文件文件名的搜索

```
[root@study ~]# whereis [-bmsu] 文件或目录名
选项与参数：
-l    :可以列出 whereis 会去查询的几个主要目录而已
-b    :只找 binary 格式的文件
-m    :只找在说明文档 manual 路径下的文件
-s    :只找 source 来源文件
-u    :搜寻不在上述三个项目当中的其他特殊文件
```



​				locate：数据库内文件名的搜索，可完成不完全名称的搜索

```
[root@study ~]# locate [-ir] keyword
选项与参数：
-i  ：忽略大小写的差异；
-c  ：不输出文件名，仅计算找到的文件数量
-l  ：仅输出几行的意思，例如输出五行则是 -l 5
-S  ：输出 locate 所使用的数据库文件的相关信息，包括该数据库纪录的文件/目录数量等
-r  ：后面可接正则表达式的显示方式
```



​				updatedb：手动更新数据库数据（数据库可能一日更新一次，但是locate查找的文件或许并没有更新）





​				find：直接搜索硬盘



​						与时间相关：

```
[root@study ~]# find [PATH] [option] [action]
选项与参数：
1\. 与时间有关的选项：共有 -atime, -ctime 与 -mtime ，以 -mtime 说明
   -mtime  n ：n 为数字，意义为在 n 天之前的“一天之内”被更动过内容的文件；
   -mtime +n ：列出在 n 天之前（不含 n 天本身）被更动过内容的文件文件名；
   -mtime -n ：列出在 n 天之内（含 n 天本身）被更动过内容的文件文件名。
   -newer file ：file 为一个存在的文件，列出比 file 还要新的文件文件名
```



​						与使用者或组群相关：

```
选项与参数：
2\. 与使用者或群组名称有关的参数：
   -uid n ：n 为数字，这个数字是使用者的帐号 ID，亦即 UID ，这个 UID 是记录在
            /etc/passwd 里面与帐号名称对应的数字。这方面我们会在第四篇介绍。
   -gid n ：n 为数字，这个数字是群组名称的 ID，亦即 GID，这个 GID 记录在
            /etc/group，相关的介绍我们会第四篇说明～
   -user name ：name 为使用者帐号名称喔！例如 dmtsai 
   -group name：name 为群组名称喔，例如 users ；
   -nouser    ：寻找文件的拥有者不存在 /etc/passwd 的人！
   -nogroup   ：寻找文件的拥有群组不存在于 /etc/group 的文件！
                当你自行安装软件时，很可能该软件的属性当中并没有文件拥有者，
                这是可能的！在这个时候，就可以使用 -nouser 与 -nogroup 搜寻。
```



​						与文件权限及名称相关：

```
选项与参数：
3\. 与文件权限及名称有关的参数：
   -name filename：搜寻文件名称为 filename 的文件；
   -size [+-]SIZE：搜寻比 SIZE 还要大（+）或小（-）的文件。这个 SIZE 的规格有：
                   c: 代表 Byte， k: 代表 1024Bytes。所以，要找比 50KB
                   还要大的文件，就是“ -size +50k ”
   -type TYPE    ：搜寻文件的类型为 TYPE 的，类型主要有：一般正规文件 （f）, 设备文件 （b, c）,
                   目录 （d）, 链接文件 （l）, socket （s）, 及 FIFO （p） 等属性。
   -perm mode  ：搜寻文件权限“刚好等于” mode 的文件，这个 mode 为类似 chmod
                 的属性值，举例来说， -rwsr-xr-x 的属性为 4755 ！
   -perm -mode ：搜寻文件权限“必须要全部囊括 mode 的权限”的文件，举例来说，
                 我们要搜寻 -rwxr--r-- ，亦即 0744 的文件，使用 -perm -0744，
                 当一个文件的权限为 -rwsr-xr-x ，亦即 4755 时，也会被列出来，
                 因为 -rwsr-xr-x 的属性已经囊括了 -rwxr--r-- 的属性了。
   -perm /mode ：搜寻文件权限“包含任一 mode 的权限”的文件，举例来说，我们搜寻
                 -rwxr-xr-x ，亦即 -perm /755 时，但一个文件属性为 -rw-------
                 也会被列出来，因为他有 -rw.... 的属性存在！
```



## 三.Linux 文件系统



##### 				磁盘的分区：

- 磁盘分区表主要有两种格式，一种是限制较多的 MBR 分区表，一种是较新且限制较少的 GPT 分区表。
- MBR 分区表中，第一个扇区最重要，里面有：（1）主要开机区（Master boot record, MBR）及分区表（partition table）， 其中 MBR 占有 446 Bytes，而 partition table 则占有 64 Bytes。
- GPT 分区表除了分区数量扩充较多之外，支持的磁盘容量也可以超过 2TB。
  至于磁盘的文件名部份，基本上，所有实体磁盘的文件名都已经被仿真成 /dev/sd[a-p] 的格式，第一颗磁盘文件名为 /dev/sda。 而分区的文件名若以第一颗磁盘为例，则为 /dev/sda[1-128] 。除了实体磁盘之外，虚拟机的磁盘通常为 /dev/vd[a-p] 的格式。 若有使用到软件磁盘阵列的话，那还有 /dev/md[0-128] 的磁盘文件名。使用的是 LVM 时，文件名则为 /dev/VGNAME/LVNAME 等格式。 关于软件磁盘阵列与 LVM 我们会在后面继续介绍，这里主要介绍的以实体磁盘及虚拟磁盘为主喔！
- /dev/sd[a-p][1-128]：为实体磁盘的磁盘文件名；
- /dev/vd[a-d][1-128]：为虚拟磁盘的磁盘文件名

​				

##### 				文件系统特性：



​		权限与属性放置到incode中，实际数据放置到date block区块中

​		超级区块（superblock）记录整个文件系统整体信息，inode 与 block 的总量、使用量、剩余量等。

- superblock：记录此 filesystem 的整体信息，包括inode/block的总量、使用量、剩余量， 以及文件系统的格式与相关信息等；
- inode：记录文件的属性，一个文件占用一个inode，同时记录此文件的数据所在的 block 号码；
- block：实际记录文件的内容，若文件太大时，会占用多个 block 。
  由于每个 inode 与 block 都有编号，而每个文件都会占用一个 inode ，inode 内则有文件数据放置的 block 号码。



##### 				Linux 的 EXT2 文件系统（inode）



​				block：实际记录文件的内容

- data block （数据区块）
  data block 是用来放置文件内容数据地方，在 Ext2 文件系统中所支持的 block 大小有 1K, 2K 及 4K 三种而已。在格式化时 block 的大小就固定了，且每个 block 都有编号，以方便 inode 的记录啦。 不过要注意的是，由于 block 大小的差异，会导致该文件系统能够支持的最大磁盘容量与最大单一文件大小并不相同。 因为 block 大小而产生的 Ext2 文件系统限制如下：[[2\]](https://www.bookstack.cn/read/vbird-linux-basic-4e/60.md#ps2)

| Block 大小         | 1KB  | 2KB   | 4KB  |
| :----------------- | :--- | :---- | :--- |
| 最大单一文件限制   | 16GB | 256GB | 2TB  |
| 最大文件系统总容量 | 2TB  | 8TB   | 16TB |



除此之外 Ext2 文件系统的 block 还有什么限制呢？有的，基本限制如下：

- 原则上，block 的大小与数量在格式化完就不能够再改变了（除非重新格式化）；

- 每个 block 内最多只能够放置一个文件的数据；

- 承上，如果文件大于 block 的大小，则一个文件会占用多个 block 数量；

- 承上，若文件小于 block ，则该 block 的剩余容量就不能够再被使用了（磁盘空间会浪费）。

  

  ​			inode：实际记录文件的内容

- inode table （inode 表格）
  再来讨论一下 inode 这个玩意儿吧！如前所述 inode 的内容在记录文件的属性以及该文件实际数据是放置在哪几号 block 内！ 基本上，inode 记录的文件数据至少有下面这些：[[4\]](https://www.bookstack.cn/read/vbird-linux-basic-4e/60.md#ps4)
- 该文件的存取模式（read/write/excute）；
- 该文件的拥有者与群组（owner/group）；
- 该文件的容量；
- 该文件创建或状态改变的时间（ctime）；
- 最近一次的读取时间（atime）；
- 最近修改的时间（mtime）；
- 定义文件特性的旗标（flag），如 SetUID…；
- 该文件真正内容的指向 （pointer）；
  inode 的数量与大小也是在格式化时就已经固定了，除此之外 inode 还有些什么特色呢？
- 每个 inode 大小均固定为 128 Bytes （新的 ext4 与 xfs 可设置到 256 Bytes）；
- 每个文件都仅会占用一个 inode 而已；
- 承上，因此文件系统能够创建的文件数量与 inode 的数量有关；
- 系统读取文件时需要先找到 inode，并分析 inode 所记录的权限与使用者是否符合，若符合才能够开始实际读取 block 的内容。





​				block能容纳的文件大小： 以1 block来举例

- 12 个直接指向： 12*1K=12K由于是直接指向，所以总共可记录 12 笔记录，因此总额大小为如上所示；
- 间接： 256*1K=256K每笔 block 号码的记录会花去 4Bytes，因此 1K 的大小能够记录 256 笔记录，因此一个间接可以记录的文件大小如上；
- 双间接： 256_256_1K=2562K第一层 block 会指定 256 个第二层，每个第二层可以指定 256 个号码，因此总额大小如上；
- 三间接： 256_256_256*1K=2563K第一层 block 会指定 256 个第二层，每个第二层可以指定 256 个第三层，每个第三层可以指定 256 个号码，因此总额大小如上；
- 总额：将直接、间接、双间接、三间接加总，得到 12 + 256 + 256_256 + 256_256*256 （K） = 16GB





​				Superblock（超级区块）：记录整个 filesystem 相关信息

- block 与 inode 的总量；
- 未使用与已使用的 inode / block 数量；
- block 与 inode 的大小 （block 为 1, 2, 4K，inode 为 128Bytes 或 256Bytes）；
- filesystem 的挂载时间、最近一次写入数据的时间、最近一次检验磁盘 （fsck） 的时间等文件系统的相关信息；
- 一个 valid bit 数值，若此文件系统已被挂载，则 valid bit 为 0 ，若未被挂载，则 valid bit 为 1 。



- Filesystem Description （文件系统描述说明）
  这个区段可以描述每个 block group 的开始与结束的 block 号码，以及说明每个区段 （superblock, bitmap, inodemap, data block） 分别介于哪一个 block 号码之间。这部份也能够用 [dumpe2fs](https://wizardforcel.gitbooks.io/vbird-linux-basic-4e/Text/index.html#dumpe2fs) 来观察的。
- block bitmap （区块对照表）
  如果你想要新增文件时总会用到 block 吧！那你要使用哪个 block 来记录呢？当然是选择“空的 block ”来记录新文件的数据啰。 那你怎么知道哪个 block 是空的？这就得要通过 block bitmap 的辅助了。从 block bitmap 当中可以知道哪些 block 是空的，因此我们的系统就能够很快速的找到可使用的空间来处置文件啰。



​				dumpe2fs：查询Ext家族superblock信息的指令（centos7无法使用，需要自己切出ext4的文件系统）

```
[root@study ~]# dumpe2fs [-bh] 设备文件名
选项与参数：
-b ：列出保留为坏轨的部分（一般用不到吧！？）
-h ：仅列出 superblock 的数据，不会列出其他的区段内容
```





​				XFS文件系统的配置



​						三个部分：数据区、文件系统活动登录区 、实时运行区



```
数据区 （data section）
基本上，数据区就跟我们之前谈到的 ext 家族一样，包括 inode/data block/superblock 等数据，都放置在这个区块。 这个数据区与 ext 家族的 block group 类似，也是分为多个储存区群组 （allocation groups） 来分别放置文件系统所需要的数据。 每个储存区群组都包含了 （1）整个文件系统的 superblock、 （2）剩余空间的管理机制、 （3）inode的分配与追踪。此外，inode与 block 都是系统需要用到时， 这才动态配置产生，所以格式化动作超级快
```



```
文件系统活动登录区 （log section）
在登录区这个区域主要被用来纪录文件系统的变化，其实有点像是日志区啦！文件的变化会在这里纪录下来，直到该变化完整的写入到数据区后， 该笔纪录才会被终结。如果文件系统因为某些缘故 （例如最常见的停电） 而损毁时，系统会拿这个登录区块来进行检验，看看系统挂掉之前， 文件系统正在运行些啥动作，借以快速的修复文件系统
```

​		

```
实时运行区 （realtime section）
当有文件要被创建时，xfs 会在这个区段里面找一个到数个的 extent 区块，将文件放置在这个区块内，等到分配完毕后，再写入到 data section 的 inode 与 block 去！ 这个 extent 区块的大小得要在格式化的时候就先指定，最小值是 4K 最大可到 1G。一般非磁盘阵列的磁盘默认为 64K 容量，而具有类似磁盘阵列的 stripe 情况下，则建议 extent 设置为与 stripe 一样大较佳。这个 extent 最好不要乱动，因为可能会影响到实体磁盘的性能喔。
```





​				XFS文件系统的描述数据观察

```
[root@study ~]# xfs_info 挂载点|设备文件名
```

```
范例一：找出系统 /boot 这个挂载点下面的文件系统的 superblock 纪录
[root@study ~]# df -T /boot
Filesystem     Type 1K-blocks   Used Available Use% Mounted on
/dev/vda2      xfs    1038336 133704    904632  13% /boot
# 没错！可以看得出来是 xfs 文件系统的！来观察一下内容吧！
[root@study ~]# xfs_info /dev/vda2
1  meta-data=/dev/vda2         isize=256    agcount=4, agsize=65536 blks
2           =                  sectsz=512   attr=2, projid32bit=1
3           =                  crc=0        finobt=0
4  data     =                  bsize=4096   blocks=262144, imaxpct=25
5           =                  sunit=0      swidth=0 blks
6  naming   =version 2         bsize=4096   ascii-ci=0 ftype=0
7  log      =internal          bsize=4096   blocks=2560, version=2
8           =                  sectsz=512   sunit=0 blks, lazy-count=1
9  realtime =none              extsz=4096   blocks=0, rtextents=0
```



- 第 1 行里面的 isize 指的是 inode 的容量，每个有 256Bytes 这么大。至于 agcount 则是前面谈到的储存区群组 （allocation group） 的个数，共有 4 个， agsize 则是指每个储存区群组具有 65536 个 block 。配合第 4 行的 block 设置为 4K，因此整个文件系统的容量应该就是 4_65536_4K 这么大
- 第 2 行里面 sectsz 指的是逻辑扇区 （sector） 的容量设置为 512Bytes 这么大的意思。
- 第 4 行里面的 bsize 指的是 block 的容量，每个 block 为 4K 的意思，共有 262144 个 block 在这个文件系统内。
- 第 5 行里面的 sunit 与 swidth 与磁盘阵列的 stripe 相关性较高。这部份我们下面格式化的时候会举一个例子来说明。
- 第 7 行里面的 internal 指的是这个登录区的位置在文件系统内，而不是外部设备的意思。且占用了 4K * 2560 个 block，总共约 10M 的容量。
- 第 9 行里面的 realtime 区域，里面的 extent 容量为 4K。不过目前没有使用。
  由于我们并没有使用磁盘阵列，因此上头这个设备里头的 sunit 与 extent 就没有额外的指定特别的值。根据 xfs（5） 的说明，这两个值会影响到你的文件系统性能， 所以格式化的时候要特别留意





##### 				磁盘目录的容量查询



​						df：列出文件系统的整体磁盘使用量

```
[root@study ~]# df [-ahikHTm] [目录或文件名]
选项与参数：
-a  ：列出所有的文件系统，包括系统特有的 /proc 等文件系统；
-k  ：以 KBytes 的容量显示各文件系统；
-m  ：以 MBytes 的容量显示各文件系统；
-h  ：以人们较易阅读的 GBytes, MBytes, KBytes 等格式自行显示；
-H  ：以 M=1000K 取代 M=1024K 的进位方式；
-T  ：连同该 partition 的 filesystem 名称 （例如 xfs） 也列出；
-i  ：不用磁盘容量，而以 inode 的数量来显示
```



范例：

```
范例一：将系统内所有的 filesystem 列出来！
[root@study ~]# df
Filesystem              1K-blocks    Used Available Use% Mounted on
/dev/mapper/centos-root  10475520 3409408   7066112  33% /
devtmpfs                   627700       0    627700   0% /dev
tmpfs                      637568      80    637488   1% /dev/shm
tmpfs                      637568   24684    612884   4% /run
tmpfs                      637568       0    637568   0% /sys/fs/cgroup
/dev/mapper/centos-home   5232640   67720   5164920   2% /home
/dev/vda2                 1038336  133704    904632  13% /boot
# 在 Linux 下面如果 df 没有加任何选项，那么默认会将系统内所有的 
# （不含特殊内存内的文件系统与 swap） 都以 1 KBytes 的容量来列出来！
# 至于那个 /dev/shm 是与内存有关的挂载，先不要理他！
```



- Filesystem：代表该文件系统是在哪个 partition ，所以列出设备名称；
- 1k-blocks：说明下面的数字单位是 1KB 呦！可利用 -h 或 -m 来改变容量；
- Used：顾名思义，就是使用掉的磁盘空间啦！
- Available：也就是剩下的磁盘空间大小；
- Use%：就是磁盘的使用率啦！如果使用率高达 90% 以上时， 最好需要注意一下了，免得容量不足造成系统问题喔！（例如最容易被灌爆的 /var/spool/mail 这个放置邮件的磁盘）
- Mounted on：就是磁盘挂载的目录所在啦！（挂载点）





​						du：评估文件系统的磁盘使用量（常用在推估目录所占容量）

```
[root@study ~]# du [-ahskm] 文件或目录名称
选项与参数：
-a  ：列出所有的文件与目录容量，因为默认仅统计目录下面的文件量而已。
-h  ：以人们较易读的容量格式 （G/M） 显示；
-s  ：列出总量而已，而不列出每个各别的目录占用容量；
-S  ：不包括子目录下的总计，与 -s 有点差别。
-k  ：以 KBytes 列出容量显示；
-m  ：以 MBytes 列出容量显示；
```





##### 				实体链接与符号链接： ln



​						Hard Link （实体链接, 硬式链接或实际链接）



限制：

- 每个文件都会占用一个 inode ，文件内容由 inode 的记录来指向；
- 想要读取该文件，必须要经过目录记录的文件名来指向到正确的 inode 号码才能读取。

- 不能跨 Filesystem；
- 不能 link 目录。





​						Symbolic Link （符号链接，亦即是捷径）（作用最广）

```
[root@study ~]# ln [-sf] 来源文件 目标文件
选项与参数：
-s  ：如果不加任何参数就进行链接，那就是hard link，至于 -s 就是symbolic link
-f  ：如果 目标文件 存在时，就主动的将目标文件直接移除后再创建！
```







#### 				磁盘的分区、格式化、检验与挂载

- 对磁盘进行分区，以创建可用的 partition ；
- 对该 partition 进行格式化 （format），以创建系统可用的 filesystem；
- 若想要仔细一点，则可对刚刚创建好的 filesystem 进行检验；
- 在 Linux 系统上，需要创建挂载点 （亦即是目录），并将他挂载上来；



##### 				观察磁盘分区状态



​						lsblk： 列出系统上的所有磁盘列表

```
[root@study ~]# lsblk [-dfimpt] [device]
选项与参数：
-d  ：仅列出磁盘本身，并不会列出该磁盘的分区数据
-f  ：同时列出该磁盘内的文件系统名称
-i  ：使用 ASCII 的线段输出，不要使用复杂的编码 （再某些环境下很有用）
-m  ：同时输出该设备在 /dev 下面的权限数据 （rwx 的数据）
-p  ：列出该设备的完整文件名！而不是仅列出最后的名字而已。
-t  ：列出该磁盘设备的详细数据，包括磁盘伫列机制、预读写的数据量大小等
```



范例：

```
范例一：列出本系统下的所有磁盘与磁盘内的分区信息
[root@study ~]# lsblk
NAME            MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sr0              11:0    1 1024M  0 rom
vda             252:0    0   40G  0 disk             # 一整颗磁盘
&#124;-vda1          252:1    0    2M  0 part
&#124;-vda2          252:2    0    1G  0 part /boot
`-vda3          252:3    0   30G  0 part
  &#124;-centos-root 253:0    0   10G  0 lvm  /           # 在 vda3 内的其他文件系统
  &#124;-centos-swap 253:1    0    1G  0 lvm  [SWAP]
  `-centos-home 253:2    0    5G  0 lvm  /home
```



- NAME：就是设备的文件名，会省略 /dev 等前导目录
- MAJ:MIN：其实核心认识的设备都是通过这两个代码来熟悉的，分别是主要：次要设备代码
- RM：是否为可卸载设备 （removable device），如光盘、USB 磁盘等等
- SIZE：当然就是容量
- RO：是否为只读设备的意思
- TYPE：是磁盘 （disk）、分区 （partition） 还是只读存储器 （rom） 等输出
- MOUTPOINT：就是前一章谈到的挂载点





​						blkid ：列出设备的 UUID 等参数，UUID：全域单一识别码

```
[root@study ~]# blkid
/dev/vda2: UUID="94ac5f77-cb8a-495e-a65b-2ef7442b837c" TYPE="xfs" 
/dev/vda3: UUID="WStYq1-P93d-oShM-JNe3-KeDl-bBf6-RSmfae" TYPE="LVM2_member"
/dev/sda1: UUID="35BC-6D6B" TYPE="vfat"
```



​						parted ：列出磁盘的分区表类型与分区信息

```
[root@study ~]# parted device_name print
```

范例：

```
[root@study ~]# parted /dev/vda print
Model: Virtio Block Device （virtblk）        # 磁盘的模块名称（厂商）
Disk /dev/vda: 42.9GB                       # 磁盘的总容量
Sector size （logical/physical）: 512B/512B   # 磁盘的每个逻辑/物理扇区容量
Partition Table: gpt                        # 分区表的格式 （MBR/GPT）
Disk Flags: pmbr_boot
Number  Start   End     Size    File system  Name  Flags      # 下面才是分区数据
 1      1049kB  3146kB  2097kB                     bios_grub
 2      3146kB  1077MB  1074MB  xfs
 3      1077MB  33.3GB  32.2GB                     lvm
```





##### 				磁盘分区： gdisk/fdisk



​						gdisk： GPT 分区表

```
[root@study ~]# gdisk 设备名称 
```

分区信息：

```
Number  Start （sector）    End （sector）  Size       Code  Name
```

- Number：分区编号，1 号指的是 /dev/vda1 这样计算。
- Start （sector）：每一个分区的开始扇区号码位置
- End （sector）：每一个分区的结束扇区号码位置，与 start 之间可以算出分区的总容量
- Size：就是分区的容量了
- Code：在分区内的可能的文件系统类型。Linux 为 8300，swap 为 8200。不过这个项目只是一个提示而已，不见得真的代表此分区内的文件系统
- Name：文件系统的名称等等。
  从上表我们可以发现几件事情：
- 整部磁盘还可以进行额外的分区，因为最大扇区为 83886080，但只使用到 65026047 号而已；
- 分区的设计中，新分区通常选用上一个分区的结束扇区号码数加 1 作为起始扇区号码



案例：

1GB 的 xfs 文件系统 （Linux）、1GB 的 vfat 文件系统 （Windows）

0.5GB 的 swap （Linux swap）（这个分区等一下会被删除）

```
[root@study ~]# gdisk /dev/vda
Command （? for help）: p
Number  Start （sector）    End （sector）  Size       Code  Name
   1            2048            6143   2.0 MiB     EF02
   2            6144         2103295   1024.0 MiB  0700
   3         2103296        65026047   30.0 GiB    8E00
# 找出最后一个 sector 的号码是很重要的！
Command （? for help）: ?  # 查一下增加分区的指令为何
Command （? for help）: n  # 就是这个，所有开始新增的行为
Partition number （4-128, default 4）: 4  # 默认就是 4 号，所以也能 enter 即可
First sector （34-83886046, default = 65026048） or {+-}size{KMGTP}: 65026048  # 也能 enter
Last sector （65026048-83886046, default = 83886046） or {+-}size{KMGTP}: +1G  # 决不要 enter
# 这个地方可有趣了，我们不需要自己去计算扇区号码，通过 +容量 的这个方式，
# 就可以让 gdisk 主动去帮你算出最接近你需要的容量的扇区号码
Current type is 'Linux filesystem'
Hex code or GUID （L to show codes, Enter = 8300）: # 使用默认值即可～直接 enter 下去
# 这里在让你选择未来这个分区预计使用的文件系统，默认都是 Linux 文件系统的 8300 
Command （? for help）: p
Number  Start （sector）    End （sector）  Size       Code  Name
   1            2048            6143   2.0 MiB     EF02
   2            6144         2103295   1024.0 MiB  0700
   3         2103296        65026047   30.0 GiB    8E00
 4        65026048        67123199   1024.0 MiB  8300  Linux filesystem
```

​																						（重点在“ Last sector ”那一行，那行绝对不要使用默认值）

```
Command （? for help）: p
Number  Start （sector）    End （sector）  Size       Code  Name
   1            2048            6143   2.0 MiB     EF02
   2            6144         2103295   1024.0 MiB  0700
   3         2103296        65026047   30.0 GiB    8E00
   4        65026048        67123199   1024.0 MiB  8300  Linux filesystem
   5        67123200        69220351   1024.0 MiB  0700  Microsoft basic data
   6        69220352        70244351   500.0 MiB   8200  Linux swap
```



​						partprobe ：更新 Linux 核心的分区表信息

```
[root@study ~]# partprobe [-s]  # 你可以不要加 -s ，那么屏幕不会出现讯息
[root@study ~]# partprobe -s    # 不过还是建议加上 -s 比较清晰
```





​						用 gdisk 删除一个分区

```
[root@study ~]# gdisk /dev/vda
Command （? for help）: p
Number  Start （sector）    End （sector）  Size       Code  Name
   1            2048            6143   2.0 MiB     EF02
   2            6144         2103295   1024.0 MiB  0700
   3         2103296        65026047   30.0 GiB    8E00
   4        65026048        67123199   1024.0 MiB  8300  Linux filesystem
   5        67123200        69220351   1024.0 MiB  0700  Microsoft basic data
   6        69220352        70244351   500.0 MiB   8200  Linux swap
Command （? for help）: d
Partition number （1-6）: 6
```





​						fdisk：MBR 分区表				（disk 跟 gdisk 使用的方式几乎一样）





##### 				磁盘格式化（创建文件系统）



​						XFS 文件系统 mkfs.xfs

```
[root@study ~]# mkfs.xfs [-b bsize] [-d parms] [-i parms] [-l parms] [-L label] [-f] \
                         [-r parms] 设备名称
选项与参数：
关於单位：下面只要谈到“数值”时，没有加单位则为 Bytes 值，可以用 k,m,g,t,p （小写）等来解释
          比较特殊的是 s 这个单位，它指的是 sector 的“个数”
-b  ：后面接的是 block 容量，可由 512 到 64k，不过最大容量限制为 Linux 的 4k 
-d  ：后面接的是重要的 data section 的相关参数值，主要的值有：
      agcount=数值  ：设置需要几个储存群组的意思（AG），通常与 CPU 有关
      agsize=数值   ：每个 AG 设置为多少容量的意思，通常 agcount/agsize 只选一个设置即可
      file          ：指的是“格式化的设备是个文件而不是个设备”的意思例如虚拟磁盘）
      size=数值     ：data section 的容量，亦即你可以不将全部的设备容量用完的意思
      su=数值       ：当有 RAID 时，那个 stripe 数值的意思，与下面的 sw 搭配使用
      sw=数值       ：当有 RAID 时，用于储存数据的磁盘数量（须扣除备份碟与备用碟）
      sunit=数值    ：与 su 相当，不过单位使用的是“几个 sector（512Bytes大小）”的意思
      swidth=数值   ：就是 su*sw 的数值，但是以“几个 sector（512Bytes大小）”来设置
-f  ：如果设备内已经有文件系统，则需要使用这个 -f 来强制格式化才行
-i  ：与 inode 有较相关的设置，主要的设置值有：
      size=数值     ：最小是 256Bytes 最大是 2k，一般保留 256 就足够使用了
      internal=[0&#124;1]：log 设备是否为内置？默认为 1 内置，如果要用外部设备，使用下面设置
      logdev=device ：log 设备为后面接的那个设备上头的意思，需设置 internal=0 才可
      size=数值     ：指定这块登录区的容量，通常最小得要有 512 个 block，大约 2M 以上才行！
-L  ：后面接这个文件系统的标头名称 Label name 的意思
-r  ：指定 realtime section 的相关设置值，常见的有：
      extsize=数值  ：就是那个重要的 extent 数值，一般不须设置，但有 RAID 时，
                      最好设置与 swidth 的数值相同较佳！最小为 4K 最大为 1G 。
```



​						EXT4 文件系统 mkfs.ext4		（格式化为 ext4 的传统 Linux 文件系统）

```
[root@study ~]# mkfs.ext4 [-b size] [-L label] 设备名称
选项与参数：
-b  ：设置 block 的大小，有 1K, 2K, 4K 的容量，
-L  ：后面接这个设备的标头名称。
```



​						其他文件系统 mkfs

```
[root@study ~]# mkfs[tab][tab]
mkfs         mkfs.btrfs   mkfs.cramfs  mkfs.ext2    mkfs.ext3    mkfs.ext4    
mkfs.fat     mkfs.minix   mkfs.msdos   mkfs.vfat    mkfs.xfs
```





##### 				文件系统检验		（当有 xfs 文件系统错乱）

```
[root@study ~]# xfs_repair [-fnd] 设备名称
选项与参数：
-f  ：后面的设备其实是个文件而不是实体设备
-n  ：单纯检查并不修改文件系统的任何数据 （检查而已）
-d  ：通常用在单人维护模式下面，针对根目录 （/） 进行检查与修复的动作，很危险，不要随便使用
```



​						fsck.ext4 处理 EXT4 文件系统（fsck 是个综合指令，只针对 ext4）

```
[root@study ~]# fsck.ext4 [-pf] [-b superblock] 设备名称
选项与参数：
-p  ：当文件系统在修复时，若有需要回复 y 的动作时，自动回复 y 来继续进行修复动作。
-f  ：强制检查！一般来说，如果 fsck 没有发现任何 unclean 的旗标，不会主动进入
      细部检查的，如果您想要强制 fsck 进入细部检查，就得加上 -f 旗标啰！
-D  ：针对文件系统下的目录进行最优化配置。
-b  ：后面接 superblock 的位置！一般来说这个选项用不到。但是如果你的 superblock 因故损毁时，
      通过这个参数即可利用文件系统内备份的 superblock 来尝试救援。一般来说，superblock 备份在：
      1K block 放在 8193, 2K block 放在 16384, 4K block 放在 32768
```





##### 				文件系统挂载与卸载



​						mount

```
[root@study ~]# mount -a
[root@study ~]# mount [-l]
[root@study ~]# mount [-t 文件系统] LABEL=''  挂载点
[root@study ~]# mount [-t 文件系统] UUID=''   挂载点  # 建议用这种方式
[root@study ~]# mount [-t 文件系统] 设备文件名  挂载点
选项与参数：
-a  ：依照配置文件 [/etc/fstab](../Text/index.html#fstab) 的数据将所有未挂载的磁盘都挂载上来
-l  ：单纯的输入 mount 会显示目前挂载的信息。加上 -l 可增列 Label 名称
-t  ：可以加上文件系统种类来指定欲挂载的类型。常见的 Linux 支持类型有：xfs, ext3, ext4,
      reiserfs, vfat, iso9660（光盘格式）, nfs, cifs, smbfs （后三种为网络文件系统类型）
-n  ：在默认的情况下，系统会将实际挂载的情况实时写入 /etc/mtab 中，以利其他程序的运行。
      但在某些情况下（例如单人维护模式）为了避免问题会刻意不写入。此时就得要使用 -n 选项。
-o  ：后面可以接一些挂载时额外加上的参数！比方说帐号、密码、读写权限等：
      async, sync:   此文件系统是否使用同步写入 （sync） 或非同步 （async） 的
                     内存机制，请参考[文件系统运行方式](../Text/index.html#harddisk-filerun)。默认为 async。
      atime,noatime: 是否修订文件的读取时间（atime）。为了性能，某些时刻可使用 noatime
      ro, rw:        挂载文件系统成为只读（ro） 或可读写（rw）
      auto, noauto:  允许此 filesystem 被以 mount -a 自动挂载（auto）
      dev, nodev:    是否允许此 filesystem 上，可创建设备文件？ dev 为可允许
      suid, nosuid:  是否允许此 filesystem 含有 suid/sgid 的文件格式
      exec, noexec:  是否允许此 filesystem 上拥有可执行 binary 文件
      user, nouser:  是否允许此 filesystem 让任何使用者执行 mount 一般来说，
                     mount 仅有 root 可以进行，但下达 user 参数，则可让
                     一般 user 也能够对此 partition 进行 mount 。
      defaults:      默认值为：rw, suid, dev, exec, auto, nouser, and async
      remount:       重新挂载，这在系统出错，或重新更新参数时，很有用！
```



​						挂载 CD 或 DVD 光盘

```
范例：将你用来安装 Linux 的 CentOS 原版光盘拿出来挂载到 /data/cdrom
[root@study ~]# blkid
.....（前面省略）.....
/dev/sr0: UUID="2015-04-01-00-21-36-00" LABEL="CentOS 7 x86_64" TYPE="iso9660" PTTYPE="dos"
[root@study ~]# mkdir /data/cdrom
[root@study ~]# mount /dev/sr0 /data/cdrom
mount: /dev/sr0 is write-protected, mounting read-only
[root@study ~]# df /data/cdrom
Filesystem     1K-blocks    Used Available Use% Mounted on
/dev/sr0         7413478 7413478         0 100% /data/cdrom
# 怎么会使用掉 100% 呢，是因为是 DVD 啊！所以无法再写入了
```



​						挂载 vfat 中文U盘 （USB磁盘）

```
范例：找出你的U盘设备的 UUID，并挂载到 /data/usb 目录中
[root@study ~]# blkid
/dev/sda1: UUID="35BC-6D6B" TYPE="vfat"
[root@study ~]# mkdir /data/usb
[root@study ~]#   mount -o codepage=950,iocharset=utf8 UUID="35BC-6D6B" /data/usb
[root@study ~]# # mount -o codepage=950,iocharset=big5 UUID="35BC-6D6B" /data/usb
[root@study ~]# df /data/usb
Filesystem     1K-blocks  Used Available Use% Mounted on
/dev/sda1        2092344     4   2092340   1% /data/usb
```





​						umount ：将设备文件卸载

```
[root@study ~]# umount [-fn] 设备文件名或挂载点
选项与参数：
-f  ：强制卸载！可用在类似网络文件系统 （NFS） 无法读取到的情况下；
-l  ：立刻卸载文件系统，比 -f 还强
-n  ：不更新 /etc/mtab 情况下卸载。
```





##### 				磁盘/文件系统参数修订



​						mknod：文件代表设备

```
[root@study ~]# mknod 设备文件名 [bcp] [Major] [Minor]
选项与参数：
设备种类：
   b  ：设置设备名称成为一个周边储存设备文件，例如磁盘等；
   c  ：设置设备名称成为一个周边输入设备文件，例如鼠标/键盘等；
   p  ：设置设备名称成为一个 FIFO 文件；
Major ：主要设备代码；
Minor ：次要设备代码；
```



​						xfs_admin ：修改 XFS 文件系统的 UUID 与 Label name

```
[root@study ~]# xfs_admin [-lu] [-L label] [-U uuid] 设备文件名
选项与参数：
-l  ：列出这个设备的 label name
-u  ：列出这个设备的 UUID
-L  ：设置这个设备的 Label name
-U  ：设置这个设备的 UUID 喔！
```



​						tune2fs :修改 ext4 的 label name 与 UUID

```
[root@study ~]# tune2fs [-l] [-L Label] [-U uuid] 设备文件名
选项与参数：
-l  ：类似 dumpe2fs -h 的功能～将 superblock 内的数据读出来～
-L  ：修改 LABEL name
-U  ：修改 UUID 啰！
```





##### 				开机挂载设置



​						挂载限制：

- 根目录 / 是必须挂载的﹐而且一定要先于其它 mount point 被挂载进来。
- 其它 mount point 必须为已创建的目录﹐可任意指定﹐但一定要遵守必须的系统目录架构原则 （FHS）
- 所有 mount point 在同一时间之内﹐只能挂载一次。
- 所有 partition 在同一时间之内﹐只能挂载一次。
- 如若进行卸载﹐您必须先将工作目录移到 mount point（及其子目录） 之外。



​						挂载信息：

```
[设备/UUID等]  [挂载点]  [文件系统]  [文件系统参数]  [dump]  [fsck]
```

- 第一栏：磁盘设备文件名/UUID/LABEL name：
  这个字段可以填写的数据主要有三个项目：
- 文件系统或磁盘的设备文件名，如 /dev/vda2 等
- 文件系统的 UUID 名称，如 UUID=xxx
- 文件系统的 LABEL 名称，例如 LABEL=xxx
  为了一致性，你还是可以将他改成 UUID 也没问题喔！（鸟哥还是比较建议使用 UUID 喔！） 要记得使用 blkid 或 xfs_admin 来查询 UUID 喔！
- 第二栏：挂载点 （mount point）：：
  就是挂载点，一定是目录
- 第三栏：磁盘分区的文件系统：
  在手动挂载时可以让系统自动测试挂载，但在这个文件当中我们必须要手动写入文件系统才行，包括 xfs, ext4, vfat, reiserfs, nfs 等等。
- 第四栏：文件系统参数：

| 参数                              | 内容意义                                                     |
| :-------------------------------- | :----------------------------------------------------------- |
| async/sync 非同步/同步            | 设置磁盘是否以非同步方式运行！默认为 async（性能较佳）       |
| auto/noauto 自动/非自动           | 当下达 mount -a 时，此文件系统是否会被主动测试挂载。默认为 auto。 |
| rw/ro 可读写/只读                 | 让该分区以可读写或者是只读的型态挂载上来，如果你想要分享的数据是不给使用者随意变更的， 这里也能够设置为只读。则不论在此文件系统的文件是否设置 w 权限，都无法写入喔！ |
| exec/noexec 可执行/不可执行       | 限制在此文件系统内是否可以进行“执行”的工作？如果是纯粹用来储存数据的目录， 那么可以设置为 noexec 会比较安全。不过，这个参数也不能随便使用，因为你不知道该目录下是否默认会有可执行文件。举例来说，如果你将 noexec 设置在 /var ，当某些软件将一些可执行文件放置于 /var 下时，那就会产生很大的问题喔！ 因此，建议这个 noexec 最多仅设置于你自订或分享的一般数据目录。 |
| user/nouser 允许/不允许使用者挂载 | 是否允许使用者使用 mount指令来挂载呢？一般而言，我们当然不希望一般身份的 user 能使用 mount 啰，因为太不安全了，因此这里应该要设置为 nouser 啰！ |
| suid/nosuid 具有/不具有 suid 权限 | 该文件系统是否允许 SUID 的存在？如果不是可执行文件放置目录，也可以设置为 nosuid 来取消这个功能！ |
| defaults                          | 同时具有 **rw, suid, dev, exec, auto, nouser, async** 等参数。 基本上，默认情况使用 defaults 设置即可！ |

- 第五栏：能否被 dump 备份指令作用：
  dump 是一个用来做为备份的指令，不过现在有太多的备份方案了，所以这个项目可以不要理会，直接输入 0 就好了
- 第六栏：是否以 fsck 检验扇区：
  早期开机的流程中，会有一段时间去检验本机的文件系统，看看文件系统是否完整 （clean）。





##### 挂载范例：		（将/dev/vda4每次开机都挂载到/date/xfs）



- 首先，先用nano将下面那一行写入/etc/fstab

```
[root@study ~]# nano /etc/fstab
UUID="e0fa7252-b374-4a06-987a-3cb14f415488"  /data/xfs  xfs  defaults  0 0
```

- 查看/dev/vda4是否已经挂载

```
[root@study ~]# df
Filesystem              1K-blocks    Used Available Use% Mounted on
/dev/vda4                 1038336   32864   1005472   4% /data/xfs
# 发现已经挂载，先卸载
# 因为，如果要被挂载的文件系统已经被挂载了（无论挂载在哪个目录），那测试就不会进行
[root@study ~]# umount /dev/vda4
```

- 测试写入/etc/fstab语法有没有错误

  ```
  [root@study ~]# mount -a
  [root@study ~]# df /data/xfs
  ```







































## 四.文件与文件系统的压缩，打包，备份

​		

##### 				常见拓展名：

```
*.Z         compress 程序压缩的文件；
*.zip       zip 程序压缩的文件；
*.gz        gzip 程序压缩的文件；
*.bz2       bzip2 程序压缩的文件；
*.xz        xz 程序压缩的文件；
*.tar       tar 程序打包的数据，并没有压缩过；
*.tar.gz    tar 程序打包的文件，其中并且经过 gzip 的压缩
*.tar.bz2   tar 程序打包的文件，其中并且经过 bzip2 的压缩
*.tar.xz    tar 程序打包的文件，其中并且经过 xz 的压缩
```



##### 				常见压缩指令：



​						gzip：应用度最广    （时间最快，压缩比较低）

​						zcat：查看解压缩之后的原始文件

```
[dmtsai@study ~]$ gzip [-cdtv#] 文件名
[dmtsai@study ~]$ zcat 文件名.gz
选项与参数：
-c  ：将压缩的数据输出到屏幕上，可通过数据流重导向来处理；
-d  ：解压缩的参数；
-t  ：可以用来检验一个压缩文件的一致性～看看文件有无错误；
-v  ：可以显示出原文件/压缩文件的压缩比等信息；
-#  ：# 为数字的意思，代表压缩等级，-1 最快，但是压缩比最差、-9 最慢，但是压缩比最好！默认是 -6
```



​						bzip：取代gzip并提供更好的压缩比

​						bzcat：查看解压缩之后的原始文件

```
[dmtsai@study ~]$ bzip2 [-cdkzv#] 文件名
[dmtsai@study ~]$ bzcat 文件名.bz2
选项与参数：
-c  ：将压缩的过程产生的数据输出到屏幕上！
-d  ：解压缩的参数
-k  ：保留原始文件，而不会删除原始的文件喔！
-z  ：压缩的参数 （默认值，可以不加）
-v  ：可以显示出原文件/压缩文件的压缩比等信息；
-#  ：与 gzip 同样的，都是在计算压缩比的参数， -9 最佳， -1 最快！
```



​						xz：压缩比最高的软件				（压缩比最高，时间较久）

​						xzcat：查看解压缩之后的原始文件

```
[dmtsai@study ~]$ xz [-dtlkc#] 文件名
[dmtsai@study ~]$ xcat 文件名.xz
选项与参数：
-d  ：就是解压缩啊！
-t  ：测试压缩文件的完整性，看有没有错误
-l  ：列出压缩文件的相关信息
-k  ：保留原本的文件不删除～
-c  ：同样的，就是将数据由屏幕上输出的意思！
-#  ：同样的，也有较佳的压缩比的意思！
```





##### 				打包指令：tar

​					

​						选项与参数：

```
[dmtsai@study ~]$ tar [-z&#124;-j&#124;-J] [cv] [-f 待创建的新文件名] filename... &lt;==打包与压缩
[dmtsai@study ~]$ tar [-z&#124;-j&#124;-J] [tv] [-f 既有的 tar文件名]             &lt;==察看文件名
[dmtsai@study ~]$ tar [-z&#124;-j&#124;-J] [xv] [-f 既有的 tar文件名] [-C 目录]   &lt;==解压缩
选项与参数：
-c  ：创建打包文件，可搭配 -v 来察看过程中被打包的文件名（filename）
-t  ：察看打包文件的内容含有哪些文件名，重点在察看“文件名”就是了；
-x  ：解打包或解压缩的功能，可以搭配 -C （大写） 在特定目录解开
      特别留意的是， -c, -t, -x 不可同时出现在一串命令行中。
-z  ：通过 gzip  的支持进行压缩/解压缩：此时文件名最好为 *.tar.gz
-j  ：通过 bzip2 的支持进行压缩/解压缩：此时文件名最好为 *.tar.bz2
-J  ：通过 xz    的支持进行压缩/解压缩：此时文件名最好为 *.tar.xz
      特别留意， -z, -j, -J 不可以同时出现在一串命令行中
-v  ：在压缩/解压缩的过程中，将正在处理的文件名显示出来！
-f filename：-f 后面要立刻接要被处理的文件名！建议 -f 单独写一个选项啰！（比较不会忘记）
-C 目录    ：这个选项用在解压缩，若要在特定目录解压缩，可以使用这个选项。
其他后续练习会使用到的选项介绍：
-p（小写） ：保留备份数据的原本权限与属性，常用于备份（-c）重要的配置文件
-P（大写） ：保留绝对路径，亦即允许备份数据中含有根目录存在之意；
--exclude=FILE：在压缩的过程中，不要将 FILE 打包！
```



&#124;（&#124	管线命令:pipe） 配合 grep 可以撷取关键字

​						最简单使用的几种方式：

- 压　缩：tar -jcv -f filename.tar.bz2 要被压缩的文件或目录名称
- 查　询：tar -jtv -f filename.tar.bz2
- 解压缩：tar -jxv -f filename.tar.bz2 -C 欲解压缩的目录





​						打包某目录，但不含该目录下的某些文件：

​								在打包的结尾加上：  --exclud=“file” （不含该目录下的文件）

​							



​						仅备份比某个时刻还要新的文件：

​								在打包的结尾加上： --newer =“xxxx/xx/xx”/文件





​						tarfile：仅仅是打包了的文件

​						tarball：有进行压缩





​						特殊应用：利用管线命令与数据流    			（将待处理的文件一边打包一边解压缩到目标目录去）

![image-20210603134615236](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\image-20210603134615236.png)







##### 				XFS 文件系统的备份与还原



​						xfsdump:可进行文件系统完整备份、累积备份



1. ```
   1. `[root@study ~]# xfsdump [-L S_label] [-M M_label] [-l #] [-f 备份文件] 待备份数据`
   2. `[root@study ~]# xfsdump -I`
   3. `选项与参数：`
   4. `-L  ：xfsdump 会纪录每次备份的 session 标头，这里可以填写针对此文件系统的简易说明`
   5. `-M  ：xfsdump 可以纪录储存媒体的标头，这里可以填写此媒体的简易说明`
   6. `-l  ：是 L 的小写，就是指定等级～有 0~9 共 10 个等级喔！ （默认为 0，即完整备份）`
   7. `-f  ：有点类似 tar 啦！后面接产生的文件，亦可接例如 /dev/st0 设备文件名或其他一般文件文件名等`
   8. `-I  ：从 /var/lib/xfsdump/inventory 列出目前备份的信息状态`
   ```

   



​						xfsdump的限制：

- xfsdump 不支持没有挂载的文件系统备份！所以只能备份已挂载的！
- xfsdump 必须使用 root 的权限才能操作 （涉及文件系统的关系）
- xfsdump 只能备份 XFS 文件系统啊！
- xfsdump 备份下来的数据 （文件或储存媒体） 只能让 xfsrestore 解析
- xfsdump 是通过文件系统的 UUID 来分辨各个备份文件的，因此不能备份两个具有相同 UUID 的文件系统喔！
  xfsdump 的选项虽然非常的繁复，不过如果只是想要简单的操作时，您只要记得下面的几个选项就很够用了！

- 不能用xfsdump去备份/etc，因为不是一个独立的文件系统



​						

​						备份完整的文件系统：



​								首先确定该文件系统是否独立：				（操作的为/boot文件）				

```
[root@study ~]# df -h /boot
Filesystem      Size  Used Avail Use% Mounted on
/dev/vda2      1014M  131M  884M  13% /boot      # 挂载 /boot 的是 /dev/vda 设备！
# 看！确实是独立的文件系统喔！ /boot 是挂载点！
```



​								将文件完整的备份为：								（此处备份为 /srv/boot.dump）

```
[root@study ~]# xfsdump -l 0 -L boot_all -M boot_all -f /srv/boot.dump /boot
----------------------------（跳过）
xfsdump: Dump Status: SUCCESS
```



​								查看是否备份成功：										（可省略部分）

```
[root@study ~]# ll /srv/boot.dump
-rw-r--r--. 1 root root 102872168 Jul  1 18:43 /srv/boot.dump
```



​								查看inventory内是否有文件产生：				（同上）

```
[root@study ~]# ll /var/lib/xfsdump/inventory
-rw-r--r--. 1 root root 5080 Jul  1 18:43 506425d2-396a-433d-9968-9b200db0c17c.StObj
-rw-r--r--. 1 root root  312 Jul  1 18:43 94ac5f77-cb8a-495e-a65b-2ef7442b837c.InvIndex
-rw-r--r--. 1 root root  576 Jul  1 18:43 fstab
```



​						累积备份的实现：

​								

​								查看有没有被xfsdump过文件的数据：					

```
xfsdump -I
file system 0:
    fs id:          94ac5f77-cb8a-495e-a65b-2ef7442b837c
    session 0:
    -----------------------------------------------（省略）
```



​								可试着创建一个文件用于实验：

```
[root@study ~]# dd if=/dev/zero of=/boot/testing.img bs=1M count=10
10+0 records in
10+0 records out
10485760 Bytes （10 MB） copied, 0.166128 seconds, 63.1 MB/s
```



​								开始创建差异备份文件：

```
[root@study ~]# xfsdump -l 1 -L boot_2 -M boot_2 -f /srv/boot.dump1 /boot
....（中间省略）....
```



​								查看boot文件内是否产生新文件：

```
[root@study ~]# ll /srv/boot*
-rw-r--r--. 1 root root 102872168 Jul  1 18:43 /srv/boot.dump
-rw-r--r--. 1 root root  10510952 Jul  1 18:46 /srv/boot.dump1
```



​								最后查看一下是否有记录备份的时间点：

```
[root@study ~]# xfsdump -I
file system 0:
    fs id:          94ac5f77-cb8a-495e-a65b-2ef7442b837c
    session 0:
        mount point:    study.centos.vbird:/boot
        device:         study.centos.vbird:/dev/vda2
....（中间省略）....
    session 1:
....（中间省略）....
```





​					xfsrestore：文件系统还原

```
[root@study ~]# xfsrestore -I &lt;==用来察看备份文件数据
[root@study ~]# xfsrestore [-f 备份文件] [-L S_label] [-s] 待复原目录 &lt;==单一文件全系统复原
[root@study ~]# xfsrestore [-f 备份文件] -r 待复原目录 &lt;==通过累积备份文件来复原系统
[root@study ~]# xfsrestore [-f 备份文件] -i 待复原目录 &lt;==进入互动模式
选项与参数：
-I  ：跟 xfsdump 相同的输出！可查询备份数据，包括 Label 名称与备份时间等
-f  ：后面接的就是备份文件！企业界很有可能会接 /dev/st0 等磁带机！我们这里接文件名！
-L  ：就是 Session 的 Label name 喔！可用 -I 查询到的数据，在这个选项后输入！
-s  ：需要接某特定目录，亦即仅复原某一个文件或目录之意！
-r  ：如果是用文件来储存备份数据，那这个就不需要使用。如果是一个磁带内有多个文件，
      需要这东西来达成累积复原
-i  ：进入互动模式，进阶管理员使用的！一般我们不太需要操作它！
```



​						首先观察xfsdump备份过的数据：（xfsretore -I 与xfsdump - I作用一致，都是在invenory捞数据）

```
[root@study ~]# xfsrestore -I 
file system 0:
    fs id:          94ac5f77-cb8a-495e-a65b-2ef7442b837c
    session 0:
```



​						查看哪个文件是挂载点，并且将数据覆盖回去：					（以boot_all为例）

```
[root@study ~]# xfsrestore -f /srv/boot.dump -L boot_all /boot
----------------------------（中间省略）------------------
xfsrestore: Restore Status: SUCCESS
```

​									tip：同名的文件会被覆盖，其他系统内新的文件会被保留

​						

​						只想要复原某一个目录或文件：			（直接加上“ -s 目录 ”这个选项与参数即可）

```
#仅复原备份文件内的 grub2 到 /tmp/boot2/ 里头去
[root@study ~]# mkdir /tmp/boot2
[root@study ~]# xfsrestore -f /srv/boot.dump -L boot_all -s grub2 /tmp/boot2
```



​						复原累积备份数据：								（一步步往下复原即可）

```
[root@study ~]# xfsrestore -f /srv/boot.dump1 /tmp/boot
```



​						仅还原部分文件的 xfsrestore 互动模式：	（不知道备份文件里的详细文件或要复原的数量太多)

```
先进入备份文件内，准备找出需要备份的文件名数据，同时预计还原到 /tmp/boot3 当中！
[root@study ~]# mkdir /tmp/boot3
[root@study ~]# xfsrestore -f /srv/boot.dump -i /tmp/boot3
 ========================== subtree selection dialog ==========================
the following commands are available:
        pwd
        ls [ &lt;path&gt; ]
        cd [ &lt;path&gt; ]
        add [ &lt;path&gt; ]       # 可以加入复原文件列表中
        delete [ &lt;path&gt; ]    # 从复原列表拿掉文件名！并非删除喔！
        extract              # 开始复原动作！
        quit
        help
 -&gt; ls
          455517 initramfs-3.10.0-229.el7.x86_64kdump.img
             138 initramfs-3.10.0-229.el7.x86_64.img
             141 initrd-plymouth.img
             140 vmlinuz-0-rescue-309eb890d09f440681f596543d95ec7a
             139 initramfs-0-rescue-309eb890d09f440681f596543d95ec7a.img
             137 vmlinuz-3.10.0-229.el7.x86_64
             136 symvers-3.10.0-229.el7.x86_64.gz
             135 config-3.10.0-229.el7.x86_64
             134 System.map-3.10.0-229.el7.x86_64
             133 .vmlinuz-3.10.0-229.el7.x86_64.hmac
         1048704 grub2/
             131 grub/
 -&gt; add grub
 -&gt; add grub2
 -&gt; add config-3.10.0-229.el7.x86_64
 -&gt; extract
[root@study ~]# ls -l /tmp/boot3
-rw-r--r--. 1 root root 123838 Mar  6 19:45 config-3.10.0-229.el7.x86_64
drwxr-xr-x. 2 root root     26 May  4 17:52 grub
drwxr-xr-x. 6 root root    104 Jun 25 00:02 grub2
# 就只会有 3 个文件名被复原，当然，如果文件名是目录，那下面的子文件当然也会被还原回来的！
```





##### 				其他常见的压缩与备份工具



​						dd：制作备份文件

```
[root@study ~]# dd if="input_file" of="output_file" bs="block_size" count="number"
选项与参数：
if   ：就是 input file 啰～也可以是设备喔！
of   ：就是 output file 喔～也可以是设备；
bs   ：规划的一个 block 的大小，若未指定则默认是 512 Bytes（一个 sector 的大小）
count：多少个 bs 的意思。

范例一：将 /etc/passwd 备份到 /tmp/passwd.back 当中
[root@study ~]# dd if=/etc/passwd of=/tmp/passwd.back
4+1 records in
4+1 records out
2092 Bytes （2.1 kB） copied, 0.000111657 s, 18.7 MB/s
[root@study ~]# ll /etc/passwd /tmp/passwd.back
-rw-r--r--. 1 root root 2092 Jun 17 00:20 /etc/passwd
-rw-r--r--. 1 root root 2092 Jul  2 23:27 /tmp/passwd.back
# 仔细的看一下，我的 /etc/passwd 文件大小为 2092 Bytes，因为我没有设置 bs ，
# 所以默认是 512 Bytes 为一个单位，因此，上面那个 4+1 表示有 4 个完整的 512 Bytes，
# 以及未满 512 Bytes 的另一个 block 的意思啦！事实上，感觉好像是 cp 这个指令啦～
范例二：将刚刚烧录的光驱的内容，再次的备份下来成为图像挡
[root@study ~]# dd if=/dev/sr0 of=/tmp/system.iso
177612+0 records in
177612+0 records out
90937344 Bytes （91 MB） copied, 22.111 s, 4.1 MB/s
# 要将数据抓下来用这个方法，如果是要将镜像文件写入 USB 磁盘，就会变如下一个范例啰！
范例三：假设你的 USB 是 /dev/sda 好了，请将刚刚范例二的 image 烧录到 USB 磁盘中
[root@study ~]# lsblk /dev/sda
NAME MAJ:MIN RM SIZE RO TYPE MOUNTPOINT
sda    8:0    0   2G  0 disk             # 确实是 disk 而且有 2GB 喔！
[root@study ~]# dd if=/tmp/system.iso of=/dev/sda
[root@study ~]# mount /dev/sda /mnt
[root@study ~]# ll /mnt
dr-xr-xr-x. 131 root root 34816 Jun 26 22:14 etc
dr-xr-xr-x.   5 root root  2048 Jun 17 00:20 home
dr-xr-xr-x.   8 root root  4096 Jul  2 18:48 root
# 如果你不想要使用 DVD 来作为开机媒体，那可以将镜像文件使用这个 dd 写入 USB 磁盘，
# 该磁盘就会变成跟可开机光盘一样的功能！可以让你用 USB 来安装 Linux 喔！速度快很多！
范例四：将你的 /boot 整个文件系统通过 dd 备份下来
[root@study ~]# df -h /boot
Filesystem      Size  Used Avail Use% Mounted on
/dev/vda2      1014M  149M  866M  15% /boot       # 请注意！备份的容量会到 1G 喔！
[root@study ~]# dd if=/dev/vda2 of=/tmp/vda2.img
[root@study ~]# ll -h /tmp/vda2.img
-rw-r--r--. 1 root root 1.0G Jul  2 23:39 /tmp/vda2.img
# 等于是将整个 /dev/vda2 通通捉下来的意思～所以，文件大小会跟整颗磁盘的最大量一样大！
```



​							Tip:   dd 可以将原本旧的 partition 上面，将 sector 表面的数据整个复制过来！ 当然连同 superblock, boot sector, meta data 等等通通也会复制过来！是否很有趣呢？未来你想要创建两颗一模一样的磁盘时， 只要下达类似： dd if=/dev/sda of=/dev/sdb ，就能够让两颗磁盘一模一样，甚至 /dev/sdb 不需要分区与格式化， 因为该指令可以将 /dev/sda 内的所有数据，包括 MBR 与 partition table 也复制到 /dev/sdb 





​						cpio：可备份任何文件，但是不会主动去找文件备份，需要配合类似find的软件



```
[root@study ~]# cpio -ovcB  &gt; [file&#124;device] &lt;==备份
[root@study ~]# cpio -ivcdu &lt; [file&#124;device] &lt;==还原
[root@study ~]# cpio -ivct  &lt; [file&#124;device] &lt;==察看
备份会使用到的选项与参数：
  -o ：将数据 copy 输出到文件或设备上
  -B ：让默认的 Blocks 可以增加至 5120 Bytes ，默认是 512 Bytes ！
　  　 这样的好处是可以让大文件的储存速度加快（请参考 i-nodes 的观念）
还原会使用到的选项与参数：
  -i ：将数据自文件或设备 copy 出来系统当中
  -d ：自动创建目录！使用 cpio 所备份的数据内容不见得会在同一层目录中，因此我们
       必须要让 cpio 在还原时可以创建新目录，此时就得要 -d 选项的帮助！
  -u ：自动的将较新的文件覆盖较旧的文件！
  -t ：需配合 -i 选项，可用在"察看"以 cpio 创建的文件或设备的内容
一些可共享的选项与参数：
  -v ：让储存的过程中文件名称可以在屏幕上显示
  -c ：一种较新的 portable format 方式储存
```



- 备份：find / | cpio -ocvB > /dev/st0
- 还原：cpio -idvc < /dev/st0



​							范例：

备份：

```
范例：找出 /boot 下面的所有文件，然后将他备份到 /tmp/boot.cpio 去！
[root@study ~]# cd /
[root@study /]# find boot -print
boot
boot/grub
boot/grub/splash.xpm.gz
....（以下省略）....
# 通过 find 我们可以找到 boot 下面应该要存在的文件名！包括文件与目录！但请千万不要是绝对路径！
[root@study /]# find boot &#124; cpio -ocvB &gt; /tmp/boot.cpio
[root@study /]# ll -h /tmp/boot.cpio
-rw-r--r--. 1 root root 108M Jul  3 00:05 /tmp/boot.cpio
[root@study ~]# file /tmp/boot.cpio
/tmp/boot.cpio: ASCII cpio archive （SVR4 with no CRC）
```



​							tip：为啥要先转换目录到 / 再去找 boot 呢？ 为何不能直接找 /boot 呢？这是因为 cpio 很笨。它不会理会你给的是绝对路径还是相对路径的文件名，所以如果你加上绝对路径的 / 开头， 那么未来解开的时候，它就一定会覆盖掉原本的 /boot



还原：

```
范例：将刚刚的文件给他在 /root/ 目录下解开
[root@study ~]# cd ~
[root@study ~]# cpio -idvc &lt; /tmp/boot.cpio
[root@study ~]# ll /root/boot
# 你可以自行比较一下 /root/boot 与 /boot 的内容是否一模一样！
```







## 五.vi 与 vim



##### 				vi：三种模式



示意图：![image-20210610230933820](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\image-20210610230933820.png)



​		

​						一般指令模式：vi打开一个文件，也可创建一个文件

​							

​								创建一个文件：绝对路径  +  vi  +文件名

​								

​								一般指令下的可操作按键：

​								

​								移动光标：

   ↑  ↓  ←  →  或  k  j  h  l

**|[Ctrl] + [b]|屏幕“向上”移动一页，相当于 [Page Up] 按键 （常用）**
|[Ctrl] + [d]|屏幕“向下”移动半页
|[Ctrl] + [u]|屏幕“向上”移动半页
|+|光标移动到非空白字符的下一列
|-|光标移动到非空白字符的上一列
|n<space>|那个 n 表示“数字”，例如 20 。按下数字后再按空白键，光标会向右移动这一列的 n 个字符。例如 20<space> 则光标会向后面移动 20 个字符距离。
**|0 或功能键[Home]|这是数字“ 0 ”：移动到这一列的最前面字符处 （常用）**
**|$ 或功能键[End]|移动到这一列的最后面字符处（常用**）
|H|光标移动到这个屏幕的最上方那一列的第一个字符
|M|光标移动到这个屏幕的中央那一列的第一个字符
|L|光标移动到这个屏幕的最下方那一列的第一个字符
**|G|移动到这个文件的最后一列（常用）**
|nG|n 为数字。移动到这个文件的第 n 列。例如 20G 则会移动到这个文件的第 20 列（可配合 :set nu）
**|gg|移动到这个文件的第一列，相当于 1G 啊！ （常用）**
**|n<Enter>|n 为数字。光标向下移动 n 列（常用）**

​								

​							搜寻与取代：

**|/word|向光标之下寻找一个名称为 word 的字串。例如要在文件内搜寻 vbird 这个字串，就输入 /vbird 即可！ （常用）**

|?word|向光标之上寻找一个字串名称为 word 的字串。
|n|这个 n 是英文按键。代表“<u>重复前一个搜寻的动作</u>”。举例来说， 如果刚刚我们执行 /vbird 去向下搜寻 vbird 这个字串，则按下 n 后，会向下继续搜寻下一个名称为 vbird 的字串。如果是执行 ?vbird 的话，那么按下 n 则会向上继续搜寻名称为 vbird 的字串！
|N|这个 N 是英文按键。与 n 刚好相反，为“反向”进行前一个搜寻动作。 例如 /vbird 后，按下 N 则表示“向上”搜寻 vbird 。
|使用 /word 配合 n 及 N 是非常有帮助的！可以让你重复的找到一些你搜寻的关键字！

**|:1,$s/word1/word2/g|从第一列到最后一列寻找 word1 字串，并将该字串取代为 word2 ！（常用）**
**|:1,$s/word1/word2/gc|从第一列到最后一列寻找 word1 字串，并将该字串取代为 word2 ！且在取代前显示提示字符给使用者确认 （confirm） 是否需要取代！（常用）**



​								删除、复制与贴上：

**x, X|在一列字当中，x 为向后删除一个字符 （相当于 [del] 按键）， X 为向前删除一个字符（相当于 [backspace] 亦即是倒退键） （常用）**
|nx|n 为数字，连续向后删除 n 个字符。举例来说，我要连续删除 10 个字符， “10x”。
**|dd|删除光标所在的那一整列（常用）**
**|ndd|n 为数字。删除光标所在的向下 n 列，例如 20dd 则是删除 20 列 （常用）**
|d1G|删除光标所在到第一列的所有数据
|dG|删除光标所在到最后一列的所有数据
|d$|删除光标所在处，到该列的最后一个字符
|d0|那个是数字的 0 ，删除光标所在处，到该列的最前面一个字符
**|yy|复制光标所在的那一列（常用）**
**|nyy|n 为数字。复制光标所在的向下 n 列，例如 20yy 则是复制 20 列（常用）**
|y1G|复制光标所在列到第一列的所有数据
|yG|复制光标所在列到最后一列的所有数据
|y0|复制光标所在的那个字符到该列行首的所有数据
|y$|复制光标所在的那个字符到该列行尾的所有数据
**|p, P|p 为将已复制的数据在光标下一列贴上，P 则为贴在光标上一列！ 举例来说，我目前光标在第 20 列，且已经复制了 10 列数据。则按下 p 后， 那 10 列数据会贴在原本的 20 列之后，亦即由 21 列开始贴。但如果是按下 P 呢？ 那么原本的第 20 列会被推到变成 30 列。 （常用）**
|J|将光标所在列与下一列的数据结合成同一列
|c|重复删除多个数据，例如向下删除 10 列，[ 10cj ]
**|u|复原前一个动作。（常用）**
**|[Ctrl]+r|重做上一个动作。（常用）**
|这个 u 与 [Ctrl]+r 是很常用的指令！一个是复原，另一个则是重做一次～ 利用这两个功能按键，你的编辑，嘿嘿！很快乐的啦！
**|.|不要怀疑！这就是小数点！意思是重复前一个动作的意思。 如果你想要重复删除、重复贴上等等动作，按下小数点“.”就好了！ （常用）**





​						编辑模式：在一般指令模式按  i  或ins进入



​								操作按键：



​										进入插入或取代的编辑模式

**|i, I|进入插入模式（Insert mode）：i 为“从目前光标所在处插入”， I 为“在目前所在列的第一个非空白字符处开始插入”。 （常用）**
**|a, A|进入插入模式（Insert mode）：a 为“从目前光标所在的下一个字符处开始插入”， A 为“从光标所在列的最后一个字符处开始插入”。（常用）**
**|o, O|进入插入模式（Insert mode）：这是英文字母 o 的大小写。o 为“在目前光标所在的下一列处插入新的一列”； O 为在目前光标所在处的上一列插入新的一列！（常用）**
**|r, R|进入取代模式（Replace mode）：r 只会取代光标所在的那一个字符一次；R会一直取代光标所在的文字，直到按下 ESC 为止；（常用）**
|上面这些按键中，在 vi 画面的左下角处会出现“—INSERT—”或“—REPLACE—”的字样。 由名称就知道该动作了吧！！特别注意的是，我们上面也提过了，你想要在文件里面输入字符时， 一定要在左下角处看到 INSERT 或 REPLACE 才能输入喔！
**|[Esc]|退出编辑模式，回到一般指令模式中 按  ：wq  保存退出     ：wq！    强制保存退出（常用）**





​						命令行模式：一般模式下输入   :   /   ?    中任何一个进入，可搜寻数据，读取、存盘等操作



​								命令行界面的储存、离开等指令：



**|:w|将编辑的数据写入硬盘文件中（常用）**
|:w!|若文件属性为“只读”时，强制写入该文件。不过，到底能不能写入， 还是跟你对该文件的文件权限有关啊！
**|:q|离开 vi （常用）**
|:q!|若曾修改过文件，又不想储存，使用 ! 为强制离开不储存盘案。
|注意一下啊，那个惊叹号 （!） 在 vi 当中，常常具有“强制”的意思～
|:wq|储存后离开，若为 :wq! 则为强制储存后离开 （常用）
|ZZ|这是大写的 Z 喔！若文件没有更动，则不储存离开，若文件已经被更动过，则储存后离开！
|:w [filename]|将编辑的数据储存成另一个文件（类似另存新文件）
|:r [filename]|在编辑的数据中，读入另一个文件的数据。亦即将 “filename” 这个文件内容加到光标所在列后面
|:n1,n2 w [filename]|将 n1 到 n2 的内容储存成 filename 这个文件。
|:! command|暂时离开 vi 到命令行界面下执行 command 的显示结果！例如 “:! ls /home”即可在 vi 当中察看 /home 下面以 ls 输出的文件信息！
|vim 环境的变更
|:set nu|显示行号，设置之后，会在每一列的字首显示该列的行号
|:set nonu|与 set nu 相反，为取消行号！





##### vim的暂存盘：   使用 vim 编辑时， vim 会在与被编辑的文件的目录下，再创建一个名为 .filename.swp 的文件



​						继续编辑该暂存盘：

```
[dmtsai@study vitest]$ vim filename
E325: ATTENTION  &lt;==错误代码
Found a swap file by the name ".man_db.conf.swp"  &lt;==下面数列说明有暂存盘的存在
          owned by: dmtsai   dated: Mon Jul  6 23:54:16 2015
         file name: /tmp/vitest/man_db.conf  &lt;==这个暂存盘属于哪个实际的文件？
          modified: no
         user name: dmtsai   host name: study.centos.vbird
        process ID: 31851
While opening file "man_db.conf"
             dated: Mon Jul  6 23:47:21 2015
下面说明可能发生这个错误的两个主要原因与解决方案！
（1） Another program may be editing the same file.  If this is the case,
    be careful not to end up with two different instances of the same
    file when making changes.  Quit, or continue with caution.
（2） An edit session for this file crashed.
    If this is the case, use ":recover" or "vim -r man_db.conf"
    to recover the changes （see ":help recovery"）.
    If you did this already, delete the swap file ".man_db.conf.swp"
    to avoid this message.
Swap file ".man_db.conf.swp" already exists! 下面说明你可进行的动作
[O]pen Read-Only, （E）dit anyway, （R）ecover, （D）elete it, （Q）uit, （A）bort:
```



​						问题1：可能有其他人或程序同时在编辑这个文件

​						解决法则：

- 找到另外那个程序或人员，请他将该 vim 的工作结束，然后你再继续处理。
- 如果你只是要看该文件的内容并不会有任何修改编辑的行为，那么可以选择打开成为只读（O）文件， 亦即上述画面反白部分输入英文“ o ”即可，其实就是 [O]pen Read-Only 的选项啦！



​						问题2：在前一个 vim 的环境中，可能因为某些不知名原因导致 vim 中断 （crashed）

​						解决法则：

- 如果你之前的 vim 处理动作尚未储存，此时你应该要按下“R”，亦即使用 （R）ecover 的项目， 此时 vim 会载入 .man_db.conf.swp 的内容，让你自己来决定要不要储存！这样就能够救回来你之前未储存的工作。 不过那个 .man_db.conf.swp 并不会在你结束 vim 后自动删除，所以你离开 vim 后还得要自行删除 .man_db.conf.swp 才能避免每次打开这个文件都会出现这样的警告！
- 如果你确定这个暂存盘是没有用的，那么你可以直接按下“D”删除掉这个暂存盘，亦即 （D）elete it 这个项目即可。 此时 vim 会载入 man_db.conf ，并且将旧的 .man_db.conf.swp 删除后，创建这次会使用的新的 .man_db.conf.swp 喔！



##### 						六个可用按钮：

- [O]pen Read-Only：打开此文件成为只读文件， 可以用在你只是想要查阅该文件内容并不想要进行编辑行为时。
- （E）dit anyway：还是用正常的方式打开你要编辑的那个文件， 并不会载入暂存盘的内容。不过很容易出现两个使用者互相改变对方的文件等问题。
- （R）ecover：就是载入暂存盘的内容，用在你要救回之前未储存的工作。 不过当你救回来并且储存离开 vim 后，还是要手动自行删除那个暂存盘。
- （D）elete it：你确定那个暂存盘是无用的！那么打开文件前会先将这个暂存盘删除！ 这个动作其实是比较常做的，因为你可能不确定这个暂存盘是怎么来的。
- （Q）uit：按下 q 就离开 vim ，不会进行任何动作回到命令提示字符。
- （A）bort：忽略这个编辑行为，感觉上与 quit 非常类似！ 也会送你回到命令提示字符





##### 				vim的额外功能：



​						在文字模式使用alias这个指令，在当使用vi的时候，其实就是在使用vim



​						在vim模式下，左下角会有文件说明，右下角多了游标所在的行和字节单，

​						并且支持多种程序语法，会自动除错





​				区块选择、复制、粘贴：可以选择处理区块

​	

​				需要按下以下空格，并且移动光标

| 区块选择的按键意义 |                                        |
| :----------------- | -------------------------------------- |
| v                  | 字符选择，会将光标经过的地方反白选择！ |
| V                  | 列选择，会将光标经过的列反白选择！     |
| [Ctrl]+v           | 区块选择，可以用长方形的方式选择数据   |
| y                  | 将反白的地方复制起来                   |
| d                  | 将反白的地方删除掉                     |
| p                  | 将刚刚复制的区块，在光标所在处贴上！   |





##### 				多文件编辑：

​				

​						首先：输入vim filename filename （即两个文件）



​						并且选择复制文件内容跳转到另一个文件中



| 多文件编辑的按键 |                                   |
| :--------------- | --------------------------------- |
| :n               | 编辑下一个文件                    |
| :N               | 编辑上一个文件                    |
| :files           | 列出目前这个 vim 的打开的所有文件 |





##### 				多功能小窗口：

​			

​						vim打开一个文件之后，可通过：sp    来打开另一个文件在页面显示。

| 多窗口情况下的按键功能 |                                                              |
| :--------------------- | ------------------------------------------------------------ |
| :sp [filename]         | 打开一个新窗口，如果有加 filename， 表示在新窗口打开一个新文件，否则表示两个窗口为同一个文件内容（同步显示）。 |
| [ctrl]+w+ j [ctrl]+w+↓ | 按键的按法是：先按下 [ctrl] 不放， 再按下 w 后放开所有的按键，然后再按下 j （或向下方向键），则光标可移动到下方的窗口。 |
| [ctrl]+w+ k [ctrl]+w+↑ | 同上，不过光标移动到上面的窗口。                             |
| [ctrl]+w+ q            | 其实就是 :q 结束离开啦！ 举例来说，如果我想要结束下方的窗口，那么利用 [ctrl]+w+↓ 移动到下方窗口后，按下 :q 即可离开， 也可以按下 [ctrl]+w+q 啊！ |





##### 				vim 的挑字补全功能：

| 组合按钮             | 补齐的内容                                                 |
| :------------------- | :--------------------------------------------------------- |
| [ctrl]+x -> [ctrl]+n | 通过目前正在编辑的这个“文件的内容文字”作为关键字，予以补齐 |
| [ctrl]+x -> [ctrl]+f | 以当前目录内的“文件名”作为关键字，予以补齐                 |
| [ctrl]+x -> [ctrl]+o | 以扩展名作为语法补充，以 vim 内置的关键字，予以补齐        |





##### 				vim的环境设置与记录：



​						vim 会主动的将你曾经做过的行为登录下来， 那个记录动作的文件就是： ~/.viminfo



​						一般指令中模式查看自己环境的设置值常用的一些简单的设置值：

| vim 的环境设置参数                |                                                              |
| :-------------------------------- | ------------------------------------------------------------ |
| :set nu :set nonu                 | 就是设置与取消行号啊！                                       |
| :set hlsearch :set nohlsearch     | hlsearch 就是 high light search（高亮度搜寻）。 这个就是设置是否将搜寻的字串反白的设置值。默认值是 hlsearch |
| :set autoindent :set noautoindent | 是否自动缩排？autoindent 就是自动缩排。                      |
| :set backup                       | 是否自动储存备份文件？一般是 nobackup 的， 如果设置 backup 的话，那么当你更动任何一个文件时，则原始文件会被另存成一个文件名为 filename~ 的文件。 举例来说，我们编辑 hosts ，设置 :set backup ，那么当更动 hosts 时，在同目录下，就会产生 hosts~ 文件名的文件，记录原始的 hosts 文件内容 |
| :set ruler                        | 还记得我们提到的右下角的一些状态列说明吗？ 这个 ruler 就是在显示或不显示该设置值的啦！ |
| :set showmode                     | 这个则是，是否要显示 —INSERT— 之类的字眼在左下角的状态列。   |
| :set backspace=（012）            | 一般来说， 如果我们按下 i 进入编辑模式后，可以利用倒退键 （backspace） 来删除任意字符的。 但是，某些 distribution 则不许如此。此时，我们就可以通过 backspace 来设置啰～ 当 backspace 为 2 时，就是可以删除任意值；0 或 1 时，仅可删除刚刚输入的字符， 而无法删除原本就已经存在的文字了！ |
| :set all                          | 显示目前所有的环境参数设置值。                               |
| :set                              | 显示与系统默认值不同的设置参数， 一般来说就是你有自行变动过的设置参数啦！ |
| :syntax on :syntax off            | 是否依据程序相关语法显示不同颜色？ 举例来说，在编辑一个纯文本文件时，如果开头是以 # 开始，那么该列就会变成蓝色。 如果你懂得写程序，那么这个 :syntax on 还会主动的帮你除错呢！但是， 如果你仅是编写纯文本，要避免颜色对你的屏幕产生的干扰，则可以取消这个设置 。 |
| :set bg=dark :set bg=light        | 可用以显示不同的颜色色调，默认是“ light ”。如果你常常发现注解的字体深蓝色实在很不容易看， 那么这里可以设置为 dark 喔！试看看，会有不同的样式呢！ |





##### 				vim中文编码的问题：

​		

​						修正语系编码的行为

```
[dmtsai@study ~]$ LANG=zh_TW.big5
[dmtsai@study ~]$ export LC_ALL=zh_TW.big5
```

然后在终端接口工具列的“终端机”—>“设置字符编码” —>“中文 （正体） （BIG5）”项目点选一下， 如果一切都没有问题了，再用 vim 去打开那个 big5 编码的文件





##### 								DOS 与 Linux 的断行字符：

​				

​						字符转换的软件安装：

```
[dmtsai@study ~]$ su -   # 安装软件一定要是 root 的权限才行！
[root@study ~]# mount /dev/sr0 /mnt
[root@study ~]# rpm -ivh /mnt/Packages/dos2unix-*
warning: /mnt/Packages/dos2unix-6.0.3-4.el7.x86_64.rpm: Header V3 RSA/SHA256 ....
Preparing...                          ################################# [100%]
Updating / installing...
   1:dos2unix-6.0.3-4.el7             ################################# [100%]
[root@study ~]# umount /mnt
[root@study ~]# exit
```



​						字符转化软件使用案例：

```
[dmtsai@study ~]$ dos2unix [-kn] file [newfile]
[dmtsai@study ~]$ unix2dos [-kn] file [newfile]
选项与参数：
-k  ：保留该文件原本的 mtime 时间格式 （不更新文件上次内容经过修订的时间）
-n  ：保留原本的旧文件，将转换后的内容输出到新文件，如： dos2unix -n old new
范例一：将 /etc/man_db.conf 重新复制到 /tmp/vitest/ 下面，并将其修改成为 dos 断行
[dmtsai@study ~]# cd /tmp/vitest
[dmtsai@study vitest]$ cp -a /etc/man_db.conf .
[dmtsai@study vitest]$ ll man_db.conf
-rw-r--r--. 1 root root 5171 Jun 10  2014 man_db.conf
[dmtsai@study vitest]$ unix2dos -k man_db.conf
unix2dos: converting file man_db.conf to DOS format ...
# 屏幕会显示上述的讯息，说明断行转为 DOS 格式了！
[dmtsai@study vitest]$ ll man_db.conf
-rw-r--r--. 1 dmtsai dmtsai 5302 Jun 10  2014 man_db.conf
# 断行字符多了 ^M ，所以容量增加了！
范例二：将上述的 man_db.conf 转成 Linux 断行字符，并保留旧文件，新文件放于 man_db.conf.linux
[dmtsai@study vitest]$ dos2unix -k -n man_db.conf man_db.conf.linux
dos2unix: converting file man_db.conf to file man_db.conf.linux in Unix format ...
[dmtsai@study vitest]$ ll man_db.conf*
-rw-r--r--. 1 dmtsai dmtsai 5302 Jun 10  2014 man_db.conf
-rw-r--r--. 1 dmtsai dmtsai 5171 Jun 10  2014 man_db.conf.linux
[dmtsai@study vitest]$ file man_db.conf*
man_db.conf:       ASCII text, with CRLF line terminators  # 很清楚说明是 CRLF 断行！
man_db.conf.linux: ASCII text
```





​						语系编码转换：

```
[dmtsai@study ~]$ iconv --list
[dmtsai@study ~]$ iconv -f 原本编码 -t 新编码 filename [-o newfile]
选项与参数：
--list ：列出 iconv 支持的语系数据
-f     ：from ，亦即来源之意，后接原本的编码格式；
-t     ：to ，亦即后来的新编码要是什么格式；
-o file：如果要保留原本的文件，那么使用 -o 新文件名，可以创建新编码文件。
范例一：将 /tmp/vitest/vi.big5 转成 utf8 编码吧！
[dmtsai@study ~]$ cd /tmp/vitest
[dmtsai@study vitest]$ iconv -f big5 -t utf8 vi.big5 -o vi.utf8
[dmtsai@study vitest]$ file vi*
vi.big5: ISO-8859 text, with CRLF line terminators
vi.utf8: UTF-8 Unicode text, with CRLF line terminators
```





## 六.Bash shell



##### 									命名别名设置功能：alias



​							可下达以下命令来设置别名：

```
alias lm=‘ls -al’
```

​							就可以达到使用lm命令得到ls -al的效果



##### 				程序化脚本：shell scripts

​				



##### 				万用字符： Wildcard



​						举例来说，想要知道 /usr/bin 下面有多少以 X 为开头的文件

​						使用：“ ls -l /usr/bin/X* ”就能够知道





##### 				查询指令是否为 Bash shell 的内置命令： type

​	

```
[dmtsai@study ~]$ type [-tpa] name
选项与参数：
：不加任何选项与参数时，type 会显示出 name 是外部指令还是 bash 内置指令
-t  ：当加入-t 参数时，type 会将 name 以下面这些字眼显示出他的意义：
      file    ：表示为外部指令；
alias：表示该指令为命令别名所设置的名称；
      builtin ：表示该指令为 bash 内置的指令功能；
-p  ：如果后面接的 name 为外部指令时，才会显示完整文件名；
-a  ：会由 PATH 变量定义的路径中，将所有含 name 的指令都列出来，包含alias
```





##### 				shell的快捷编辑按钮：

| 组合键            | 功能与示范                                                   |
| :---------------- | :----------------------------------------------------------- |
| [ctrl]+u/[ctrl]+k | 分别是从光标处向前删除指令串 （[ctrl]+u） 及向后删除指令串 （[ctrl]+k）。 |
| [ctrl]+a/[ctrl]+e | 分别是让光标移动到整个指令串的最前面 （[ctrl]+a） 或最后面 （[ctrl]+e）。 |







##### 				shell的变量：



​						查看变量：echo

```
echo ￥{PATH}
```


​    
​    [root@localhost home]# help echo
​    echo: echo [-neE] [参数 ...]
​        将参数写到标准输出。
​        
​    在标准输出上显示 ARG 参数后跟一个换行。
​    
​    选项：
​      -n	不要追加换行
​      -e	启用下列反斜杠转义的解释
​      -E	显式地抑制对于反斜杠转义的解释
​    
​    `echo' 对下列反斜杠字符进行转义：
​      \a	警告（响铃）
​      \b	退格
​      \c	抑制更多的输出
​      \e	转义字符
​      \f	格式提供
​      \n	换行
​      \r	回车
​      \t	横向制表符
​      \v	纵向制表符
​      \\	反斜杠
​      \0nnn	以 NNN （八进制）为 ASCII 码的字符。 NNN 可以是
​    	0到3个八进制数字
​      \xHH	以 HH （十六进制）为值的八比特字符。HH可以是
​    	一个或两个十六进制数字

​						可 cd 进入自己设置的变量



​						设置变量内容： =

```
[dmtsai@study ~]$ echo ${myname}
       &lt;==这里并没有任何数据～因为这个变量尚未被设置，是空的
[dmtsai@study ~]$ myname=VBird
[dmtsai@study ~]$ echo ${myname}
VBird  &lt;==出现了，因为这个变量已经被设置了
```

​						

​						设置规则：

- 变量与变量内容以一个等号“=”来链接
- 等号两边不能直接接空白字符
- 变量名称只能是英文字母与数字，但是开头字符不能是数字
- 变量内容若有空白字符可使用双引号“"”或单引号“'”将变量内容结合起来，但
  - 双引号内的特殊字符如 $ 等，可以保有原本的特性
  - 单引号内的特殊字符则仅为一般字符 （纯文本）
- 可用跳脱字符“ \ ”将特殊符号（如 [Enter], $, \, 空白字符, '等）变成一般字符
- 在一串指令的执行中，还需要借由其他额外的指令所提供的信息时，可以使用反单引号“指令”或 “$（指令）”。特别注意，那个 ` 是键盘上方的数字键 1 左边那个按键，而不是单引号， 例如想要取得核心版本的设置：“version=$（uname -r）”再“echo $version”可得“3.10.0-229.el7.x86_64”
- 若该变量为扩增变量内容时，则可用 "$变量名称" 或 ${变量} 累加内容
- 若该变量需要在其他子程序执行，则需要以 export 来使变量变成环境变量：“export PATH”
- 通常大写字符为系统默认变量，自行设置变量可以使用小写字符，方便判断 （纯粹依照使用者兴趣与嗜好） 
- 取消变量的方法为使用 unset ：“unset 变量名称”例如取消 myname 的设置：“unset myname”



```
范例一：我要在 PATH 这个变量当中“累加”:/home/dmtsai/bin 这个目录
[dmtsai@study ~]$ PATH=$PATH:/home/dmtsai/bin
[dmtsai@study ~]$ PATH="$PATH":/home/dmtsai/bin
[dmtsai@study ~]$ PATH=${PATH}:/home/dmtsai/bin
# 上面这三种格式在 PATH 里头的设置都是 OK 的！但是下面的例子就不见得啰！
范例二：承范例一，我要将 name 的内容多出 "yes" 呢？
[dmtsai@study ~]$ name=$nameyes  
# 知道了吧？如果没有双引号，那么变量成了啥？name 的内容是 $nameyes 这个变量！
# 呵呵！我们可没有设置过 nameyes 这个变量呐！所以，应该是下面这样才对！
[dmtsai@study ~]$ name="$name"yes
[dmtsai@study ~]$ name=${name}yes  &lt;==以此例较佳！
范例三：如何让我刚刚设置的 name=VBird 可以用在下个 shell 的程序？
[dmtsai@study ~]$ name=VBird
[dmtsai@study ~]$ bash        &lt;==进入到所谓的子程序
[dmtsai@study ~]$ echo $name  &lt;==子程序：再次的 echo 一下；
       &lt;==嘿嘿！并没有刚刚设置的内容喔！
[dmtsai@study ~]$ exit        &lt;==子程序：离开这个子程序
[dmtsai@study ~]$ export name
[dmtsai@study ~]$ bash        &lt;==进入到所谓的子程序
[dmtsai@study ~]$ echo $name  &lt;==子程序：在此执行！
VBird  &lt;==看吧！出现设置值了！
[dmtsai@study ~]$ exit        &lt;==子程序：离开这个子程序
```





##### 				环境变量的功能：



​						列出目前的 shell 环境：env



​						一些重要的变量：

- PS1：（提示字符的设置）



```
[dmtsai@study ~]$ ps1=‘[\u@\h \w \a]
```



- 这是 PS1 （数字的 1 不是英文字母），这个东西就是我们的“命令提示字符”喔！ 当我们每次按下 [Enter] 按键去执行某个指令后，最后要再次出现提示字符时， 就会主动去读取这个变量值了。上头 PS1 内显示的是一些特殊符号，这些特殊符号可以显示不同的信息， 每个 distributions 的 bash 默认的 PS1 变量内容可能有些许的差异，不要紧，“习惯你自己的习惯”就好了。 你可以用 man bash [[3\]](https://www.bookstack.cn/read/vbird-linux-basic-4e/89.md#ps3)去查询一下 PS1 的相关说明，以理解下面的一些符号意义。
- \d ：可显示出“星期 月 日”的日期格式，如："Mon Feb 2"
- \H ：完整的主机名称。举例来说，鸟哥的练习机为“study.centos.vbird”
- \h ：仅取主机名称在第一个小数点之前的名字，如鸟哥主机则为“study”后面省略
- \t ：显示时间，为 24 小时格式的“HH![:MM:](https://www.emoji-cheat-sheet.com/graphics/emojis/MM.png)SS”
- \T ：显示时间，为 12 小时格式的“HH![:MM:](https://www.emoji-cheat-sheet.com/graphics/emojis/MM.png)SS”
- \A ：显示时间，为 24 小时格式的“HH:MM”
- \@ ：显示时间，为 12 小时格式的“am/pm”样式
- \u ：目前使用者的帐号名称，如“dmtsai”；
- \v ：BASH 的版本信息，如鸟哥的测试主机版本为 4.2.46（1）-release，仅取“4.2”显示
- \w ：完整的工作目录名称，由根目录写起的目录名称。但主文件夹会以 ~ 取代；
- \W ：利用 basename 函数取得工作目录名称，所以仅会列出最后一个目录名。
- ：下达的第几个指令
- ￥：提示字符，如果是root时，提示字符为#，否则就是￥
- $:（关于本 shell 的 PID）,可使用echo ￥￥  查看shell的PID
- ?：（关于上个执行指令的回传值）

```
[dmtsai@study ~]$ echo $SHELL
/bin/bash                                  &lt;==可顺利显示，没有错误
[dmtsai@study ~]$ echo $?
0                                          &lt;==因为没问题，所以回传值为 0
[dmtsai@study ~]$ 12name=VBird
bash: 12name=VBird: command not found...   &lt;==发生错误了，bash回报有问题
[dmtsai@study ~]$ echo $?
127                                        &lt;==因为有问题，回传错误代码（非为0）
# 错误代码回传值依据软件而有不同，我们可以利用这个代码来搜寻错误的原因
[dmtsai@study ~]$ echo $?
0
```

- OSTYPE, HOSTTYPE, MACHTYPE：（主机硬件与核心的等级）
- export： 自订变量转成环境变量



##### 				语系变量 （locale）





##### 				变量键盘读取、阵列与宣告： read, array, declare





​						read：键盘读取

```
[dmtsai@study ~]$ read [-pt] variable
选项与参数：
-p  ：后面可以接提示字符
-t  ：后面可以接等待的“秒数”这个比较有趣～不会一直等待使用者
范例一：让使用者由键盘输入一内容，将该内容变成名为 atest 的变量
[dmtsai@study ~]$ read atest
This is a test        &lt;==此时光标会等待你输入，请输入左侧文字看看
[dmtsai@study ~]$ echo ${atest}
This is a test          &lt;==你刚刚输入的数据已经变成一个变量内容
范例二：提示使用者 30 秒内输入自己的大名，将该输入字串作为名为 named 的变量内容
[dmtsai@study ~]$ read -p "Please keyin your name: " -t 30 named
Please keyin your name: VBird Tsai   &lt;==注意看，会有提示字符
[dmtsai@study ~]$ echo ${named}
VBird Tsai        &lt;==输入的数据又变成一个变量的内容了
```



​						declare / typeset：一样的功能，宣告变量的类型

```
[dmtsai@study ~]$ declare [-aixr] variable
选项与参数：
-a  ：将后面名为 variable 的变量定义成为阵列 （array） 类型
-i  ：将后面名为 variable 的变量定义成为整数数字 （integer） 类型
-x  ：用法与 export 一样，就是将后面的 variable 变成环境变量；
-r  ：将变量设置成为 readonly 类型，该变量不可被更改内容，也不能 unset
范例一：让变量 sum 进行 100+300+50 的加总结果
[dmtsai@study ~]$ sum=100+300+50
[dmtsai@study ~]$ echo ${sum}
100+300+50  怎么没有帮我计算加总，因为这是文字体态的变量属性
[dmtsai@study ~]$ declare -i sum=100+300+50
[dmtsai@study ~]$ echo ${sum}
450         
范例二：将 sum 变成环境变量
[dmtsai@study ~]$ declare -x sum
[dmtsai@study ~]$ export |; grep sum
declare -ix sum="450"  &lt;==果然出现了！包括有 i 与 x 的宣告！
范例三：让 sum 变成只读属性，不可更动！
[dmtsai@study ~]$ declare -r sum
[dmtsai@study ~]$ sum=tesgting
-bash: sum: readonly variable  &lt;==老天爷～不能改这个变量了！
范例四：让 sum 变成非环境变量的自订变量吧！
[dmtsai@study ~]$ declare +x sum  &lt;== 将 - 变成 + 可以进行“取消”动作
[dmtsai@study ~]$ declare -p sum  &lt;== -p 可以单独列出变量的类型
declare -ir sum="450" &lt;== 看吧！只剩下 i, r 的类型，不具有 x 啰
```



##### 				与文件系统及程序的限制关系： ulimit

```
[dmtsai@study ~]$ ulimit [-SHacdfltu] [配额]
选项与参数：
-H  ：hard limit ，严格的设置，必定不能超过这个设置的数值；
-S  ：soft limit ，警告的设置，可以超过这个设置值，但是若超过则有警告讯息。
      在设置上，通常 soft 会比 hard 小，举例来说，soft 可设置为 80 而 hard
      设置为 100，那么你可以使用到 90 （因为没有超过 100），但介于 80~100 之间时，
      系统会有警告讯息通知你！
-a  ：后面不接任何选项与参数，可列出所有的限制额度；
-c  ：当某些程序发生错误时，系统可能会将该程序在内存中的信息写成文件（除错用），
      这种文件就被称为核心文件（core file）。此为限制每个核心文件的最大容量。
-f  ：此 shell 可以创建的最大文件大小（一般可能设置为 2GB）单位为 KBytes
-d  ：程序可使用的最大断裂内存（segment）容量；
-l  ：可用于锁定 （lock） 的内存量
-t  ：可使用的最大 CPU 时间 （单位为秒）
-u  ：单一使用者可以使用的最大程序（process）数量。
```





##### 				变量内容的删除、取代与替换 （Optional）



```
范例一：先让小写的 path 自订变量设置的与 PATH 内容相同
[dmtsai@study ~]$ path=${PATH}
[dmtsai@study ~]$ echo ${path}
/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/home/dmtsai/.local/bin:/home/dmtsai/bin
范例二：假设我不喜欢 local/bin，所以要将前 1 个目录删除掉，如何显示
[dmtsai@study ~]$ echo ${path#/*local/bin:}
/usr/bin:/usr/local/sbin:/usr/sbin:/home/dmtsai/.local/bin:/home/dmtsai/bin
```

| 变量设置方式                                     | 说明                                                         |
| :----------------------------------------------- | :----------------------------------------------------------- |
| `${变量#关键字}` `${变量##关键字}`               | 若变量内容从头开始的数据符合“关键字”，则将符合的最短数据删除 若变量内容从头开始的数据符合“关键字”，则将符合的最长数据删除 |
| `${变量%关键字}` `${变量%%关键字}`               | 若变量内容从尾向前的数据符合“关键字”，则将符合的最短数据删除 若变量内容从尾向前的数据符合“关键字”，则将符合的最长数据删除 |
| `${变量/旧字串/新字串}` `${变量//旧字串/新字串}` | 若变量内容符合“旧字串”则“第一个旧字串会被新字串取代” 若变量内容符合“旧字串”则“全部的旧字串会被新字串取代” |



##### 				变量的测试与内容替换



​					减号“ - ”后面接的关键字，是给予未设置变量的内容

```
范例一：测试一下是否存在 username 这个变量，若不存在则给予 username 内容为 root
[dmtsai@study ~]$ echo ${username}
           &lt;==由于出现空白，所以 username 可能不存在，也可能是空字串
[dmtsai@study ~]$ username=${username-root}
[dmtsai@study ~]$ echo ${username}
root       &lt;==因为 username 没有设置，所以主动给予名为 root 的内容。
[dmtsai@study ~]$ username="vbird tsai" &lt;==主动设置 username 的内容
[dmtsai@study ~]$ username=${username-root}
[dmtsai@study ~]$ echo ${username}
vbird tsai &lt;==因为 username 已经设置了，所以使用旧有的设置而不以 root 取代

范例二：若 username 未设置或为空字串，则将 username 内容设置为 root
[dmtsai@study ~]$ username=""
[dmtsai@study ~]$ username=${username-root}
[dmtsai@study ~]$ echo ${username}
      &lt;==因为 username 被设置为空字串了，所以当然还是保留为空字串
[dmtsai@study ~]$ username=${username:-root}
[dmtsai@study ~]$ echo ${username}
root  &lt;==加上“ : ”后若变量内容为空或者是未设置，都能够以后面的内容替换
```



| 变量设置方式     | str 没有设置       | str 为空字串       | str 已设置非为空字串 |
| :--------------- | :----------------- | :----------------- | :------------------- |
| var=${str-expr}  | var=expr           | var=               | var=$str             |
| var=${str:-expr} | var=expr           | var=expr           | var=$str             |
| var=${str+expr}  | var=               | var=expr           | var=expr             |
| var=${str:+expr} | var=               | var=               | var=expr             |
| var=${str=expr}  | str=expr var=expr  | str 不变 var=      | str 不变 var=$str    |
| var=${str:=expr} | str=expr var=expr  | str=expr var=expr  | str 不变 var=$str    |
| var=${str?expr}  | expr 输出至 stderr | var=               | var=$str             |
| var=${str:?expr} | expr 输出至 stderr | expr 输出至 stderr | var=$str             |





##### 				命令别名的设置和取消： alias, unalias

```
alias rm='rm -i'

unalias rm
```





##### 				历史命令：history

```
[dmtsai@study ~]$ history [n]
[dmtsai@study ~]$ history [-c]
[dmtsai@study ~]$ history [-raw] histfiles
选项与参数：
n   ：数字，意思是“要列出最近的 n 笔命令列表”的意思！
-c  ：将目前的 shell 中的所有 history 内容全部消除
-a  ：将目前新增的 history 指令新增入 histfiles 中，若没有加 histfiles ，
      则默认写入 ~/.bash_history
-r  ：将 histfiles 的内容读到目前这个 shell 的 history 记忆中；
-w  ：将目前的 history 记忆内容写入 histfiles 中！
```

```
[dmtsai@study ~]$ history
   66  man rm
   67  alias
   68  man history
   69  history 
[dmtsai@study ~]$ !66  &lt;==执行第 66 笔指令
[dmtsai@study ~]$ !!   &lt;==执行上一个指令，本例中亦即 !66 
[dmtsai@study ~]$ !al  &lt;==执行最近以 al 为开头的指令（上头列出的第 67 个）
```





##### 				bash的进站与欢迎讯息： /etc/issue, /etc/motd

| issue 内的各代码意义                         |
| :------------------------------------------- |
| \d 本地端时间的日期；                        |
| \l 显示第几个终端机接口；                    |
| \m 显示硬件的等级 （i386/i486/i586/i686…）； |
| \n 显示主机的网络名称；                      |
| \O 显示 domain name；                        |
| \r 操作系统的版本 （相当于 uname -r）        |
| \t 显示本地端时间的时间；                    |
| \S 操作系统的名称；                          |
| \v 操作系统的版本。                          |





##### 				终端机的环境设置： stty, set

```
[dmtsai@study ~]$ stty [-a]
选项与参数：
-a  ：将目前所有的 stty 参数列出来；
```

- intr : 送出一个 interrupt （中断） 的讯号给目前正在 run 的程序 （就是终止啰！）；
- quit : 送出一个 quit 的讯号给目前正在 run 的程序；
- erase : 向后删除字符，
- kill : 删除在目前命令行上的所有文字；
- eof : End of file 的意思，代表“结束输入”。
- start : 在某个程序停止后，重新启动他的 output
- stop : 停止目前屏幕的输出；
- susp : 送出一个 terminal stop 的讯号给正在 run 的程序。



```
[dmtsai@study ~]$ set [-uvCHhmBx]
选项与参数：
-u  ：默认不启用。若启用后，当使用未设置变量时，会显示错误讯息；
-v  ：默认不启用。若启用后，在讯息被输出前，会先显示讯息的原始内容；
-x  ：默认不启用。若启用后，在指令被执行前，会显示指令内容（前面有 ++ 符号）
-h  ：默认启用。与历史命令有关；
-H  ：默认启用。与历史命令有关；
-m  ：默认启用。与工作管理有关；
-B  ：默认启用。与刮号 [] 的作用有关；
-C  ：默认不启用。若使用 &gt; 等，则若文件存在时，该文件不会被覆盖。
```

| 组合按键 | 执行结果                               |
| :------- | :------------------------------------- |
| Ctrl + C | 终止目前的命令                         |
| Ctrl + D | 输入结束 （EOF），例如邮件结束的时候； |
| Ctrl + M | 就是 Enter 啦！                        |
| Ctrl + S | 暂停屏幕的输出                         |
| Ctrl + Q | 恢复屏幕的输出                         |
| Ctrl + U | 在提示字符下，将整列命令删除           |
| Ctrl + Z | “暂停”目前的命令                       |





##### 				万用字符与特殊符号

​						万用：		

| 符号  | 意义                                                         |
| :---- | :----------------------------------------------------------- |
| *     | 代表“ 0 个到无穷多个”任意字符                                |
| ?     | 代表“一定有一个”任意字符                                     |
| [ ]   | 同样代表“一定有一个在括号内”的字符（非任意字符）。例如 [abcd] 代表“一定有一个字符， 可能是 a, b, c, d 这四个任何一个” |
| [ - ] | 若有减号在中括号内时，代表“在编码顺序内的所有字符”。例如 [0-9] 代表 0 到 9 之间的所有数字，因为数字的语系编码是连续的！ |
| [^ ]  | 若中括号内的第一个字符为指数符号 （^） ，那表示“反向选择”，例如 [^abc] 代表 一定有一个字符，只要是非 a, b, c 的其他字符就接受的意思。 |

```
[dmtsai@study ~]$ LANG=C              &lt;==由于与编码有关，先设置语系一下
范例一：找出 /etc/ 下面以 cron 为开头的文件名
[dmtsai@study ~]$ ll -d /etc/cron*    &lt;==加上 -d 是为了仅显示目录而已
范例二：找出 /etc/ 下面文件名“刚好是五个字母”的文件名
[dmtsai@study ~]$ ll -d /etc/?????    &lt;==由于 ? 一定有一个，所以五个 ? 就对了
范例三：找出 /etc/ 下面文件名含有数字的文件名
[dmtsai@study ~]$ ll -d /etc/*[0-9]*  &lt;==记得中括号左右两边均需 *
范例四：找出 /etc/ 下面，文件名开头非为小写字母的文件名：
[dmtsai@study ~]$ ll -d /etc/[^a-z]*  &lt;==注意中括号左边没有 *
范例五：将范例四找到的文件复制到 /tmp/upper 中
[dmtsai@study ~]$ mkdir /tmp/upper; cp -a /etc/[^a-z]* /tmp/upper
```

​						特殊：

| 符号  | 内容                                                         |      |
| :---- | :----------------------------------------------------------- | ---- |
| #     | 注解符号：这个最常被使用在 script 当中，视为说明！在后的数据均不执行 |      |
| \     | 跳脱符号：将“特殊字符或万用字符”还原成一般字符               |      |
| \|    | 分隔两个管线命令的界定（后两节介绍）；                       |      |
| ;     | 连续指令下达分隔符号：连续性命令的界定 （注意！与管线命令并不相同） |      |
| ~     | 使用者的主文件夹                                             |      |
| $     | 取用变量前置字符：亦即是变量之前需要加的变量取代值           |      |
| &     | 工作控制 （job control）：将指令变成背景下工作               |      |
| !     | 逻辑运算意义上的“非” not 的意思！                            |      |
| /     | 目录符号：路径分隔的符号                                     |      |
| >, >> | 数据流重导向：输出导向，分别是“取代”与“累加”                 |      |
| <, << | 数据流重导向：输入导向 （这两个留待下节介绍）                |      |
| ' '   | 单引号，不具有变量置换的功能 （$ 变为纯文本）                |      |
| " "   | 具有变量置换的功能！ （$ 可保留相关功能）                    |      |
| ` `   | 两个“ ` ”中间为可以先执行的指令，亦可使用 $（ ）             |      |
| （ ） | 在中间为子 shell 的起始与结束                                |      |
| { }   | 在中间为命令区块的组合！                                     |      |





##### 				数据流重导向

- 标准输入　　（stdin） ：代码为 0 ，使用 < 或 << ；
- 标准输出　　（stdout）：代码为 1 ，使用 > 或 >> ；
- 标准错误输出（stderr）：代码为 2 ，使用 2> 或 2>> ；

```
范例一：观察你的系统根目录 （/） 下各目录的文件名、权限与属性，并记录下来
[dmtsai@study ~]$ ll /  &lt;==此时屏幕会显示出文件名信息
xxx
[dmtsai@study ~]$ ll / > ~/rootfile &lt;==屏幕并无任何信息
[dmtsai@study ~]$ ll  ~/rootfile &lt;==有个新文件被创建了！
-rw-rw-r--. 1 dmtsai dmtsai 1078 Jul  9 18:51 /home/dmtsai/rootfile
```



- 1> ：以覆盖的方法将“正确的数据”输出到指定的文件或设备上；
- 1>>：以累加的方法将“正确的数据”输出到指定的文件或设备上；
- 2> ：以覆盖的方法将“错误的数据”输出到指定的文件或设备上；
- 2>>：以累加的方法将“错误的数据”输出到指定的文件或设备上；

```
将 stdout 与 stderr 分存到不同的文件去
[dmtsai@study ~]$ find /home -name .bashrc > list_right 2> list_error
```



​						/dev/null 垃圾桶黑洞设备与特殊写法

​								可以吃掉任何导向这个设备的信息

```
[dmtsai@study ~]$ find /home -name .bashrc 2> /dev/null
/home/dmtsai/.bashrc  &lt;==只有 stdout 会显示到屏幕上， stderr 被丢弃了
```



​						将正确与错误的数据通通写入到同一个文件里

```
[dmtsai@study ~]$ find /home -name .bashrc > list 2> list  &lt;==错误
[dmtsai@study ~]$ find /home -name .bashrc > list 2>&1     &lt;==正确
[dmtsai@study ~]$ find /home -name .bashrc >> list         &lt;==正确
```





​				standard input ：<  和  <<     (将原本需要由键盘输入的数据，改由文件内容来取代)





​						举例：

使用>来创造文件

```
利用 cat 指令来创建一个文件的简单流程
[dmtsai@study ~]$ cat >catfile
testing
cat file test
&lt;==这里按下[ctrl]+d 来离开
[dmtsai@study ~]$ cat catfile
testing
cat file test
```

使用<来用文件的内容取代键盘输入

```
用 stdin 取代键盘的输入以创建新文件的简单流程
[dmtsai@study ~]$ cat >catfile   <~/.bashrc
[dmtsai@study ~]$ ll catfile ~/.bashrc
-rw-r--r--.1 dmtsai dmtsai 231Mar606:06/home/dmtsai/.bashrc
-rw-rw-r--.1 dmtsai dmtsai 231Jul918:58 catfile
# 注意看，这两个文件的大小会一模一样！几乎像是使用 cp 来复制一般！
```

使用<<后接“  ”，当输入到“  ''里面的内容时，结束退出

```
[dmtsai@study ~]$ cat >catfile <<"eof"
>Thisis a test.
> OK now stop
> eof           &lt;==输入这关键字，立刻就结束而不需要输入[ctrl]+d
[dmtsai@study ~]$ cat catfile
Thisis a test.
OK now stop     &lt;==只有这两行，不会存在关键字那一行！
```





##### 				命令执行的判断依据： ; , &&, ||



​						(  ;  )  :在指令与指令中间利用分号 （;） 来隔开

| cmd1;cmd2 | 分号前的指令执行完后就会立刻接着执行后面的指令了。 |
| --------- | -------------------------------------------------- |



​						(&&)  :

| cmd1 && cmd2 | 若 cmd1 执行完毕且正确执行，则开始执行 cmd2。 2. 若 cmd1 执行完毕且为错误 ，则 cmd2 不执行。 |
| ------------ | ------------------------------------------------------------ |



​						(||) :

| cmd1 |      | 若 cmd1 执行完毕且正确执行，则 cmd2 不执行。若 cmd1 执行完毕且为错误 ，则开始执行 cmd2。 |
| ---- | ---- | ------------------------------------------------------------ |





##### 				管线命令：|



​						管线命令“ | ”仅能处理经由前面一个指令传来的正确信息，也就是 standard output 的信息，对于 						stdandard error 并没有直接处理的能力。



- 管线命令仅会处理 standard output，对于 standard error output 会予以忽略
- 管线命令必须要能够接受来自前一个指令的数据成为 standard input 继续处理才行。



​								例如：

```
[dmtsai@study ~]$ ls -al /etc | less
```





##### 				撷取命令： cut, grep



​						cut： 将一段讯息的某一段给他“切”出来～ 处理的讯息是以“行”为单位

```
[dmtsai@study ~]$ cut -d'分隔字符' -f fields &lt;==用于有特定分隔字符
[dmtsai@study ~]$ cut -c 字符区间            &lt;==用于排列整齐的讯息
选项与参数：
-d  ：后面接分隔字符。与 -f 一起使用；
-f  ：依据 -d 的分隔字符将一段讯息分区成为数段，用 -f 取出第几段的意思；
-c  ：以字符 （characters） 的单位取出固定字符区间；
```



​						举例：

```
范例一：将 PATH 变量取出，我要找出第五个路径。
[dmtsai@study ~]$ echo ${PATH}
/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/home/dmtsai/.local/bin:/home/dmtsai/bin
#      1      | 2      |        3      |   4     |     5                 |      6       |
[dmtsai@study ~]$ echo ${PATH} |cut -d ':' -f 5
# 如同上面的数字显示，我们是以“ : ”作为分隔，因此会出现 /home/dmtsai/.local/bin
# 那么如果想要列出第 3 与第 5 呢？，就是这样：
[dmtsai@study ~]$ echo ${PATH} | cut -d ':' -f 3,5
范例二：将 export 输出的讯息，取得第 12 字符以后的所有字串
[dmtsai@study ~]$ export
declare -x HISTCONTROL="ignoredups"
declare -x HISTSIZE="1000"
declare -x HOME="/home/dmtsai"
declare -x HOSTNAME="study.centos.vbird"
.....（其他省略）.....
# 注意看，每个数据都是排列整齐的输出！如果我们不想要“ declare -x ”时，就得这么做：
[dmtsai@study ~]$ export |cut -c 12-
HISTCONTROL="ignoredups"
HISTSIZE="1000"
HOME="/home/dmtsai"
HOSTNAME="study.centos.vbird"
.....（其他省略）.....
# 知道怎么回事了吧？用 -c 可以处理比较具有格式的输出数据！
# 我们还可以指定某个范围的值，例如第 12-20 的字符，就是 cut -c 12-20 等等！
范例三：用 last 将显示的登陆者的信息中，仅留下使用者大名
[dmtsai@study ~]$ last
root   pts/1    192.168.201.101  Sat Feb  7 12:35   still logged in
root   pts/1    192.168.201.101  Fri Feb  6 12:13 - 18:46  （06:33）
root   pts/1    192.168.201.254  Thu Feb  5 22:37 - 23:53  （01:16）
# last 可以输出“帐号/终端机/来源/日期时间”的数据，并且是排列整齐的
[dmtsai@study ~]$ last |cut -d ' ' -f 1
# 由输出的结果我们可以发现第一个空白分隔的字段代表帐号，所以使用如上指令：
# 但是因为 root   pts/1 之间空格有好几个，并非仅有一个，所以，如果要找出 
# pts/1 其实不能以 cut -d ' ' -f 1,2 喔！输出的结果会不是我们想要的。
```



​				grep：分析一行讯息， 若当中有我们所需要的信息，就将该行拿出来

```
[dmtsai@study ~]$ grep [-acinv] [--color=auto] '搜寻字串' filename
选项与参数：
-a ：将 binary 文件以 text 文件的方式搜寻数据
-c ：计算找到 '搜寻字串' 的次数
-i ：忽略大小写的不同，所以大小写视为相同
-n ：顺便输出行号
-v ：反向选择，亦即显示出没有 '搜寻字串' 内容的那一行！
--color=auto ：可以将找到的关键字部分加上颜色的显示喔！
```



```
在 last 的输出讯息中，只要有 root 就取出，并且仅取第一栏
[dmtsai@study ~]$ last |grep 'root' |cut -d ' ' -f1
# 在取出 root 之后，利用上个指令 cut 的处理，就能够仅取得第一栏啰！
范例四：取出 /etc/man_db.conf 内含 MANPATH 的那几行
[dmtsai@study ~]$ grep --color=auto 'MANPATH' /etc/man_db.conf
....（前面省略）....
MANPATH_MAP     /usr/games              /usr/share/man
MANPATH_MAP     /opt/bin                /opt/man
MANPATH_MAP     /opt/sbin               /opt/man
# 神奇的是，如果加上 --color=auto 的选项，找到的关键字部分会用特殊颜色显示
```





##### 				排序命令： sort, wc, uniq	



​						sort：  （建议用LANG=C 来让语系统一）

```
[dmtsai@study ~]$ sort [-fbMnrtuk] [file or stdin]
选项与参数：
-f  ：忽略大小写的差异，例如 A 与 a 视为编码相同；
-b  ：忽略最前面的空白字符部分；
-M  ：以月份的名字来排序，例如 JAN, DEC 等等的排序方法；
-n  ：使用“纯数字”进行排序（默认是以文字体态来排序的）；
-r  ：反向排序；
-u  ：就是 uniq ，相同的数据中，仅出现一行代表；
-t  ：分隔符号，默认是用 [tab] 键来分隔；
-k  ：以那个区间 （field） 来进行排序的意思
```



​						uniq：    重复的行删除掉只显示一个        （多配合排序过的文件来处理）

```
[dmtsai@study ~]$ uniq [-ic]
选项与参数：
-i  ：忽略大小写字符的不同；
-c  ：进行计数
```



​						wc：     查看文件内有多少字？多少行？多少字符

```
[dmtsai@study ~]$ wc [-lwm]
选项与参数：
-l  ：仅列出行；
-w  ：仅列出多少字（英文单字）；
-m  ：多少字符；
```





##### 				双向重导向:tee

```
[dmtsai@study ~]$ tee [-a] file
选项与参数：
-a  ：以累加 （append） 的方式，将数据加入 file 当中！
```



```
[dmtsai@study ~]$ last |tee last.list |cut -d " " -f1
# 这个范例可以让我们将 last 的输出存一份到 last.list 文件中；
[dmtsai@study ~]$ ls -l /home |tee ~/homefile |more
# 这个范例则是将 ls 的数据存一份到 ~/homefile ，同时屏幕也有输出讯息！
[dmtsai@study ~]$ ls -l / |tee -a ~/homefile |more
# 要注意！ tee 后接的文件会被覆盖，若加上 -a 这个选项则能将讯息累加。
```



##### 				字符转换命令： tr, col, join, paste, expand



​						tr：可以用来删除一段讯息当中的文字，或者是进行文字讯息的替换！

```
[dmtsai@study ~]$ tr [-ds] SET1 ...
选项与参数：
-d  ：删除讯息当中的 SET1 这个字串；
-s  ：取代掉重复的字符！
```



```
范例一：将 last 输出的讯息中，所有的小写变成大写字符：
[dmtsai@study ~]$ last |tr '[a-z]' '[A-Z]'
# 事实上，没有加上单引号也是可以执行的，如：“ last |tr [a-z] [A-Z] ”
范例二：将 /etc/passwd 输出的讯息中，将冒号 （:） 删除
[dmtsai@study ~]$ cat /etc/passwd |tr -d ':'
范例三：将 /etc/passwd 转存成 dos 断行到 /root/passwd 中，再将 ^M 符号删除
[dmtsai@study ~]$ cp /etc/passwd ~/passwd && unix2dos ~/passwd
[dmtsai@study ~]$ file /etc/passwd ~/passwd
/etc/passwd:         ASCII text
/home/dmtsai/passwd: ASCII text, with CRLF line terminators  &lt;==就是 DOS 断行
[dmtsai@study ~]$ cat ~/passwd |tr -d '\r' > ~/passwd.linux
# 那个 \r 指的是 DOS 的断行字符，关于更多的字符，请参考 man tr
[dmtsai@study ~]$ ll /etc/passwd ~/passwd*
-rw-r--r--. 1 root   root   2092 Jun 17 00:20 /etc/passwd
-rw-r--r--. 1 dmtsai dmtsai 2133 Jul  9 22:13 /home/dmtsai/passwd
-rw-rw-r--. 1 dmtsai dmtsai 2092 Jul  9 22:13 /home/dmtsai/passwd.linux
# 处理过后，发现文件大小与原本的 /etc/passwd 就一致了！
```





​				col：				将[Tab]转换为空白键

```
[dmtsai@study ~]$ col [-xb]
选项与参数：
-x  ：将 tab 键转换成对等的空白键
```



​				join：		两个文件当中，有 **"相同数据"** 的那一行，将他加在一起  （最好先事先排序）

```
[dmtsai@study ~]$ join [-ti12] file1 file2
选项与参数：
-t  ：join 默认以空白字符分隔数据，并且比对“第一个字段”的数据，
      如果两个文件相同，则将两笔数据联成一行，且第一个字段放在第一个！
-i  ：忽略大小写的差异；
-1  ：这个是数字的 1 ，代表“第一个文件要用那个字段来分析”的意思；
-2  ：代表“第二个文件要用那个字段来分析”的意思。
```





​				paste：    将两行贴在一起，且中间以 [tab] 键隔开

```
[dmtsai@study ~]$ paste [-d] file1 file2
选项与参数：
-d  ：后面可以接分隔字符。默认是以 [tab] 来分隔的！
-   ：如果 file 部分写成 - ，表示来自 standard input 的数据的意思。
```





​				expand：	将 [tab] 按键转成空白键

```
[dmtsai@study ~]$ expand [-t] file
选项与参数：
-t  ：后面可以接数字。一般来说，一个 tab 按键可以用 8 个空白键取代。
      我们也可以自行定义一个 [tab] 按键代表多少个字符呢！
```





​				分区命令： split

```
[dmtsai@study ~]$ split [-bl] file PREFIX
选项与参数：
-b  ：后面可接欲分区成的文件大小，可加单位，例如 b, k, m 等；
-l  ：以行数来进行分区。
PREFIX ：代表前置字符的意思，可作为分区文件的前导文字。
```



```
范例一：我的 /etc/services 有六百多K，若想要分成 300K 一个文件时？
[dmtsai@study ~]$ cd /tmp; split -b 300k /etc/services services
[dmtsai@study tmp]$ ll -k services*
-rw-rw-r--. 1 dmtsai dmtsai 307200 Jul  9 22:52 servicesaa
-rw-rw-r--. 1 dmtsai dmtsai 307200 Jul  9 22:52 servicesab
-rw-rw-r--. 1 dmtsai dmtsai  55893 Jul  9 22:52 servicesac
# 那个文件名可以随意取的啦！我们只要写上前导文字，小文件就会以
# xxxaa, xxxab, xxxac 等方式来创建小文件的！
范例二：如何将上面的三个小文件合成一个文件，文件名为 servicesback
[dmtsai@study tmp]$ cat services* >>servicesback
# 很简单吧？就用数据流重导向就好啦！简单！
范例三：使用 ls -al / 输出的信息中，每十行记录成一个文件
[dmtsai@study tmp]$ ls -al / |split -l 10 - lsroot
[dmtsai@study tmp]$ wc -l lsroot*
  10 lsrootaa
  10 lsrootab
   4 lsrootac
  24 total
# 重点在那个 - 啦！一般来说，如果需要 stdout/stdin 时，但偏偏又没有文件，
# 有的只是 - 时，那么那个 - 就会被当成 stdin 或 stdout ～
```



​				参数代换： xargs      (不支持管线的指令使用)

```
[dmtsai@study ~]$ xargs [-0epn] command
选项与参数：
-0  ：如果输入的 stdin 含有特殊字符，例如 `, \, 空白键等等字符时，这个 -0 参数
      可以将他还原成一般字符。这个参数可以用于特殊状态喔！
-e  ：这个是 EOF （end of file） 的意思。后面可以接一个字串，当 xargs 分析到这个字串时，
      就会停止继续工作！
-p  ：在执行每个指令的 argument 时，都会询问使用者的意思；
-n  ：后面接次数，每次 command 指令执行时，要使用几个参数的意思。
当 xargs 后面没有接任何的指令时，默认是以 echo 来进行输出喔！
```



```
找出 /usr/sbin 下面具有特殊权限的文件名，并使用 ls -l 列出详细属性
[dmtsai@study ~]$ find /usr/sbin -perm /7000 |xargs ls -l
-rwx--s--x. 1 root lock      11208 Jun 10  2014 /usr/sbin/lockdev
-rwsr-xr-x. 1 root root     113400 Mar  6 12:17 /usr/sbin/mount.nfs
-rwxr-sr-x. 1 root root      11208 Mar  6 11:05 /usr/sbin/netreport
.....（下面省略）.....
# 聪明的读者应该会想到使用“ ls -l $（find /usr/sbin -perm /7000） ”来处理这个范例！
# 都 OK！能解决问题的方法，就是好方法！
```





​				减号 - 的用途

```
[root@study ~]# mkdir /tmp/homeback
[root@study ~]# tar -cvf - /home |tar -xvf - -C /tmp/homeback
```

​						将 /home 里面的文件给他打包，但打包的数据不是纪录到文件，而是传送到 stdout；

​                        经过管线后，将 tar -cvf - /home 传送给后面的 tar -xvf -。

​						后面的这个 - 则是取用前一个指令的 stdout， 因此，我们就不需要使用 filename 了





## 七.正则表达式





##### 				语系：

 						LANG=C 时：0 1 2 3 4 … A B C D … Z a b c d …z

​				

| 特殊符号      | 代表意义                                                     |
| ------------- | ------------------------------------------------------------ |
| **[:alnum:]** | 代表英文大小写字符及数字，亦即 0-9, A-Z, a-z                 |
| **[:alpha:]** | 代表任何英文大小写字符，亦即 A-Z, a-z                        |
| [:blank:]     | 代表空白键与 [Tab] 按键两者                                  |
| [:cntrl:]     | 代表键盘上面的控制按键，亦即包括 CR, LF, Tab, Del.. 等等     |
| **[:digit:]** | 代表数字而已，亦即 0-9                                       |
| [:graph:]     | 除了空白字符 （空白键与 [Tab] 按键） 外的其他所有按键        |
| **[:lower:]** | 代表小写字符，亦即 a-z                                       |
| [:print:]     | 代表任何可以被打印出来的字符                                 |
| [:punct:]     | 代表标点符号 （punctuation symbol），亦即：" ' ? ! ; : # $... |
| **[:upper:]** | 代表大写字符，亦即 A-Z                                       |
| [:space:]     | 任何会产生空白的字符，包括空白键, [Tab], CR 等等             |
| [:xdigit:]    | 代表 16 进位的数字类型，因此包括： 0-9, A-F, a-f 的数字与字符 |





##### 				grep进阶：

```
[dmtsai@study ~]$ grep [-A] [-B] [--color=auto] '搜寻字串' filename
选项与参数：

-A ：后面可加数字，为 after 的意思，除了列出该行外，后续的 n 行也列出来；
-B ：后面可加数字，为 befer 的意思，除了列出该行外，前面的 n 行也列出来；

-a ：将 binary 文件以 text 文件的方式搜寻数据
-c ：计算找到 '搜寻字串' 的次数
-i ：忽略大小写的不同，所以大小写视为相同
-n ：顺便输出行号
-v ：反向选择，亦即显示出没有 '搜寻字串' 内容的那一行！
--color=auto 可将正确的那个撷取数据列出颜色
```



##### 				利用[]来搜寻集合字符:

```
grep -n 't[ae]st' regular_express.txt
```



​				任意一个字符 . 与重复字符:

​						. （小数点）：代表“一定有一个任意字符”的意思

​						*（星星号）：代表“重复前一个字符， 0 到无穷多次”的意思，为组合形态





##### 				限定连续RE字符范围：{}



​						寻找两个o的字符			（	{}在shell中是有特殊意义的，因此要使用跳脱字符	\	）

```
grep -n 'o\{2\}' regular_express.txt
```





##### 				基础正则表达式字符汇整：

| RE 字符 | 意义与范例                                                   |
| :------ | :----------------------------------------------------------- |
| ^word   | 意义：待搜寻的字串（word）在行首！范例：搜寻行首为 # 开始的那一行，并列出行号 `> grep -n '^#' regular_express.txt` |
| word$   | 意义：待搜寻的字串（word）在行尾！范例：将行尾为 ! 的那一行打印出来，并列出行号 `> grep -n '!$' regular_express.txt` |
| .       | 意义：代表“一定有一个任意字符”的字符！范例：搜寻的字串可以是 （eve） （eae） （eee） （e e）， 但不能仅有 （ee） ！亦即 e 与 e 中间“一定”仅有一个字符，而空白字符也是字符！ `> grep -n 'e.e' regular_express.txt` |
| \       | 意义：跳脱字符，将特殊符号的特殊意义去除！范例：搜寻含有单引号 ' 的那一行！ `> grep -n \' regular_express.txt` |
| *       | 意义：重复零个到无穷多个的前一个 RE 字符 范例：找出含有 （es） （ess） （esss） 等等的字串，注意，因为 *可以是 0 个，所以 es 也是符合带搜寻字串。另外，因为* 为重复“前一个 RE 字符”的符号， 因此，在 *之前必须要紧接着一个 RE 字符喔！例如任意字符则为 “.*” ！ `> grep -n 'ess*' regular_express.txt` |
| [list]  | 意义：字符集合的 RE 字符，里面列出想要撷取的字符！范例：搜寻含有 （gl） 或 （gd） 的那一行，需要特别留意的是，在 [] 当中“谨代表一个待搜寻的字符”， 例如“ a[afl]y ”代表搜寻的字串可以是 aay, afy, aly 即 [afl] 代表 a 或 f 或 l 的意思！ `> grep -n 'g[ld]' regular_express.txt` |
| [n1-n2] | 意义：字符集合的 RE 字符，里面列出想要撷取的字符范围！范例：搜寻含有任意数字的那一行！需特别留意，在字符集合 [] 中的减号 - 是有特殊意义的，他代表两个字符之间的所有连续字符！但这个连续与否与 ASCII 编码有关，因此，你的编码需要设置正确（在 bash 当中，需要确定 LANG 与 LANGUAGE 的变量是否正确！） 例如所有大写字符则为 [A-Z] `> grep -n '[A-Z]' regular_express.txt` |
| [^list] | 意义：字符集合的 RE 字符，里面列出不要的字串或范围！范例：搜寻的字串可以是 （oog） （ood） 但不能是 （oot） ，那个 ^ 在 [] 内时，代表的意义是“反向选择”的意思。 例如，我不要大写字符，则为 [^A-Z]。但是，需要特别注意的是，如果以 grep -n [^A-Z] regular_express.txt 来搜寻，却发现该文件内的所有行都被列出，为什么？因为这个 [^A-Z] 是“非大写字符”的意思， 因为每一行均有非大写字符，例如第一行的 "Open Source" 就有 p,e,n,o…. 等等的小写字 `> grep -n 'oo[^t]' regular_express.txt` |
| {n,m}   | 意义：连续 n 到 m 个的“前一个 RE 字符” 意义：若为 {n} 则是连续 n 个的前一个 RE 字符， 意义：若是 {n,} 则是连续 n 个以上的前一个 RE 字符！ 范例：在 g 与 g 之间有 2 个到 3 个的 o 存在的字串，亦即 （goog）（gooog） `> grep -n 'go{2,3}g' regular_express.txt` |





##### 				sed工具



​				sed本身也是一个管线命令，以下是sed的一些用法：

​				（取代、删除、新增、撷取）

```
[dmtsai@study ~]$ sed [-nefr] [动作]
选项与参数：
-n  ：使用安静（silent）模式。在一般 sed 的用法中，所有来自 STDIN 的数据一般都会被列出到屏幕上。
      但如果加上 -n 参数后，则只有经过 sed 特殊处理的那一行（或者动作）才会被列出来。
-e  ：直接在命令行界面上进行 sed 的动作编辑；
-f  ：直接将 sed 的动作写在一个文件内， -f filename 则可以执行 filename 内的 sed 动作；
-r  ：sed 的动作支持的是延伸型正则表达式的语法。（默认是基础正则表达式语法）
-i  ：直接修改读取的文件内容，而不是由屏幕输出。
动作说明：  [n1[,n2]]function
n1, n2 ：不见得会存在，一般代表“选择进行动作的行数”，举例来说，如果我的动作
         是需要在 10 到 20 行之间进行的，则“ 10,20[动作行为] ”
function 有下面这些咚咚：
a   ：新增， a 的后面可以接字串，而这些字串会在新的一行出现（目前的下一行）～
c   ：取代， c 的后面可以接字串，这些字串可以取代 n1,n2 之间的行！
d   ：删除，因为是删除啊，所以 d 后面通常不接任何咚咚；		￥为最后一行
i   ：插入， i 的后面可以接字串，而这些字串会在新的一行出现（目前的上一行）；
p   ：打印，亦即将某个选择的数据印出。通常 p 会与参数 sed -n 一起运行～
s   ：取代，可以直接进行取代的工作哩！通常这个 s 的动作可以搭配正则表达式！
      例如 1,20s/old/new/g 就是啦！
```



​						删：

```
nl /etc/passwd |sed '2,5d'
```



​						添：		(行后)

```
nl /etc/passwd |sed '2a drink tea'
```

​										(行前)

```
nl /etc/passwd |sed '2i drink tea' 
```

​										(添两行)

```
nl /etc/passwd |sed '2a Drink tea or ......\
drink beer ?'
```



​						取：		(整行取代)

```
nl /etc/passwd |sed '2,5c No 2-5 number'
```

​										(行号取代)

```
nl /etc/passwd |sed -n '5,7p'
```

​						

​						查：			(部分搜寻并取代)

```
sed 's/要被取代的字串/新的字串/g'
```





##### 				延伸型正则表达式

| RE 字符 | 意义与范例                                                   |      |
| :------ | :----------------------------------------------------------- | ---- |
| +       | 意义：重复“一个或一个以上”的前一个 RE 字符 范例：搜寻 （god） （good） （goood）… 等等的字串。 那个 o+ 代表“一个以上的 o ”所以，下面的执行成果会将第 1, 9, 13 行列出来。 `> egrep -n 'go+d' regular_express.txt` |      |
| ?       | 意义：“零个或一个”的前一个 RE 字符 范例：搜寻 （gd） （god） 这两个字串。 那个 o? 代表“空的或 1 个 o ”所以，上面的执行成果会将第 13, 14 行列出来。 有没有发现到，这两个案例（ 'go+d' 与 'go?d' ）的结果集合与 'go*d' 相同？ 想想看，这是为什么喔！ ^_^ `> egrep -n 'go?d' regular_express.txt` |      |
| \|      | 意义：用或（ or ）的方式找出数个字串 范例：搜寻 gd 或 good 这两个字串，注意，是“或”！ 所以，第 1,9,14 这三行都可以被打印出来喔！那如果还想要找出 dog 呢？ `> egrep -n 'gd|good' regular_express.txt` `> egrep -n 'gd|good|dog' regular_express.txt` |      |
| （）    | 意义：找出“群组”字串 范例：搜寻 （glad） 或 （good） 这两个字串，因为 g 与 d 是重复的，所以， 我就可以将 la 与 oo 列于 （ ） 当中，并以来分隔开来，就可以啦！ `> egrep -n 'g（la|oo）d' regular_express.txt` |      |
| （）+   | 意义：多个重复群组的判别 范例：将“AxyzxyzxyzxyzC”用 echo 叫出，然后再使用如下的方法搜寻一下！ `> echo 'AxyzxyzxyzxyzC' | egrep 'A（xyz）+C'` |      |





##### 				文件的格式化与相关处理

```
[dmtsai@study ~]$ printf '打印格式' 实际内容
选项与参数：
关于格式方面的几个特殊样式：
       \a    警告声音输出
       \b    倒退键（backspace）
       \f    清除屏幕 （form feed）
       \n    输出新的一行
       \r    亦即 Enter 按键
       \t    水平的 [tab] 按键
       \v    垂直的 [tab] 按键
       \xNN  NN 为两位数的数字，可以转换数字成为字符。
关于 C 程序语言内，常见的变量格式
       %ns   那个 n 是数字， s 代表 string ，亦即多少个字符；
       %ni   那个 n 是数字， i 代表 integer ，亦即多少整数码数；
       %N.nf 那个 n 与 N 都是数字， f 代表 floating （浮点），如果有小数码数，
             假设我共要十个位数，但小数点有两位，即为 %10.2f 
```



​						固定字段长度操作：

```
[dmtsai@study ~]$ printf '%10s %5i %5i %5i %8.2f \n' $（cat printf.txt &#124; grep -v Name）
    DmTsai    80    60    92    77.33
     VBird    75    55    80    70.00
       Ken    60    90    70    73.33
```

​																		(%8.2f代表长度为八个字符的具有小数点的字段，小数点表示字符宽度)



​						十六进位数值表示：

```
[dmtsai@study ~]$ printf '\x45\n'
E
# 可以将数值转换成为字符
```





##### 				数据处理工具awk



​						运行模式：

```
[dmtsai@study ~]$ awk '条件类型1{动作1} 条件类型2{动作2} ...' filename
```



​						运行步骤：

- 读入第一行，并将第一行的数据填入 $0, $1, $2…. 等变量当中；
- 依据 "条件类型" 的限制，判断是否需要进行后面的 "动作"；
- 做完所有的动作与条件类型；
- 若还有后续的“行”的数据，则重复上面 1~3 的步骤，直到所有的数据都读完为止。



​						内置变量：

| 变量名称 | 代表意义                        |
| :------- | :------------------------------ |
| NF       | 每一行 （$0） 拥有的字段总数    |
| NR       | 目前 awk 所处理的是“第几行”数据 |
| FS       | 目前的分隔字符，默认是空白键    |



​						awk 的逻辑运算字符：

| 运算单元 | 代表意义   |
| :------- | :--------- |
| >        | 大于       |
| <        | 小于       |
| >=       | 大于或等于 |
| <=       | 小于或等于 |
| ==       | 等于       |
| !=       | 不等于     |



​			

##### 				文件比对工具



​						diff：以行为单位，对比两个文件的差距

```
[dmtsai@study ~]$ diff [-bBi] from-file to-file
选项与参数：
from-file ：一个文件名，作为原始比对文件的文件名；
to-file   ：一个文件名，作为目的比对文件的文件名；
注意，from-file 或 to-file 可以 - 取代，那个 - 代表“Standard input”之意。
-b  ：忽略一行当中，仅有多个空白的差异（例如 "about me" 与 "about     me" 视为相同
-B  ：忽略空白行的差异。
-i  ：忽略大小写的不同。

```



​						cmp：以字节为单位，对比两个文件的差距

```
[dmtsai@study ~]$ cmp [-l] file1 file2
选项与参数：
-l  ：将所有的不同点的字节处都列出来。因为 cmp 默认仅会输出第一个发现的不同点。
```





​						patch：将旧的文件升级成新的文件

```
[dmtsai@study ~]$ patch -R -pN > patch_file &lt;==还原
选项与参数：
-p  ：后面可以接“取消几层目录”的意思。
-R  ：代表还原，将新的文件还原成原来旧的版本。
```





##### 				文件打印准备：pr

```
[dmtsai@study ~]$ pr /etc/man_db.conf

2014-06-10 05:35                 /etc/man_db.conf                 Page 1
```

​																标题中会有“文件时间”、“文件文件名”及“页码”三大项目





## 八.Shell Scripts



##### 												写法格式：

- 第一行 #!/bin/bash 在宣告这个 script 使用的 shell 名称：因为我们使用的是 bash ，所以，必须要以“ **#!/bin/bash** ”来宣告这个文件内的语法使用 bash 的语法

- 程序内容的说明：整个 script 当中，除了第一行的“ #! ”是用来宣告 shell 的之外，其他的 # 都是“注解”用途！ 

- 主要环境变量的宣告：要将一些重要的环境变量设置好， PATH 与 LANG （如果有使用到输出相关的信息时） 是当中最重要的

  `PATH=/bin:/sbin:/usr/bin:/usr/sbin:/usr/local/bin:/usr/local/sbin:~/bin`

- 主要程序部分就将主要的程序写好即可！在这个例子当中，就是 echo 那一行啦！

- 执行成果告知 （定义回传值）



##### 												任意精度计算bc：

```
-i：强制进入交互式模式；

-l：定义使用的标准数学库

-w：对POSIX bc的扩展给出警告信息；

-q：不打印正常的GNU bc环境信息；

-v：显示指令版本信息；

-h：显示指令的帮助信息。
```





##### 												script 的执行方式差异



​						直接执行的方式来执行script：

​									该script使用新的bash环境来执行脚本内的指令，其实 script 是在子程序的 bash 内执行的

![image-20210623202334384](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\image-20210623202334384.png)







​						利用source来执行script：

​								在父程序中执行

![image-20210623202348291](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\image-20210623202348291.png)





##### 								判断式



​						利用test指令测试功能：

```
[dmtsai@study ~]$ test -e /filename && echo "exist" ||echo "Not exist"

Not exist  &lt;==结果显示不存在啊！
```



​						测试标志：

| 测试的标志                                                   | 代表意义                                                     |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| 1. 关于某个文件名的“文件类型”判断，如 test -e filename 表示存在否 |                                                              |
| **-e**                                                       | **该“文件名”是否存在？（常用）**                             |
| **-f**                                                       | **该“文件名”是否存在且为文件（file）？（常用）**             |
| **-d**                                                       | **该“文件名”是否存在且为目录（directory）？（常用）**        |
| -b                                                           | 该“文件名”是否存在且为一个 block device 设备？               |
| -c                                                           | 该“文件名”是否存在且为一个 character device 设备？           |
| -S                                                           | 该“文件名”是否存在且为一个 Socket 文件？                     |
| -p                                                           | 该“文件名”是否存在且为一个 FIFO （pipe） 文件？              |
| -L                                                           | 该“文件名”是否存在且为一个链接文件？                         |
| 2. 关于文件的权限侦测，如 test -r filename 表示可读否 （但 root 权限常有例外） |                                                              |
| -r                                                           | 侦测该文件名是否存在且具有“可读”的权限？                     |
| -w                                                           | 侦测该文件名是否存在且具有“可写”的权限？                     |
| -x                                                           | 侦测该文件名是否存在且具有“可执行”的权限？                   |
| -u                                                           | 侦测该文件名是否存在且具有“SUID”的属性？                     |
| -g                                                           | 侦测该文件名是否存在且具有“SGID”的属性？                     |
| -k                                                           | 侦测该文件名是否存在且具有“Sticky bit”的属性？               |
| -s                                                           | 侦测该文件名是否存在且为“非空白文件”？                       |
| 3. 两个文件之间的比较，如： test file1 -nt file2             |                                                              |
| -nt                                                          | （newer than）判断 file1 是否比 file2 新                     |
| -ot                                                          | （older than）判断 file1 是否比 file2 旧                     |
| -ef                                                          | 判断 file1 与 file2 是否为同一文件，可用在判断 hard link 的判定上。 主要意义在判定，两个文件是否均指向同一个 inode 哩！ |
| 4. 关于两个整数之间的判定，例如 test n1 -eq n2               |                                                              |
| -eq                                                          | 两数值相等 （equal）                                         |
| -ne                                                          | 两数值不等 （not equal）                                     |
| -gt                                                          | n1 大于 n2 （greater than）                                  |
| -lt                                                          | n1 小于 n2 （less than）                                     |
| -ge                                                          | n1 大于等于 n2 （greater than or equal）                     |
| -le                                                          | n1 小于等于 n2 （less than or equal）                        |
| 5. 判定字串的数据                                            |                                                              |
| test -z string                                               | 判定字串是否为 0 ？若 string 为空字串，则为 true             |
| test -n string                                               | 判定字串是否非为 0 ？若 string 为空字串，则为 false。 -n 亦可省略 |
| test str1 == str2                                            | 判定 str1 是否等于 str2 ，若相等，则回传 true                |
| test str1 != str2                                            | 判定 str1 是否不等于 str2 ，若相等，则回传 false             |
| 6. 多重条件判定，例如： test -r filename -a -x filename      |                                                              |
| -a                                                           | （and）两状况同时成立！例如 test -r file -a -x file，则 file 同时具有 r 与 x 权限时，才回传 true。 |
| -o                                                           | （or）两状况任何一个成立！例如 test -r file -o -x file，则 file 具有 r 或 x 权限时，就可回传 true。 |
| !                                                            | 反相状态，如 test ! -x file ，当 file 不具有 x 时，回传 true |





​				判断符号：[]			（与test用法一致）

​				

```
[dmtsai@study ~]$ [-z "${HOME}"]; echo $?
```



​						注意事项：



- 在中括号 [] 内的每个元件都需要有空白键来分隔；
- 在中括号内的变量，最好都以双引号括号起来；
- 在中括号内的常数，最好都以单或双引号括号起来。



​				

##### 								Shell script 的默认变量（$0, $1…）



- $# ：代表后接的参数“个数”，以上表为例这里显示为“ 4 ”；
- $@ ：代表“ "$1" "$2" "$3" "$4" ”之意，每个变量是独立的（用双引号括起来）；
- $ *：代表“ "$1c$2c$3c$4" ”，其中 c 为分隔字符，默认为空白键， 所以本例中代表“ "$1 $2 $3 $4" ”之意。
  那个 $@ 与 $* 基本上还是有所不同啦！不过，一般使用情况下可以直接记忆 $@ 即可！ 好了，来做个例子吧～假设我要执行一个可以携带参数的 script ，执行该脚本后屏幕会显示如下的数据：
- 程序的文件名为何？
- 共有几个参数？
- 若参数的个数小于 2 则告知使用者参数数量太少
- 全部的参数内容为何？
- 第一个参数为何？
- 第二个参数为何





##### 				shift：参数变量号码偏移

​				

​						例：

```
[dmtsai@study bin]$ vim shift_paras.sh
#!/bin/bash
# Program:
#    Program shows the effect of shift function.
# History:
# 2009/02/17    VBird    First release
PATH=/bin:/sbin:/usr/bin:/usr/sbin:/usr/local/bin:/usr/local/sbin:~/bin
export PATH
echo "Total parameter number is ==> $#"
echo "Your whole parameter is   ==> '$@'"
shift   # 进行第一次“一个变量的 shift ”
echo "Total parameter number is ==> $#"
echo "Your whole parameter is   ==> '$@'"
shift 3# 进行第二次“三个变量的 shift ”
echo "Total parameter number is ==> $#"
echo "Your whole parameter is   ==> '$@'"
```



​						结果：

```
[dmtsai@study bin]$ sh shift_paras.sh one two three four five six &lt;==给予六个参数
Total parameter number is==>6     &lt;==最原始的参数变量情况
Your whole parameter is==>'one two three four five six'
Total parameter number is==>5     &lt;==第一次偏移，看下面发现第一个 one 不见了
Your whole parameter is==>'two three four five six'
Total parameter number is==>2     &lt;==第二次偏移掉三个，two three four 不见了
```





##### 				条件判断式



​						if....then：

```
f [ 条件判断式一 ]; then
    当条件判断式成立时，可以进行的指令工作内容；
else [ 条件判断式二 ]; then
	当条件判断式二成立时，可以进行的指令工作内容
fi   （结束 if 的意思）
```

- && 代表 and；
- || 代表 or ；



​		

​						netstat：    查询目前主机打开的网络服务端口



```
netstat -tuln   （获取目前主机所有启动的服务）

-a或--all 显示所有连线中的Socket。
-A<网络类型>或--<网络类型> 列出该网络类型连线中的相关地址。
-c或--continuous 持续列出网络状态。
-C或--cache 显示路由器配置的快取信息。
-e或--extend 显示网络其他相关信息。
-F或--fib 显示路由缓存。
-g或--groups 显示多重广播功能群组组员名单。
-h或--help 在线帮助。
-i或--interfaces 显示网络界面信息表单。
-l或--listening 显示监控中的服务器的Socket。
-M或--masquerade 显示伪装的网络连线。
-n或--numeric 直接使用IP地址，而不通过域名服务器。
-N或--netlink或--symbolic 显示网络硬件外围设备的符号连接名称。
-o或--timers 显示计时器。
-p或--programs 显示正在使用Socket的程序识别码和程序名称。
-r或--route 显示Routing Table。
-s或--statistics 显示网络工作信息统计表。
-t或--tcp 显示TCP传输协议的连线状况。
-u或--udp 显示UDP传输协议的连线状况。
-v或--verbose 显示指令执行过程。
-V或--version 显示版本信息。
-w或--raw 显示RAW传输协议的连线状况。
-x或--unix 此参数的效果和指定"-A unix"参数相同。
--ip或--inet 此参数的效果和指定"-A inet"参数相同。
```



​						case.....esac		（多个既定变量的判断）

```
case  $变量名称 in   	&lt;==关键字为 case ，还有变量前有钱字号
  "第一个变量内容"）   	 &lt;==每个变量内容建议用双引号括起来，关键字则为小括号 ）
    程序段
    ;;                &lt;==每个类别结尾使用两个连续的分号来处理！
  "第二个变量内容"）
    程序段
    ;;
  *）                  &lt;==最后一个变量内容都会用 * 来代表所有其他值
    不包含第一个变量内容与第二个变量内容的其他程序执行段
    exit 1
    ;;
esac                  &lt;==最终的 case 结尾
```



​							￥变量    的两种取得方式：

- 直接下达式：直接给予 $1 这个变量的内容，这也是在 /etc/init.d 目录下大多数程序的设计方式。
- 互动式：通过 read 这个指令来让使用者输入变量的内容。





​						

​						function：自订执行指令

```
function fname（） {
    程序段
}，
```





##### 				循环式



​						while do done, until do done （不定循环）：

​			

​			while：成立执行

```
while [ condition ]  &lt;==中括号内的状态就是判断式
do            &lt;==do 是循环的开始！
    程序段落
done          &lt;==done 是循环的结束
```



​			until：不成立执行

```
until [ condition ]
do
    程序段落
done
```





​						for…do…done （固定循环）：已知多少次循环

​			第一种写法

```
for var in con1 con2 con3 ...（seq 1 100）  #循环一百次   也可使用{1..100}
do
    程序段
done
```



​			第二种写法



```
for （（ 初始值; 限制值; 执行步阶 ））
do
    程序段
done
```

- 初始值：某个变量在循环当中的起始值，直接以类似 i=1 设置好；
- 限制值：当变量的值在这个限制值的范围内，就继续进行循环。例如 i<=100；
- 执行步阶：每作一次循环时，变量的变化量。例如 i=i+1。







##### 				shell script 的追踪与 debug

```
[dmtsai@study ~]$ sh [-nvx] scripts.sh
选项与参数：
-n  ：不要执行 script，仅查询语法的问题；
-v  ：再执行 sccript 前，先将 scripts 的内容输出到屏幕上；
-x  ：将使用到的 script 内容显示到屏幕上，这是很有用的参数


范例一：测试 dir_perm.sh 有无语法的问题？
[dmtsai@study ~]$ sh -n dir_perm.sh 
# 若语法没有问题，则不会显示任何信息！
范例二：将 show_animal.sh 的执行过程全部列出来
[dmtsai@study ~]$ sh -x show_animal.sh 
+ PATH=/bin:/sbin:/usr/bin:/usr/sbin:/usr/local/bin:/usr/local/sbin:/root/bin
+ export PATH
+ for animal in dog cat elephant
+ echo 'There are dogs.... '
There are dogs....
+ for animal in dog cat elephant
+ echo 'There are cats.... '
There are cats....
+ for animal in dog cat elephant
+ echo 'There are elephants.... '
There are elephants....
```









## 九、Linux账号管理与ACL权限设置



##### 				Linux账号与组群



​						UID：用户id

​						GID：组群id



##### 				使用者账号：

​						UID/GID ： /etc/passwd 

​						密码相关数据：/etc/shadow



​				

##### 				/etc/passwd文件结构：

```
[root@study ~]# head -n 2 /etc/passwd
root:x:0:0:root:/root:/bin/bash 
bin:x:1:1:bin:/bin:/sbin/nologin
```

- 帐号名称：就是帐号。例如 root 的 UID 对应就是 0 （第三字段）；

- 密码：因为这个文件的特性是所有的程序都能够读取，很容易造成密码数据被窃取， 因此后来就将这个字段的密码数据给他改放到 [/etc/shadow](https://wizardforcel.gitbooks.io/vbird-linux-basic-4e/Text/index.html#shadow_file) 中了。所以这里你会看到一个“ x ”

- UID：使用者识别码：

  

  | 0（**系统管理员**） | 当 UID 是 0 时，代表这个帐号是“系统管理员”
  | 1~999（**系统帐号，不可登录**） | 保留给系统使用的 ID，其实除了 0 之外，其他的 UID 权限与特性并没有不一样。1~200：由 distributions 自行创建的系统帐号；201~999：若使用者有系统帐号需求时，可以使用的帐号 UID |
  | 1000~60000（**可登陆帐号**） | 给一般使用者用的。事实上，目前的 linux 核心 （3.10.x 版）已经可以支持到 4294967295 （2^32-1）  |



- GID：这个与 /etc/group 有关！其实 /etc/group 的观念与 /etc/passwd 差不多，只是他是用来规范群组名称与 GID 的对应而已！
- 使用者信息说明栏：这个字段基本上并没有什么重要用途，只是用来解释这个帐号的意义而已
- 主文件夹：这是使用者的主文件夹，以上面为例， root 的主文件夹在 /root ，所以当 root 登陆之后，就会立刻跑到 /root 目录里头啦！呵呵！ 如果你有个帐号的使用空间特别的大，你想要将该帐号的主文件夹移动到其他的硬盘去该怎么作？ 没有错！可以在这个字段进行修改呦！默认的使用者主文件夹在 /home/yourIDname
- Shell：默认 shell 会使用 bash ，就是在这个字段指定的， 这里比较需要注意的是，有一个 shell 可以用来替代成让帐号无法取得 shell 环境的登陆动作！那就是 /sbin/nologin 这个东西！这也可以用来制作纯 pop 邮件帐号者的数据呢！



##### 				/etc/shadow文件构造：

```
[root@study ~]# head -n 4 /etc/shadow
root:$6$wtbCCce/PxMeE5wm$KE2IfSJr.YLP7Rcai6oa/T7KFhO...:16559:0:99999:7:::  &lt;==下面说明用
bin:*:16372:0:99999:7:::
daemon:*:16372:0:99999:7:::
adm:*:16372:0:99999:7:::
```



- 帐号名称：由于密码也需要与帐号对应，因此，这个文件的第一栏就是帐号，必须要与 /etc/passwd 相同
- 密码：这个字段内的数据才是真正的密码，而且是经过编码的密码 （加密），这个文件的默认权限是“-rw———-”或者是“—————”，亦即只有 root 才可以读写就是了

- 最近更动密码的日期：这个字段记录了“更动密码那一天”的日期，不过，很奇怪呀！在我的例子中怎么会是 16559 呢？呵呵，这个是因为计算 Linux 日期的时间是以 1970 年 1 月 1 日作为 1 而累加的日期，想要知道某个日期的累积日数， 可使用如下的程序计算：

[root[@study](https://github.com/study) ~]# echo $（（$（date —date="2015/05/04" +%s）/86400+1））
16559

上述指令中，2015/05/04 为你想要计算的日期，86400 为每一天的秒数， %s 为 1970/01/01 以来的累积总秒数。 由于 bash 仅支持整数，因此最终需要加上 1 补齐 1970/01/01 当天。



- 密码不可被更动的天数：（与第 3 字段相比）第四个字段记录了：这个帐号的密码在最近一次被更改后需要经过几天才可以再被变更！如果是 0 的话， 表示密码随时可以更动的意思。
- 密码需要重新变更的天数：（与第 3 字段相比）经常变更密码是个好习惯！为了强制要求使用者变更密码，这个字段可以指定在最近一次更改密码后， 在多少天数内需要再次的变更密码才行。你必须要在这个天数内重新设置你的密码，否则这个帐号的密码将会“变为过期特性”。 而如果像上面的 99999 （计算为 273 年） 的话，那就表示，呵呵，密码的变更没有强制性之意。
- 密码需要变更期限前的警告天数：（与第 5 字段相比）当帐号的密码有效期限快要到的时候 （第 5 字段），系统会依据这个字段的设置，发出“警告”言论给这个帐号，提醒他“再过 n 天你的密码就要过期了，请尽快重新设置你的密码”，如上面的例子，则是密码到期之前的 7 天之内，系统会警告该用户。
- 密码过期后的帐号宽限时间（密码失效日）：（与第 5 字段相比）密码有效日期为“更新日期（第3字段）”+“重新变更日期（第5字段）”，过了该期限后使用者依旧没有更新密码，那该密码就算过期了。 虽然密码过期但是该帐号还是可以用来进行其他工作的，包括登陆系统取得 bash 。不过如果密码过期了， 那当你登陆系统时，系统会强制要求你必须要重新设置密码才能登陆继续使用喔，这就是密码过期特性。

那这个字段的功能是什么呢？是在密码过期几天后，如果使用者还是没有登陆更改密码，那么这个帐号的密码将会“失效”， 亦即该帐号再也无法使用该密码登陆了。要注意密码过期与密码失效并不相同。

- 帐号失效日期：这个日期跟第三个字段一样，都是使用 1970 年以来的总日数设置。这个字段表示： 这个帐号在此字段规定的日期之后，将无法再使用。 就是所谓的“帐号失效”，此时不论你的密码是否有过期，这个“帐号”都不能再被使用！ 这个字段会被使用通常应该是在“收费服务”的系统中，你可以规定一个日期让该帐号不能再使用啦！
- 保留：最后一个字段是保留的，看以后有没有新功能加入。





##### 				/etc/group文件结构：

```
[root@study ~]# head -n 4 /etc/group
root:x:0:
bin:x:1:
daemon:x:2:
sys:x:3:
```



- 群组名称：就是群组名称，基本上需要与第三字段的 GID 对应。
- 群组密码：通常不需要设置，这个设置通常是给“群组管理员”使用的，目前很少有这个机会设置群组管理员, 同样的，密码已经移动到 /etc/gshadow 去，因此这个字段只会存在一个“x”而已；
- GID：就是群组的 ID 啊。 /etc/passwd 第四个字段使用的 GID 对应的群组名，就是由这里对应出来的
- 此群组支持的帐号名称：我们知道一个帐号可以加入多个群组，那某个帐号想要加入此群组时，将该帐号填入这个字段即可。 



![image-20210625145544063](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\image-20210625145544063.png)





##### 				有效群组（effective group）与初始群组（initial group）



​						 /etc/passwd 里面的第四栏有个GID ，那个 GID 就是所谓的“初始群组 （initial group）

​						因为是初始群组， 使用者一登陆就会主动取得，不需要在 /etc/group 的第四个字段写入该帐号的！







##### 				groups: 有效与支持群组的观察



```
[dmtsai@study ~]$ groups
dmtsai wheel users
```

​						可查看用户属于哪几个组群







##### 				newgrp: 有效群组的切换

​			

​						使用newgrp变更组群，但是有限制，要切换的组群必须是以及支持的组群

```
[dmtsai@study ~]$ newgrp users			（在三个组群间切换有效组群）
[dmtsai@study ~]$ groups
users wheel dmtsai
[dmtsai@study ~]$ touch test2
[dmtsai@study ~]$ ll test*
-rw-rw-r--. 1 dmtsai dmtsai 0 Jul 20 19:54 test
-rw-r--r--. 1 dmtsai users  0 Jul 20 19:56 test2
[dmtsai@study ~]$ exit 
```





##### 				etc/gshadow：主要用来创建群组管理员

```
[root@study ~]# head -n 4 /etc/gshadow
root:::
bin:::
daemon:::
sys:::
```

- 群组名称
- 密码栏，同样的，开头为 ! 表示无合法密码，所以无群组管理员
- 群组管理员的帐号 （相关信息在 [gpasswd](https://wizardforcel.gitbooks.io/vbird-linux-basic-4e/Text/index.html#gpasswd) 中介绍）
- 有加入该群组支持的所属帐号 （与 /etc/group 内容相同！）





##### 				新增和移除使用者



​				useradd：

```
[root@study ~]# useradd [-u UID] [-g 初始群组] [-G 次要群组] [-mM]\
&gt;  [-c 说明栏] [-d 主文件夹绝对路径] [-s shell] 使用者帐号名
选项与参数：
-u  ：后面接的是 UID ，是一组数字。直接指定一个特定的 UID 给这个帐号；
-g  ：后面接的那个群组名称就是我们上面提到的 initial group 啦～
该群组的 GID 会被放置到 /etc/passwd 的第四个字段内。
-G  ：后面接的群组名称则是这个帐号还可以加入的群组。
这个选项与参数会修改 /etc/group 内的相关数据喔！
-M  ：强制！不要创建使用者主文件夹！（系统帐号默认值）
-m  ：强制！要创建使用者主文件夹！（一般帐号默认值）
-c  ：这个就是 /etc/passwd 的第五栏的说明内容啦～可以随便我们设置的啦～
-d  ：指定某个目录成为主文件夹，而不要使用默认值。务必使用绝对路径！
-r  ：创建一个系统的帐号，这个帐号的 UID 会有限制 （参考 /etc/login.defs）
-s  ：后面接一个 shell ，若没有指定则默认是 /bin/bash 的啦～
-e  ：后面接一个日期，格式为“YYYY-MM-DD”此项目可写入 shadow 第八字段，
亦即帐号失效日的设置项目啰；
-f  ：后面接 shadow 的第七字段项目，指定密码是否会失效。0为立刻失效，
-1 为永远不失效（密码只会过期而强制于登陆时重新设置而已。）
```





​						useradd 参考档

```
[root@study ~]# useradd -D
GROUP=100        &lt;==默认的群组
HOME=/home        &lt;==默认的主文件夹所在目录
INACTIVE=-1        &lt;==密码失效日，在 shadow 内的第 7 栏
EXPIRE=            &lt;==帐号失效日，在 shadow 内的第 8 栏
SHELL=/bin/bash        &lt;==默认的 shell
SKEL=/etc/skel        &lt;==使用者主文件夹的内容数据参考目录
CREATE_MAIL_SPOOL=yes   &lt;==是否主动帮使用者创建邮件信箱（mailbox）
```



- GROUP=100：新建帐号的初始群组使用 GID 为 100 者
  系统上面 GID 为 100 者即是 users 这个群组，此设置项目指的就是让新设使用者帐号的初始群组为 users 这一个的意思。 但是我们知道 CentOS 上面并不是这样的，在 CentOS 上面默认的群组为与帐号名相同的群组。 举例来说， vbird1 的初始群组为 vbird1 。怎么会这样啊？这是因为针对群组的角度有两种不同的机制所致， 这两种机制分别是：

  ​	私有群组机制：
  ​	系统会创建一个与帐号一样的群组给使用者作为初始群组。 这种群组的设置机制会比较有保密性，这是因	为使用者都有自己的群组，而且主文件夹权限将会设置为 700 （仅有自己可进入自己的主文件夹） 之故。	使用这种机制将不会参考 GROUP=100 这个设置值。代表性的 distributions 有 RHEL, Fedora, CentOS 	

  ​	公共群组机制：
  ​	就是以 GROUP=100 这个设置值作为新建帐号的初始群组，因此每个帐号都属于 users 这个群组， 且默认	主文件夹通常的权限会是“ drwxr-xr-x … username users … ”，由于每个帐号都属于 users 群组，因此大家	都可以互相分享主文件夹内的数据之故。代表 distributions 如 SuSE等。

- HOME=/home：使用者主文件夹的基准目录（basedir）
  使用者的主文件夹通常是与帐号同名的目录，这个目录将会摆放在此设置值的目录后。所以 vbird1 的主文件夹就会在 /home/vbird1/ 了
- INACTIVE=-1：密码过期后是否会失效的设置值
  我们在 [shadow](https://wizardforcel.gitbooks.io/vbird-linux-basic-4e/Text/index.html#shadow_file) 文件结构当中谈过，第七个字段的设置值将会影响到密码过期后， 在多久时间内还可使用旧密码登陆。这个项目就是在指定该日数啦！如果是 0 代表密码过期立刻失效， 如果是 -1 则是代表密码永远不会失效，如果是数字，如 30 ，则代表过期 30 天后才失效。
- EXPIRE=：帐号失效的日期
  就是 [shadow](https://wizardforcel.gitbooks.io/vbird-linux-basic-4e/Text/index.html#shadow_file) 内的第八字段，你可以直接设置帐号在哪个日期后就直接失效，而不理会密码的问题。 通常不会设置此项目
- SHELL=/bin/bash：默认使用的 shell 程序文件名
  系统默认的 shell 就写在这里。假如你的系统为 mail server ，你希望每个帐号都只能使用 email 的收发信件功能， 而不许使用者登陆系统取得 shell ，那么可以将这里设置为 /sbin/nologin ，如此一来，新建的使用者默认就无法登陆！ 也免去后续使用 [usermod](https://wizardforcel.gitbooks.io/vbird-linux-basic-4e/Text/index.html#usermod) 进行修改的手续！
- SKEL=/etc/skel：使用者主文件夹参考基准目录
  指定使用者主文件夹的参考基准目录，举我们的范例一为例， vbird1 主文件夹 /home/vbird1 内的各项数据，都是由 /etc/skel 所复制过去的～所以呢，未来如果我想要让新增使用者时，该使用者的环境变量 ~/.bashrc 就设置妥当的话，您可以到 /etc/skel/.bashrc 去编辑一下，也可以创建 /etc/skel/www 这个目录，那么未来新增使用者后，在他的主文件夹下就会有 www 那个目录
- CREATE_MAIL_SPOOL=yes：创建使用者的 mailbox
  你可以使用“ ll /var/spool/mail/vbird1 ”看一下，会发现有这个文件的存在，这就是使用者的邮件信箱

​				



##### 				passwd账号密码设置：			（创建账号后默认是封锁的，需要设置密码）

```
[root@study ~]# passwd [--stdin] [帐号名称]  &lt;==所有人均可使用来改自己的密码
[root@study ~]# passwd [-l] [-u] [--stdin] [-S] \
&gt;  [-n 日数] [-x 日数] [-w 日数] [-i 日期] 帐号 &lt;==root 功能
选项与参数：
--stdin ：可以通过来自前一个管线的数据，作为密码输入，对 shell script 有帮助！
-l  ：是 Lock 的意思，会将 /etc/shadow 第二栏最前面加上 ! 使密码失效；
-u  ：与 -l 相对，是 Unlock 的意思！
-S  ：列出密码相关参数，亦即 shadow 文件内的大部分信息。
-n  ：后面接天数，shadow 的第 4 字段，多久不可修改密码天数
-x  ：后面接天数，shadow 的第 5 字段，多久内必须要更动密码
-w  ：后面接天数，shadow 的第 6 字段，密码过期前的警告天数
-i  ：后面接“日期”，shadow 的第 7 字段，密码失效日期
```







##### 				chage密码参数显示功能:

```
[root@study ~]# chage [-ldEImMW] 帐号名
选项与参数：
-l ：列出该帐号的详细密码参数；
-d ：后面接日期，修改 shadow 第三字段（最近一次更改密码的日期），格式 YYYY-MM-DD
-E ：后面接日期，修改 shadow 第八字段（帐号失效日），格式 YYYY-MM-DD
-I ：后面接天数，修改 shadow 第七字段（密码失效日期）
-m ：后面接天数，修改 shadow 第四字段（密码最短保留天数）
-M ：后面接天数，修改 shadow 第五字段（密码多久需要进行变更）
-W ：后面接天数，修改 shadow 第六字段（密码过期前警告日期）
```



​						案例：

```
创建一个名为 agetest 的帐号，该帐号第一次登陆后使用默认密码，但必须要更改过密码后，
        使用新密码才能够登陆系统使用 bash 环境
[root@study ~]# useradd agetest
[root@study ~]# echo "agetest" |passwd --stdin agetest
[root@study ~]# chage -d 0 agetest
[root@study ~]# chage -l agetest |head -n 3
Last password change                : password must be changed
Password expires                    : password must be changed
Password inactive                   : password must be changed
```





##### 				usermod：		（微调 useradd 增加的使用者参数）

```
[root@study ~]# usermod [-cdegGlsuLU] username
选项与参数：
-c  ：后面接帐号的说明，即 /etc/passwd 第五栏的说明栏，可以加入一些帐号的说明。
-d  ：后面接帐号的主文件夹，即修改 /etc/passwd 的第六栏；
-e  ：后面接日期，格式是 YYYY-MM-DD 也就是在 /etc/shadow 内的第八个字段数据啦！
-f  ：后面接天数，为 shadow 的第七字段。
-g  ：后面接初始群组，修改 /etc/passwd 的第四个字段，亦即是 GID 的字段！
-G  ：后面接次要群组，修改这个使用者能够支持的群组，修改的是 /etc/group 啰～
-a  ：与 -G 合用，可“增加次要群组的支持”而非“设置”喔！
-l  ：后面接帐号名称。亦即是修改帐号名称， /etc/passwd 的第一栏！
-s  ：后面接 Shell 的实际文件，例如 /bin/bash 或 /bin/csh 等等。
-u  ：后面接 UID 数字啦！即 /etc/passwd 第三栏的数据；
-L  ：暂时将使用者的密码冻结，让他无法登陆。其实仅改 /etc/shadow 的密码栏。
-U  ：将 /etc/shadow 密码栏的 ! 拿掉，解冻啦！
```





##### 				userdel：		（删除使用者相关数据）



```
[root@study ~]# userdel [-r] username
选项与参数：
-r  ：连同使用者的主文件夹也一起删除
```







##### 				使用者功能



​						id：		查询UID/GID等相关信息

```
[root@study ~]# id [username]
```





​						finger：		查询使用这者相关信息

```
[root@study ~]# finger [-s] username
选项与参数：
-s  ：仅列出使用者的帐号、全名、终端机代号与登陆时间等等；
-m  ：列出与后面接的帐号相同者，而不是利用部分比对 （包括全名部分）
```



- Login：为使用者帐号，亦即 /etc/passwd 内的第一字段；
- Name：为全名，亦即 /etc/passwd 内的第五字段（或称为注解）；
- Directory：就是主文件夹了；
- Shell：就是使用的 Shell 文件所在；
- Never logged in.：figner 还会调查使用者登陆主机的情况喔！
- No mail.：调查 /var/spool/mail 当中的信箱数据；
- No Plan.：调查 ~vbird1/.plan 文件，并将该文件取出来说明！





​						chfn：		类似change finger的意思

```
[root@study ~]# chfn [-foph] [帐号名]
选项与参数：
-f  ：后面接完整的大名；
-o  ：您办公室的房间号码；
-p  ：办公室的电话号码；
-h  ：家里的电话号码！
```





​						chsh：		change shell 的简写

```
[vbird1@study ~]$ chsh [-ls]
选项与参数：
-l  ：列出目前系统上面可用的 shell ，其实就是 /etc/shells 的内容
-s  ：设置修改自己的 Shell 
```



 



##### 				新增与移除组群



​						groupadd：

```
root@study ~]# groupadd [-g gid] [-r] 群组名称
选项与参数：
-g  ：后面接某个特定的 GID ，用来直接给予某个 GID ～
-r  ：创建系统群组啦！与 /etc/login.defs 内的 GID_MIN 有关。
```





​						groupmod：				(group相关信息的修改)

```
[root@study ~]# groupmod [-g gid] [-n group_name] 群组名
选项与参数：
-g  ：修改既有的 GID 数字；
-n  ：修改既有的群组名称
```



​						案例

```
将刚刚上个指令创建的 group1 名称改为 mygroup ， GID 为 201
[root@study ~]# groupmod -g 201 -n mygroup group1
[root@study ~]# grep mygroup /etc/group /etc/gshadow
/etc/group:mygroup:x:201:
/etc/gshadow:mygroup:!::
```





​						groupdel：				（组群的删除）

```
[root@study ~]# groupdel [groupname]
```





​						gpasswd：				（组群管理员设置）

```
# 关于系统管理员（root）做的动作：
[root@study ~]# gpasswd groupname
[root@study ~]# gpasswd [-A user1,...] [-M user3,...] groupname
[root@study ~]# gpasswd [-rR] groupname
选项与参数：
    ：若没有任何参数时，表示给予 groupname 一个密码（/etc/gshadow）
-A  ：将 groupname 的主控权交由后面的使用者管理（该群组的管理员）
-M  ：将某些帐号加入这个群组当中
-r  ：将 groupname 的密码移除
-R  ：让 groupname 的密码栏失效
```



```
# 关于群组管理员（Group administrator）做的动作：
[someone@study ~]$ gpasswd [-ad] user groupname
选项与参数：
-a  ：将某位使用者加入到 groupname 这个群组当中
-d  ：将某位使用者移除出 groupname 这个群组当中。
```





​							案例：

```
范例一：创建一个新群组，名称为 testgroup 且群组交由 vbird1 管理：
[root@study ~]# groupadd testgroup  		&lt;==先创建群组
[root@study ~]# gpasswd testgroup   		&lt;==给这个群组一个密码吧！
Changing the password for group testgroup
New Password:
Re-enter new password:
# 输入两次密码就对了
[root@study ~]# gpasswd -A vbird1 testgroup  &lt;==加入群组管理员为 vbird1
[root@study ~]# grep testgroup /etc/group /etc/gshadow
/etc/group:testgroup:x:1503:
/etc/gshadow:testgroup:$6$MnmChP3D$mrUn.Vo.buDjObMm8F2emTkvGSeuWikhRzaKHxpJ...:vbird1:
# 很有趣吧！此时 vbird1 则拥有 testgroup 的主控权喔！身份有点像板主啦！


范例二：以 vbird1 登陆系统，并且让他加入 vbird1, vbird3 成为 testgroup 成员
[vbird1@study ~]$ id
uid=1003（vbird1） gid=1004（vbird1） groups=1004（vbird1） ...
# 看得出来，vbird1 尚未加入 testgroup 群组喔！
[vbird1@study ~]$ gpasswd -a vbird1 testgroup
[vbird1@study ~]$ gpasswd -a vbird3 testgroup
[vbird1@study ~]$ grep testgroup /etc/group
testgroup:x:1503:vbird1,vbird3
```







##### 				账户管理实例



任务一：单纯的完成上头交代的任务，假设我们需要的帐号数据如下，你该如何实作？

| 帐号名称 | 帐号全名 | 支持次要群组 | 是否可登陆主机 | 密码     |
| :------- | :------- | :----------- | :------------- | :------- |
| myuser1  | 1st user | mygroup1     | 可以           | password |
| myuser2  | 2nd user | mygroup1     | 可以           | password |
| myuser3  | 3rd user | 无额外支持   | 不可以         | password |

处理的方法如下所示：

```
# 先处理帐号相关属性的数据：
[root@study ~]# groupadd mygroup1
[root@study ~]# useradd -G mygroup1 -c "1st user" myuser1
[root@study ~]# useradd -G mygroup1 -c "2nd user" myuser2
[root@study ~]# useradd -c "3rd user" -s /sbin/nologin myuser3
# 再处理帐号的密码相关属性的数据：
[root@study ~]# echo "password" |passwd --stdin myuser1
[root@study ~]# echo "password" |passwd --stdin myuser2
[root@study ~]# echo "password" |passwd --stdin myuser3
```





任务二：我的使用者 pro1, pro2, pro3 是同一个专案计划的开发人员，我想要让这三个用户在同一个目录下面工作， 但这三个用户还是拥有自己的主文件夹与基本的私有群组。假设我要让这个专案计划在 /srv/projecta 目录下开发， 可以如何进行



处理的方法如下所示：

```
# 1\. 假设这三个帐号都尚未创建，可先创建一个名为 projecta 的群组，
#    再让这三个用户加入其次要群组的支持即可：
[root@study ~]# groupadd projecta
[root@study ~]# useradd -G projecta -c "projecta user" pro1
[root@study ~]# useradd -G projecta -c "projecta user" pro2
[root@study ~]# useradd -G projecta -c "projecta user" pro3
[root@study ~]# echo "password" &#124; passwd --stdin pro1
[root@study ~]# echo "password" &#124; passwd --stdin pro2
[root@study ~]# echo "password" &#124; passwd --stdin pro3

# 2\. 开始创建此专案的开发目录：
[root@study ~]# mkdir /srv/projecta
[root@study ~]# chgrp projecta /srv/projecta
[root@study ~]# chmod 2770 /srv/projecta
[root@study ~]# ll -d /srv/projecta
drwxrws---. 2 root projecta 6 Jul 20 23:32 /srv/projecta
```









##### 				ACL的使用

- 使用者 （user）：可以针对使用者来设置权限；
- 群组 （group）：针对群组为对象来设置其权限；
- 默认属性 （mask）：还可以针对在该目录下在创建新文件/目录时，规范新数据的默认权限；
  也就是说，如果你有一个目录，需要给一堆人使用，每个人或每个群组所需要的权限并不相同时，在过去，传统的 Linux 三种身份的三种权限是无法达到的， 因为基本上，传统的 Linux 权限只能针对一个用户、一个群组及非此群组的其他人设置权限而已，无法针对单一用户或个人来设计权限。 而 ACL 就是为了要改变这个问题啊！好了，稍微了解之后，再来看看如何让你的文件系统可以支持 ACL 吧！
- 如何启动 ACL
  事实上，原本 ACL 是 unix-like 操作系统的额外支持项目，但因为近年以来 Linux 系统对权限细部设置的热切需求， 因此目前 ACL 几乎已经默认加入在所有常见的 Linux 文件系统的挂载参数中 （ext2/ext3/ext4/xfs等等）！所以你无须进行任何动作， ACL 就可以被你使用啰！不过，如果你不放心系统是否真的有支持 ACL 的话，那么就来检查一下核心挂载时显示的信息





##### 				ACL的设置技巧：getfacl、setfacl





​						setfacl：设置某个目录/文件的 ACL 规范

```
[root@study ~]# setfacl [-bkRd] [{-m&|-x} acl参数] 目标文件名
选项与参数：
-m ：设置后续的 acl 参数给文件使用，不可与 -x 合用；
-x ：删除后续的 acl 参数，不可与 -m 合用；
-b ：移除“所有的” ACL 设置参数；
-k ：移除“默认的” ACL 参数，关于所谓的“默认”参数于后续范例中介绍；
-R ：递回设置 acl ，亦即包括次目录都会被设置起来；
-d ：设置“默认 acl 参数”的意思！只对目录有效，在该目录新建的数据会引用此默认值
```



​						getfacl：取得某个文件/目录的 ACL 设置项目

```
[root@study ~]# getfacl filename
选项与参数：
getfacl 的选项几乎与 setfacl 相同
```



- 特定的单一群组的权限设置：“ g:群组名:权限 ”

```
规范：“ g:[群组列表]:[rwx] ”，例如针对 mygroup1 的权限规范 rx ：
[root@study ~]# setfacl -m g:mygroup1:rx acl_test1
[root@study ~]# getfacl acl_test1
# file: acl_test1
# owner: root
# group: root
user::rwx
user:vbird1:r-x
group::r--
group:mygroup1:r-x  &lt;==这里就是新增的部分,多了这个群组的权限设置
mask::r-x
other::r--
```



- 针对有效权限设置：“ m:权限 ”

```
规范：“ m:[rwx] ”，例如针对刚刚的文件规范为仅有 r ：
[root@study ~]# setfacl -m m:r acl_test1
[root@study ~]# getfacl acl_test1
# file: acl_test1
# owner: root
# group: root
user::rwx
user:vbird1:r-x        #effective:r-- 	&lt;==vbird1+mask均存在者，仅有 r 而已，x 不会生效
group::r--
group:mygroup1:r-x     #effective:r--
mask::r--
other::r--
```







##### 								使用者身份切换



​						su  命令				（推荐使用sudo，避免密码流出）

```
[root@study ~]# su [-lm] [-c 指令] [username]
选项与参数：
-   ：单纯使用 - 如“ su - ”代表使用 login-shell 的变量文件读取方式来登陆系统；
      若使用者名称没有加上去，则代表切换为 root 的身份。
-l  ：与 - 类似，但后面需要加欲切换的使用者帐号！也是 login-shell 的方式。
-m  ：-m 与 -p 是一样的，表示“使用目前的环境设置，而不读取新使用者的配置文件”
-c  ：仅进行一次指令，所以 -c 后面可以加上指令
```





​						sudo  命令

```
[root@study ~]# sudo [-b] [-u 新使用者帐号]
选项与参数：
-b  ：将后续的指令放到背景中让系统自行执行，而不与目前的 shell 产生影响
-u  ：后面可以接欲切换的使用者，若无此项则代表切换身份为 root 。
```





​						sudo搭配su使用

​								（一口气将身份转变成root，并且使用自己的密码）

```
[root@study ~]# visudo
User_Alias  ADMINS = pro1, pro2, pro3, myuser1
ADMINS ALL=（root）/bin/su -
```





##### 				特殊的shell，/sbin/nologin



​						不能登录系统，可作为mail账号的shell





##### 				 PAM			(认证系统程序接口)



```
PAM 是一个独立的 API 存在，只要任何程序有需求时，可以向 PAM 发出验证要求的通知， PAM 经过一连串的验证后，将验证的结果回报给该程序，然后该程序就能够利用验证的结果来进行可登陆或显示其他无法使用的讯息
PAM 用来进行验证的数据称为模块 （Modules），每个 PAM 模块的功能都不太相同
```





​						PAM 模块设置语法



调用PAM的流程：

- 使用者开始执行 /usr/bin/passwd 这支程序，并输入密码；
- passwd 调用 PAM 模块进行验证；
- PAM 模块会到 /etc/pam.d/ 找寻与程序 （passwd） 同名的配置文件；
- 依据 /etc/pam.d/passwd 内的设置，引用相关的 PAM 模块逐步进行验证分析；
- 将验证结果 （成功、失败以及其他讯息） 回传给 passwd 这支程序；
- passwd 这支程序会根据 PAM 回传的结果决定下一个动作 （重新输入新密码或者通过验证！）



![](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\image-20210626194339339.png)



​				



​						PAM常用模块简介



- /etc/pam.d/*：每个程序个别的 PAM 配置文件；
- /lib64/security/*：PAM 模块文件的实际放置目录；
- /etc/security/*：其他 PAM 环境的配置文件；
- /usr/share/doc/pam-
- pam_securetty.so：限制系统管理员 （root） 只能够从安全的 （secure） 终端机登陆；
- pam_nologin.so：这个模块可以限制一般使用者是否能够登陆主机之用。
- pam_selinux.so：SELinux 是个针对程序来进行细部管理权限的功能
- pam_console.so：当系统出现某些问题，或者是某些时刻你需要使用特殊的终端接口 （例如 RS232 之类的终端连线设备） 登陆主机时， 这个模块可以帮助处理一些文件权限的问题，让使用者可以通过特殊终端接口 （console） 顺利的登陆系统。
- pam_loginuid.so：我们知道系统帐号与一般帐号的 UID 是不同的！一般帐号 UID 均大于 1000 才合理。 因此，为了验证使用者的 UID 真的是我们所需要的数值，可以使用这个模块来进行规范
- pam_env.so：用来设置环境变量的一个模块，如果你有需要额外的环境变量设置，可以参考 /etc/security/pam_env.conf 这个文件的详细说明。
- pam_unix.so：这是个很复杂且重要的模块，这个模块可以用在验证阶段的认证功能，可以用在授权阶段的帐号授权管理， 可以用在会议阶段的登录文件记录等，甚至也可以用在密码更新阶段的检验
- pam_pwquality.so：可以用来检验密码的强度！包括密码是否在字典中，密码输入几次都失败就断掉此次连线等功能，都是这模块提供， 同时提供了 /etc/security/pwquality.conf 这个文件可以额外指定默认值！比较容易处理修改！
- pam_limits.so：ulimit， 其实那就是这个模块提供的能力







​						login 的PAM的验证机制



- 验证阶段 （auth）：首先，（a）会先经过 pam_securetty.so 判断，如果使用者是 root 时，则会参考 /etc/securetty 的设置； 接下来（b）经过 pam_env.so 设置额外的环境变量；再（c）通过 pam_unix.so 检验密码，若通过则回报 login 程序；若不通过则（d）继续往下以 pam_succeed_if.so 判断 UID 是否大于 1000 ，若小于 1000则回报失败，否则再往下 （e）以 pam_deny.so 拒绝连线。
- 授权阶段 （account）：（a）先以 pam_nologin.so 判断 /etc/nologin 是否存在，若存在则不许一般使用者登陆； （b）接下来以 pam_unix.so 及 pam_localuser.so 进行帐号管理，再以 （c） pam_succeed_if.so 判断 UID 是否小于 1000 ，若小于 1000 则不记录登录信息。（d）最后以 pam_permit.so 允许该帐号登陆。
- 密码阶段 （password）：（a）先以 pam_pwquality.so 设置密码仅能尝试错误 3 次；（b）接下来以 pam_unix.so 通过 sha512, shadow 等功能进行密码检验，若通过则回报 login 程序，若不通过则 （c）以 pam_deny.so 拒绝登陆。
- 会议阶段 （session）：（a）先以 pam_selinux.so 暂时关闭 SELinux；（b）使用 pam_limits.so 设置好使用者能够操作的系统资源； （c）登陆成功后开始记录相关信息在登录文件中； （d）以 pam_loginuid.so 规范不同的 UID 权限；（e）打开 pam_selinux.so 的功能。







##### 				查询使用者： w、who、last、lastlog





​						w、who : 目前已登录的使用者

```
[root@study ~]# w
 01:49:18 up 25 days,  3:34,  3 users,  load average: 0.00, 0.01, 0.05
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
dmtsai   tty2                      07Jul15 12days  0.03s  0.03s -bash
dmtsai   pts/0    172.16.200.254   00:18    6.00s  0.31s  0.11s sshd: dmtsai [priv]
# 第一行显示目前的时间、开机 （up） 多久，几个使用者在系统上平均负载等；
# 第二行只是各个项目的说明，
# 第三行以后，每行代表一个使用者。如上所示，dmtsai 登陆并取得终端机名 tty2 之意。
```



​						last、lastlog	每个账号最近登陆的时间

```
[root@study ~]# lastlog
Username         Port     From             Latest
root             pts/0                     Wed Jul 22 00:26:08 +0800 2015
bin                                        **Never logged in**
....（中间省略）....
dmtsai           pts/1    192.168.1.100    Wed Jul 22 01:08:07 +0800 2015
vbird1           pts/0                     Wed Jul 22 01:32:17 +0800 2015
pro3                                       **Never logged in**
....（以下省略）....
```





##### 				使用者对谈： write, mesg, wall

```
[root@study ~]# write 使用者帐号 [使用者所在终端接口]
								
								(Ctrl + d 退出)
```







​						mesg：不接受任何信息		(除了root)

```
[vbird1@study ~]$ mesg n
[vbird1@study ~]$ mesg		#查看信息
is n
									
								(解开：mesg y)
```





​						wall：对于系统上所有使用者传讯

```
[root@study ~]# wall "I will shutdown my linux server..."
```







##### 				使用者邮件信箱： mail



​						寄信：mail -s

```
[root@study ~]# mail -s "nice to meet you" vbird1
```





​						收信：mail



```
输入 mail 之后，我可以看到我有一封信件， 这封信件的前面那个 > 代表目前处理的信件，而在大于符号的右边那个 N 代表该封信件尚未读过， 如果我想要知道这个 mail 内部的指令有哪些，可以在 & 之后输入“ ? ”
```

​							

​							查看的指令

| 指令 | 意义                                                         |
| :--- | :----------------------------------------------------------- |
| h    | 列出信件标头；如果要查阅 40 封信件左右的信件标头，可以输入“ h 40 ” |
| d    | 删除后续接的信件号码，删除单封是“ d10 ”，删除 20~40 封则为“ d20-40 ”。 不过，这个动作要生效的话，必须要配合 q 这个指令才行（参考下面说明）！ |
| s    | 将信件储存成文件。例如我要将第 5 封信件的内容存成 ~/mail.file:“s 5 ~/mail.file” |
| x    | 或者输入 exit 都可以。这个是“不作任何动作离开 mail 程序”的意思。 不论你刚刚删除了什么信件，或者读过什么，使用 exit 都会直接离开 mail，所以刚刚进行的删除与阅读工作都会无效。 如果您只是查阅一下邮件而已的话，一般来说，建议使用这个离开啦！除非你真的要删除某些信件。 |
| q    | 相对于 exit 是不动作离开， q 则会实际进行你刚刚所执行的任何动作 （尤其是删除） |





##### 				帐号相关的检查工具： pwck 、 pwconv 、 pwuconv 





​						pwck：		(检查输入是否正确)

```
检查 /etc/passwd 这个帐号配置文件内的信息，与实际的主文件夹是否存在等信息， 还可以比对 /etc/passwd /etc/shadow 的信息是否一致，另外，如果 /etc/passwd 内的数据字段错误时，会提示使用者修订
```





​						pwconv：		(将 /etc/passwd 内的帐号与密码，移动到 /etc/shadow 当中)  （手动创建账号）

```
比对 /etc/passwd 及 /etc/shadow ，若 /etc/passwd 内存在的帐号并没有对应的 /etc/shadow 密码时，则 pwconv 会去 /etc/login.defs 取用相关的密码数据，并创建该帐号的 /etc/shadow 数据；

若 /etc/passwd 内存在加密后的密码数据时，则 pwconv 会将该密码栏移动到 /etc/shadow 内，并将原本的 /etc/passwd 内相对应的密码栏变成 x 
```

​			

​						

​						pwuconv：		

```
将 /etc/shadow 内的密码栏数据写回 /etc/passwd 当中， 并且删除 /etc/shadow 文件。
```





​						chpasswd：		(创建大量账号中使用)

```
读入未加密前的密码，并且经过加密后， 将加密后的密码写入 /etc/shadow 当中
```





##### 				创建大量文件范本

```
[root@study ~]# vim accountadd.sh
#!/bin/bash
# This shell script will create amount of linux login accounts for you.
# 1\. check the "accountadd.txt" file exist? you must create that file manually.
#    one account name one line in the "accountadd.txt" file.
# 2\. use openssl to create users password.
# 3\. User must change his password in his first login.
# 4\. more options check the following url:
# 0410accountmanager.html#manual_amount
# 2015/07/22    VBird
export PATH=/bin:/sbin:/usr/bin:/usr/sbin
# 0\. userinput
usergroup=""                   # if your account need secondary group, add here.
pwmech="openssl"               # "openssl" or "account" is needed.
homeperm="no"                  # if "yes" then I will modify home dir permission to 711
# 1\. check the accountadd.txt file
action="${1}"                  # "create" is useradd and "delete" is userdel.
if [ ! -f accountadd.txt ]; then
    echo "There is no accountadd.txt file, stop here."
        exit 1
fi
[ "${usergroup}" != "" ] && groupadd -r ${usergroup}
rm -f outputpw.txt
usernames=$（cat accountadd.txt）
for username in ${usernames}
do
    case ${action} in
        "create"）
            [ "${usergroup}" != "" ] && usegrp=" -G ${usergroup} " ||usegrp=""
            useradd ${usegrp} ${username}               # 新增帐号
            [ "${pwmech}" == "openssl" ] && usepw=$（openssl rand -base64 6） || usepw=${username}
            echo ${usepw} |passwd --stdin ${username}  # 创建密码
            chage -d 0 ${username}                      # 强制登陆修改密码
            [ "${homeperm}" == "yes" ] && chmod 711 /home/${username}
        echo "username=${username}, password=${usepw}" >>outputpw.txt
            ;;
        "delete"）
            echo "deleting ${username}"
            userdel -r ${username}
            ;;
        *）
            echo "Usage: $0 [create|delete]"
            ;;
    esac
done
```







# 十、磁盘配额（Quota）与进阶文件系统管理





##### 				Quota:容量限制



​						Quota 的一般用途：



- quota 比较常使用的几个情况是：

- 针对 WWW server ，例如：每个人的网页空间的容量限制！

- 针对 mail server，例如：每个人的邮件空间限制。

- 针对 file server，例如：每个人最大的可用网络硬盘空间

  

  上头讲的是针对网络服务的设计，如果是针对 Linux 系统主机上面的设置那么使用的方向有下面这一些：

- 限制某一群组所能使用的最大磁盘配额 （使用群组限制）：你可以将你的主机上的使用者分门别类，有点像是目前很流行的付费与免付费会员制的情况， 你比较喜好的那一群的使用配额就可以给高一些

- 限制某一使用者的最大磁盘配额 （使用使用者限制）：在限制了群组之后，你也可以再继续针对个人来进行限制，使得同一群组之下还可以有更公平的分配

- 限制某一目录 （directory, project） 的最大磁盘配额：在旧版的 CentOS 当中，使用的默认文件系统为 EXT 家族，这种文件系统的磁盘配额主要是针对整个文件系统来处理，所以大多针对“挂载点”进行设计。 新的 xfs 可以使用 project 这种模式，就能够针对个别的目录 （非文件系统喔） 来设计磁盘配额





​						Quota使用限制：

- 在 EXT 文件系统家族仅能针对整个 filesystem：EXT 文件系统家族在进行 quota 限制的时候，它仅能针对整个文件系统来进行设计，无法针对某个单一的目录来设计它的磁盘配额。 因此，如果你想要使用不同的文件系统进行 quota 时，请先搞清楚该文件系统支持的情况喔！因为 XFS 已经可以使用 project 模式来设计不同目录的磁盘配额。
- 核心必须支持 quota ：Linux 核心必须有支持 quota 这个功能才行：
- 只对一般身份使用者有效：这就有趣了！并不是所有在 Linux 上面的帐号都可以设置 quota 呢，例如 root 就不能设置 quota ， 因为整个系统所有的数据几乎都是他的
- 若启用 SELinux，非所有目录均可设置 quota ：新版的 CentOS 默认都有启用 SELinux 这个核心功能，该功能会加强某些细部的权限控制！由于担心管理员不小心设置错误，因此默认的情况下， quota 似乎仅能针对 /home 进行设置而已～因此，如果你要针对其他不同的目录进行设置，请参考到后续章节查阅解开 SELinux 限制的方法喔！ 这就不是 quota 的问题了…





​						Quota规范设置目录：

- 分别针对使用者、群组或个别目录 （user, group & project）：
  XFS 文件系统的 quota 限制中，主要是针对群组、个人或单独的目录进行磁盘使用率的限制！

- 容量限制或文件数量限制 （block 或 inode）：
  文件系统主要规划为存放属性的 inode 与实际文件数据的 block 区块，Quota 既然是管理文件系统，所以当然也可以管理 inode 或 block 啰！ 这两个管理的功能为：

  ​	限制 inode 用量：可以管理使用者可以创建的“文件数量”；

  ​	限制 block 用量：管理使用者磁盘容量的限制，较常见为这种方式。

- 柔性劝导与硬性规定 （soft/hard）： soft 与 hard。 通常 hard 限制值要比 soft 还要高。



​		hard：表示使用者的用量绝对不会超过这个限制值

​		soft：表示使用者在低于 soft 限值时 （此例中为 400MBytes），可以正常使用磁盘，但若超过 soft 且低于 		hard 的限值 （介于 400~500MBytes 之间时），每次使用者登陆系统时，系统会主动发出磁盘即将爆满的警		告讯息， 且会给予一个宽限时间 （grace time）。不过，若使用者在宽限时间倒数期间就将容量再次降低于 		soft 限值之下， 则宽限时间会停止。

![image-20210702110555813](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\image-20210702110555813.png)























