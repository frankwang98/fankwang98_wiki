







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍Windows端深度学习环境配置。  
>  **学其所用，用其所学。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. 训练环境cuda+miniconda+pycharm+Pytorch](#smirk1_cudaminicondapycharmPytorch_7)
	+ [:blush:2. 测试安装](#blush2__34)




### 😏1. 训练环境cuda+miniconda+pycharm+Pytorch


有显卡的笔记本或台式可通过`nvidia-smi`查看最高支持cuda版本，例如我的支持12.0，安装了cuda11.8（`nvcc -V`）。


cuda11.8下载地址：`https://developer.nvidia.com/cuda-11-8-0-download-archive?target_os=Windows&target_arch=x86_64&target_version=11&target_type=exe_local`


安装cuDNN加速库，一定要和cuda版本对应（可选）。


cuDNN下载地址：`https://developer.nvidia.com/rdp/cudnn-archive`


miniconda相对anaconda更小巧灵活，也可创建多个python环境，可在国内源中下载安装（这里我选择了py38版本）。


miniconda下载地址：`https://mirrors.tuna.tsinghua.edu.cn/anaconda/miniconda/`


pycharm是python一个好用的IDE，这个比较好装。


pycharm下载地址：`https://www.jetbrains.com/pycharm/download/?section=windows`


pytorch是模型训练常用的框架/库，这里安装的时候要对应自己的环境，例如我的是Windows+pip+cuda11.8，然后就会有安装命令出来。


pytorch下载地址：`https://pytorch.org/`


安装其他依赖项，如：



```
pip install opencv-python

```

### 😊2. 测试安装


一个简单的 Python 脚本来测试 PyTorch GPU 版本是否正常工作：



```
import torch

device = torch.device("cuda" if torch.cuda.is_available() else "cpu")

x = torch.tensor([1., 2.]).to(device)
y = torch.tensor([3., 4.]).to(device)

z = x + y

print("Device:", device)
print("Result:", z)

```

如果安装成功，将输出：



```
Device: cuda
Result: tensor([4., 6.], device='cuda:0')

```

![请添加图片描述](https://img-blog.csdnimg.cn/5ea93bb657184b9eb8515cc76047c16a.png)


以上。





