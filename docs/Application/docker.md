# Docker云原生实践

## 一、Docker介绍
Docker 是一个开源的应用容器引擎，基于 Go 语言 并遵从 Apache2.0 协议开源。

Docker 可以让开发者打包他们的应用以及依赖包到一个轻量级、可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。

容器是完全使用沙箱机制，相互之间不会有任何接口（类似 iPhone 的 app）,更重要的是容器性能开销极低。

Docker 从 17.03 版本之后分为 CE（Community Edition: 社区版） 和 EE（Enterprise Edition: 企业版），我们用社区版就可以了。

## 二、Docker安装
这里推荐[国内道客](https://get.daocloud.io/)做的一键安装脚本`curl -sSL https://get.daocloud.io/docker | sh`，省去了多条命令安装的繁杂。

如果想亲自试试手动安装，可以参考：[菜鸟Docker](https://www.runoob.com/docker/ubuntu-docker-install.html)

启动服务：`systemctl start docker.service`

开机自启：`sudo systemctl enable docker`

设置国内源

Docker在默认安装之后，通过命令docker pull 拉取镜像时，默认访问docker hub上的镜像，在国内网络环境下，下载时间较久或者拉取失败，所以要配置国内镜像仓库。

新建文件：`sudo gedit /etc/docker/daemon.json`

编辑：
```
{
    "registry-mirrors": ["https://docker.mirrors.ustc.edu.cn"]
}
```

重启docker：`systemctl restart docker.service`

查看更改：`sudo docker info`

其他加速地址：
```
##网易
http://hub-mirror.c.163.com
##Docker中国区官方镜像
https://registry.docker-cn.com
##中国科技大学
https://docker.mirrors.ustc.edu.cn
##阿里云容器  服务
https://cr.console.aliyun.com/
```
## 三、Docker常用命令
可以参考[Docker命令实践](http://t.csdn.cn/Oqqx1)

## 四、容器图形界面-Portainer
安装：
```
sudo systemctl restart docker
sudo docker pull portainer/portainer
sudo docker run -d -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock --restart=always --name prtainer portainer/portainer
```
进入127.0.0.1:9000设置帐号密码：
```
admin
21xyschb
```
进入local容器管理，通过Dashboard可查看image、container、volume、networks等。

还可以查看到运行容器内部的信息，也可以快速的删除废弃的容器及镜像。也可以构建虚拟网络实现容器间隔离。

## 五、Docker运行ros
搜索镜像：`sudo docker search ros`

选用一个桌面环境什么的都安装完整的melodic版本的docker镜像：`sudo docker pull osrf/ros:melodic-desktop-full`

创建Dockerfile（可选）：
```
mkdir rocker
cd rocker
vim Dockerfile
#######复制下面的Dockerfile
FROM osrf/ros:melodic-desktop-full
# nvidia-container-runtime
ENV NVIDIA_VISIBLE_DEVICES \
${NVIDIA_VISIBLE_DEVICES:-all}

ENV NVIDIA_DRIVER_CAPABILITIES \
${NVIDIA_DRIVER_CAPABILITIES:+$NVIDIA_DRIVER_CAPABILITIES,}graphics

RUN apt-get update && \
apt-get install -y \
build-essential \
libgl1-mesa-dev \
libglew-dev \
libsdl2-dev \
libsdl2-image-dev \
libglm-dev \
libfreetype6-dev \
libglfw3-dev \
libglfw3 \
libglu1-mesa-dev \
freeglut3-dev \
vim
```

构建容器（可选）：`docker build -t rocker .`

启动容器：
```
sudo xhost +local:
sudo docker run -it --device=/dev/dri --group-add video --volume=/tmp/.X11-unix:/tmp/.X11-unix  --env="DISPLAY=$DISPLAY"  --network host --name=rocker osrf/ros:melodic-desktop-full  /bin/bash
```
通过加入`--network host`可实现容器与本地环境互联，也就是容器启动的roscore节点是我本地机器的。

在容器内部有一个ros_entrypoint.sh的文件，`./ros_entrypoint.sh`执行这个脚本，然后就可以正常使用roscore和rviz了。

在使用rviz的时候我们当然需要再开启一个终端，那么对应的我们要进入启动roscore的这个容器：
```
sudo docker ps
###找到运行的rocker容器的id
####进入容器
sudo docker exec -it f62d8436e5c2 /bin/bash
rviz（我没显示出来）
```
即可显示rviz的界面，原理的话实际上是linux和UNIX对X11的支持。X11也叫做X Window系统，X Window系统 (X11或X)是一种 位图 显示的 视窗系统 。它是在 Unix 和 类Unix 操作系统 ，以及 OpenVMS 上建立图形用户界面的标准工具包和协议，并可用于几乎所有已有的现代操作系统。

或者选择带有vnc的ROS容器：`sudo docker pull tiryoh/ros-desktop-vnc:melodic`

启动：`sudo docker run -p 6080:80 --shm-size=512m --device=/dev/dri --group-add video --volume=/tmp/.X11-unix:/tmp/.X11-unix  --env="DISPLAY=$DISPLAY" -v /home/wangzf/7-Darknet_yolo_ros/Darknet_yolo_ros:/home/darknet --device=/dev/video2  --name=ros-vnc tiryoh/ros-desktop-vnc:melodic`

打开浏览器，输入`localhost:6080`

docker可以支持把一个宿主机上的目录挂载到镜像里。

`docker run -it -v /home/dock/downloads:/usr/downloads ubuntu64 /bin/bash`

通过-v参数，冒号前为宿主机目录，必须为绝对路径，冒号后为镜像内挂载的路径。

docker加载相机设备：`--device=/dev/video2`

USB设备加载后，需要更新权限：`sudo chmod 777 /dev/video2`

