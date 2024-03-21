






今天遇到在windows命令行下输入ipconfig，显示无效命令，令人费解，查找一番，原来是这个原因。


打开计算机的 **高级系统设置-环境变量-系统变量-Path变量** 下，看自己的这几条是不是在最前端，就类似于MATLAB的路径设置一样，在最前端的最先被执行，这里ipconfig命令无效就是因为这几条变量没有前置的缘故。


![在这里插入图片描述](https://img-blog.csdnimg.cn/20210625150458204.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMzQ0Nzkw,size_16,color_FFFFFF,t_70)  
 以上。





