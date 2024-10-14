# nginx

# 一、安装

```shell
# centos
# 1. 安装EPEL仓库
sudo yum install epel-release

# 2. 更新repo
sudo yum update

# 3. 安装nginx
sudo yum install nginx

# 4. 验证安装
sudo nginx -V



# Ubuntu
# 1. 更新仓库信息
sudo apt-get update

# 2. 安装nginx
sudo apt-get install nginx

# 3. 验证安装
sudo nginx -V
```



# 二、常用命令

```shell
# 启动Nginx
nginx 

# 指定配置⽂件
nginx -c filename 

# 查看Nginx的版本和编译参数等信息
nginx -V 

# 检查配置⽂件是否正确，也可⽤来定位配置⽂件的位置
nginx -t 

# 优雅停⽌Nginx
nginx -s quit 

# 快速停⽌Nginx
nginx -s stop 

# 重新加载配置⽂件
nginx -s reload 

# 重新打开⽇志⽂件
nginx -s reopen 
```





# 三、搭建简单示例

### 3、1安装前置环境

```shell
# 安装git
# 启动EPEL仓库
yum install -y epel-release

# 安装git
yum install -y git

# 验证安装
git --version


# 安装Node.js
# 官网下载二进制包
wget https://nodejs.org/dist/v14.17.0/node-v14.17.0-linux-x64.tar.xz

# 解压安装包
tar -xf node-v14.17.0-linux-x64.tar.xz

# 配置环境变量
编辑：
vi ~/.bashrc
添加具体解压目标路径：
export PATH=/path/to/node-v14.17.0-linux-x64/bin:$PATH

# 使环境生效
source ~/.bashrc

# 验证安装
node -v
npm -v
```



### 3、2安装Hexo

```

```



























