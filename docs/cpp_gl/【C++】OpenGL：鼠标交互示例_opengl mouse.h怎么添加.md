








#### 文章目录


* + [在窗体中创建多边形](#_1)
	+ [写入鼠标交互函数](#_66)
	+ [完整程序](#_92)




### 在窗体中创建多边形


新建opengl项目，安装好nupengl程序包，开始main函数编写。


跟前面创建窗体不同，这次我们将窗体的长和宽都设置为全局变量，以方便后面的操作：



```
GLint w = 600, h = 500;	//窗体变量

```

另外，为了方便窗体中多边形移动，创建dx和dy两个全局变量，并分别加到多边形的各个顶点：



```
GLint dx = 0, dy = 0;	//移动变量

```

创建窗体多边形完整程序：



```
#include <GL/glut.h>

GLint w = 600, h = 500;	//窗体变量

GLint dx = 0, dy = 0;	//移动变量

void myDisplay()
{
	glClear(GL_COLOR_BUFFER_BIT);
	glColor3f(0.87, 0.56, 0.4);
	glPointSize(2);
	glBegin(GL_POLYGON);
	glVertex2i(10 + dx, 10 + dy);
	glVertex2i(10 + dx, 100 + dy);
	glVertex2i(100 + dx, 100 + dy);
	glVertex2i(100 + dx, 10 + dy);
	glEnd();
	glFlush();
}

void init()
{
	glClearColor(1.0, 1.0, 1.0, 0.0);
	glMatrixMode(GL_PROJECTION);
	glLoadIdentity();
	gluOrtho2D(0,w,0,h);	//可视的范围，类似鼠标滚轮的远近
}

int main(int argc, char\* argv[])
{
	glutInit(&argc, argv);
	glutInitDisplayMode(GLUT_RGB | GLUT_SINGLE);
	glutInitWindowPosition(300, 100);
	glutInitWindowSize(w, h);
	glutCreateWindow("mouse interaction");

	init();
	glutDisplayFunc(myDisplay);	//传递需要勾画的函数
	glutMainLoop();
	return 0;
}



```

生成的多边形如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/98e7459c6341428fa9cc149a0a5b3082.png)


### 写入鼠标交互函数


跟前面键盘交互类似，这次我们的目的是让多边形跟着鼠标移动，也就是鼠标点到哪里，多边形就跟到哪里。


首先在main函数中加入鼠标操作：



```
glutMouseFunc(mouseMotion);		//调用鼠标函数

```

然后创建鼠标函数：



```
void mouseMotion(GLint button, GLint state, GLint x, GLint y)
{
	if (button == GLUT_LEFT_BUTTON && state == GLUT_DOWN)
	{
		//左键按下，图元移动到鼠标位置
		dx = x;
		dy = h - y;		//左上为（0，0）
		glutPostRedisplay();
	}
}

```

运行程序，就可以通过鼠标控制多边形移动了，移动后的效果如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/245bd38c297647b589f94eb32108474a.png)


### 完整程序


main.cpp



```
#include <GL/glut.h>

GLint w = 600, h = 500;	//窗体变量

GLint dx = 0, dy = 0;	//移动变量

void myDisplay()
{
	glClear(GL_COLOR_BUFFER_BIT);
	glColor3f(0.87, 0.56, 0.4);
	glPointSize(2);
	glBegin(GL_POLYGON);
	glVertex2i(10 + dx, 10 + dy);
	glVertex2i(10 + dx, 100 + dy);
	glVertex2i(100 + dx, 100 + dy);
	glVertex2i(100 + dx, 10 + dy);
	glEnd();
	glFlush();
}

void mouseMotion(GLint button, GLint state, GLint x, GLint y)
{
	if (button == GLUT_LEFT_BUTTON && state == GLUT_DOWN)
	{
		//左键按下，图元移动到鼠标位置
		dx = x;
		dy = h - y;		//左上为（0，0）
		glutPostRedisplay();
	}
}

void init()
{
	glClearColor(1.0, 1.0, 1.0, 0.0);
	glMatrixMode(GL_PROJECTION);
	glLoadIdentity();
	gluOrtho2D(0,w,0,h);	//可视的范围，类似鼠标滚轮的远近
}

int main(int argc, char\* argv[])
{
	glutInit(&argc, argv);
	glutInitDisplayMode(GLUT_RGB | GLUT_SINGLE);
	glutInitWindowPosition(300, 100);
	glutInitWindowSize(w, h);
	glutCreateWindow("mouse interaction");

	init();
	glutDisplayFunc(myDisplay);	//传递需要勾画的函数
	glutMouseFunc(mouseMotion);		//调用鼠标函数
	glutMainLoop();
	return 0;
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/64cf4e0c687e44bc9ac2d48b4b4149a2.png#pic_center)


以上。





