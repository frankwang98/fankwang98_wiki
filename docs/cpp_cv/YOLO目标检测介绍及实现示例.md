### 1. 原理


我们都知道，`yolo`这些深度学习检测算法都是在python下用`pytorch`或`tf`框架这些训练的，训练得到的是pt或者weight权重文件，这些是算法开发人员做的事情，如何让算法的检测精度更高、速度更快。


但在工程化的时候，一般还是要用`C++`实现的，`OpenCV`不只是能进行图像的基本处理（以前我太肤浅了），它还有很多能处理深度学习的模块，比如`DNN`模块就支持调用多种框架下训练的权重文件。


下面就在`VS2017+OpenCV454`环境下进行演示。可以选择4种yolo变体，可以检测图片或视频。  
 （代码参考[这位博主](http://t.csdn.cn/158zh)，以下是集成和演示）


### 2. 图片检测程序


运行代码前，请先配置好VS和OpenCV环境，然后准备好yolo相关权重文件（cfg+weight）。


首先定义yolo.h头文件：



```cpp
#include <fstream>
#include <sstream>
#include <iostream>
#include <opencv2/dnn.hpp> //调用dnn模块
#include <opencv2/imgproc.hpp>
#include <opencv2/highgui.hpp>

using namespace cv;
using namespace dnn;
using namespace std;

//结构体定义：网络配置参数
struct Net\_config
{
	float confThreshold; // 置信度阈值
	float nmsThreshold;  // 非极大值抑制（重叠率）阈值
	int inpWidth;  
	int inpHeight; 
	string classesFile;	//类别文件名
	string modelConfiguration;	//模型配置文件
	string modelWeights;	//模型权重
	string netname;	//模型名称
};

//定义yolo类
class YOLO
{
	public:
		YOLO(Net_config config);
		void detect(Mat& frame);	//检测函数
	private:
		float confThreshold;	//类别置信度阈值
		float nmsThreshold;		//重叠率阈值
		int inpWidth;	//图片宽度
		int inpHeight;	//图片高度
		char netname[20];	//网络名称
		vector<string> classes;	//存储类别的数组
		Net net;	//深度学习模型读取
		void postprocess(Mat& frame, const vector<Mat>& outs);	//后处理函数
		void drawPred(int classId, float conf, int left, int top, int right, int bottom, Mat& frame);	//画框
};

//定义网络数组
Net_config yolo_nets[4] = {
	{0.5, 0.4, 416, 416,"coco.names", "yolov3/yolov3.cfg", "yolov3/yolov3.weights", "yolov3"},
	{0.5, 0.4, 608, 608,"coco.names", "yolov4/yolov4-tiny.cfg", "yolov4/yolov4-tiny.weights", "yolov4-tiny"},
	{0.5, 0.4, 320, 320,"coco.names", "yolo-fastest/yolo-fastest-xl.cfg", "yolo-fastest/yolo-fastest-xl.weights", "yolo-fastest"},
	{0.5, 0.4, 320, 320,"coco.names", "yolobile/csdarknet53s-panet-spp.cfg", "yolobile/yolobile.weights", "yolobile"}
};

```

然后进入main主程序：



```cpp
#include "yolo.h"

//网络配置构造函数
YOLO::YOLO(Net_config config)
{
	cout << "Net use " << config.netname << endl;
	this->confThreshold = config.confThreshold;
	this->nmsThreshold = config.nmsThreshold;
	this->inpWidth = config.inpWidth;
	this->inpHeight = config.inpHeight;
	strcpy\_s(this->netname, config.netname.c\_str());

	ifstream ifs(config.classesFile.c\_str());
	string line;
	while (getline(ifs, line)) this->classes.push\_back(line);

	this->net = readNetFromDarknet(config.modelConfiguration, config.modelWeights);
	this->net.setPreferableBackend(DNN_BACKEND_OPENCV);
	this->net.setPreferableTarget(DNN_TARGET_CPU);
}

//后处理
void YOLO::postprocess(Mat& frame, const vector<Mat>& outs)   // Remove the bounding boxes with low confidence using non-maxima suppression
{
	vector<int> classIds;	//类别
	vector<float> confidences;	//置信度
	vector<Rect> boxes;	//框

	for (size_t i = 0; i < outs.size(); ++i)
	{
		// Scan through all the bounding boxes output from the network and keep only the
		// ones with high confidence scores. Assign the box's class label as the class
		// with the highest score for the box.
		float\* data = (float\*)outs[i].data;
		for (int j = 0; j < outs[i].rows; ++j, data += outs[i].cols)
		{
			Mat scores = outs[i].row(j).colRange(5, outs[i].cols);
			Point classIdPoint;
			double confidence;
			// Get the value and location of the maximum score
			minMaxLoc(scores, 0, &confidence, 0, &classIdPoint);
			//当置信度大于阈值
			if (confidence > this->confThreshold)
			{
				int centerX = (int)(data[0] \* frame.cols);
				int centerY = (int)(data[1] \* frame.rows);
				int width = (int)(data[2] \* frame.cols);
				int height = (int)(data[3] \* frame.rows);
				int left = centerX - width / 2;
				int top = centerY - height / 2;

				classIds.push\_back(classIdPoint.x);
				confidences.push\_back((float)confidence);
				boxes.push\_back(Rect(left, top, width, height));
			}
		}
	}

	// Perform non maximum suppression to eliminate redundant overlapping boxes with
	// lower confidences
	vector<int> indices;
	NMSBoxes(boxes, confidences, this->confThreshold, this->nmsThreshold, indices);
	for (size_t i = 0; i < indices.size(); ++i)
	{
		int idx = indices[i];
		Rect box = boxes[idx];
		this->drawPred(classIds[idx], confidences[idx], box.x, box.y,
			box.x + box.width, box.y + box.height, frame);
	}
}

//画预测框
void YOLO::drawPred(int classId, float conf, int left, int top, int right, int bottom, Mat& frame)   // Draw the predicted bounding box
{
	//Draw a rectangle displaying the bounding box 画框
	rectangle(frame, Point(left, top), Point(right, bottom), Scalar(0, 0, 255), 3);

	//Get the label for the class name and its confidence 打标签
	string label = format("%.2f", conf);
	if (!this->classes.empty())
	{
		CV\_Assert(classId < (int)this->classes.size());
		label = this->classes[classId] + ":" + label;
	}

	//Display the label at the top of the bounding box 展示标签
	int baseLine;
	Size labelSize = getTextSize(label, FONT_HERSHEY_SIMPLEX, 0.5, 1, &baseLine);
	top = max(top, labelSize.height);
	//rectangle(frame, Point(left, top - int(1.5 \* labelSize.height)), Point(left + int(1.5 \* labelSize.width), top + baseLine), Scalar(0, 255, 0), FILLED);
	putText(frame, label, Point(left, top), FONT_HERSHEY_SIMPLEX, 0.75, Scalar(0, 255, 0), 1);
}

//detect检测
void YOLO::detect(Mat& frame)
{
	Mat blob;	//blob预处理
	blobFromImage(frame, blob, 1 / 255.0, Size(this->inpWidth, this->inpHeight), Scalar(0, 0, 0), true, false);
	this->net.setInput(blob);
	vector<Mat> outs;
	this->net.forward(outs, this->net.getUnconnectedOutLayersNames());	//前向处理
	this->postprocess(frame, outs);	//后处理

	vector<double> layersTimes;
	double freq = getTickFrequency() / 1000;
	double t = net.getPerfProfile(layersTimes) / freq;
	string label = format("%s Inference time : %.2f ms", this->netname, t);
	putText(frame, label, Point(0, 30), FONT_HERSHEY_SIMPLEX, 1, Scalar(0, 0, 255), 2);
	//imwrite(format("%s\_out.jpg", this->netname), frame);
}

//main入口
int main()
{
	YOLO yolo\_model(yolo_nets[2]);	//选择网络

	//1.图片检测
	string imgpath = "dog.jpg";
	Mat srcimg = imread(imgpath);	//读取照片
	yolo_model.detect(srcimg);	//调用检测程序

	//图片检测界面
	static const string kWinName = "Deep learning object detection in OpenCV C++";
	namedWindow(kWinName, WINDOW_NORMAL);
	imshow(kWinName, srcimg);
	waitKey(0);
	destroyAllWindows();


}

```

运行结果如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/9bdcfce1246a4f11a7cf3ae140cf7888.png)


