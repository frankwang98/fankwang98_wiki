








#### 文章目录


* + [DDA数值微分线段算法](#DDA_1)
	+ [中点画线法（简）](#_75)
	+ [Bresenham画线算法](#Bresenham_77)




### DDA数值微分线段算法


数值微分法即DDA法(Digital Differential Analyzer)，是一种基于微分方程来生成直线的方法。在计算机图形学中，并没有线段的概念，而是一个个像素点组成了线段。


DDA法生成线段的步骤一般如下：


1. 有了起始点（x1，y1）和终点（xn，yn）；
2. ▲x=|xn-x1|，▲y=|yn-y1|；
3. 比较▲x和▲y的大小；
4. steps=▲x和▲y中较大者；
5. stepx=▲x/steps，stepy=▲y/steps。


DDA算法实现如下：



```
#include <GL/glut.h>
#include <math.h>

void myDDA(GLfloat x1, GLfloat y1, GLfloat xn, GLfloat yn)
{
	float dx = fabs(xn - x1);
	float dy = fabs(yn - y1);
	float steps;
	if (dx > dy)
		steps = dx;
	else
		steps = dy;
	float stepX = dx / steps;
	float stepY = dy / steps;
	glBegin(GL_POINTS);
	for (int i = 0; i < (int)steps; i++)
	{
		glVertex2f(x1, y1);
		x1 += stepX;
		y1 += stepY;
	}
	glEnd();
}

void myDisplay()
{
	glClear(GL_COLOR_BUFFER_BIT);
	glColor3f(0.87, 0.56, 0.4);
	glPointSize(3);
	myDDA(1.5, 3.8, 189.8, 267.5);	//调用DDA，定义起点和终点
	glFlush();
}

void init()
{
	glClearColor(1.0, 1.0, 1.0, 0.0);
	glMatrixMode(GL_PROJECTION);
	glLoadIdentity();
	gluOrtho2D(0, 600, 0, 500);	//可视的范围，类似鼠标滚轮的远近
}

int main(int argc, char\* argv[])
{
	glutInit(&argc, argv);
	glutInitDisplayMode(GLUT_RGB | GLUT_SINGLE);
	glutInitWindowPosition(300, 100);
	glutInitWindowSize(600,500);
	glutCreateWindow("Test DDA");

	init();
	glutDisplayFunc(myDisplay);	//传递需要勾画的函数
	glutMainLoop();
	return 0;
}



```

DDA画线算法的效果如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/18d61b2e5d50488aa40bcb282658cc7e.png)


### 中点画线法（简）


看它位于中点的上边还是下边。


### Bresenham画线算法


这种画线算法的思想和中点画线的一致，只是在判断取哪个点时，不是看它位于中点的上边还是下边，而是将这两个点与直线上对应点的距离进行比较，如果`du＞dl`，取下面的点，反之则取上面的点：


![在这里插入图片描述](https://img-blog.csdnimg.cn/c9adc7c0545f4cfba30cad42cad883d3.png)  
 最后推出以下公式：


![在这里插入图片描述](https://img-blog.csdnimg.cn/6f2df27173984678af3ace7901991ec6.png)  
 Bresenham算法步骤如下：


1. 输入（x1，y1），（xn，yn）
2. dx=xn-x1，dy=yn-y1
3. 2dx，2dy
4. p0=2dy-dx
5. 循环，如果pk＞0，选上面点；如果pk＜0，选下面点


![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/f01b4153b353413f808036f8d82368bb.png#pic_center)


以上。





