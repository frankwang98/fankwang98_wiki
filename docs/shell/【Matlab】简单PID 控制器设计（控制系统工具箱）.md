






这里展示如何设计一个简单的PID控制器。


传递函数如下：  
  
 
 
 
 
 s 
 
 
 y 
 
 
 s 
 
 
 = 
 
 
 
 1 
 
 
 
 ( 
 
 
 s 
 
 
 + 
 
 
 1 
 
 
 
 ) 
 
 
 3 
 
 
 
 
 
 
 sys=\frac{1}{(s+1)^3} 
 
 
 sys=(s+1)31​  
 首先，创建模型并选用PI控制器：



```
sys = zpk([],[-1 -1 -1],1); 
[C_pi,info] = pidtune(sys,'PI')	% pidtune整定函数

```

生成结果如下：  
 （交叉频率约为0.52 rad/s，相位裕度为60）


![在这里插入图片描述](https://img-blog.csdnimg.cn/ded3f1aef63d41c59147ba287e9406d1.png)


检查受控系统的闭环阶跃响应：



```
T_pi = feedback(C_pi\*sys, 1);
step(T_pi)

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6a05b851b413435786472f9270c19ca6.png)


为了缩短响应时间，可以设置比自动选择的结果更高的目标交叉频率，即0.52。将交叉频率增加到 1.0。


定义c\_pi\_fast：



```
[C_pi_fast,info] = pidtune(sys,'PI',1.0)

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/15f2a1f3b6a64e01b48b9c55d990fa39.png)


新控制器可实现更高的交叉频率，但代价是相位裕量减小。


将两个控制器的闭环阶跃响应进行比较。



```
T_pi_fast = feedback(C_pi_fast\*sys,1);
step(T_pi,T_pi_fast)
axis([0 30 0 1.4])
legend('PI','PI,fast')

```

这种性能降低的结果是，PI控制器没有足够的自由度在1.0 rad/s的交叉频率下实现良好的相位裕量。添加微分操作可改善响应。


将 PIDF 控制器设计为目标交叉频率为 1.0 rad/s。



```
[C_pidf_fast,info] = pidtune(sys,'PIDF',1.0)

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/82d6a0b14365442cb3876afd9fe41420.png)


可以看出，在微分作用下， 算法的交叉频率和相位裕量都达到了较好值。


比较pi\_fast 和 pidf\_fast两个控制器的闭环阶跃响应：



```
T_pidf_fast =  feedback(C_pidf_fast\*sys,1);
step(T_pi_fast, T_pidf_fast);
axis([0 30 0 1.4]);
legend('PI,fast','PIDF,fast');

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/3ba3dec620ad428fb2957954763f0030.png)  
 可以将受控系统的输入（负载）抗扰度添加到快速 PI 和 PIDF 控制器再次进行比较：



```
S_pi_fast = feedback(sys,C_pi_fast);
S_pidf_fast = feedback(sys,C_pidf_fast);
step(S_pi_fast,S_pidf_fast);
axis([0 50 0 0.4]);
legend('PI,fast','PIDF,fast');

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6ec896b38d0244809600b7f6df2b14f6.png)  
 以上。





