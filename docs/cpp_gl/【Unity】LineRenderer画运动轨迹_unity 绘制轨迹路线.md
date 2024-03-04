








#### 文章目录


* + [LineRenderer画运动轨迹](#LineRenderer_1)




### LineRenderer画运动轨迹


网上关于LineRenderer的资料比较少，最后参考了[这篇](http://t.csdn.cn/AnjzS)，应用到自己的场景中。


首先定义空物体，并转为预制体；默认创建了两个点，并定义线的宽度，最后给线上材质：


![在这里插入图片描述](https://img-blog.csdnimg.cn/33886242390a4450ad95e87bf9d4aceb.png)


然后创建运动轨迹脚本并关联到运动的物体上：


![在这里插入图片描述](https://img-blog.csdnimg.cn/bcb1b845fd584f3199b61343a1d1185b.png)


脚本如下，供参考：



```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

// 创建LineRenderer画出运动轨迹
public class MotorLine : MonoBehaviour
{
    public GameObject lineprefab;
    public GameObject currentline;

    public GameObject emptyPrefab;
    public GameObject lineObject;

    public LineRenderer line;
    private Vector3[] path;

    private List<Vector3> pos = new List<Vector3>();
    private float timer;

    private void Start()
    {
        lineObject = Instantiate(emptyPrefab, transform.position, Quaternion.identity, gameObject.transform);
    }

    // Update is called once per frame
    private void FixedUpdate()
    {
        //每过5s消除之前轨迹
        if (Time.time % 5 == 0 && Time.time >= 5)
        {
            pos.Clear();
            path = pos.ToArray();
            Destroy(lineObject);
            lineObject = Instantiate(emptyPrefab, transform.position, Quaternion.identity, gameObject.transform);
        }

        //每过0.1s画一次
        if (timer <= 0)
        {
            currentline = Instantiate(lineprefab, transform.position, Quaternion.identity, lineObject.transform);
            line = currentline.GetComponent<LineRenderer>();
            pos.Add(transform.position);
            path = pos.ToArray();
            timer = 0.1f;
        }
        timer -= Time.deltaTime;

        if (path.Length != 0)
        {
            line.positionCount = path.Length;
            line.SetPositions(path);
        }
    }
}



```

最后结果如下（蓝线）：


![在这里插入图片描述](https://img-blog.csdnimg.cn/bb563f4605024c069cd8f8250cbacf8b.png)


以上。





