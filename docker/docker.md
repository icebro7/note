# docker

# 一、基本原理和概念

```
隔离的进程独立于宿主机和其他隔离的进程，也被称之为容器
golang开发而来。对进程进行封装隔离，属于操作系统层面的虚拟化技术

核心组件
。image镜像，构建容器(我们讲应用程序运行所需的环境，打包为镜像文件)
。Container，容器(你的应用程序，就跑在容器中)
。镜像仓库(dockerhub)(保存镜像文件，提供上传，下载镜像)作用好比github
。Dockerfile，将你部署项目的操作，写成一个部署脚本，这就是dockerfile，且该脚本还能够构建出镜像文件
```





# 二、docker安装

## centos

```
。获取镜像，如 docker pu11 centos ，从镜像仓库拉取
。使用镜像创建容器
。分配文件系统，挂载一个读写层，在读写层加载镜像
。分配网络/网桥接口，创建一个网络接口，让容器和宿主机通信
。容器获取IP地址
。执行容器命令，如/bin/bash
。反馈容器启动结果。
```

### 1、移除以前docker相关包

```
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```



###  2、配置yum源

```
sudo yum install -y yum-utils
sudo yum-config-manager \
--add-repo \
http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

```

`若显示yum无法检索列表，首先备份当前yum源，再下载新的CentOS-Base.repo`

```
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup

wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
```



### 3、安装docker

```
sudo yum install -y docker-ce docker-ce-cli containerd.io
```



### 4、启动

```
systemctl enable docker --now
```



### 5、配置加速

```
sudo mkdir -p /etc/docker

sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://82m9ar63.mirror.aliyuncs.com"],
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2"
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```





## Ubuntu

```
1、更新系统包
sudo apt update
sudo apt upgrade


2、安装依赖
sudo apt install apt-transport-https ca-certificates curl software-properties-common


3、添加 Docker 的官方 GPG 密钥和 APT 仓库
curl -fsSL https://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null


4、更新 APT 包索引
sudo apt update

5、安装 Docker
sudo apt install docker-ce docker-ce-cli containerd.io

6、将当前用户添加到docker组
sudo usermod -aG docker $USER
```



# 三、docker部署

![](C:\Users\98680\Desktop\学习笔记\docker\img\image.png)



### 1、找镜像

```
docker pull nginx  #下载最新版

镜像名:版本名（标签）

docker pull nginx:1.20.1


docker pull redis  #下载最新
docker pull redis:6.2.4

## 下载来的镜像都在本地
docker images  #查看所有镜像

redis = redis:latest

docker rmi 镜像名:版本号/镜像id
```



### 2、启动容器

```
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]

【docker run  设置项   镜像名  】 镜像启动运行的命令（镜像里面默认有的，一般不会写）

# -d：后台运行
# --restart=always: 开机自启，从80端口映射88端口 
docker run --name=mynginx   -d  --restart=always -p  88:80   nginx




# 查看正在运行的容器
docker ps
# 查看所有
docker ps -a
# 删除停止的容器
docker rm  容器id/名字
docker rm -f mynginx   #强制删除正在运行中的

#停止容器
docker stop 容器id/名字
#再次启动
docker start 容器id/名字

#应用开机自启
docker update 容器id/名字 --restart=always
```



### 3、修改容器内容

```
# 进入容器内部的系统，修改容器内容

docker exec -it 容器id  /bin/bash

```

```
# 在主机的/data/html进行挂载，修改容器内容

docker run --name=mynginx   \
-d  --restart=always \
-p  88:80 -v /data/html:/usr/share/nginx/html:ro  \
nginx


```



### 4、提交修改

```
docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]

docker commit -a "ice"  -m "首页变化" 341d81f7504f app:v1.0

```



### 5、镜像文件传输

```
# 将镜像保存成压缩包
docker save -o abc.tar app:v1.0

# 别的机器加载这个镜像
docker load -i abc.tar


# 离线安装
```



### 6、镜像推送远程仓库传输

```
# 把旧镜像的名字，改成仓库要求的新版名字
docker tag app:v1.0 leifengyang/guignginx:v1.0

# 登录到docker hub
docker login       


docker logout（推送完成镜像后退出）

# 推送
docker push leifengyang/guignginx:v1.0


# 别的机器下载
docker pull leifengyang/guignginx:v1.0
```



### 7、补充

```
docker logs 容器名/id   排错

docker exec -it 容器id /bin/bash


# docker 经常修改nginx配置文件,挂载
docker run -d -p 80:80 \
-v /data/html:/usr/share/nginx/ html:ro \
-v /data/conf/nginx.conf:/etc/nginx/nginx.conf \
--name mynginx-02 \
nginx


#把容器指定位置的东西复制出来 
docker cp 5eff66eec7e1:/etc/nginx/nginx.conf  /data/conf/nginx.conf
#把外面的内容复制到容器里面
docker cp  /data/conf/nginx.conf  5eff66eec7e1:/etc/nginx/nginx.conf
```





