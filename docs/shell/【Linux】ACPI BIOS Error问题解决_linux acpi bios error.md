







> 
> 今天帮朋友装个ubuntu系统，遇到一个问题记录一下。
> 
> 
> 


### 报错与现象：


ACPI BIOS Error…


电脑花屏


### 解决方法：


1. 插入启动盘，当进入引导界面后，键盘输入’`e`’，编辑Linux启动命令，把命令中的"`---`“替换成"`nomodeset`”，按下`F10`保存。即忽略错误继续安装。
2. 安装完成后，重启，进入系统选择引导界面后，同样输入’`e`’，编辑Linux启动命令，在`splash`后添加`nomodeset`，按下`F10`保存。即临时忽略错误进入桌面。
3. 进入系统后修改`/etc/default/grub`文件，在`splash`后添加`nomodeset`，执行`sudo update-grub`指令，然后ubuntu就可以正常启动了。


![在这里插入图片描述](https://img-blog.csdnimg.cn/49ed7a2cb6f949c595d6188e3476717a.png)


以上。





