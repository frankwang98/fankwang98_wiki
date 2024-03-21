






MATLAB软件每年更新两次，由于各种原因我们无法去随时更新自己的matlab版本，一般情况下我们会选择次新版本，所以这个时候我们开发的simulink模型就会有一个高低版本转换的问题。




#### 解决simulink模型高低版本的转换问题


* + [高转低](#_3)
	+ [低转高](#_10)
	+ [低版本软件打开高版本模型](#_13)
	+ [批量低转高](#_20)




### 高转低


针对于此，matlab软件已经有了解决方法，如果我当前在2018b做的simulink模型，想要转换成2018a，打开File->Export Model to->Previous Version，就会向文件操作中的”另存为“一样，出现很多以前的版本，我们选择自己想要转的版本即可。


![在这里插入图片描述](https://img-blog.csdnimg.cn/20210608165358970.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMzQ0Nzkw,size_16,color_FFFFFF,t_70#pic_center)  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210608165542767.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMzQ0Nzkw,size_16,color_FFFFFF,t_70#pic_center)  
 这样，对方低版本的matlab就可以打开该模型了。


### 低转高


但是有人会说，那怎么由低版本转为高版本呢，这也不用担心，一般情况下，如果自己的高版本软件打开一个低版本模型后，在模型上方会弹出一行对话，问是否要使用”Upgrade Advisor“对模型进行升级，我们直接点升级就可以了。


### 低版本软件打开高版本模型


还有一个问题，假如我们是在网上下载的高版本simulink模型，而且此时我们身边没有该版本的软件，这时候怎么操作呢？可以参照这位博主的文章：


[解决MATLAB Simulink 无法打开高版本模型的问题](https://blog.csdn.net/Cui_Hongwei/article/details/109663701?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522162314009216780265499925%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=162314009216780265499925&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_v2~rank_v29-3-109663701.pc_search_result_cache&utm_term=simulink%E6%A8%A1%E5%9E%8B%E5%A6%82%E4%BD%95%E8%BF%9B%E8%A1%8C%E9%AB%98%E4%BD%8E%E7%89%88%E6%9C%AC%E8%BD%AC%E6%8D%A2&spm=1018.2226.3001.4187)


按照上述流程操作后，就可以在低版本simulink中浏览使用高版本创建的模型文件（.slx文件）了。


### 批量低转高


还有一种情况，我们需要将很多高版本simulink模型转为低版本，可以参照下面这位博主的文章：


[MATLAB/Simulink模型版本批量转换为早期版本脚本](https://blog.csdn.net/acknole/article/details/113948550?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522162314009216780265499925%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=162314009216780265499925&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_v2~rank_v29-5-113948550.pc_search_result_cache&utm_term=simulink%E6%A8%A1%E5%9E%8B%E5%A6%82%E4%BD%95%E8%BF%9B%E8%A1%8C%E9%AB%98%E4%BD%8E%E7%89%88%E6%9C%AC%E8%BD%AC%E6%8D%A2&spm=1018.2226.3001.4187)


以上就是今天的内容，基本上涵盖了simulink高低版本转换的各种情况，如果以上有什么问题欢迎大佬批评指正，喜欢的话记得一键三连哦！





