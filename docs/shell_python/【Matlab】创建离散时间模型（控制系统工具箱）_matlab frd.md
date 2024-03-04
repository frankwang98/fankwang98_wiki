






这节用tf, zpk, ss,和frd命令来创建离散时间模型。




#### 文章目录


* + [定义离散时间模型](#_4)
	+ [分析离散时间系统](#_46)




### 定义离散时间模型


创建离散时间模型的语法与连续时间模型的语法类似，只是还必须提供采样时间（采样间隔以秒为单位）。


例如，要指定离散时间传递函数：  
  
 
 
 
 
 H 
 
 
 ( 
 
 
 z 
 
 
 ) 
 
 
 = 
 
 
 
 
 z 
 
 
 − 
 
 
 1 
 
 
 
 
 
 z 
 
 
 2 
 
 
 
 a 
 
 
 − 
 
 
 1.85 
 
 
 z 
 
 
 + 
 
 
 0.9 
 
 
 
 
 
 H(z) = \frac{z - 1}{z^2a - 1.85 z + 0.9} 
 
 
 H(z)=z2a−1.85z+0.9z−1​  
 采样周期，Ts = 0.1 s。


可以用以下代码表示：



```
num = [ 1  -1 ];
den = [ 1  -1.85  0.9 ];
H = tf(num,den,0.1)

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/651200423eaa465a90e69285e9f1f24e.png)


用有理式表示如下：



```
z = tf('z',0.1);
H = (z - 1) / (z^2 - 1.85\*z + 0.9);

```

类似的，要指定离散时间状态空间模型：  
  
 
 
 
 
 x 
 
 
 [ 
 
 
 k 
 
 
 + 
 
 
 1 
 
 
 ] 
 
 
 = 
 
 
 0.5 
 
 
 x 
 
 
 [ 
 
 
 k 
 
 
 ] 
 
 
 + 
 
 
 u 
 
 
 [ 
 
 
 k 
 
 
 ] 
 
 
 
 x[k+1] = 0.5 x[k] + u[k] 
 
 
 x[k+1]=0.5x[k]+u[k]  
  
 
 
 
 
 y 
 
 
 [ 
 
 
 k 
 
 
 ] 
 
 
 = 
 
 
 0.2 
 
 
 x 
 
 
 [ 
 
 
 k 
 
 
 ] 
 
 
 . 
 
 
 
 y[k] = 0.2 x[k] . 
 
 
 y[k]=0.2x[k].  
 采样周期：Ts = 0.1 s


用以下代码表示：



```
sys = ss(.5,1,.2,0,0.1);
step(sys)

```

画出阶梯响应图如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/621f722b685d409aaeb71e3116e44c75.png)


### 分析离散时间系统


有几种方法可以确定 LTI 模型是否离散：


1. 显示屏显示非零采样时间值
2. sys.Ts或返回非零采样时间值。get(sys,‘Ts’)
3. isdt(sys)返回真值。


例如，对于上面指定的传递函数H：



```
H.Ts

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/b234cf6f797e4321b781f06ca8d088ef.png)



```
isdt(H)

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/8012f299ea1b4493a4c14d0c350de2a3.png)  
 也可以画出 时间响应图 或 伯德图 来看：



```
step(H)

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/d4774edbcdfa4f41a5f59776eb65c8a9.png)



```
bode(H), grid

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/57a01eb7218840ecb8dc1c2f47292c9c.png)


以上。





