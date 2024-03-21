








#### 文章目录


* + [Unity导航模块](#Unity_1)




### Unity导航模块


看到一篇[自动寻路车辆](http://t.csdn.cn/pYMRp)的Unity仿真，简单使用一下导航模块。


前面已经创建好了一个小车的场景，因此直接来到导航模块。


首先将地面及静态物体设置为navigation static（这是后面bake的前提）：


![在这里插入图片描述](https://img-blog.csdnimg.cn/db9614fb314b45918fef69b8b33c24a4.png)


然后打开导航组件模块，选择Bake烘培：


![在这里插入图片描述](https://img-blog.csdnimg.cn/f23ee02f81634318856be55f18e62de5.png)  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/a41b5e2458f14b27961b987be102efbf.png)


烘培好地图后，为Car添加NavMeshAgent组件：


![在这里插入图片描述](https://img-blog.csdnimg.cn/b08dc34d27a64e44a788e3df835a6279.png)


这时，Car知道自己要Nav了，但还没有目标地点，我们先创建一个空对象TargetObject，并创建Navigation脚本，将两者关联：


![在这里插入图片描述](https://img-blog.csdnimg.cn/e30cf4d2d01d4c5490c697d1aea0ac2f.png)


脚本如下：



```
using UnityEngine;
using UnityEngine.AI;
using System.Collections;

//小车AI导航demo
public class Navigation : MonoBehaviour
{

    public Transform TargetObject = null;

    void Start()
    {
        if (TargetObject != null)
        {
            GetComponent<NavMeshAgent>().destination = TargetObject.position;
        }
    }
    void Update()
    {

    }
}


```

效果如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/6881e298c95e41378600c4fdd2daf4d5.png)


2022.10.26  
 上面只是实现了固定目标地点的导航，每次都要修改目标位置然后重新运行，有点麻烦。下面再记录一种通过鼠标点击确定目的地并实现导航的方法，依然使用的`Navigation`脚本：



```
using UnityEngine;
using UnityEngine.AI;	// 导航系统需要的命名空间
using System.Collections;

//小车AI导航demo
public class Navigation : MonoBehaviour
{

    //public Transform TargetObject = null; //定义空物体
    private NavMeshAgent agent; //导航网格代理组件

    void Start()
    {
        // 移动到空物体所在位置
        //if (TargetObject != null)
        //{
        // GetComponent<NavMeshAgent>().destination = TargetObject.position;
        //}
        agent = GetComponent<NavMeshAgent>();   //获取组件
    }
    void Update()
    {
        // 单击鼠标右键
        if (Input.GetMouseButtonDown(1))
        {
            Ray ray = Camera.main.ScreenPointToRay(Input.mousePosition);    //鼠标指针射线
            RaycastHit hit; //碰撞信息
            bool res = Physics.Raycast(ray, out hit);   //射线碰撞检测
            if (res)
            {
                Vector3 point = hit.point;  //如果检测到碰撞，获取碰撞点
                agent.SetDestination(point);    //将添加了NavMesh的物体移动到碰撞点
            }
        }
    }
}


```

以上。