### 3. 视频检测程序


要调用视频，只需在main函数中加入：



```cpp
	//2.视频检测/实时摄像头
	VideoCapture capture("test.avi");	//0
	Mat frame;
	while (true) {
		int ret = capture.read(frame);
		if (!ret) {
			break;
		}
		//imshow("input", frame); //显示原视频
		yolo_model.detect(frame);	//调用process
		static const string kWinName = "Deep learning object detection in OpenCV C++";
		namedWindow(kWinName, WINDOW_NORMAL);
		imshow(kWinName, frame);

		char c = waitKey(5);
		if (c == 27) {
			break;
		}
	}

```

运行结果如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/594f28eb806c4b39a559dfcc2cc82540.png)


### 其他


还有一个用SSD MobileNet检测的示例：


项目Github地址：`https://github.com/ChiekoN/OpenCV_SSD_MobileNet`



```bash
#编译
mkdir build && cd build
cmake ..
make
./ssd_obj_detect

```

基于ROS的人脸检测的示例：


项目Github地址：`https://github.com/1417265678/robot_vision`



```bash
# 先起相机节点
roslaunch robot_vision usb_cam.launch
# 检测节点
roslaunch robot_vision face_detector.launch

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/bc72e9627a274b5c80b8730dfaa67c0b.png#pic_center)


以上。





