# Docker 学习

> 知识点

- docker概述

- docker安装

- docker命令

- docker镜像

- docker容器数据卷

- Dockerfile

- idea 整合docker

- docker compose

- docker swarm

  

#### docker概述

Java - jar --打包带上镜像---放进docker仓库----下载镜像 ---运行

核心思想：隔离，每个项目打包装箱，并且相互独立

使用的容器技术，相比于vm而言，十分轻巧。

虚拟机技术缺点

1.资源占用十分多

2.启动十分慢

3.虚拟出一套硬件，相当于一台电脑

容器的优点：

1.容器没有自己的内核，是运行在宿主机上的

2.每个容器相互隔离互不影响

#### docker的一些名词

![Docker Architecture Diagram](https://docs.docker.com/engine/images/architecture.svg)



镜像（images）:相当于模板，一个镜像可以创建多个容器

容器（containers）:相当于一个简单的linux系统

仓库（repository）:放镜像的仓库，有私有和公有的

docker 安装

```
1.卸载老的docker
sudo apt-get remove docker docker-engine docker.io containerd runc
2.更新索引
apt-get update
3. allow apt to use a repository over HTTPS:
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
 curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
 sudo apt-key fingerprint 0EBFCD88
 4.安装仓库
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
 $ sudo apt-get update
 $ sudo apt-get install docker-ce docker-ce-cli containerd.io
 5.运行docker hello 
 sudo docker run hello-world
```

> docker 常见命令

镜像命令

```
docker images
REPOSITORY               TAG                 IMAGE ID            CREATED             SIZE
luonna/hello             latest              120b96985d9d        22 months ago       127MB
friday1992/hello-world   latest              1b75c7e02fca        22 months ago       913kB
luonan/hello-world       latest              1b75c7e02fca        22 months ago       913kB
hello-world              latest              4ab4c602aa5e        23 months ago       1.84kB
ubuntu                   latest              cd6d8154f1e1        23 months ago       84.1MB
docker search mysql 查找镜像
可以 --filter
docker pull mysql 下载镜像 默认最新版本 :5.7
docker rmi -f 删除镜像
打包镜像
Dockerfile 
FROM java:8-alpine
ADD zn-unionpay.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java","-jar","/app.jar"]
jar 包和Dockerfile 放在同一个文件下下面执行build 命令
docker build -t 镜像名 .
运行镜像
docker run -d -p 主机端口:docker端口 镜像名
```

容器命令

```
docker run --参数 centos

--d 以后台方式运行
--P 制定端口 主机端口：容器端口
--p  随机制定端口
--it 可以进入容器查看内容  docker run -it centos /bin/bash
exit 退出容器
ctrl +p+Q 不停止退出
docker ps 查看运行容器 -a 查看历史


```

> dockerhub

```
推送镜像到dockerhub

```

