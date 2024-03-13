








#### 文章目录


* + [在窗体中创建多边形](#_1)
	+ [写入键盘交互函数](#_50)
	+ [完整程序](#_92)




### 在窗体中创建多边形


新建opengl项目，安装好nupengl程序包，开始main函数编写。


创建多边形窗体，相信大家已经熟悉了：



```
#include <GL/glut.h>

void myDisplay()
{
	glClear(GL_COLOR_BUFFER_BIT);
	glColor3f(0.8, 0.5, 0.6);
	glPointSize(5);
	glBegin(GL_POLYGON);
	glVertex2i(10 , 10 );
	glVertex2i(20 , 10 );
	glVertex2i(20 , 0 );
	glVertex2i(10 , 0 );
	glEnd();
	glFlush();
}

void init()
{
	glClearColor(1.0, 1.0, 1.0, 0.0);
	glMatrixMode(GL_PROJECTION);
	glLoadIdentity();
	gluOrtho2D(-20, 30, -20, 30);	//可视的范围，类似鼠标滚轮的远近
}

int main(int argc, char\* argv[])
{
	glutInit(&argc, argv);
	glutInitDisplayMode(GLUT_RGB | GLUT_SINGLE);
	glutInitWindowPosition(300, 100);
	glutInitWindowSize(600, 500);
	glutCreateWindow("key interaction");

	init();
	glutDisplayFunc(myDisplay);	//传递需要勾画的函数
	glutMainLoop();
	return 0;
}


```

多边形窗体效果如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/88886960ccdb45acb295987bc92bec30.png)


### 写入键盘交互函数


我们的目的是通过键盘交互，使窗体中的多边形依次上下左右移动。


因此，我们要先改变一下多边形函数-myDisplay()，首先创建全局变量：



```
int xd = 0, yd = 0;	//全局变量

```

然后在多边形的几个顶点的`（x,y）`坐标后分别加上`xd`和`yd`：



```
	glVertex2i(10 + xd, 10 + yd);
	glVertex2i(20 + xd, 10 + yd);
	glVertex2i(20 + xd, 0 + yd);
	glVertex2i(10 + xd, 0 + yd);

```

**移动需要的变量有了，接着我们要创建键盘动作函数了。**


首先在main函数中加入键盘操作：



```
glutKeyboardFunc(myKeyboard);		//调用键盘函数

```

然后创建键盘函数：



```
void myKeyboard(unsigned char key, int x, int y)
{
	switch (key)
	{
	case 'w':yd++; break;
	case 's':yd--; break;
	case 'a':xd--; break;
	case 'd':xd++; break;
	}
	glutPostRedisplay();	//刷新显示
}

```

运行程序，就可以通过键盘控制多边形移动了，移动后的效果如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/1facebfad3a74b928f70df9e92718454.png)


### 完整程序


main.cpp



```
#include <GL/glut.h>

int xd = 0, yd = 0;	//全局变量

void myDisplay()
{
	glClear(GL_COLOR_BUFFER_BIT);
	glColor3f(0.8, 0.5, 0.6);
	glPointSize(5);
	glBegin(GL_POLYGON);
	glVertex2i(10 + xd, 10 + yd);
	glVertex2i(20 + xd, 10 + yd);
	glVertex2i(20 + xd, 0 + yd);
	glVertex2i(10 + xd, 0 + yd);
	glEnd();
	glFlush();
}

void myKeyboard(unsigned char key, int x, int y)
{
	switch (key)
	{
	case 'w':yd++; break;
	case 's':yd--; break;
	case 'a':xd--; break;
	case 'd':xd++; break;
	}
	glutPostRedisplay();	//刷新显示
}

void init()
{
	glClearColor(1.0, 1.0, 1.0, 0.0);
	glMatrixMode(GL_PROJECTION);
	glLoadIdentity();
	gluOrtho2D(-20, 30, -20, 30);	//可视的范围，类似鼠标滚轮的远近
}


int main(int argc, char\* argv[])
{
	glutInit(&argc, argv);
	glutInitDisplayMode(GLUT_RGB | GLUT_SINGLE);
	glutInitWindowPosition(300, 100);
	glutInitWindowSize(600, 500);
	glutCreateWindow("key interaction");

	init();
	glutDisplayFunc(myDisplay);	//传递需要勾画的函数
	glutKeyboardFunc(myKeyboard);		//调用键盘函数
	glutMainLoop();
	return 0;
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/f6bf7c9cd60a42efb7699e4aec859595.png#pic_center)


以上。





