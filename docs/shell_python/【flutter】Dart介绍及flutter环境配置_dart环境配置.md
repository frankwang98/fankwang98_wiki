







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍Dart介绍及flutter环境配置。  
>  **学其所用，用其所学。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. Dart及flutter介绍](#smirk1_Dartflutter_7)
	+ [:blush:2. 环境安装与配置](#blush2__41)
	+ [:satisfied:3. 应用示例](#satisfied3__73)




### 😏1. Dart及flutter介绍


Dart官网：`https://dart.dev/`


`Dart` 是一种面向对象的编程语言，由 Google 开发，用于构建高性能、跨平台的移动、Web 和桌面应用程序。


`Dart` 具有以下特点：



> 
> 1.面向对象：Dart 是一种完全面向对象的语言，支持类、继承、接口和 mixin 等常见的面向对象概念。
> 
> 
> 



> 
> 2.静态类型检查：Dart 支持静态类型检查，它可以在编译时捕获类型错误，提供更强的代码安全性和可靠性。
> 
> 
> 



> 
> 3.单线程异步编程：Dart 支持使用 async 和 await 关键字进行单线程的异步编程，使得编写异步代码更加简洁和可读。
> 
> 
> 



> 
> 4.JIT（即时编译器）和 AOT（预先编译器）：Dart 提供了两种运行模式。在开发阶段，可以使用 JIT 模式进行快速的开发和调试；在发布阶段，可以使用 AOT 模式将 Dart 代码预先编译为本机机器码，以提高性能和运行效率。
> 
> 
> 



> 
> 5.跨平台开发：Dart 可以用于开发移动应用（使用 Flutter 框架）、Web 应用（使用 AngularDart 或单纯的 Dart）以及服务器端应用（使用 Dart 本身或 Aqueduct 框架）。这使得开发人员可以使用相同的语言和代码库跨不同平台构建应用程序。
> 
> 
> 



> 
> 6.开发工具和生态系统：Dart 提供了丰富的开发工具和生态系统，包括 Dart SDK、Flutter 框架、DartPad 在线编辑器、Dart DevTools 开发者工具等，
> 
> 
> 


Flutter官网：`https://flutter.dev/`


`Flutter` 是一种开源的移动应用程序开发框架，由谷歌开发并维护。它旨在帮助开发者使用单一代码库构建高性能、美观且跨平台的移动应用程序（iOS 和 Android），甚至可以扩展到 Web、桌面和嵌入式设备。


以下是 `Flutter` 的一些主要特点和优势：



> 
> 1.快速开发：Flutter 提供了丰富的组件库和开发工具，使开发者能够快速构建出漂亮、流畅的用户界面。它还支持热重载，可以实时预览代码更改的效果，加快开发迭代速度。
> 
> 
> 



> 
> 2.单一代码库：Flutter 使用 Dart 编程语言，可以通过编写单一代码库来同时构建 iOS 和 Android 应用程序。这意味着开发者不再需要为不同的平台编写和维护不同的代码，减少了开发和测试的工作量。
> 
> 
> 



> 
> 3.响应式框架：Flutter 使用基于组件的架构，可以轻松构建复杂的用户界面。它采用了响应式编程的思想，界面的状态变化会被自动更新到 UI 上，使得开发者可以更加直观地管理和控制应用程序的状态。
> 
> 
> 



> 
> 4.高性能：Flutter 使用自己的渲染引擎（Skia）进行绘制，并通过硬件加速来提供高性能的用户界面。它可以实现平滑的动画效果和流畅的滚动操作，提供接近原生应用的性能体验。
> 
> 
> 



> 
> 5.热修复：Flutter 允许开发者在不需要重新发布应用程序的情况下快速修复 bug 或添加新功能。借助 Flutter 的热修复功能，开发者可以将变更推送到应用程序，使用户能够立即获得更新。
> 
> 
> 


### 😊2. 环境安装与配置


Dart单独安装：



```
# windows安装
# 可以在官网安装SDK，也可以在这个网址下载打包好的安装程序
https://gekorm.com/dart-windows/

# ubuntu安装
sudo apt-get update
sudo apt-get install apt-transport-https
sudo sh -c 'curl https://dl-ssl.google.com/linux/linux\_signing\_key.pub | apt-key add -'
sudo sh -c 'curl https://storage.googleapis.com/download.dartlang.org/linux/debian/dart\_stable.list > /etc/apt/sources.list.d/dart\_stable.list'

sudo apt-get update
sudo apt-get install dart
echo 'export PATH="$PATH:/usr/lib/dart/bin"' >> ~/.profile
source ~/.profile

dart --version # 查看版本

dart main.dart # 编译运行

```

Flutter SDK安装（包含Dart）：



```
# windows安装，可参考国内网站
https://flutter.cn/docs/get-started/install/windows # flutter SDK里应该是包含了Dart的SDK
# 解压SDK后，添加bin目录到系统环境变量
flutter doctor # 检查安装结果

```

此外，如果在`VS Code`上开发，可以安装官方的dart和flutter插件，不同的平台查看对应的工具。


### 😆3. 应用示例


Dart示例：



```
// 定义一个 Person 类
class Person {
  String name;
  int age;

  // 构造函数
  Person(this.name, this.age);

  // 方法
  void sayHello() {
    print('Hello, my name is $name and I am $age years old.');
  }
}

void main() {
  print('Hello, Dart!');

  // 创建一个 Person 对象
  var person = Person('John', 30);

  // 调用对象的方法
  person.sayHello();
}

```

Flutter示例：



```
在VS Code中，Ctrl+Shift+P打开命令面板，开始输入“flutter new”。选择 Flutter: New Project 命令。

选择对应的平台（如Web、Windows、Android等），创建示例程序，按F5运行，示例如下：

此外，flutter有热重载功能，体现了flutter开发界面程序的优势。

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/a2ef0d7961474907b66ea539d7907b82.png)


![请添加图片描述](https://img-blog.csdnimg.cn/5ea93bb657184b9eb8515cc76047c16a.png)


以上。





