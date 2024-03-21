







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍MinIO的使用。  
>  **学其所用，用其所学。——梁启超**  
>  欢迎来到我的博客，一起学习知识，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. MinIO介绍](#smirk1_MinIO_7)
	+ [:blush:2. MinIO文件服务器安装](#blush2_MinIO_15)
	+ [:satisfied:3. SDK安装与C++实现上传下载](#satisfied3_SDKC_62)




### 😏1. MinIO介绍


MinIO是一种高性能、可扩展的**对象存储服务**，它可以在私有云、公共云和边缘计算环境中运行。MinIO的设计目标是为了满足现代应用程序对数据存储的需求，例如视频流处理、机器学习、大数据分析等。


MinIO使用分布式架构来实现高可用性和可伸缩性。它可以在多个服务器之间分配数据，以提供更高的存储容量和更快的读写速度。此外，MinIO还支持S3 API，这使得它可以轻松地与其他S3兼容的服务集成。


MinIO的另一个优点是它的易用性。通过简单的命令行界面或API，用户可以轻松地创建、删除和管理存储桶，上传和下载文件，以及进行其他常见的对象存储操作。


总之，MinIO是一种高性能、易用、可扩展的对象存储解决方案，适用于各种规模的应用场景。


### 😊2. MinIO文件服务器安装


MinIO支持k8s、docker、Linux、Win、MacOS多种安装方式，这里我用的Linux安装。


下载minio：



```
cd /opt && sudo mkdir minio && cd minio
sudo wget https://dl.minio.io/server/minio/release/linux-amd64/minio
sudo touch minio.log && sudo mkdir data && sudo chmod 777 minio

```

启动minio：



```
sudo ./minio server /opt/minio/data （/opt/minio/data 为存放静态文件的目录）
# 但控制台端口会动态变化，可使用 `--console-address “:PORT”` 选择静态端口。
sudo ./minio server /opt/minio/data --console-address ":62222"

```

另外可通过这样设置登录名和密码：



```
sudo vim /etc/profile
# set minio environment
export MINIO\_ROOT\_USER=fileadmin
export MINIO\_ROOT\_PASSWORD=fileadmin
source /etc/profile

```

访问Web界面：



```
如：127.0.0.1:62222

```

设置后台启动：



```
vim minio-start.sh
sudo nohup /opt/minio/minio server  /opt/minio/data --console-address ":62222" | sudo tee /opt/minio/minio.log &
bash minio-start.sh
# 或单独启动
sudo /opt/minio/minio server /opt/minio/data --console-address ":62222"

```

运行如下（创建存储桶，可上传下载文件）：


![在这里插入图片描述](https://img-blog.csdnimg.cn/b3661e6bb4574c19a58e0cd69ba90510.png)


### 😆3. SDK安装与C++实现上传下载


Github地址：`https://github.com/minio/minio-cpp`


SDK参考：`https://minio-cpp.min.io/`


安装SDK：



```
# vcpkg
vcpkg install minio-cpp
# 源码安装
git clone https://github.com/minio/minio-cpp
cd minio-cpp
wget --quiet -O vcpkg-master.zip https://github.com/microsoft/vcpkg/archive/refs/heads/master.zip
unzip -qq vcpkg-master.zip
./vcpkg-master/bootstrap-vcpkg.sh
./vcpkg-master/vcpkg integrate install
cmake -B ./build -DCMAKE\_BUILD\_TYPE=Debug -DCMAKE\_TOOLCHAIN\_FILE=./vcpkg-master/scripts/buildsystems/vcpkg.cmake
cmake --build ./build --config Debug

```

C++上传下载示例：



```
#include <iostream>
#include <minio/minio.h>

int main() {
    // MinIO服务器的连接信息
    std::string minioEndpoint = "your\_minio\_endpoint";
    std::string accessKey = "your\_access\_key";
    std::string secretKey = "your\_secret\_key";
    bool useSSL = false;

    // 创建Minio对象
    Minio::MinioClient minio(minioEndpoint, accessKey, secretKey, useSSL);

    // 上传文件
    std::string bucketName = "your\_bucket\_name";
    std::string objectName = "your\_object\_name";
    std::string filePath = "path\_to\_your\_file";

    try {
        minio.PutObject(bucketName, objectName, filePath);
        std::cout << "File uploaded successfully." << std::endl;
    } catch (const std::exception& e) {
        std::cerr << "Error occurred: " << e.what() << std::endl;
        return 1;
    }

    // 下载文件
    std::string downloadPath = "path\_to\_save\_downloaded\_file";
    try {
        minio.GetObject(bucketName, objectName, downloadPath);
        std::cout << "File downloaded successfully." << std::endl;
    } catch (const std::exception& e) {
        std::cerr << "Error occurred: " << e.what() << std::endl;
        return 1;
    }

    return 0;
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6567c240f1b5443ab60e194d3ffe3803.png)


以上。





