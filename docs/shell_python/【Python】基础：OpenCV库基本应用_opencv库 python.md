







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍OpenCV库基本应用。  
>  **学其所用，用其所学。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. opencv-python介绍](#smirk1_opencvpython_7)
	+ [:blush:2. 环境安装与配置](#blush2__25)
	+ [:satisfied:3. 应用示例](#satisfied3__37)




### 😏1. opencv-python介绍


OpenCV（Open Source Computer Vision Library）是一个广泛使用的开源计算机视觉库，它提供了用于处理图像和视频的各种功能和算法。以下是一些常见的功能和应用：



> 
> 1.图像和视频的读取和写入：OpenCV 可以读取和写入各种图像和视频格式，包括常见的 JPEG、PNG、BMP 和视频文件。
> 
> 
> 



> 
> 2.图像处理和操作：OpenCV 提供了许多图像处理和操作功能，如调整大小、裁剪、旋转、翻转、平移、滤波、边缘检测、直方图均衡化等。
> 
> 
> 



> 
> 3.特征检测和描述：OpenCV 提供各种特征检测和描述算法，包括关键点检测（如 Harris 角点检测、FAST 特征检测等）、特征描述（如 SIFT、SURF、ORB、BRISK 算法等）和特征匹配。
> 
> 
> 



> 
> 4.目标检测和跟踪：OpenCV 支持多种目标检测和跟踪算法，如 Haar 特征分类器、HOG+SVM、深度学习框架（如 TensorFlow、PyTorch）集成等。
> 
> 
> 



> 
> 5.图像分割和轮廓提取：OpenCV 提供了各种图像分割算法，如基于阈值的方法、基于边缘的方法（如 Canny 边缘检测）以及更高级的分割算法（如 GrabCut、分水岭算法等）。
> 
> 
> 



> 
> 6.相机标定和几何校正：OpenCV 支持相机标定和几何校正，帮助消除图像中的畸变，并恢复真实世界中的几何信息。
> 
> 
> 



> 
> 7.计算机视觉应用：OpenCV 在计算机视觉领域有广泛的应用，包括人脸检测和识别、目标跟踪、图像拼接、图像增强、实时图像处理、虚拟和增强现实等。
> 
> 
> 


OpenCV 支持多种编程语言，包括 C++、Python、Java 等。在 Python 中使用 OpenCV，可以通过安装相应的 Python 包 `opencv-python` 来使用。


### 😊2. 环境安装与配置


pip包安装比较简单，用 `pip install opencv-python` 即可，但有时因为版本的不同，安装会出错，我的错误是：



```
ERROR: Could not find a version that satisfies the requirement opencv-python (from versions: none)
ERROR: No matching distribution found for opencv-python

```

然后是通过下载离线的whl文件安装可以，whl包地址：`https://pypi.tuna.tsinghua.edu.cn/simple/opencv-python/`


然后安装：`pip install xxx.whl`


最后说一句，python的生态很强，封装的库太好用了，若是配c++的opencv环境，得麻烦10倍。


### 😆3. 应用示例


人脸识别示例：



```
import cv2

# 加载人脸识别的模型文件
face_cascade = cv2.CascadeClassifier(cv2.data.haarcascades + 'haarcascade\_frontalface\_default.xml')

# 打开摄像头
cap = cv2.VideoCapture(0)

while True:
    # 读取摄像头帧
    ret, frame = cap.read()

    # 将帧转换为灰度图像
    gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)

    # 在灰度图像上进行人脸检测
    faces = face_cascade.detectMultiScale(gray, scaleFactor=1.1, minNeighbors=5, minSize=(30, 30))

    # 在检测到的每个人脸周围画一个矩形框
    for (x, y, w, h) in faces:
        cv2.rectangle(frame, (x, y), (x+w, y+h), (0, 255, 0), 3)

    # 显示结果帧
    cv2.imshow('Face Detection', frame)

    # 按下 'q' 键退出循环
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

# 释放资源
cap.release()
cv2.destroyAllWindows()


```

眼动检测：



```
import cv2

# 加载眼睛检测器
# xml下载：https://github.com/anaustinbeing/haar-cascade-files/blob/master/haarcascade\_eye.xml
eye_cascade = cv2.CascadeClassifier('haarcascade\_eye.xml')

# 打开摄像头
cap = cv2.VideoCapture(0)

# 定义用于跟踪眼睛位置的变量
prev_eye_x = 0
prev_eye_y = 0

while True:
    # 从摄像头捕获视频帧
    ret, frame = cap.read()

    # 将图像转换为灰度图像
    gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)

    # 检测眼睛
    eyes = eye_cascade.detectMultiScale(gray, 1.3, 5)

    # 遍历检测到的眼睛
    for (x, y, w, h) in eyes:
        # 绘制矩形框显示眼睛位置
        cv2.rectangle(frame, (x, y), (x + w, y + h), (0, 255, 0), 2)

        # 计算眼睛的中心坐标
        eye_x = x + w // 2
        eye_y = y + h // 2

        # 计算眼睛的运动
        eye_dx = eye_x - prev_eye_x
        eye_dy = eye_y - prev_eye_y

        # 更新之前的眼睛位置
        prev_eye_x = eye_x
        prev_eye_y = eye_y

        # 在图像上绘制眼睛运动向量
        cv2.arrowedLine(frame, (eye_x, eye_y), (eye_x + eye_dx, eye_y + eye_dy), (0, 0, 255), 2)

    # 显示视频帧
    cv2.imshow('Eye Tracking', frame)

    # 按下 "q" 键退出循环
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

# 释放摄像头资源
cap.release()
cv2.destroyAllWindows()

```

手势控制电脑音量示例：



```
import cv2
import mediapipe as mp
from ctypes import cast, POINTER
from comtypes import CLSCTX_ALL
from pycaw.pycaw import AudioUtilities, IAudioEndpointVolume
import time
import math
import numpy as np

# 手势控制音量
class HandControlVolume:
    def \_\_init\_\_(self):
        # 初始化medialpipe
        self.mp_drawing = mp.solutions.drawing_utils
        self.mp_drawing_styles = mp.solutions.drawing_styles
        self.mp_hands = mp.solutions.hands

        # 获取电脑音量范围
        devices = AudioUtilities.GetSpeakers()
        interface = devices.Activate(IAudioEndpointVolume._iid_, CLSCTX_ALL, None)
        self.volume = cast(interface, POINTER(IAudioEndpointVolume))
        self.volume.SetMute(0, None)
        self.volume_range = self.volume.GetVolumeRange()

    # 主函数
    def recognize(self):
        # 计算刷新率
        fpsTime = time.time()

        # OpenCV读取视频流(0)
        cap = cv2.VideoCapture(0)
        # 视频分辨率
        resize_w = 640
        resize_h = 480

        # 画面显示初始化参数
        rect_height = 0
        rect_percent_text = 0

        with self.mp_hands.Hands(min_detection_confidence=0.7,
                                 min_tracking_confidence=0.5,
                                 max_num_hands=2) as hands:
            while cap.isOpened():
                success, image = cap.read()
                image = cv2.resize(image, (resize_w, resize_h))

                if not success:
                    print("空帧.")
                    continue

                # 提高性能
                image.flags.writeable = True
                # 转为RGB
                image = cv2.cvtColor(image, cv2.COLOR_BGR2RGB)
                # 镜像
                image = cv2.flip(image, 1)
                # mediapipe模型处理
                results = hands.process(image)

                image.flags.writeable = True
                image = cv2.cvtColor(image, cv2.COLOR_RGB2BGR)
                # 判断是否有手掌
                if results.multi_hand_landmarks:
                    # 遍历每个手掌
                    for hand_landmarks in results.multi_hand_landmarks:
                        # 在画面标注手指
                        self.mp_drawing.draw_landmarks(
                            image,
                            hand_landmarks,
                            self.mp_hands.HAND_CONNECTIONS,
                            self.mp_drawing_styles.get_default_hand_landmarks_style(),
                            self.mp_drawing_styles.get_default_hand_connections_style())

                        # 解析手指，存入各个手指坐标
                        landmark_list = []
                        for landmark_id, finger_axis in enumerate(
                                hand_landmarks.landmark):
                            landmark_list.append([
                                landmark_id, finger_axis.x, finger_axis.y,
                                finger_axis.z
                            ])
                        if landmark_list:
                            # 获取大拇指指尖坐标
                            thumb_finger_tip = landmark_list[4]
                            thumb_finger_tip_x = math.ceil(thumb_finger_tip[1] \* resize_w)
                            thumb_finger_tip_y = math.ceil(thumb_finger_tip[2] \* resize_h)
                            # 获取食指指尖坐标
                            index_finger_tip = landmark_list[8]
                            index_finger_tip_x = math.ceil(index_finger_tip[1] \* resize_w)
                            index_finger_tip_y = math.ceil(index_finger_tip[2] \* resize_h)
                            # 中间点
                            finger_middle_point = (thumb_finger_tip_x + index_finger_tip_x) // 2, (
                                    thumb_finger_tip_y + index_finger_tip_y) // 2
                            # print(thumb\_finger\_tip\_x)
                            thumb_finger_point = (thumb_finger_tip_x, thumb_finger_tip_y)
                            index_finger_point = (index_finger_tip_x, index_finger_tip_y)
                            # 画指尖2点
                            image = cv2.circle(image, thumb_finger_point, 10, (255, 0, 255), -1)
                            image = cv2.circle(image, index_finger_point, 10, (255, 0, 255), -1)
                            image = cv2.circle(image, finger_middle_point, 10, (255, 0, 255), -1)
                            # 画2点连线
                            image = cv2.line(image, thumb_finger_point, index_finger_point, (255, 0, 255), 5)
                            # 勾股定理计算长度
                            line_len = math.hypot((index_finger_tip_x - thumb_finger_tip_x),
                                                  (index_finger_tip_y - thumb_finger_tip_y))

                            # 获取电脑最大最小音量
                            min_volume = self.volume_range[0]
                            max_volume = self.volume_range[1]
                            # 将指尖长度映射到音量上
                            vol = np.interp(line_len, [50, 300], [min_volume, max_volume])
                            # 将指尖长度映射到矩形显示上
                            rect_height = np.interp(line_len, [50, 300], [0, 200])
                            rect_percent_text = np.interp(line_len, [50, 300], [0, 100])

                            # 设置电脑音量
                            self.volume.SetMasterVolumeLevel(vol, None)

                # 显示矩形
                cv2.putText(image, str(math.ceil(rect_percent_text)) + "%", (10, 350),
                            cv2.FONT_HERSHEY_PLAIN, 3, (255, 0, 0), 3)
                image = cv2.rectangle(image, (30, 100), (70, 300), (255, 0, 0), 3)
                image = cv2.rectangle(image, (30, math.ceil(300 - rect_height)), (70, 300), (255, 0, 0), -1)

                # 显示刷新率FPS
                cTime = time.time()
                fps_text = 1 / (cTime - fpsTime)
                fpsTime = cTime
                cv2.putText(image, "FPS: " + str(int(fps_text)), (10, 70),
                            cv2.FONT_HERSHEY_PLAIN, 3, (255, 0, 0), 3)
                # 显示画面
                cv2.imshow('HandControlVolume', image)
                if cv2.waitKey(1) & 0xFF == ord('q'):
                    break
            cap.release()

# 初始化与识别调用
control = HandControlVolume()
control.recognize()

```

yolo目标检测示例：



```
import cv2

# 加载目标检测器
net = cv2.dnn.readNetFromDarknet("yolov3.cfg", "yolov3.weights")  # 替换为实际的模型和权重文件路径
layers_names = net.getLayerNames()
output_layers = [layers_names[i[0] - 1] for i in net.getUnconnectedOutLayers()]

# 加载图像
image = cv2.imread("image.jpg")  # 替换为实际的图像文件路径

# 对图像进行预处理
blob = cv2.dnn.blobFromImage(image, 0.00392, (416, 416), (0, 0, 0), True, crop=False)
net.setInput(blob)
outs = net.forward(output_layers)

# 解析输出并绘制边界框
class_ids = []
confidences = []
boxes = []
width = image.shape[1]
height = image.shape[0]

for out in outs:
    for detection in out:
        scores = detection[5:]
        class_id = np.argmax(scores)
        confidence = scores[class_id]
        if confidence > 0.5:  # 设置置信度阈值
            center_x = int(detection[0] \* width)
            center_y = int(detection[1] \* height)
            w = int(detection[2] \* width)
            h = int(detection[3] \* height)
            x = int(center_x - w / 2)
            y = int(center_y - h / 2)

            boxes.append([x, y, w, h])
            confidences.append(float(confidence))
            class_ids.append(class_id)

indexes = cv2.dnn.NMSBoxes(boxes, confidences, 0.5, 0.4)  # 设置非最大抑制阈值

font = cv2.FONT_HERSHEY_SIMPLEX
for i in range(len(boxes)):
    if i in indexes:
        x, y, w, h = boxes[i]
        label = str(class_ids[i])
        confidence = confidences[i]
        color = (0, 0, 255)
        cv2.rectangle(image, (x, y), (x + w, y + h), color, 2)
        cv2.putText(image, label, (x, y - 10), font, 0.5, color, 2)

# 显示结果图像
cv2.imshow("Object Detection", image)
cv2.waitKey(0)
cv2.destroyAllWindows()


```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6567c240f1b5443ab60e194d3ffe3803.png)


以上。





