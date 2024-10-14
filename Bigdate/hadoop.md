# Hadoop是什么



Hadoop是一个由Apache基金会所开发的分布式系统基础架构。

主要是为了解决海量数据的存储和海量数据的分析计算问题。

# Hadoop的组成

## HDFS 架构概述

Hadoop Distributed File System，简称 HDFS，是一个分布式文件系统。

## YARN 架构概述

Yet Another Resource Negotiator 简称 YARN ，另一种资源协调者，是 Hadoop 的资源管理器。

## MapReduce 架构概述

MapReduce 将计算过程分为两个阶段：Map 和 Reduce 

1. Map 阶段并行处理输入数据 

2. Reduce 阶段对 Map 结果进行汇总

# Hadoop集群搭建流程

## 新建虚拟机

1. 选择最小化安装

![image-20220228132607121](hadoop3.0.assets/image-20220228132607121.png)

2. 选择网络设置，**设置hostname与开启网络设置ip地址**

![image-20220228132734199](hadoop3.0.assets/image-20220228132734199.png)

3. 设置root密码后耐心等待即可

## 下载常用工具

```bash
yum install -y epel-release
yum install net-tools -y
yum install lrzsz -y
```

## 创建文件夹

用于存放安装包和软件

```bash
cd /opt
mkdir software module
```

## 关闭防火墙

```bash
systemctl stop firewalld
systemctl disable firewalld
```

## 修改机子的IP地址

```bash
vi /etc/sysconfig/network-scripts/ifcfg-ens16667
```

修改dhcp为static

```
BOOTPROTO="static"
```

在最后面加上

```
IPADDR=192.168.200.130
GATEWAY=192.168.200.2
DNS1=8.8.8.8
DNS2=114.114.114.114
```

刷新网络服务

```bash
systemctl restart network

```

**注意网关是最容易错的，要和虚拟网络设置的网关一样**

![image-20220228151515180](hadoop3.0.assets/image-20220228151515180.png)

## 修改机子的主机名

```bash
vi /etc/hostname

hostnamectl set-hostname xxxxx
```

## 映射地址

```bash
vi /etc/hosts
```

```python
192.168.200.130 master
192.168.200.131 slave1
192.168.200.132 slave2
//行尾不能有空格，最后一行不能有回车符
```

## 克隆另外两台机儿

```bash
vi /etc/sysconfig/network-scripts/ifcfg-ens16667
systemctl restart network
hostnamectl set-hostname xxxxx
//每台机子记得要改
```

## REBOOT

## 上传JDK与hadoop到主节点

```bash
cd /opt/software
rz jdk and hadoop
```

## 解压JDK和hadoop

```bash
tar -zxvf jdk-8u212-linux-x64.tar.gz  -C /opt/module
tar -zxvf hadoop-3.1.3.tar.gz -C /opt/module
```

## 重命名

```bash
cd /opt/module
mv hadoop-3.1.3 hadoop
mv jdk1.8.0_212 jdk
```

## 配置环境变量

```bash
vi /etc/profile.d/my_env.sh
```

```bash
#JAVA_HOME
export JAVA_HOME=/opt/module/jdk
export PATH=$PATH:$JAVA_HOME/bin
#HADOOP_HOME
export HADOOP_HOME=/opt/module/hadoop
export PATH=$PATH:$HADOOP_HOME/bin
export PATH=$PATH:$HADOOP_HOME/sbin
#hadoop_settings
export HDFS_NAMENODE_USER=root
export HDFS_DATANODE_USER=root
export HDFS_SECONDARYNAMENODE_USER=root
export YARN_RESOURCEMANAGER_USER=root
export YARN_NODEMANAGER_USER=root
```

```bash
source /etc/profile
```

然后测试java hadoop 能否使用，不行就g

## 配置集群的配置文件

