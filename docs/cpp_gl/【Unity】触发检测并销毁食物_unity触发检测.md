






上节用到了碰撞检测，有了碰撞效果后食物才会消失，玩的时候小球会碰飞起来；如何让他没有碰撞效果呢，下面就来看看触发检测。




#### 文章目录


* + [设置触发检测](#_3)
	+ [脚本调用触发函数](#_8)




### 设置触发检测


还是在碰撞检测Box Collider这里，有一个是否是触发器，这里勾上，就从碰撞检测转为触发检测了；


![在这里插入图片描述](https://img-blog.csdnimg.cn/5c6615aaacc145c2a0f9be09aa6d16a1.png)


### 脚本调用触发函数


触发检测是OnTriggerEnter函数，然后销毁的判断从碰撞检测那边迁移过来，如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/20a64e6d75fb4ad6888d5f0fa1be1fcc.png)


脚本代码如下：



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

    //private void OnCollisionEnter(Collision collision)
    //{
    // Debug.Log("发生碰撞了");

    // //给Food模板设置好标签，检测到物体对应标签就销毁
    // if (collision.gameObject.tag == "Food")
    // {
    // Destroy(collision.gameObject);
    // }
    //}

    // 多行注释ctrl+k ctrl+c 取消注释ctrl+k ctrl+u
    //private void OnCollisionExit(Collision collision)
    //{
    // Debug.Log("结束碰撞了");
    //}

    //private void OnCollisionStay(Collision collision)
    //{
    // //Debug.Log("保持碰撞了");
    //}

    private void OnTriggerEnter(Collider other)
    {
        //Debug.Log("触发进入" + other.tag);
        if (other.tag == "Food")
        {
            Destroy(other.gameObject);
        }
    }

    //private void OnTriggerExit(Collider other)
    //{
    // Debug.Log("触发退出" + other.tag);
    //}

    //private void OnTriggerStay(Collider other)
    //{
    // Debug.Log("触发保持" + other.tag);
    //}
}


```

效果如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/b6136d617f384648bf0a54948ca12ca8.png)


以上。





