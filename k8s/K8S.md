# K8S

# 一、基础概念

### Node

```
集群节点，可以是虚拟机也可以是物理机
```



### Pod

```
最小调度单元，一个或者多个容器的组合
```



### Service

```
将一组Pod封装成一个服务，提供一个统一的入口来访问这个服务
```



### Ingress

```
将外部的请求路由转发到集群内部的Service上
```



### ConfigMap、Secret

```
将一些配置信息和敏感信息封装起来，避免配置变更带来的重新编译和部署的问题
```



### Volumes

```
将一些数据挂载到集群中的本地磁盘或者远程存储上，实现集群中数据的持久化存储
```



### Deployment

```
部署无状态应用程序，将一个或多个Pod组合在一起，并且具有副本控制、滚动更新、自动拓缩容
```



### StatefulSet

```
部署有状态应用程序，将一个或多个Pod组合在一起，并且具有副本控制、滚动更新、自动拓缩容
```



# 二、架构



### Master-Node

![image-20240713140913863](C:\Users\98680\Desktop\学习笔记\k8s\img\image-20240713140913863.png)

#### API-server

```
1、提供集群的API接口服务，所有组件通过这个接口进行通信
2、整个系统的网关和入口，所有请求先经过他再分发给不同的组件进行处理
3、增删改查等操作进行认证、授权和访问控制
```



#### Scheduler调度器

```
监控集群中所有节点的资源使用情况，根据策略将Pod调度到合适的节点运行
```



#### ControllerManager(管理器控制器)

```
管理集群中各节点的状态
```



#### etcd

```
数据存储中心
高可用的键-值存储系统
存储集群中所有资源对象的状态信息
```



#### c-c-m(云控制管理器)

```
云平台相关控制器，负责与云平台的api进行交互
提供一致的管理接口
```





### Worker-Node

![image-20240713135958163](C:\Users\98680\Desktop\学习笔记\k8s\img\image-20240713135958163.png)

#### kubelet

```
1、负责管理和维护每个节点上的Pod，确保正确运行
2、定期从api-server组件接受新的或者修改后的Pod规范
3、监控运行情况，将信息汇报给api-server
```



#### kube-proxy

```
为Pod对象提供网络代理和负载均衡服务
```



#### container-runti(容器运行时)

```
运行容器的软件，负责拉取容器镜像、创建容器、启动或停止容器
```



# 三、环境搭建

### minikube搭建

```
# linux
1、镜像下载：
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
(可以官网下载拉取)

sudo install minikube-linux-amd64 /usr/local/bin/minikube



2、切换用户保证安全：
sudo useradd -m -s /bin/bash minikubeuser

sudo passwd minikubeuser

sudo usermod -aG docker minikubeuser

su - minikubeuser



3、启动minik8s：
minikube start
```



### multipass + k3s

```
# linux

1、下载epel储存库
yum install epel-release

2、安装snapd
yum install snapd

3、启动管理主 snap 通信套接字的systemd单元
systemctl enable --now snapd.socket

4、启用经典/var/lib/snapd/snapsnap 支持
ln -s /var/lib/snapd/snap /snap

5、运行snapd
systemctl start snapd

6、安装multipass
snap install multipass




```



### 云端k8s

```
killercode
```



# 四、常用命令

```
获取节点信息
kubectl get nodes

获取服务信息
kubectl get svc

获取Pod信息
kubectl get pod 

获取ip信息
kubectl get pod -o wide

获取部署信息
kubectl get deployment

获取replicaset信息
kubectl get replicaset

查看所有节点信息
kubectl get all

查看日志
kubectl logs nginx-deployment-c45d79c8

查看服务信息
kubectl describe service nginx-deployment



创建Pod
kubectl run nginx --image=nginx

创建部署
kubectl create deployment nginx-deployment --image=nginx

创建服务
kubectl create service nginx-service



删除
kubectl delete deployment nginx-deployment



进入容器
kubectl exec -it nginx-deployment-c45d79c8-zf6tx -- /bin/bash

访问服务(内部)
curl IP


```



### 4.1 基础使用

```bash
# 查看帮助
kubectl --help

# 查看API版本
kubectl api-versions

# 查看集群信息
kubectl cluster-info
```

