# linux网络连接

## 一、基础网络连接步骤：

1.**编辑虚拟网络设置**，选择NAT模式



2.**点击NAT设置**，查看网络信息（错点：网关得与网络配置文件中的GATEWAY一致）



3.**主机名称设置**，需要使用代码：hostnamectl set-hostname xx



4.**网络配置**，需要使用代码：cd /etc/sysconfig/network-scripts/ （查看） 

​												  ls	显示网卡文件，	vi	进入配置

​	需要修改网络IP（自定义）		网卡地址GATEWAY（与真机一致）

​												  ins	插入修改，	Esc   +  :  +  wq	保存退出



5**.建立主机与ip的映射**，代码：vim /etc/hosts （添加各主机IP与主机名）

​	如：	192.168.1.1 pc1

​	后ping 另一主机尝试是否成功



6.**配置ssh免密码登录**，在root用户下输入	ssh-keygen -t rsa 	一路回车生成密钥



​	秘钥生成后在~/.ssh/目录下，有两个文件id_rsa(私钥)和id_rsa.pub（公钥），将公钥复制到

​	authorized_keys，代码：	cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys



​	并赋予authorized_keys600权限，代码：	chmod 600 ~/.ssh/authorized_keys 	

​	查看是否成功，代码：	cat authorized_keys 



​	并且给另外两台机子配置，将pc1节点上的authoized_keys远程传输到pc2和pc3的~/.ssh/目录下

​	代码：	scp ~/.ssh/authorized_keys root@pc2:~/.ssh/



​	使用命令在机子间切换，代码：	ssh pc2



7.**关闭防火墙**，进入修改代码：	 vim /etc/selinux/config

​	修改：SELINUX=disabled



> 小结：网络ip设置可自行定义，但虚拟机中的网关ip须与真机一致。
>
> ​			网络设置中可能会出现机器ping不通的情况，如果步骤正确无误有可能是服务冲突的原因，可自行
>
> ​			百度解决，此不再做详细描述





## 二、环境安装步骤：

### （1）Java环境安装

​	1.**查看系统是否自带JDK**，代码：	java -version



​	2.**查看相关java文件**，代码：	rpm -qa | grep java



​	3.**删除相关文件**，代码：	rpm -e --nodeps java-1.8.0	（例，即所有带openjdk的文件）



​	4.**查看删除结果**，代码：	java -version	（出现：Java：未找到命令...	即成功）



​	5.**下载JDK**，下载地址：	

​		(https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html)



​		（下载这个版本JDK需要登录，账号：986807058bin	密码：1915790282Bin.）



​		jdk-8u211-linux-x64.tar.gz	（版本需以tar.gz结尾）



​	6.**解压安装JDK**，将下载好的安装包拖入到桌面



​	输入代码：	cp jdk-8u291-linux-x64.tar.gz /usr/java1.8 	将安装包改名移动到usr

​													（注意需要使用root权限）



​	 输入代码：	cd /usr 	来到刚才的复制文件处，输入代码：     tar -zxvf java1.8	进行解压	

​																														（注意需加上zxvf）



​	7.**配置JDK环境变量**，输入代码：	 vim /etc/profile	修改配置文件



​	ins插入，将文件修改至最后一行：	

​	#java environment

​	export JAVA_HOME=/usr/jdk1.8.0_211
​	export CLASSPATH=.:${JAVA_HOME}/jre/lib/rt.jar:${JAVA_HOME}/lib/dt.jar:${JAVA_HOME}/lib/tools.jar
​	export PATH=$PATH:${JAVA_HOME}/bin

​	（根据自己的实际文件进行修改）

​	

​	保存退出，输入代码：source /etc/profile	使配置文件生效



​	8.**测试安装效果**，输入代码：	java -version	查看是否成功





> 小结：JDK安装出现的主要问题是安装包的正确下载，有些版本并不能正常使用，请使用1.8版本
>
> ​			在第6步开始建议进入root账号进行操作，因为有可能引起错误

​	

（4.23，明天：mysql的安装配置）



