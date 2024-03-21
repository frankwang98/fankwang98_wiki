






图像处理和计算机视觉是Matlab的一个主要应用领域，这部分包括4个工具箱——图像处理、计算机视觉、雷达、医学图像。由于视觉的东西容易呈现，所以先从计算机视觉工具箱学起。


官方文档对[计算机视觉工具箱](https://ww2.mathworks.cn/help/vision/index.html?s_tid=hc_product_card)的介绍如下：设计和测试计算机视觉、3D 视觉和视频处理系统，提供了算法、函数和应用可用于特征检测、对象识别、语义分割和相机的标定校准等，此外还有视觉和点云 SLAM、立体视觉、点云处理和运动估计等，不过关于雷达点云的相关处理目前有独立出来一个雷达工具箱，后面再介绍。


![在这里插入图片描述](https://img-blog.csdnimg.cn/a024ede2dba74c70967003f004d569c1.png)  
 



#### 文章目录


* + [相机标定](#_6)
	+ [使用校准相机测量平面物体](#_84)




### 相机标定


相机标定用于估计图像或摄像机的镜头和图像传感器的参数。通过标定校准，可以处理镜头失真、深度估计、物体测量和3D场景重建等。


![在这里插入图片描述](https://img-blog.csdnimg.cn/5395ffbd13f6457eb9c4e04d5d4acec2.png)


相机参数包括相机内参、相机外参和畸变系数。通过相机的标定校准，可以：


1. 绘制相机的相对位置（坐标系转换）和校准模式。
2. 计算重新投影误差。
3. 计算参数估计误差。


可以通过MATLAB应用`Camera Calibrator`来自动校准相机，适用于针孔相机和鱼眼相机。


通过APP打开或直接在命令行输入`cameraCalibrator`打开相机标定程序：


![在这里插入图片描述](https://img-blog.csdnimg.cn/50fa42b1e9d44ccbaff5cea4ad8ddb0a.png)


标定之前，先打印或买一张棋盘格纸，可以从[这个网站](https://calib.io/pages/camera-calibration-pattern-generator)生成。用A4纸打印出来后，测量一个格子的实际长度是20mm。


![在这里插入图片描述](https://img-blog.csdnimg.cn/06feef82b06f4e25ab421c566eb28f9c.png)


进入相机标定程序，可以提前拍好棋盘格照片，也可以在线拍，将棋盘格纸分别在远处和近处，上下左右中几个位置短暂停留。


![在这里插入图片描述](https://img-blog.csdnimg.cn/f86dfd4eecef40cdbc56f3fc9dd623f9.png)


设置好时间间隔和拍摄数量后，就可以进行照片的自动拍摄：


![在这里插入图片描述](https://img-blog.csdnimg.cn/45afb7815b8c447eb3d682db679eb24f.png)


然后会提示输入棋盘格边长：


![在这里插入图片描述](https://img-blog.csdnimg.cn/be532e9a3178467b935450f23b35a904.png)


然后关闭采集器，点击Calibrate开始标定：


![在这里插入图片描述](https://img-blog.csdnimg.cn/81fc3cf7adf94a0d83e63eb7eac42a91.png)  
 标定完成后，效果如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/4356a89120474e908e59b6a716ba531f.png)


点击 Export Camera Parameters，可在matlab工作空间里可以看到相机参数：


![在这里插入图片描述](https://img-blog.csdnimg.cn/d9b7bd1292124d2c9ca7b46a95384fbe.png)  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/d73cb549629f44a9840fc7b29171bff1.png)


命令行输入`cameraParams.IntrinsicMatrix`可以查看相机内参：


![在这里插入图片描述](https://img-blog.csdnimg.cn/5b555190632a45638081604b7735f69b.png)


对应工作区中的参数：


![在这里插入图片描述](https://img-blog.csdnimg.cn/c7c353e7f9f04d57aa1ed04f5f5ef0c0.png)


同理可以查看其他参数：


![在这里插入图片描述](https://img-blog.csdnimg.cn/a5005859904342e09c2144f8227e1af1.png)


标定完后，可以评估单相机校准的准确性并进行改进，  
 1.检查重投影误差  
 重投影误差是检测到的点与相应的重投影点之间的距离（以像素为单位），以条形图的形式显示。一般情况下，小于一个像素的平均重投影误差是可以接受的，可以选择条形图以选择图像，删除误差大的图像。


![在这里插入图片描述](https://img-blog.csdnimg.cn/e721f933c1934fefa9237a1155f29a4c.png)


2.检查外在参数可视化  
 3-D 外在参数图提供了以摄像机为中心的棋盘格视图和以棋盘格为中心的摄像机视图。如果相机在捕获图像时处于静止状态，则以相机为中心的视图非常有用。如果棋盘格是静止的，则以棋盘格为中心的视图有用。可以单击并拖动图形以旋转它。单击棋盘（或相机）以将其选中。可视化效果中突出显示的数据与列表中的所选图像相对应。检查棋盘格和相机的相对位置，以确定它们是否符合您的预期。例如，相机后面出现的棋盘格表示标定错误。


![在这里插入图片描述](https://img-blog.csdnimg.cn/dd8cc071288945c1b4b7dbe92354d90f.png)


3.检查未失真的图像  
 要查看消除镜头失真的效果，点击视图上的显示未失真，如果校准准确，则图像预览中扭曲的线条将变为直线。


![在这里插入图片描述](https://img-blog.csdnimg.cn/e824b525f1c447479523277ba8689150.png)


要改进标定结果，可以删除高误差图像、添加更多图像或修改标定程序设置，直到得到满意的结果。


改进后的结果，可以再次输出到工作空间中。


### 使用校准相机测量平面物体


。。。


以上。





