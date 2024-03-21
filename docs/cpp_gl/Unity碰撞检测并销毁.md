






下面模拟一个小球吃食物Food的场景，首先要做一个Food的旋转效果，并封装成预制体Prefabs；然后给所有Food设置一个共同标签Label，当小球检测到对应标签时，调用`OnCollisionEnter`碰撞检测函数，让食物消失。




#### 文章目录


* + [搭建Food效果](#Food_3)
	+ [碰撞检测并消掉Food](#Food_36)




### 搭建Food效果


新建Prefabs，创建一个cube并旋转成任意角度，上材质，转为预制体；


![在这里插入图片描述](https://img-blog.csdnimg.cn/f898f6fc4545420292e154be4dde59d1.png)


ctrl+D复制几个相同的Food，这时它们已经是预制体了，所以修改预制体就会同步修改所有组件；然后给他们打上标签Label：


![在这里插入图片描述](https://img-blog.csdnimg.cn/6f231b27757149e1b9b3719356e6702d.png)


同理，所有Food都有了标签；然后编辑Food脚本，让其有旋转效果：



```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Food : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {
        
    }

    // Update is called once per frame
    void Update()
    {
        transform.Rotate(Vector3.up);   //旋转效果
    }
}


```

### 碰撞检测并消掉Food


打开前面创建的小球运动Player脚本，修改如下（加上碰撞函数）：


![在这里插入图片描述](https://img-blog.csdnimg.cn/894e4dd1f00240d596533fe1b89989c3.png)


当检测到有“`Food`”标签的物体时，就会调用`Destroy`让这个物体消失。


代码如下：



```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Player : MonoBehaviour
{
    public Rigidbody rd;    //public或者private（接口）

    // Start is called before the first frame update
    void Start()
    {
        //Debug.Log("游戏开始了！");
        rd = GetComponent<Rigidbody>(); // 调用刚体组件

    }

    // Update is called once per frame
    void Update()
    {
        //Debug.Log("游戏正在运行!")；
        //rd.AddForce(Vector3.right); //施加1N（vector3.right left forward back）
        //rd.AddForce(new Vector3(10, 0, 0)); //自定义力

        float h = Input.GetAxis("Horizontal");  //keyboard A/D~~~-1/1
        float v = Input.GetAxis("Vertical");    //keyboard W/S~~~-1/1
        //Debug.Log(h); (1,2,3) \* 2 = (2,4,6) //加速
        rd.AddForce(new Vector3(h, 0, v));  //x y z
    }

    private void OnCollisionEnter(Collision collision)
    {
        Debug.Log("发生碰撞了");

        //给Food模板设置好标签，检测到物体对应标签就销毁
        if (collision.gameObject.tag == "Food")
        {
            Destroy(collision.gameObject);
        }
    }

    // 多行注释ctrl+k ctrl+c 取消注释ctrl+k ctrl+u
    //private void OnCollisionExit(Collision collision)
    //{
    // Debug.Log("结束碰撞了");
    //}

    //private void OnCollisionStay(Collision collision)
    //{
    // //Debug.Log("保持碰撞了");
    //}
}


```

效果如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/246292ed238848e89d558164a37e85fc.png)


以上。





