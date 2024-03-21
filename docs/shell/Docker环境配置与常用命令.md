







> 
> Docker 是一个开源的应用容器引擎，基于Go语言，能够将应用程序与基础设施分离，让开发者可以打包他们的应用以及依赖包到一个可移植的容器中，然后发布到任何流行的Linux或Windows 操作系统的机器上，也可以实现虚拟化、容器完全使用沙箱机制，彼此之间没有任何接口。
> 
> 
> 




#### 文章目录


* + [1. Docker介绍](#1_Docker_3)
	+ [2. Docker环境安装](#2_Docker_24)
	+ - [Windows端](#Windows_25)
		- [Ubuntu端](#Ubuntu_64)
	+ [3. Docker常用命令](#3_Docker_66)
	+ [4. 常见问题](#4__163)




### 1. Docker介绍


Docker 是一个开源的容器化平台，用于构建、发布和运行应用程序。通过使用容器技术，Docker 允许开发人员将应用程序及其依赖项打包为一个独立的、可移植的容器，以确保应用程序在不同环境中具有一致的运行行为。


以下是 Docker 的一些核心概念和特性：



> 
> 1.容器：容器是一个轻量级、独立运行的软件单元，包含了应用程序及其所有依赖项。与虚拟机不同，容器之间共享操作系统内核，并且可以更高效地启动、停止和迁移。容器提供了一个隔离的执行环境，可以确保应用程序在不同环境中的一致性和可移植性。
> 
> 
> 



> 
> 2.镜像：镜像是容器的基础，它包含了一个完整的文件系统和运行时所需的所有组件，如代码、运行时环境、库、环境变量等。镜像是只读的，通过镜像可以创建多个可运行的容器。Docker Hub 是一个公共的镜像注册表，供用户分享和获取镜像。
> 
> 
> 



> 
> 3.容器注册表：容器注册表用于存储和分享镜像。Docker Hub 是 Docker 的默认公共注册表，包含了大量的官方和第三方镜像。除了 Docker Hub，还可以搭建私有的容器注册表来管理自己的镜像。
> 
> 
> 



> 
> 4.Dockerfile：Dockerfile 是一个文本文件，用于定义如何构建一个 Docker 镜像。通过编写 Dockerfile，可以指定镜像的基础操作系统、安装依赖项、配置环境变量、运行命令等。使用 Dockerfile 可以实现镜像的版本控制和自动化构建。
> 
> 
> 



> 
> 5.容器编排：Docker 提供了一些工具和技术来协调和管理多个容器的部署和管理，例如 Docker Compose、Docker Swarm、Kubernetes 等。这些工具使得容器的编排和扩展变得更加简单和可靠。
> 
> 
> 


Docker 的优势包括：



> 
> * 简化环境配置：使用 Docker，开发人员可以将应用程序及其依赖项打包为一个独立的容器，消除了环境配置的复杂性和不一致性。
> * 提高可移植性：Docker 容器可以在不同的环境中运行，保证了应用程序的可移植性和一致性。
> * 资源利用率高：由于容器共享操作系统内核，相比传统虚拟化技术，Docker 的容器更加轻量级，可以更高效地利用资源。
> * 快速部署和扩展：Docker 容器可以快速启动、停止和迁移，可以方便地进行应用程序的部署和扩展。
> 
> 
> 


### 2. Docker环境安装


#### Windows端


首先打开Hyper-V虚拟化环境，并用`systeminfo`查看。


![在这里插入图片描述](https://img-blog.csdnimg.cn/d22c02218d7546e496332691cb38a97c.png)


然后打开地址下载：`https://docs.docker.com/desktop/install/windows-install/`


![在这里插入图片描述](https://img-blog.csdnimg.cn/14fed37f6f9142c6bd172b11ab8b6013.png)


若出现错误hardware assisted virtualization and data execution protection must be enable，执行：`bcdedit /set hypervisorlaunchtype Auto`


若出现错误Update the WSL kernel by running “wsl --update” or follow instructions ，参考：`https://learn.microsoft.com/en-us/windows/wsl/install-manual#step-4---download-the-linux-kernel-update-package`


首先安装`WSL2 Linux kernel update package for x64 machines`，


![在这里插入图片描述](https://img-blog.csdnimg.cn/9dacfe8656c0485a8121ada1584ce935.png)


然后配置wsl版本并重启：



```
wsl --set-default-version 2

```

docker desktop登录后可以方便访问`dockerhub`资源。可在`Setting-Resources`中配置镜像保存的目录，网络等。在`Setting-Docker Engine`中配置国内镜像源 ，即在后面加上（配置间用，隔开）：



```
"registry-mirrors": [
    "https://docker.mirrors.ustc.edu.cn",
    "https://registry.docker-cn.com",
    "http://hub-mirror.c.163.com",
    "https://mirror.ccs.tencentyun.com"
]

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/60a35486cc4b4209983b7beb27433f52.png)


然后测试demo：在图形窗口运行或CLI运行 `docker run hello-world`


![在这里插入图片描述](https://img-blog.csdnimg.cn/41a4211ca99f4eb498d8c6c922a03bd4.png)


#### Ubuntu端


可以使用国内`daocloud`的一键安装命令：`curl -sSL https://get.daocloud.io/docker | sh`


### 3. Docker常用命令


下面是Docker的常用命令：[Docker常用命令大全](https://mp.weixin.qq.com/s/8AUx_ivbMbydb143SkwbgQ)


常用命令：



```
info
version
run # 运行容器 （-it表示交互，-d表示后台运行）
start/stop/restart # 启动、停止、重启
kill
rm # 删除容器 （docker rm -f 1e560fca3906）
create
exec # 退出后容器不会终止
pause/unpause
inspect # 查看 Docker 的底层信息
top # 查看容器内部运行的进程
events
logs # 查看容器日志
export # 导出容器到本地 （docker export 1e560fca3906 > ubuntu.tar）
import # 导入本地到容器 （cat docker/ubuntu.tar | docker import - test/ubuntu:v1）
port # 查看端口映射情况
commit # 提交容器副本 （docker commit -m="has update" -a="runoob" e218edb10161 runoob/ubuntu:v2）
cp
diff # 对比
login/logout
pull # 获取镜像
push # 推送镜像 （docker push username/ubuntu:18.04）
search # 查找镜像
rmi # 删除镜像
tag # 设置镜像标签 （docker tag 860c279d2fec runoob/centos:dev）
build # 从零开始构建镜像 （Dockerfile）（docker build -t runoob/centos:6.7 .）
history
save
load
network # 查看网络 （docker run -itd --name test1 --network test-net ubuntu /bin/bash）

```

登录并创建镜像，上传到hub示例：



```
docker login/logout （也可在客户端登录，然后在wsl中直接使用）
# 先下载个官方镜像
docker search ubuntu
docker pull ubuntu
# 给这个镜像打上自己的tag
docker tag ubuntu wangzf98/ubuntu:22.04
# 查看并推送
docker images
docker push wangzf/ubuntu:22.04
# 然后就可以search到自己上传的镜像了
docker search wangzf98/ubuntu

```

运行NVIDIA镜像示例：



```
docker images # 查看所有镜像
docker ps  -a # 查看所有容器
# nvidia-docker镜像：https://hub.docker.com/r/nvidia/cuda
# docker pull nvidia/cuda:11.2.0-cudnn8-devel-ubuntu18.04
image=nvidia/cuda:11.2.0-cudnn8-devel-ubuntu18.04 # nvidia-docker基础镜像
image=docker.dm-ai.cn/algorithm-research/py38-cuda11.2-cudnn8.1-ubuntu18.04:base
image=docker.dm-ai.cn/algorithm-research/py38-cuda11.2-cudnn8.1-ubuntu18.04:torch1.8.1
image=docker.dm-ai.cn/algorithm-research/py38-cuda11.2-cudnn8.1-ubuntu18.04:torch1.8.1-trt8.2
image=docker.dm-ai.cn/algorithm-research/py38-cuda11.2-cudnn8.1-ubuntu18.04:torch1.8.1-trt8.4
docker run -it --gpus all -p 40000:80 -v `pwd`:/app $image /bin/bash
# 将容器转为镜像
docker commit -m "info" -a "test" container_id image_id:tag
docker push image_id:tag

```

此外，还可以基于`Dockerfile`构建自己的镜像，然后通过`Docker Compose`（docker-compose.yml）定义和运行多个容器，通过`docker-compose up`来启动。具体可[参考](https://www.runoob.com/docker/docker-compose.html)构建一个多容器。


Dockerfile示例：



```
FROM python:3.7-alpine
WORKDIR /code
ENV FLASK_APP app.py
ENV FLASK_RUN_HOST 0.0.0.0
RUN apk add --no-cache gcc musl-dev linux-headers
COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt
COPY . .
CMD ["flask", "run"]

```

docker-compose.yml示例：



```
# yaml 配置
version: '3'
services:
  web:
    build: .
    ports:
      - "5000:5000"
  redis:
    image: "redis:alpine"

```

### 4. 常见问题


Docker 网络模式有哪些？



```
host：使用 host 网络模式，容器的网络栈与 Docker 主机共享网络命名空间，容器不会被分配自己的 IP 地址。
bridge：它使用软件桥接，允许连接到同一桥接网络的容器进行通信，同时提供与未连接到该桥接网络的容器的隔离。
container：这种模式指定新创建的容器与现有容器共享网络命名空间，而不是与主机共享。
none：使用 none 模式，Docker 容器拥有自己的网络命名空间，但不为 Docker 容器进行任何网络配置。也就是说，该 Docker 容器没有网络接口卡、IP、路由和其他信息。在这种网络模式下，容器只有 lo 回环网络，没有其他网络接口卡。无法连接到此类型的网络，但封闭的网络可以确保容器的安全性。

```

以上。





