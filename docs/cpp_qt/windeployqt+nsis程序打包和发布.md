我们常见的软件都会有一个安装程序，安装后，会生成安装目录，还可以创建桌面快捷方式等等。

那么，如何将我们编译好的`Qt release`程序打包成安装程序呢？这里记录一下`NSIS+NISEdit`方法。

### 1.windeployqt和nsis介绍

`windeployqt`是一个Qt提供的非常有用的命令行工具，用于将Qt应用程序所需的所有依赖项自动复制到应用程序的构建目录中，以便在没有Qt安装的计算机上运行应用程序。

windeployqt可以自动查找并复制应用程序所需的Qt库文件、插件、QML文件以及其他依赖的库文件。它还会自动解析应用程序的依赖关系，确保所有依赖的库文件都正确复制到目标目录中，以便应用程序能够正确运行。

使用windeployqt非常简单。只需在命令行中运行以下命令：

```bash
# 编译好release程序后，进入对应路径
windeployqt <path_to_application>

```

这样就能确保应用程序能够在没有Qt安装的计算机上独立运行，而无需手动复制所有的依赖项。


nsis我用的版本是： **nsis-3.02.1**（最新版nsis是包含了下面的nisedit的）


![在这里插入图片描述](https://img-blog.csdnimg.cn/6456b1ff3a564ea1ae427fc767e0f4c1.png)  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/16e2013f58c54db7aebf1747c9b1b2bc.png)


### 2.用nisedit制作脚本


1.新建向导脚本


![在这里插入图片描述](https://img-blog.csdnimg.cn/a24118cb85104691a2d08505e1fa1e2f.png)


2.填写应用信息


![在这里插入图片描述](https://img-blog.csdnimg.cn/df63dc6cf7e74482a10a363e7da22151.png)


3.设置图标、语言等


![在这里插入图片描述](https://img-blog.csdnimg.cn/863bedc9b7af45799a35dd5b43678562.png)


4.设置目录与授权信息


![在这里插入图片描述](https://img-blog.csdnimg.cn/eab2045377404db6835508b77e61480b.png)


5.选择打包的程序文件


![在这里插入图片描述](https://img-blog.csdnimg.cn/76dd94fef9b94e708ddc0f85f61df70d.png)


6.设置快捷方式


![在这里插入图片描述](https://img-blog.csdnimg.cn/275460a3122a49fd9362dac3dceac3a1.png)


7.安装后运行方式（只有一个`.exe`就默认）


![在这里插入图片描述](https://img-blog.csdnimg.cn/bc6a4aaa446a4060b67506727bc7d4a6.png)


8.设置卸载提示


![在这里插入图片描述](https://img-blog.csdnimg.cn/12707a01afb84b76bcac8420c8b36b0b.png)


9.完成向导


![在这里插入图片描述](https://img-blog.csdnimg.cn/0350c2568f384be784cebb7941c283e9.png)


生成的脚本如下，可自定义更改：


![在这里插入图片描述](https://img-blog.csdnimg.cn/511d7e10e62a4b02ad9bc57da7d0eb62.png)


比如要在安装开始的时候选择语言，可以添加以下脚本：

```bash
; Language files
!insertmacro MUI_LANGUAGE "English"
!insertmacro MUI_LANGUAGE "SimpChinese"

;初始化函数
Function .onInit
  Push ""
  Push ${LANG\_ENGLISH} ;添加英文代码 语言代码是系统变量，多语言引入后，自动加载，拼接方式是“LANG_语言”,可以查看NSIS手册，LANG_ENGLISH的编号为1033，LANG_SIMPCHINESE为2052；
  Push "English"
  Push ${LANG\_SIMPCHINESE}   ;添加简体中文选项
  Push "简体中文"
  Push A ; A means auto count languages for the auto count to work the first empty push (Push "") must remain
  LangDLL::LangDialog "Installer Language" "Please select the language of the installer" ;显示语言选择对话框
  Pop $LANGUAGE ;获得用户对于语言的选择结果 ‘$LANGUAGE’是多语言变量，在安装程序结束后，语言代码会存储在这个变量中，手动修改‘$LANGUAGE’的值后，安装包会重新选择最匹配的语言，参考最上面NSIS手册中选择界面语言步骤
  StrCmp $LANGUAGE "cancel" 0 +2
  Abort
  StrCmp $LANGUAGE 2052 ZH_INI EN_INI
  EN_INI:
    ;想干啥干啥
    Goto END
  ZH_INI:
        ;想干啥干啥
  END:
FunctionEnd

```

### 3.用NSIS软件编译脚本


加载上一步生成的脚本，编译即可：


![在这里插入图片描述](https://img-blog.csdnimg.cn/e15214228d1544a692e6b848130f6bdf.png)  
 测试安装完成后，生成桌面快捷方式如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/a1318cc2bacc43bb84065f698e6baf0a.png)  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/bb99c5cf4e2644d0a821b5c8b42ce5da.png#pic_center)


以上。





