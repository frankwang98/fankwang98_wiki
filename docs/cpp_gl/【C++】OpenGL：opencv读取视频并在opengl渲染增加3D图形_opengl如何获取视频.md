







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍opencv读取视频并在opengl渲染增加3D图形。  
>  **学其所用，用其所学。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:OpenCV读取视频并在OpenGL渲染](#smirkOpenCVOpenGL_7)




### 😏OpenCV读取视频并在OpenGL渲染



```
// main.cpp
#include <iostream>
#include <opencv2/opencv.hpp>
#include <GL/glut.h>

using namespace std;

// OpenGL窗口宽度和高度
int windowWidth = 1080;
int windowHeight = 960;

// OpenCV视频捕捉对象
cv::VideoCapture videoCapture;
cv::Mat currentFrame;
cv::Mat pausedFrame; // 用于存储暂停时的图像

// 3D立方体旋转角度
float cubeRotation = 45.0f;

// OpenGL纹理ID
GLuint textureID;

// 文字绘制函数
void drawText(const string& text, float x, float y, float r, float g, float b) {
    glColor3f(r, g, b);
    glRasterPos2f(x, y);

    for (const char& c : text) {
        glutBitmapCharacter(GLUT_BITMAP_TIMES_ROMAN_24, c);
    }
}

// 视频播放控制变量
bool isPlaying = true;

// OpenGL显示回调函数
void display() {
    glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);

    if (isPlaying) {
        if (!currentFrame.empty()) {
            cv::flip(currentFrame, currentFrame, 0);  // 可选：翻转图像以匹配OpenGL坐标系
            glDrawPixels(currentFrame.cols, currentFrame.rows, GL_BGR_EXT, GL_UNSIGNED_BYTE, currentFrame.data);
        }
    } else {
        if (!pausedFrame.empty()) {
            cv::flip(pausedFrame, pausedFrame, 0);  // 可选：翻转图像以匹配OpenGL坐标系
            glDrawPixels(pausedFrame.cols, pausedFrame.rows, GL_BGR_EXT, GL_UNSIGNED_BYTE, pausedFrame.data);
        }
    }

    // 绘制3D立方体
    glLoadIdentity();
    glTranslatef(0.0f, 0.0f, -5.0f);
    glRotatef(cubeRotation, 1.0f, 1.0f, 1.0f);
    glutWireCube(1.0f);

    glutSwapBuffers();
}

// 定时器回调函数
void timer(int value) {
    if (isPlaying) {
        bool success = videoCapture.read(currentFrame);

        if (!success || currentFrame.empty()) {
            cout << "Video end or cannot read frame" << endl;
            exit(0);
        }
    }

    glutPostRedisplay();
    glutTimerFunc(33, timer, 0);  // 设置33毫秒（约30帧每秒）的定时器
}

// 键盘按键回调函数
void keyboard(unsigned char key, int x, int y) {
    switch (key) {
        case 'p':  // 播放/暂停切换
            isPlaying = !isPlaying;
            if (!isPlaying) {
                pausedFrame = currentFrame.clone(); // 复制当前帧到暂停帧
            }
            break;
        case 'q':  // 退出程序
            exit(0);
            break;
    }
}

int main(int argc, char\*\* argv) {
    // 打开视频文件
    videoCapture.open("test.mp4");

    if (!videoCapture.isOpened()) {
        cout << "Could not open video file" << endl;
        return -1;
    }

    // 初始化OpenGL窗口
    glutInit(&argc, argv);
    glutInitDisplayMode(GLUT_DOUBLE | GLUT_RGB | GLUT_DEPTH);
    glutInitWindowSize(windowWidth, windowHeight);
    glutCreateWindow("Video Playback");

    // 设置OpenGL投影矩阵和视口
    glMatrixMode(GL_PROJECTION);
    glLoadIdentity();
    gluPerspective(45.0, (float)windowWidth / windowHeight, 0.1, 100.0);
    glMatrixMode(GL_MODELVIEW);

    // 注册OpenGL显示回调函数和键盘按键回调函数
    glutDisplayFunc(display);
    glutKeyboardFunc(keyboard);

    // 启动定时器
    glutTimerFunc(0, timer, 0);

    // 启动主循环
    glutMainLoop();

    return 0;
}

```

编译：`g++ -o video_rendering main.cpp -lglut -lGL -lGLU` pkg-config --cflags --libs opencv``


效果如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/e9fa2fb392034465a4cd86e40d907b17.png)


![在这里插入图片描述](https://img-blog.csdnimg.cn/6567c240f1b5443ab60e194d3ffe3803.png)


以上。





