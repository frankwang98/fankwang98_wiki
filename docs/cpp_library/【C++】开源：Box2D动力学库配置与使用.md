







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍Box2D动力学库配置与使用。  
>  **无专精则不能成，无涉猎则不能通。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. 项目介绍](#smirk1__7)
	+ [:blush:2. 环境配置](#blush2__27)
	+ [:satisfied:3. 使用说明](#satisfied3__54)




### 😏1. 项目介绍


项目Github地址：`https://github.com/erincatto/box2d`


官网：`https://box2d.org/documentation/index.html`


`Box2D` 是一个开源的C++物理引擎，用于模拟和模拟二维物理系统。它提供了一套强大的工具和功能，使开发者能够创建逼真的物理效果和交互。


下面是一些关于 `Box2D` 的介绍：



> 
> 1.物理仿真：Box2D 可以处理刚体的运动、碰撞检测和碰撞响应等物理仿真任务。它允许您模拟刚体的运动、旋转、加速度以及受力和力矩的影响。
> 
> 
> 



> 
> 2.约束和关节：Box2D 提供了多种约束类型，例如距离约束、旋转约束和弹簧约束等。这些约束可以被用来模拟各种物体之间的连接和互动关系。
> 
> 
> 



> 
> 3.冲突检测：Box2D 提供了高效的碰撞检测算法，可以检测物体之间的碰撞，并触发相应的碰撞事件。这使得开发者能够实现真实的物体交互效果，如弹球、堆叠物体等。
> 
> 
> 



> 
> 4.多边形碰撞检测：Box2D 支持多边形形状的碰撞检测和处理，使您能够使用各种形状的物体来建模和仿真。
> 
> 
> 



> 
> 5.用户交互：Box2D 允许开发者通过鼠标和键盘输入与物体进行交互，并可以实现拖动、旋转和施加力等交互操作。
> 
> 
> 



> 
> 6.跨平台支持：Box2D 可以在多个平台上运行，包括 Windows、Mac、Linux 和移动平台（Android 和 iOS）等。这使得它适用于各种不同的应用程序和游戏。
> 
> 
> 


### 😊2. 环境配置


下面进行环境配置：



```
# apt安装
sudo apt install libbox2d-dev

```


```
# 源码编译
git clone https://github.com/erincatto/box2d.git
cd box2d
mkdir build && cd build
cmake ..
make
sudo make install

```

编译运行：



```
# 头文件引用
#include <Box2D/Box2D.h> # apt
#include <box2d/box2d.h> # 源码
# 编译
g++ -o main main.cpp -lBox2D && ./main # apt
g++ -o main main.cpp -lbox2d && ./main # 源码

```

### 😆3. 使用说明


物体重力掉落仿真分析示例：



```
#include <iostream>
#include <Box2D/Box2D.h>

int main() {
    // 创建 Box2D 世界
    b2Vec2 gravity(0.0f, -10.0f);
    b2World world(gravity);

    // 创建地面刚体
    b2BodyDef groundBodyDef;
    groundBodyDef.position.Set(0.0f, -10.0f);
    b2Body\* groundBody = world.CreateBody(&groundBodyDef);
    b2PolygonShape groundBox;
    groundBox.SetAsBox(50.0f, 10.0f);
    groundBody->CreateFixture(&groundBox, 0.0f);

    // 创建动态刚体
    b2BodyDef bodyDef;
    bodyDef.type = b2_dynamicBody;
    bodyDef.position.Set(0.0f, 4.0f);
    b2Body\* body = world.CreateBody(&bodyDef);

    b2PolygonShape dynamicBox;
    dynamicBox.SetAsBox(1.0f, 1.0f); // dynamicBox

    b2FixtureDef fixtureDef;
    fixtureDef.shape = &dynamicBox;
    fixtureDef.density = 1.0f;
    fixtureDef.friction = 0.3f;
    body->CreateFixture(&fixtureDef); // fixtureDef

    // 模拟运动（盒子掉落在地上的运动）
    float timeStep = 1.0f / 60.0f;
    int32 velocityIterations = 6;
    int32 positionIterations = 2;
    for (int32\_t i = 0; i < 60; ++i) {
        world.Step(timeStep, velocityIterations, positionIterations);

        b2Vec2 position = body->GetPosition();
        float_t angle = body->GetAngle();

        std::cout << "位置: (" << position.x << ", " << position.y << ")"
                  << " 角度: " << angle << std::endl;
    }

    return 0;
}

```

刚体约束示例：



```
#include <iostream>
#include <Box2D/Box2D.h>

int main() {
    // 创建世界对象
    b2Vec2 gravity(0.0f, -9.81f);
    b2World world(gravity);

    // 创建两个矩形刚体
    b2BodyDef bodyDef;
    bodyDef.type = b2_dynamicBody;

    b2PolygonShape boxShape;
    boxShape.SetAsBox(1.0f, 1.0f);

    b2FixtureDef fixtureDef;
    fixtureDef.shape = &boxShape;
    fixtureDef.density = 1.0f;
    fixtureDef.friction = 0.3f;

    // 刚体1
    bodyDef.position.Set(-2.0f, 10.0f);
    b2Body\* body1 = world.CreateBody(&bodyDef);
    body1->CreateFixture(&fixtureDef);

    // 刚体2
    bodyDef.position.Set(2.0f, 10.0f);
    b2Body\* body2 = world.CreateBody(&bodyDef);
    body2->CreateFixture(&fixtureDef);

    // 创建距离关节
    b2DistanceJointDef distanceJointDef;
    distanceJointDef.Initialize(body1, body2, body1->GetWorldCenter(), body2->GetWorldCenter());
    distanceJointDef.collideConnected = true;
    b2DistanceJoint\* distanceJoint = (b2DistanceJoint\*)world.CreateJoint(&distanceJointDef);

    // 创建旋转关节
    b2RevoluteJointDef revoluteJointDef;
    revoluteJointDef.Initialize(body1, body2, body1->GetWorldCenter());
    b2RevoluteJoint\* revoluteJoint = (b2RevoluteJoint\*)world.CreateJoint(&revoluteJointDef);

    // 创建滑动关节
    b2PrismaticJointDef prismaticJointDef;
    prismaticJointDef.Initialize(body1, body2, body1->GetWorldCenter(), b2Vec2(1.0f, 0.0f));
    b2PrismaticJoint\* prismaticJoint = (b2PrismaticJoint\*)world.CreateJoint(&prismaticJointDef);

    // 模拟世界
    for (int i = 0; i < 60; ++i) {
        world.Step(1.0f / 60.0f, 6, 2);
        b2Vec2 position1 = body1->GetPosition();
        b2Vec2 position2 = body2->GetPosition();
        float32 angle1 = body1->GetAngle();
        float32 angle2 = body2->GetAngle();
        std::cout << "Body1: x=" << position1.x << ", y=" << position1.y << ", angle=" << angle1 << std::endl;
        std::cout << "Body2: x=" << position2.x << ", y=" << position2.y << ", angle=" << angle2 << std::endl;
    }

    return 0;
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6cbcd6c17cec4dba9bb3c0f895f02fa2.png)


以上。





