






一直听说Java是C++的升级和优化，却一直没用过，今天来简单学习一下。  
 



#### 文章目录


* + [1. JDK安装](#1_JDK_2)
	+ [2. 第一个java程序](#2_java_16)
	+ [3. 系统环境变量配置](#3__40)
	+ [4. Java语法基础](#4_Java_60)
	+ [5. Java UI基础](#5_Java_UI_314)
	+ [6. IDEA安装与代理](#6_IDEA_354)




### 1. JDK安装


JDK（JavaDevelopment Kit）：是指Java开发套件，类似gcc/g++，有了它就可以编译运行java程序。


JDK8好像是大家最常用的版本，这里我用的JDK17，也是一个长期支持版。  
 官网下载链接：



> 
> https://www.oracle.com/java/technologies/downloads/
> 
> 
> 


网盘下载链接如下：



> 
> 链接：https://pan.baidu.com/s/1irjtM4d846FTUzmtno1EZA  
>  提取码：3h53
> 
> 
> 


按照提示一步步安装即可。  
 ![请添加图片描述](https://img-blog.csdnimg.cn/ccc2867fc08844fba54492ce120cfe21.png)


### 2. 第一个java程序


类似C++，java的程序也有编写、编译和运行三步骤。


首先，创建 HelloWorld.java 文件，写入以下程序：



```
public class HelloWorld {
	public static void main(String[] args) {
		System.out.println("HelloWorld!");
	}
}

```

在此处打开 cmd ，通过 javac 命令编译程序：  
 `javac HelloWorld.java`


最后，通过 java 命令运行程序：  
 `java HelloWorld`


![请添加图片描述](https://img-blog.csdnimg.cn/e4826f33764445dc97059c011350d6a6.png)


另外，从JDK11开始，可以支持用 java 命令直接运行Java文件了，如：  
 `java HelloWorld.java`


![请添加图片描述](https://img-blog.csdnimg.cn/2d90293369f64e42a8327628795995ee.png)


### 3. 系统环境变量配置


此处以JDK17为例，其他版本可能还需要配置CLASSPATH，在此说明。


第一步：配置 JAVA\_HOME 变量


![请添加图片描述](https://img-blog.csdnimg.cn/22cc8726413f4a61b34cd8a65b140b74.png)


第二步：配置 Path 变量


![请添加图片描述](https://img-blog.csdnimg.cn/074b18eb8cba419ba461e63db11aa810.png)


**配置完环境变量后，就可以在电脑任意位置编译运行 java 程序了，否则只能在 JDK 的安装盘内编译运行。**


几条检查java是否安装的命令：



```
java
javac
java -version

```

### 4. Java语法基础


Java与C++很多方面是类似的，如下面的程序：



```
/\*
 \* 项目
 \* 模块
 \* 包
 \*/
package test;

import java.util.Date;  // Java日期和时间
import java.util.Random;  // 获取随机数
import java.util.Scanner;  // 数据输入
/\*
 \* Java程序中最基本的单位是类；
 \* 类的定义：public class 类名{
 \* 
 \* }
 \* 这是我定义的hello类；
 \*/
public class hello {
	/\*
 \* main函数是程序的入口函数；
 \*/
    public static void main(String[] args) {
    	// 程序输出用 System.out.println ，""中的字符可以修改；
    	
        // 打印日期和时间，初始化 Date 对象
        Date date = new Date();
        // 使用 toString() 函数显示日期时间
        System.out.println(date.toString());
        
        /\*
 \* 调用方法示例
 \*/
// test\_out();
// test\_in();
// test\_structure();
// test\_dataStructure();
    }
    
    /\*
 \* 定义test方法，这样就不用把长篇大论都写在main方法中；
 \*/
    public static void test\_out(){
    	System.out.println("test方法");
    	
    	/\*
 \* 常量有以下几种；
 \*/
    	// 字符串常量
        System.out.println("Hello World!!!");
        System.out.println("你好，世界!!!");
        System.out.println("------------");
        // 整数常量
        System.out.println(666);
        System.out.println(-88);
        System.out.println("------------");
        // 小数常量
        System.out.println(13.14);
        System.out.println(-3.14);
        System.out.println("------------");
        // 字符常量
        System.out.println('A');
        System.out.println('0');
        System.out.println("------------");
        // 布尔常量
        System.out.println(true);
        System.out.println(false);
        System.out.println("------------");
        // 空常量
        // 空常量是不能直接输出的
        //System.out.println(null);
        
        /\*
 \* 数据类型有基本数据类型和引用数据类型；
 \* 基本数据类型：整型（byte、short、int、long）、浮点型（float、double）、字符型（char）、布尔型（boolean）
 \* 引用数据类型：类（class）、接口（interface）、数组（【】）
 \* 整数默认是int，浮点型默认是double； 
 \*/
        
        /\*
 \* 变量：程序运行过程中，其值可以改变的量。
 \* 变量定义：数据类型+变量名+变量值
 \* 自动类型转换：范围小的变量》范围大的变量
 \* 强制类型转换：范围大的变量》范围小的变量
 \*/
        int i = (int)8.8;
        System.out.println("更新前的i：" + i);
        i++;
        System.out.println("更新后的i：" + i);
        System.out.println("------------");
        
        /\*
 \* 
 \* 有两种命名约定：
 \* 针对方法和变量，常采用小驼峰命名法，即首字母小写，其余单词首字母大写；
 \* 针对类，常采用大驼峰命名法，即单词的首字母都大写；
 \*/
        
       /\*
 \* 算术运算符：+-\*÷%
 \* 赋值运算符
 \* 关系运算符：==、！=、>=、<=
 \* 逻辑运算符：& | ！^
 \* 短路逻辑运算符：&& ||（作用相同，但有短路效果）
 \* 三元运算符： 关系表达式？表达式1：表达式2
 \* 字符串的+操作=拼接
 \*/
        int a = 1,b = 2;
        int max = a>b?a:b;
        System.out.println("wzf"+666+"\nmax:"+max);
        //案例：比较3个和尚的最高身高
        int height1 = 150;
        int height2 = 165;
        int height3 = 210;
        int tempHeight = height1 > height2 ? height1 : height2;
        int maxHeight = tempHeight > height3 ? tempHeight : height3;
        System.out.println("maxHeight:" + maxHeight);
        
        //获取随机数[0,10)
        Random r = new Random();
        int num_random = r.nextInt(10);
        System.out.println("随机数：" + num_random);
    }
    
    public static void test\_in(){
    	/\*
 \* 数据输入
 \* 1.导入Scanner
 \* 2.创建对象
 \* 3.接收对象
 \*/
    	Scanner sc = new Scanner(System.in);
    	System.out.println("请输入一个整数x");
    	int x = sc.nextInt();
    	System.out.println("x:" + x);
    	sc.close();
    }
    
    public static void test\_structure(){
    	/\*
 \* 顺序结构：依次执行
 \* 条件结构
 \* 循环结构
 \*/
    	//顺序结构
    	System.out.println("开始");
    	System.out.println("语句1");
    	System.out.println("语句2");
    	System.out.println("结束");
    	System.out.println("--------");
    	
    	//条件结构 if(关系表达式){结构体}
    	System.out.println("开始");
    	Scanner sc = new Scanner(System.in);
    	System.out.println("输入小明的分数：");
    	int score = sc.nextInt();
    	if (score >= 90 && score <= 100){
    		System.out.println("山地自行车的一辆");
    	}else if(score >= 80 && score <= 90){
    		System.out.println("变形金刚一套");
    	}else if(score >= 70 && score <= 80){
    		System.out.println("胖揍一顿");
    	}
    	System.out.println("结束");
    	System.out.println("--------");
    	
    	//条件结构 switch-case
    	System.out.println("开始");
    	Scanner sc_week = new Scanner(System.in);
    	System.out.println("输入星期数：");
    	int week = sc_week.nextInt();
    	switch (week){
	    	case 1:
	    		System.out.println("星期一");
	    		break;
	    	case 2:
	    		System.out.println("星期二");
	    		break;
	    	case 3:
	    		System.out.println("星期三");
	    		break;
	    	case 4:
	    		System.out.println("星期四");
	    		break;
	    	case 5:
	    		System.out.println("星期五");
	    		break;
	    	case 6:
	    		System.out.println("星期六");
	    		break;
	    	case 7:
	    		System.out.println("星期日");
	    		break;
	    	default:
	    		System.out.println("输入星期数有误");
	    		break;
    	}
    	System.out.println("结束");
    	System.out.println("--------");
    	
    	//循环结构：初始化+条件判断+条件控制+循环体
    	for(int i = 1; i <= 5; i++)
    		System.out.println("for循环"+ i +"次");
    	
    	int j = 1;
    	while(j <= 5){
    		System.out.println("while循环"+ j +"次");
    		j++;
    	}
    	
    	int k = 1;
    	do{
    		System.out.println("doWhile循环"+ k +"次");
    		k++;
    	}while(k <= 5);
    }
    
    public static void test\_dataStructure(){
    	/\*
 \* 数组：存储相同类型数据
 \* dataType[] arrayRefVar; // 声明数组变量
 \* 结构体：存储不同类型数据
 \*/
    	int size = 10;
    	int[] arr1 = new int[size];
    	arr1[0] = 1;
    	arr1[1] = 1;
    	arr1[2] = 1;
    	arr1[3] = 1;
    	arr1[4] = 1;
    	arr1[5] = 1;
    	arr1[6] = 1;
    	arr1[7] = 1;
    	arr1[8] = 1;
    	arr1[9] = 1;
    	int total = 0;
    	for (int i = 0; i < size; i++)
    	{
    		total += arr1[i];
    	}
    	for (int element: arr1)
    	{
    		System.out.println("打印所有数组：" + element);
    	}
    	System.out.println("数组的和：" + total);
    }
    
}


```

### 5. Java UI基础


Swing是一个为Java设计的GUI工具包，属于Java基础类的一部分。下面通过swing做一个简单的图形界面：



```
package test;

import javax.swing.\*;
public class ui {
    /\*\*{
 \* 创建并显示GUI。出于线程安全的考虑，
 \* 这个方法在事件调用线程中调用。
 \*/
    private static void createAndShowGUI() {
        // 确保一个漂亮的外观风格
        JFrame.setDefaultLookAndFeelDecorated(true);

        // 创建及设置窗口
        JFrame frame = new JFrame("HelloWorldSwing");
        frame.setDefaultCloseOperation(JFrame.EXIT\_ON\_CLOSE);

        // 添加 "Hello World" 标签
        JLabel label = new JLabel("Hello World");
        frame.getContentPane().add(label);

        // 显示窗口
        frame.pack();
        frame.setVisible(true);
    }

    public static void main(String[] args) {
        // 显示应用 GUI
        javax.swing.SwingUtilities.invokeLater(new Runnable() {
            public void run() {
                createAndShowGUI();
            }
        });
    }
}

```

### 6. IDEA安装与代理


IDEA下载地址：`https://www.jetbrains.com/idea/download/?section=windows`


安装好后，创建新项目，JDK会自动检测（前面安装了`JDK17`），也可以自己新增。构建可选择`Maven`或者`Gradle`（这里用Maven）。


Maven下载需要先设置代理：


![在这里插入图片描述](https://img-blog.csdnimg.cn/d89359cbe8df4e90a72e65560859d2f3.png)


在该目录下新建`settings.xml`，写入：



```
<?xml version="1.0" encoding="UTF-8"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
    <mirrors>
        <!-- mirror
         | Specifies a repository mirror site to use instead of a given repository. The repository that
         | this mirror serves has an ID that matches the mirrorOf element of this mirror. IDs are used
         | for inheritance and direct lookup purposes, and must be unique across the set of mirrors.
         |
        <mirror>
          <id>mirrorId</id>
          <mirrorOf>repositoryId</mirrorOf>
          <name>Human Readable Name for this Mirror.</name>
          <url>http://my.repository.com/repo/path</url>
        </mirror>
         -->

        <mirror>
            <id>alimaven</id>
            <name>aliyun maven</name>
            <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
            <mirrorOf>central</mirrorOf>
        </mirror>

        <mirror>
            <id>uk</id>
            <mirrorOf>central</mirrorOf>
            <name>Human Readable Name for this Mirror.</name>
            <url>http://uk.maven.org/maven2/</url>
        </mirror>

        <mirror>
            <id>CN</id>
            <name>OSChina Central</name>
            <url>http://maven.oschina.net/content/groups/public/</url>
            <mirrorOf>central</mirrorOf>
        </mirror>

        <mirror>
            <id>nexus</id>
            <name>internal nexus repository</name>
            <url>http://repo.maven.apache.org/maven2</url>
            <mirrorOf>central</mirrorOf>
        </mirror>

    </mirrors>
</settings>

```

然后就可以正常下载`Maven`插件，下载好后就可以运行程序了。


以上。