```bash
cd /opt/module/hadoop/etc/hadoop
vi core-site.xml

<!-- 指定 NameNode 的地址 -->
 <property>
 <name>fs.defaultFS </name>
 <value>hdfs://master:9000</value>
 </property>

<!-- 指定 hadoop 数据的存储目录 -->
 <property>
 <name>hadoop.tmp.dir</name>
 <value>/opt/module/hadoop/data/tmp</value>
 </property>
```

```bash
vi hdfs-site.xml

<!-- 设置副本数-->
<property>
 <name>dfs.replication</name>
 <value>3</value>
 </property>

<!-- 2nn web 端访问地址-->
 <property>
 <name>dfs.namenode.secondary.http-address</name>
 <value>slave2:50090</value>
 </property>
 
 <property>
　　<name>dfs.permissions.enabled</name>
　　<value>false</value>
</property>
```

```sh
vi yarn-site.xml

<!-- 指定shuffle-->
<property>
 <name>yarn.nodemanager.aux-services</name>
 <value>mapreduce_shuffle</value>
 </property>

<!-- 指定ResourceManager主机-->
<property>
 <name>yarn.resourcemanager.hostname</name>
 <value>slave1</value>
 </property>
```

```sh
vi mapred-site.xml

<!-- 指定MapReduce在Yarn上运行-->
<property>
 <name>mapreduce.framework.name</name>
 <value>yarn</value>
 </property>
```

```bash
vi hadoop-env.sh

去掉 # 修改为
export JAVA_HOME=/opt/module/jdk
```

```sh
vi yarn-env.sh

最后插入
export JAVA_HOME=/opt/module/jdk
```

```sh
vi mapred-env.sh

最后
export JAVA_HOME=/opt/module/jdk
```

```sh
vi workers

master
slave1
slave2
```

## 免密登入

```sh
三台机子都需执行
ssh-keygen -t rsa
ssh-copy-id master
ssh-copy-id slave1
ssh-copy-id slave2
```

## 分发配置文件

```bash
scp  -r  /opt/module/ root@slave1:/opt/
scp  -r  /opt/module/ root@slave2:/opt/

scp -r  /etc/profile.d/ root@slave1:/etc/
scp -r  /etc/profile.d/ root@slave2:/etc/

其他机子
source /etc/profile
```

# Hadoop基础操作

## Web端查看HDFS的NameNode

```
http://192.168.200.130:9870
```

## Web 端查看 YARN 的 ResourceManager

```
http://192.168.200.131:8088
```

## 上传小文件

```sh
hadoop fs -mkdir /input
hadoop fs -put $HADOOP_HOME/wcinput/word.txt /input
```

## 上传大文件

```sh
hadoop fs -put /opt/software/jdk-8u212-linux-x64.tar.gz
```

## 下载

```sh
hadoop fs -get /jdk-8u212-linux-x64.tar.gz ./
```

## 执行wordcount程序

```sh
hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-3.1.3.jar wordcount /input /output
```

# HDFS概述

HDFS（Hadoop Distributed File System），它是一个文件系统，用于存储文件，通过目 录树来定位文件；其次，它是分布式的，由很多服务器联合起来实现其功能，集群中的服务 器有各自的角色。

**HDFS适合一次写入，多次读出的场景。一个文件经过创建、写入和关闭 之后就不需要改变。**

## HDFS优点

![image-20220302110154301](hadoop3.0.assets/image-20220302110154301.png)

## HDFS缺点

![image-20220302110234234](hadoop3.0.assets/image-20220302110234234.png)

## HDFS组成架构

![image-20220303101939826](hadoop3.0.assets/image-20220303101939826.png)

![image-20220303102036145](hadoop3.0.assets/image-20220303102036145.png)

## HDFS文件块大小

