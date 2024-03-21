






游戏是给用户玩的，所以需要设置一些UI提示给用户看。




#### 文章目录


* + [分数UI](#UI_3)
	+ [胜利UI](#UI_32)
	+ [完整脚本](#_52)




### 分数UI


创建Text（新版本这个组件隐藏在旧版中了），设置为ScoreText；


![在这里插入图片描述](https://img-blog.csdnimg.cn/2a42be8d1035410bbba095f5a3835c3d.png)


双击文本组件，并设置为2D视图，可以修改属性；


![在这里插入图片描述](https://img-blog.csdnimg.cn/2505ceee9d824879b8493003dde2c021.png)


然后添加文本，首先加入头文件：



```
using UnityEngine.UI;

```

设置分数初值并定义分数文本：



```
    public int score = 0;   //分数初值
    public Text scoreText;  //定义分数UI

```

然后在触发检测中设置每吃掉一个Food加+1：



```
            score++;    //吃一个Food分数+1
            scoreText.text = "分数：" + score;

```

### 胜利UI


同理，添加胜利文本，但要注意一点就是，默认情况下这个文本是不显示的（组件取消勾选），只有分数达到胜利的标准才会显示该文本：


![在这里插入图片描述](https://img-blog.csdnimg.cn/7bfa9710b471494dba230667ad1ad5f4.png)


然后添加脚本：



```
    public GameObject winText;  //将胜利的UI定位为游戏物体（默认不显示，结束后显示）

```

添加判断胜利逻辑：



```
            //判断游戏胜利
            if (score == 8)
            {
                winText.SetActive(true);    //激活UI
            }

```

### 完整脚本



```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class Player : MonoBehaviour
{
    public Rigidbody rd;    //public或者private（接口）
    public int score = 0;   //分数初值
    public Text scoreText;  //定义分数UI
    public GameObject winText;  //将胜利的UI定位为游戏物体（默认不显示，结束后显示）

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

            score++;    //吃一个Food分数+1
            scoreText.text = "分数：" + score;

            //判断游戏胜利
            if (score == 8)
            {
                winText.SetActive(true);    //激活UI
            }
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


![在这里插入图片描述](https://img-blog.csdnimg.cn/fbf62f538bea493eb670b83ed349a468.png)


以上。





