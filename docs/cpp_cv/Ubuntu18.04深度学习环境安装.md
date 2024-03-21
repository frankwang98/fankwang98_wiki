








#### 文章目录


* + [1. 环境安装](#1__1)
	+ [2. 显卡查看与配置](#2__17)
	+ [3. 安装CUDA](#3_CUDA_28)
	+ [4. 安装cudnn深度神经网络基元库](#4_cudnn_55)
	+ [5. python3环境配置](#5_python3_66)




### 1. 环境安装


首先默认大家已安装好Ubuntu 18.04系统。


安装gcc、cmake：



```
sudo apt update
sudo apt-get install build-essential 
sudo apt-get install cmake

```

查看编译环境版本：



```
gcc  --version

cmake --version 

```

### 2. 显卡查看与配置


打开“软件与更新”，找到“附加驱动”，选择适合自己的驱动并应用，然后重启计算机；


![在这里插入图片描述](https://img-blog.csdnimg.cn/0874b58e1bea4cb38f576e17866a23c6.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBARnJhbmvlrabkuaDot6_kuIo=,size_20,color_FFFFFF,t_70,g_se,x_16)  
 打开终端，输入`nvidia-smi`查看显卡信息；  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/2135bfe8ffea42b88e998d232127053a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBARnJhbmvlrabkuaDot6_kuIo=,size_19,color_FFFFFF,t_70,g_se,x_16)


更详细的深度学习环境配置参见：  
 [Ubuntu系统深度学习环境配置](https://blog.whuzfb.cn/blog/2020/12/13/ubuntu_config_deep_learning_new/)


### 3. 安装CUDA


搜索CUDA Toolkit 11.0（对应版本号），选择对应的系统和位数，官方会提供安装命令；  
 如下所示：



```
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/cuda-ubuntu1804.pin
sudo mv cuda-ubuntu1804.pin /etc/apt/preferences.d/cuda-repository-pin-600
wget https://developer.download.nvidia.com/compute/cuda/11.0.3/local_installers/cuda-repo-ubuntu1804-11-0-local_11.0.3-450.51.06-1_amd64.deb
sudo dpkg -i cuda-repo-ubuntu1804-11-0-local_11.0.3-450.51.06-1_amd64.deb
sudo apt-key add /var/cuda-repo-ubuntu1804-11-0-local/7fa2af80.pub
sudo apt-get update
sudo apt-get install cuda

```

安装完成后，在终端输入`cd /usr/local/cuda-11.0/bin && ./nvcc -V`，得到如下输出则表示安装成功。  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/1baa53a49d7b4875bce8bb286cb5f380.png)  
 但为了方便深度学习软件的使用，还要把相关路径加入PATH。打开文件~/.profile（若不存在则新建） ，在文档末尾添加以下内容：



```
# set PATH for cuda 11.0(对应版本) installation
if [ -d "/usr/local/cuda-11.0/bin/" ]; then
    export PATH=/usr/local/cuda-11.0/bin${PATH:+:${PATH}}
    export LD\_LIBRARY\_PATH=/usr/local/cuda-11.0/lib64${LD\_LIBRARY\_PATH:+:${LD\_LIBRARY\_PATH}}
fi

```

重启计算机使环境生效。


### 4. 安装cudnn深度神经网络基元库


[选择适合自己系统的版本下载](https://developer.nvidia.com/rdp/cudnn-archive)  
 点击`cuDNN v8.0.4 (September 28th, 2020), for CUDA 11.0`并根据自己的操作系统选择合适的版本。其中，`cuDNN Runtime Library`和`cuDNN Developer Library`是必须要下载的，`cuDNN Code Samples and User Guide`为可选项目。然后依次安装前面下载的几个文件：



```
sudo dpkg -i libcudnn8_8.0.4.30-1+cuda11.0_amd64.deb
sudo dpkg -i libcudnn8-dev_8.0.4.30-1+cuda11.0_amd64.deb
sudo dpkg -i libcudnn8-samples_8.0.4.30-1+cuda11.0_amd64.deb

```

此时，显卡已经配置完成。


### 5. python3环境配置


创建基于python3的虚拟环境，然后安装深度学习需要用到的库：



```
# 安装python3开发库
sudo apt-get install python3-pip python3-venv
# 创建名称为myvenv的虚拟环境
python3 -m venv myvenv
# 激活myvenv虚拟环境
source myvenv/bin/activate
# pip安装深度学习相关第三方库
pip install tensorflow-gpu

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/2ebef6918d8241be8d3912b5df1f5647.png)


监控显卡性能：`watch -n 1 nvidia-smi`


以上。