HDFS中的文件在物理上是分块存储（Block），块的大小可以通过配置参数 ( dfs.blocksize）来规定，默认大小在Hadoop2.x/3.x版本中是128M，1.x版本中是64M。

1. 如果寻址时间约为10ms， 即查找到目标block的时间为 10ms。
2. 寻址时间为传输时间的1% 时，则为最佳状态。（专家） 因此，传输时间 =10ms/0.01=1000ms=1s
3. 而目前磁盘的传输速率普 遍为100MB/s。
4. block大小 =1s*100MB/s=100MB

**为什么块的大小不能太小，也不能太大呢？**

（1）HDFS的块设置**太小**，**会增加寻址时间**，程序一直在找块的开始位置； 

（2）如果块设置的**太大**，从**磁盘传输数据的时间会明显大于定位这个块开始位置所需的时间**。导致程序在处理这块数据时，会非常慢。

 **总结：HDFS块的大小设置主要取决于磁盘传输速率。**

# HDFS的shell操作

## 创建文件夹

```sh
hadoop fs -mkdir /sanguo
```

## 拷贝文件到HDFS路径中去

```sh
hadoop fs -put ./wuguo.txt /sanguo
/
hadoop fs -copyFromLocal weiguo.txt /sanguo
```

## 剪切文件到HDFS路径中去

```sh
 hadoop fs -moveFromLocal ./shuguo.txt /sanguo
```

## 追加到文件的末尾

```sh
hadoop fs -appendToFile liubei.txt /sanguo/shuguo.txt
```

## 下载文件到本地

```sh
hadoop fs -get /sanguo/shuguo.txt ./shuguo2.txt
/
hadoop fs -copyToLocal /sanguo/shuguo.txt ./
```

## 显示目录信息

```sh
hadoop fs -ls /sanguo
```

## 显示文件内容

```sh
hadoop fs -cat /sanguo/shuguo.txt
```

## 修改文件权限

```sh
hadoop fs -chmod 666 /sanguo/shuguo.txt
```

## 拷贝文件

```sh
hadoop fs -cp /sanguo/shuguo.txt /jinguo
```

## 移动文件

```sh
hadoop fs -mv /sanguo/weiguo.txt /jinguo
```

## 显示末尾1kb

```sh
hadoop fs -tail /jinguo/shuguo.txt
```

## 删除文件

```sh
hadoop fs -rm /sanguo/shuguo.txt
/ 递归删除
hadoop fs -rm -r /sanguo
```

**其实呢，操作都差不多的，记一下上传下载差不多了**

# HDFS的API操作

## win环境变量

![image-20220305124553451](hadoop3.0.assets/image-20220305124553451.png)

![image-20220305124636494](hadoop3.0.assets/image-20220305124636494.png)

## maven中添加依赖

```
<dependencies>
	<dependency>
		<groupId>org.apache.hadoop</groupId>
			<artifactId>hadoop-client</artifactId>
			<version>3.1.3</version>
	</dependency>
	<dependency>
		<groupId>junit</groupId>
		<artifactId>junit</artifactId>
		<version>4.12</version>
	</dependency>
	<dependency> 
		<groupId>org.slf4j</groupId>
		<artifactId>slf4j-log4j12</artifactId>
		<version>1.7.30</version>
	</dependency>
</dependencies>
```

**src/main/resources新建文件log4j.properties**

```
log4j.rootLogger=INFO, stdout
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%d %p [%c] - %m%n
log4j.appender.logfile=org.apache.log4j.FileAppender
log4j.appender.logfile.File=target/spring.log
log4j.appender.logfile.layout=org.apache.log4j.PatternLayout
log4j.appender.logfile.layout.ConversionPattern=%d %p [%c] - %m%n
```

## 新建目录

```java
public class HdfsClient {
    @Test
    // 新建目录
    public void testmkdir() throws InterruptedException, URISyntaxException, IOException {
        // 链接集群nn
        URI uri = new URI("hdfs://192.168.200.130:9000");
        // 创建配置文件
        Configuration configuration = new Configuration();
        // 用户
        String user = "root";
        // 获取客户端对象
        FileSystem fs = FileSystem.get(uri, configuration, user);
        // 要执行的命令
        fs.mkdirs(new Path("/xiyou/huaguoshan"));
        // 关闭资源
        fs.close();
    }
}
```

## 文件上传

```java
    @Test
    public void testCopyFromLocalFile() throws IOException,InterruptedException, URISyntaxException {
        // 1 获取文件系统
        Configuration configuration = new Configuration();
        configuration.set("dfs.replication", "2");
        FileSystem fs = FileSystem.get(new URI("hdfs://hadoop102:8020"), configuration, "atguigu");
        // 2 上传文件
        fs.copyFromLocalFile(new Path("d:/sunwukong.txt"), new Path("/xiyou/huaguoshan"));
        // 3 关闭资源
        fs.close();
    }
```

## 文件下载

```java
@Test
    public void testCopyToLocalFile() throws IOException,
            InterruptedException, URISyntaxException {
        // 1 获取文件系统
        Configuration configuration = new Configuration();
        FileSystem fs = FileSystem.get(new URI("hdfs://192.168.200.130:9000"),
                configuration, "root");

        // 2 执行下载操作
        // boolean delSrc 指是否将原文件删除
        // Path src 指要下载的文件路径
        // Path dst 指将文件下载到的路径
        // boolean useRawLocalFileSystem 是否开启文件校验
        fs.copyToLocalFile(false, new
                        Path("/xiyou/huaguoshan/sunwukong.txt"), new Path("d:/sunwukong2.txt"),
                true);

        // 3 关闭资源
        fs.close();
    }
```

## 文件改名或移动

```java
// 修改文件名称
fs.rename(new Path("/xiyou/huaguoshan/sunwukong.txt"), newPath("/xiyou/huaguoshan/meihouwang.txt"));
```

## 删除文件和目录

```java
fs.delete(new Path("/xiyou"), true);
```

## 文件详情查看

```java
@Test
    public void testListFiles() throws IOException, InterruptedException,
            URISyntaxException {
        // 1 获取文件系统
        Configuration configuration = new Configuration();
        FileSystem fs = FileSystem.get(new URI("hdfs://hadoop102:8020"),
                configuration, "atguigu");
        // 2 获取文件详情
        RemoteIterator<LocatedFileStatus> listFiles = fs.listFiles(new Path("/"),
                true);
        while (listFiles.hasNext()) {
            LocatedFileStatus fileStatus = listFiles.next();
            System.out.println("========" + fileStatus.getPath() + "=========");
            System.out.println(fileStatus.getPermission());
            System.out.println(fileStatus.getOwner());
            System.out.println(fileStatus.getGroup());
            System.out.println(fileStatus.getLen());
            System.out.println(fileStatus.getModificationTime());
            System.out.println(fileStatus.getReplication());
            System.out.println(fileStatus.getBlockSize());
            System.out.println(fileStatus.getPath().getName());
            // 获取块信息
            BlockLocation[] blockLocations = fileStatus.getBlockLocations();
            System.out.println(Arrays.toString(blockLocations));
        }
        // 3 关闭资源
        fs.close();
    }
```

# HDFS写数据流程

![image-20220305151450000](hadoop3.0.assets/image-20220305151450000.png)

# HDFS读数据流程

![image-20220305151647781](hadoop3.0.assets/image-20220305151647781.png)

## **NN** **和** **2NN** 的工作机制

**NameNode 中的元数据是存储在哪里的？**

​	首先，我们做个假设，如果存储在 NameNode 节点的磁盘中，因为经常需要进行随机访问，还有响应客户请求，必然是效率过低。

​	**因此，元数据需要存放在内存中**。

​	但如果只存在内存中，一旦断电，元数据丢失，整个集群就无法工作了。因此产生在磁盘中备份元数据的FsImage。

​	这样又会带来新的问题，当在内存中的元数据更新时，如果同时更新 FsImage，就会导致效率过低，但如果不更新，就会发生**一致性问题**，一旦 NameNode 节点断电，就会产生数据丢失。

​	因此，**引入 Edits 文件**（只进行追加操作，效率很高）。每当元数据有更新或者添加元数据时，修改内存中的元数据并追加到 Edits 中。这样，一旦 NameNode 节点断电，可以通过 **FsImage 和 Edits 的合并，合成元数据。**但是，如果长时间添加数据到 Edits 中，会导致该文件数据过大，效率降低，而且一旦断电，恢复元数据需要的时间过长。

​	因此，需要**定期进行 FsImage 和 Edits 的合并**，如果这个操作由NameNode节点完成，又会效率过低。**因此，引入一个新的节点SecondaryNamenode，专门用于 FsImage 和 Edits 的合并。**

![image-20220305153305303](hadoop3.0.assets/image-20220305153305303.png)

1）第一阶段：**NameNode启动**

（1）第一次启动 NameNode 格式化后，创建 Fsimage 和 Edits 文件。如果不是第一次启动，直接加载编辑日志和镜像文件到内存。

（2）客户端对元数据进行增删改的请求。 

（3）NameNode 记录操作日志，更新滚动日志。 

（4）NameNode 在内存中对元数据进行增删改。 

2）第二阶段：**Secondary NameNode工作**

（1）Secondary NameNode 询问 NameNode 是否需要 CheckPoint。直接带回 NameNode是否检查结果。

（2）Secondary NameNode 请求执行 CheckPoint。 

（3）NameNode 滚动正在写的 Edits 日志。 

（4）将滚动前的编辑日志和镜像文件拷贝到 Secondary NameNode。 

（5）Secondary NameNode 加载编辑日志和镜像文件到内存，并合并。

（6）生成新的镜像文件 fsimage.chkpoint。 

（7）拷贝 fsimage.chkpoint 到 NameNode。 

（8）NameNode 将 fsimage.chkpoint 重新命名成 fsimage。

# **DataNode** 工作机制

![image-20220305153332800](hadoop3.0.assets/image-20220305153332800.png)

（1）一个数据块在 DataNode 上以文件形式存储在磁盘上，包括两个文件，一个是数据本身，一个是元数据包括数据块的长度，块数据的校验和，以及时间戳。 

（2）DataNode 启动后向 NameNode 注册，通过后，周期性（6 小时）的向 NameNode 上报所有的块信息。

（3）心跳是每 3 秒一次，心跳返回结果带有 NameNode 给该 DataNode 的命令如复制块数据到另一台机器，或删除某个数据块。如果超过 10 分钟没有收到某个 DataNode 的心跳，则认为该节点不可用。

（4）集群运行中可以安全加入和退出一些机器。 

# MapReduce的定义

​	MapReduce 是一**个分布式运算程序**的编程框架，是用户开发“基于 Hadoop 的数据分析应用”的核心框架。

​	MapReduce 核心功能是将**用户编写的业务逻辑代码和自带默认组件整合成一个完整的分布式运算程序**，并发运行在一个 Hadoop 集群上。

# MapReduce优缺点

## 优点

1. **MapReduce 易于编程**

​		**它简单的实现一些接口，就可以完成一个分布式程序**，这个分布式程序可以分布到大量廉价的 PC 机器上运行。也就是说你写一个分布式程序，跟写一个简单的串行程序是一模一样的。就是因为这个特点使得 MapReduce 编程变得非常流行。

2. **良好的扩展性**

​		当你的计算资源不能得到满足的时候，你可以通过**简单的增加机器**来扩展它的计算能力。

3. **高容错性**

​		MapReduce 设计的初衷就是使程序能够部署在**廉价的 PC 机器**上，这就要求它具有很高的**容错性**。比如其中一台机器挂了，它可以把**上面的计算任务转移到另外一个节点上运行**，不至于这个任务运行失败，而且这个过程不需要人工参与，而完全是由 Hadoop 内部完成的。

4. **适合PB级以上海量数据的离线处理**

​		可以实现上千台服务器集群并发工作，提供数据处理能力。

## 缺点

1. **不擅长实时计算**
2. **不擅长流式计算**

流式计算的输入数据是动态的，而 MapReduce 的输入数据集是静态的，不能动态变化。这是因为 MapReduce 自身的设计特点决定了数据源**必须是静态的**。

3. **不擅长** **DAG****（有向无环图）计算**

# MapReduce编程





