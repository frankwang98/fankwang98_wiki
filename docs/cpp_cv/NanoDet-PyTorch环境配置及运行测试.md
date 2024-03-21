







> 
> 近几年目标检测模型发展很快，最近接触到一款智能小车用到了Nanodet这种目标检测模型，便拿下来试一试，在这过程中，发现一些作者在环境配置方面未提到的细节并在requirements.txt中进行了完善，可以说是手把手教你运行这个目标检测模型。
> 
> 
> 


**完善后的模型文件如下：**  
 `https://download.csdn.net/download/qq_40344790/62403360`




#### 文章目录


* + - [1. 准备NanoDet-PyTorch工程](#1_NanoDetPyTorch_6)
		- [2. 创建python虚拟环境](#2_python_17)
		- [3. pip安装依赖包](#3_pip_25)
		- [4. 测试图片检测、视频检测、摄像头检测](#4__35)
		- [5. 模型转换及部署](#5__56)




#### 1. 准备NanoDet-PyTorch工程


该代码基于NanoDet项目进行小裁剪，专门用来实现Python语言、PyTorch 版本的代码，下载直接能使用，支持图片、视频文件、摄像头实时目标检测。


用于目标检测，模型小，检测速度快速，适合没GPU显卡的嵌入式设备运行，比如“树莓派”、ARM开发板、嵌入式开发板。


**本文在Ubuntu18.04环境下进行测试：**


首先将python的源更换为国内源：[ubuntu修改python的pip源为国内源](https://blog.csdn.net/limengshi138392/article/details/111315014?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522163937806816780274121222%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=163937806816780274121222&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~baidu_landing_v2~default-1-111315014.first_rank_v2_pc_rank_v29&utm_term=ubuntu%20python%E6%8D%A2%E5%9B%BD%E5%86%85%E6%BA%90&spm=1018.2226.3001.4187)


![在这里插入图片描述](https://img-blog.csdnimg.cn/0a1230c931ea4ecfb5a7e217e08ba53e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBARnJhbmvlrabkuaDot6_kuIo=,size_20,color_FFFFFF,t_70,g_se,x_16)


#### 2. 创建python虚拟环境



```
python -m venv Virtual-NanoDet

```


```
 source myvenv/bin/activate

```

#### 3. pip安装依赖包


如：



```
pip install cmake
pip install numpy matplotlib pandas scipy opencv-python imutils -i https://pypi.tuna.tsinghua.edu.cn/simple

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/de0d38395ff3472c91cd23d3ba5c45a2.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBARnJhbmvlrabkuaDot6_kuIo=,size_14,color_FFFFFF,t_70,g_se,x_16)  
 基本上按照这个流程安装这些包下来是没问题的，预计这个虚拟环境大小为2.5G；


#### 4. 测试图片检测、视频检测、摄像头检测


文件中提供了图片和视频素材，摄像头用usb接口的就可以，下面开始运行程序：



```
## 运行程序(先进入自建的python venv中，再到目标文件夹中运行以下程序)

'''目标检测-图片'''
# python detect\_main.py image --config ./config/nanodet-m.yml --model model/nanodet\_m.pth --path street.png

'''目标检测-视频文件'''
# python detect\_main.py video --config ./config/nanodet-m.yml --model model/nanodet\_m.pth --path test.mp4

'''目标检测-摄像头'''
# python detect\_main.py webcam --config ./config/nanodet-m.yml --model model/nanodet\_m.pth --path 0

```

接下来就可以看到目标检测后的效果了：


![在这里插入图片描述](https://img-blog.csdnimg.cn/87ee912818454151a03aace867f0af7c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBARnJhbmvlrabkuaDot6_kuIo=,size_14,color_FFFFFF,t_70,g_se,x_16)  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/06a0e966eaaf4f70b2a890b053779175.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBARnJhbmvlrabkuaDot6_kuIo=,size_20,color_FFFFFF,t_70,g_se,x_16)


#### 5. 模型转换及部署


运行`tools/export.py`脚本可将`pth`转为`onnx`模型（注意路径）：



```
import os
import torch
from nanodet.model.arch import build_model
from nanodet.util import Logger, cfg, load_config, load_model_weight

def main(config, model_path, output_path, input_shape=(320, 320)):
    logger = Logger(-1, config.save_dir, False)
    model = build_model(config.model)
    checkpoint = torch.load(model_path, map_location=lambda storage, loc: storage)
    load_model_weight(model, checkpoint, logger)
    dummy_input = torch.autograd.Variable(torch.randn(1, 3, input_shape[0], input_shape[1]))
    torch.onnx.export(model, dummy_input, output_path, verbose=True, keep_initializers_as_inputs=True, opset_version=11)
    print('finished exporting onnx ')

if __name__ == '\_\_main\_\_':
    cfg_path = r"../config/nanodet-m.yml"
    model_path = r"../model/nanodet\_m.pth"
    out_path = r'../model/output.onnx'
    load_config(cfg, cfg_path)
    main(cfg, model_path, out_path, input_shape=(320, 320))

```

生成onnx通用模型后，可转为部署需要的格式，如`ncnn`，我们已经在ubuntu装好了ncnn，然后在`ncnn/build/install/bin/`下有一个onnx2ncnn脚本，执行转换程序：`./onnx2ncnn output.onnx output.param output.bin`


然后就可以在移动端程序中使用ncnn框架所需要的模型了（`bin、param`）。


以上。





