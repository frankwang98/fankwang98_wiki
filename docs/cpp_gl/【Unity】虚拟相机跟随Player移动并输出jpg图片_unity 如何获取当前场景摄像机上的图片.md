








#### 文章目录


* + [添加相机输出图片](#_1)
	+ [相机跟随移动](#_52)




### 添加相机输出图片


添加相机，创建`GetImage`脚本：


![在这里插入图片描述](https://img-blog.csdnimg.cn/d62b4d263b3e46c0ae1821399142b8fe.png)


思路是创建相机对象，建立事件，按下空格键即将所看到的画面渲染到目标纹理，然后选择保存路径，代码如下：



```
using UnityEngine;
using System.Collections;
using System.IO;

// 获取副相机图像，空格键截取
public class GetImage : MonoBehaviour
{

    public Camera mainCam; //待截图的目标摄像机
    RenderTexture rt;  //声明一个截图时候用的中间变量 
    Texture2D t2d;  //目标纹理
    int num = 0;  //截图计数

    void Start()
    {
        t2d = new Texture2D(400, 300, TextureFormat.RGB24, false);
        rt = new RenderTexture(400, 300, 24);
        mainCam.targetTexture = rt;
    }

    void Update()
    {
        //按下空格键来截图
        if (Input.GetKeyDown(KeyCode.Space))
        {
            //截图到t2d中
            RenderTexture.active = rt;
            t2d.ReadPixels(new Rect(0, 0, rt.width, rt.height), 0, 0);
            t2d.Apply();
            RenderTexture.active = null;

            //将图片保存起来
            byte[] byt = t2d.EncodeToJPG(); //转成jpg格式
            File.WriteAllBytes(Application.dataPath + "//" + num.ToString() + ".jpg", byt); //文件写入

            Debug.Log("当前截图序号为：" + num.ToString());
            num++;  //文件序号
        }
    }
}


```

### 相机跟随移动


不过，以上相机是固定的，也就只能截图一个位置的图片，我们想要的效果是跟随小车，模拟采集小车相机的画面。


参考[这篇](https://blog.csdn.net/ghl1390490928/article/details/80139439?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2~default~CTRLIST~Rate-1-80139439-blog-106318488.pc_relevant_multi_platform_whitelistv3&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2~default~CTRLIST~Rate-1-80139439-blog-106318488.pc_relevant_multi_platform_whitelistv3&utm_relevant_index=2)，采用固定摄像机方法。


创建 `TheThirdPersonCamera`脚本并添加到副相机上：


![在这里插入图片描述](https://img-blog.csdnimg.cn/55869fa4236f4a8c9d5fb00af4430b13.png)


思路是获取Player的位置，然后在此基础上确定相机位置，来实时跟随获取图像，脚本如下：



```
using UnityEngine;
using System.Collections;

/// <summary>
/// Third person camera.
/// </summary>
public class TheThirdPersonCamera : MonoBehaviour
{
    public float distanceAway = 10f;
    public float distanceUp = 10f;
    public float smooth = 2f;               // how smooth the camera movement is

    private Vector3 m_TargetPosition;       // the position the camera is trying to be in)

    Transform follow;        //the position of Player

    void Start()
    {
        follow = GameObject.FindWithTag("Player").transform;
    }

    void LateUpdate()
    {
        // setting the target position to be the correct offset from the 
        m_TargetPosition = follow.position + Vector3.up \* distanceUp + follow.forward \* distanceAway;
        //Debug.Log(follow.position);
        //Debug.Log(follow.forward);

        // 相机移动更平滑
        transform.position = Vector3.Lerp(transform.position, m_TargetPosition, Time.deltaTime \* smooth);

        // make sure the camera is looking the right way!
        transform.LookAt(follow);
    }
}


```

最后，截取到的图像如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/e26b8ece31d34e4086d97d892bf34c90.png)


以上。