### 4.2 资源的创建和运行

```bash
# 创建并运行一个指定的镜像
kubectl run NAME --image=image [params...]
# e.g. 创建并运行一个名字为nginx的Pod
kubectl run nginx --image=nginx

# 根据YAML配置文件或者标准输入创建资源
kubectl create RESOURCE
# e.g.
# 根据nginx.yaml配置文件创建资源
kubectl create -f nginx.yaml
# 根据URL创建资源
kubectl create -f https://k8s.io/examples/application/deployment.yaml
# 根据目录下的所有配置文件创建资源
kubectl create -f ./dir

# 通过文件名或标准输入配置资源
kubectl apply -f (-k DIRECTORY | -f FILENAME | stdin)
# e.g.
# 根据nginx.yaml配置文件创建资源
kubectl apply -f nginx.yaml
```

### 4.3 查看资源信息

```bash
# 查看集群中某一类型的资源
kubectl get RESOURCE
# 其中，RESOURCE可以是以下类型：
kubectl get pods / po         # 查看Pod
kubectl get svc               # 查看Service
kubectl get deploy            # 查看Deployment
kubectl get rs                # 查看ReplicaSet
kubectl get cm                # 查看ConfigMap
kubectl get secret            # 查看Secret
kubectl get ing               # 查看Ingress
kubectl get pv                # 查看PersistentVolume
kubectl get pvc               # 查看PersistentVolumeClaim
kubectl get ns                # 查看Namespace
kubectl get node              # 查看Node
kubectl get all               # 查看所有资源

# 后面还可以加上 -o wide 参数来查看更多信息
kubectl get pods -o wide

# 查看某一类型资源的详细信息
kubectl describe RESOURCE NAME
# e.g. 查看名字为nginx的Pod的详细信息
kubectl describe pod nginx
```

### 4.4 资源的修改、删除和清理

```bash
# 更新某个资源的标签
kubectl label RESOURCE NAME KEY_1=VALUE_1 ... KEY_N=VALUE_N
# e.g. 更新名字为nginx的Pod的标签
kubectl label pod nginx app=nginx

# 删除某个资源
kubectl delete RESOURCE NAME
# e.g. 删除名字为nginx的Pod
kubectl delete pod nginx

# 删除某个资源的所有实例
kubectl delete RESOURCE --all
# e.g. 删除所有Pod
kubectl delete pod --all

# 根据YAML配置文件删除资源
kubectl delete -f FILENAME
# e.g. 根据nginx.yaml配置文件删除资源
kubectl delete -f nginx.yaml

# 设置某个资源的副本数
kubectl scale --replicas=COUNT RESOURCE NAME
# e.g. 设置名字为nginx的Deployment的副本数为3
kubectl scale --replicas=3 deployment/nginx

# 根据配置文件或者标准输入替换某个资源
kubectl replace -f FILENAME
# e.g. 根据nginx.yaml配置文件替换名字为nginx的Deployment
kubectl replace -f nginx.yaml
```

### 4.5 调试和交互

```bash
# 进入某个Pod的容器中
kubectl exec [-it] POD [-c CONTAINER] -- COMMAND [args...]
# e.g. 进入名字为nginx的Pod的容器中，并执行/bin/bash命令
kubectl exec -it nginx -- /bin/bash

# 查看某个Pod的日志
kubectl logs [-f] [-p] [-c CONTAINER] POD [-n NAMESPACE]
# e.g. 查看名字为nginx的Pod的日志
kubectl logs nginx

# 将某个Pod的端口转发到本地
kubectl port-forward POD [LOCAL_PORT:]REMOTE_PORT [...[LOCAL_PORT_N:]REMOTE_PORT_N]
# e.g. 将名字为nginx的Pod的80端口转发到本地的8080端口
kubectl port-forward nginx 8080:80

# 连接到现有的某个Pod（将某个Pod的标准输入输出转发到本地）
kubectl attach POD -c CONTAINER
# e.g. 将名字为nginx的Pod的标准输入输出转发到本地
kubectl attach nginx

# 运行某个Pod的命令
kubectl run NAME --image=image -- COMMAND [args...]
# e.g. 运行名字为nginx的Pod
kubectl run nginx --image=nginx -- /bin/bash

```

## 























