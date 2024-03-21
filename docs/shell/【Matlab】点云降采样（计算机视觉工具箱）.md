






使用框网格滤波器进行点云降采样案例，降采样有随机、网格平均和非均匀网格采样三种。




#### 文章目录


* + [可视化点云](#_3)
	+ [网格平均降采样](#_13)
	+ [固定步长大小降采样](#_30)




### 可视化点云


首先，读取点云：



```
ptCloud = pcread('teapot.ply');

```

原始点云如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/503f5daae19446cebe00db15db2f63cc.png)


### 网格平均降采样


将 3-D 分辨率设置为 （0.1 x 0.1 x 0.1）：



```
gridStep = 0.1;
ptCloudA = pcdownsample(ptCloud,'gridAverage',gridStep);

```

可视化降采样点云数据：



```
figure;
pcshow(ptCloudA);

```

生成降采样后的点云图如下（示例给的茶壶teapot）：


![在这里插入图片描述](https://img-blog.csdnimg.cn/86c2b5e1350149f2b3b62cc9a3c2431a.png)


### 固定步长大小降采样


固定步长大小降采样只能在随机或非均匀网格采样模式下才能使用。


降采样点云与使用固定步长大小降采样的数据进行比较：



```
stepSize = floor(ptCloud.Count/ptCloudA.Count);
indices = 1:stepSize:ptCloud.Count;
ptCloudB = select(ptCloud, indices);

```

可视化固定步长降采样点云：



```
figure;
pcshow(ptCloudB);

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/1f3a07ee0f794673b056b4c3ef38124a.png)


以上。





