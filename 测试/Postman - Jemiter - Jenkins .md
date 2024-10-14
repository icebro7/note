# Postman - Jemiter - Jenkins 



## 一、postman

### 1、安装部分

```
1、postman
遇到错误：打开软件一直处于加载状态
解决方法：删除C:\Users\986807\data\Roaming\Postman文件，并且重新打开软件

2、newman
npm i newman
```



### 2、使用部分

##### 1、基础使用

```
1、复制接口的CURL(bash)
2、粘贴至request中，可查看不同语言的request语句
```

![image-20240121112928265](C:\Users\98680\Desktop\学习笔记\测试\img\image-20240121112928265.png)

```
3、tests中可以查看用法(https://learning.postman.com/docs/writing-scripts/test-scripts/)
```

![image-20240121113026870](C:\Users\98680\Desktop\学习笔记\测试\img\image-20240121113026870.png)

 

```
4、可以导出编辑好的脚本导出，通过命令行newman运行测试脚本:newman run xxx.json
```

![image-20240121120941322](C:\Users\98680\Desktop\学习笔记\测试\img\image-20240121120941322.png)

![image-20240121165257515](C:\Users\98680\Desktop\学习笔记\测试\img\image-20240121165257515.png)



```
5、使用allure生成更为优雅的测试报告文件
newman run xxx.json --reporters allure --reporters-allure-export allure-results

将生成的allure的json格式生成为html格式：
allure serve
```



#####   2、接口关联

```
1、JSON提取器
//打印
console.log(responseBody)
//通过JSON提取器提取鉴权码token，把返回值转化成json格式的字典
var jsData = JSON.parse(responseBody)
//提取token值
console.log(jsData.access_token)
//把鉴权码设置为全局变量
pm.globals.set("access_token",jsData.access_token)

2、正则表达式提取
// match匹配
var jsData = responseBody.match(new RegExp('左边界(.+?)右边界'))
```



##### 3、动态参数

```
内置动态参数：
{{$timestamp}}	 获得时间戳
{{$randint}}	 获得0-1000的随机数
{{$guid}}		 获得guid的随机字符串
```

 

## 二、Jenkins

### 1、安装部分

```bash
Windows：
官网下载war包

在下载目录中启动终端，输入命令：java -jar jenkins.war
在127.0.0.1：8080页面打开

等待页面加载，下载插件并且登陆

Linux：

docker拉取镜像
docker pull jenkins/jenkins

创建文件夹
mkdir -p /mydata/jenkins_home


Ubuntu：


更新包系统：
sudo apt update
sudo apt upgrade

安装Java：
sudo apt install openjdk-17-jdk

添加源
wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -

sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'

更新包索引
sudo apt update

安装jenkins：
sudo apt install jenkins

启动服务
sudo systemctl start jenkins
sudo systemctl enable jenkins
sudo systemctl status jenkins
```



```bash
windos
具体构建任务的参数：
	源码管理 --拉起svn或者git上面的文件
	构建触发器 --任务执行的多种方式（定时执行、远程触发）
	构建 --任务运行内容的主体
	

Linux

设置权限
chown -R 1000 /mydata/jenkins_home

启动jenkins容器
docker run -di --name=jenkins -p 8080:8080 -v /mydata/jenkins_home/:/var/jenkins_home jenkins/jenkins

网页打开服务
ip:8080

解锁jenkins
docker logs jenkins

```



### 2、配置部分

```
1、安装jdk
sudo apt update
sudo apt install openjdk-11-jdk
java -version

2、安装git
sudo apt update
sudo apt install git
git --version

3、安装maven
sudo apt install maven
mvn -version

4、全局工具配置
jdk路径
/usr/lib/jvm/

git路径
/usr/bin/git

maven路径
/usr/share/maven

5、ssh配置
加入服务器ip和端口
配置凭据
检测
```



### 3、应用配置

```
创建项目
选择git源码
填入相应信息执行
```

