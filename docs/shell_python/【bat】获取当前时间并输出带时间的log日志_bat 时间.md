






Windows系统中，%date%和%time%是系统内置的日期变量和时间变量，我们用bat脚本基于这两个变量来测试。


测试脚本如下：



```
// bat脚本获取日期2023/02/12
echo %date:~0,10%

// bat脚本获取时间10:00:00 (空格)8:00
echo %time:~0,5%

// 操作字符串（x是开始位置，y是取得字符数）
echo %time:~x,y%

// 输出带时间的log日志
set hour=%time:~0,2%
if %hour% LSS 10 (set hour=0%time:~1,1%)
set filename=%date:~0,4%%date:~5,2%%date:~8,2%_%hour%%time:~3,2%%time:~6,2%
echo 123 > %filename%.log

// 自动删除旧log日志（-i i是几天，如-1就是删除前一天的日志）
forfiles /p "C:\Users\dev\Desktop\logs" /s /m *.txt /d -1 /c "cmd /c del /f @path"

```

效果如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/20c1b0455ebc422a9162ca3831e3ddbe.png)


以上。





