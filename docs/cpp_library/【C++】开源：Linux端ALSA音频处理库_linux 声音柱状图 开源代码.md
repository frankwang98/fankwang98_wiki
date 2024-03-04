







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍Linux端ALSA音频处理库。  
>  **无专精则不能成，无涉猎则不能通。。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. 项目介绍](#smirk1__7)
	+ [:blush:2. 环境配置](#blush2__25)
	+ [:satisfied:3. 使用说明](#satisfied3__41)




### 😏1. 项目介绍


项目Github地址：`https://github.com/alsa-project/alsa-lib`


ALSA（Advanced Linux Sound Architecture）是Linux操作系统上的音频处理框架。它提供了对音频设备的抽象和控制，使应用程序能够与音频硬件进行交互。


ALSA库是ALSA项目的一部分，它为开发者提供了一组API（应用程序编程接口），用于与音频设备进行通信。以下是ALSA库的一些主要特点和功能：



> 
> 1.音频设备访问：ALSA库允许应用程序以底层的方式访问音频硬件设备，如声卡、麦克风、扬声器等。它提供了一套丰富的API，用于打开、关闭、读取和写入音频设备。
> 
> 
> 



> 
> 2.多通道支持：ALSA库支持多通道音频处理，允许应用程序同时处理多个音频流，并在不同的通道上进行独立控制和处理。这对于音频混音、空间处理和音频录制等场景非常有用。
> 
> 
> 



> 
> 3.低延迟音频处理：ALSA库被设计为支持低延迟音频处理，这对于实时音频应用程序（如音频编辑软件、游戏和电话会议）至关重要。它提供了一些特性和配置选项，帮助减少音频传输和处理的延迟。
> 
> 
> 



> 
> 4.硬件控制和参数设置：ALSA库允许应用程序直接访问音频设备的硬件控制参数，如采样率、声道数、音量和音效等。开发者可以使用ALSA库来配置和控制音频设备以满足具体需求。
> 
> 
> 



> 
> 5.MIDI支持：除了音频处理，ALSA库还提供了对MIDI（Musical Instrument Digital Interface）设备的支持。它允许应用程序通过ALSA API与MIDI设备进行通信，实现音乐合成、音序器和控制器等功能。
> 
> 
> 


ALSA库是一个功能强大且广泛使用的音频处理工具，可用于创建各种音频应用程序，包括音乐播放器、音频编辑器、语音识别和合成系统等。它提供了灵活的接口和丰富的功能，使开发者能够轻松地与音频设备进行交互，并实现高质量的音频处理。


### 😊2. 环境配置


下面进行安装运行：



```
# apt安装
sudo apt install libasound2-dev
# g++编译时 -lasound

```


```
# 源码编译
./configure
make 
sudo make install

```

### 😆3. 使用说明


音量控制示例：



```
#include <iostream>
#include <alsa/asoundlib.h>

int main() {
    // 打开默认音频设备
    snd_mixer_t \*handle;
    int res = snd\_mixer\_open(&handle, 0);
    if (res < 0) {
        std::cerr << "无法打开音频设备" << std::endl;
        return 1;
    }

    // 设置音频设备为非阻塞模式
    res = snd\_mixer\_attach(handle, "default");
    if (res < 0) {
        std::cerr << "无法附加到音频设备" << std::endl;
        snd\_mixer\_close(handle);
        return 1;
    }

    res = snd\_mixer\_selem\_register(handle, NULL, NULL);
    if (res < 0) {
        std::cerr << "无法注册音频元素" << std::endl;
        snd\_mixer\_close(handle);
        return 1;
    }

    res = snd\_mixer\_load(handle);
    if (res < 0) {
        std::cerr << "无法加载音频设备" << std::endl;
        snd\_mixer\_close(handle);
        return 1;
    }

    // 获取默认音频元素
    snd_mixer_selem_id_t \*sid;
    snd\_mixer\_selem\_id\_alloca(&sid);
    snd\_mixer\_selem\_id\_set\_index(sid, 0);
    snd\_mixer\_selem\_id\_set\_name(sid, "Master");

    snd_mixer_elem_t\* elem = snd\_mixer\_find\_selem(handle, sid);
    if (!elem) {
        std::cerr << "无法找到音频元素" << std::endl;
        snd\_mixer\_close(handle);
        return 1;
    }

    // 获取音量范围
    long minVolume, maxVolume;
    snd\_mixer\_selem\_get\_playback\_volume\_range(elem, &minVolume, &maxVolume);

    // 增加音量
    long volume;
    snd\_mixer\_selem\_get\_playback\_volume(elem, SND_MIXER_SCHN_FRONT_LEFT, &volume);
    std::cout << "当前音量：" << volume << "/" << maxVolume << std::endl;

    long newVolume = volume + 10;  // 增加10单位的音量
    if (newVolume > maxVolume) {
        newVolume = maxVolume;
    }
    snd\_mixer\_selem\_set\_playback\_volume(elem, SND_MIXER_SCHN_FRONT_LEFT, newVolume);

    std::cout << "增加音量后的音量：" << newVolume << "/" << maxVolume << std::endl;

    // 关闭音频设备
    snd\_mixer\_close(handle);

    return 0;
}


```

编译运行：



```
g++ volume_control.cpp -o volume_control -lasound
./volume_control

```

读取并播放pcm音频文件：



```
#include <alsa/asoundlib.h>

int main() {
    // 打开默认的音频设备
    snd_pcm_t \*pcm;
    if (snd\_pcm\_open(&pcm, "default", SND_PCM_STREAM_PLAYBACK, 0) < 0) {
        printf("无法打开音频设备\n");
        return -1;
    }

    // 配置音频参数
    snd\_pcm\_set\_params(pcm,
                       SND_PCM_FORMAT_S16_LE,   // 采样格式为16位小端
                       SND_PCM_ACCESS_RW_INTERLEAVED,
                       2,                       // 通道数为2（立体声）
                       44100,                   // 采样率为44100Hz
                       1,                        // 精确度为1微秒
                       50000);                   // 缓冲大小设置为50000字节

    // 读取音频数据并播放
    char buffer[1024];
    FILE\* file = fopen("audio.pcm", "rb");  // 以二进制只读方式打开音频文件
    if (file) {
        while (!feof(file)) {
            size_t bytesRead = fread(buffer, 1, sizeof(buffer), file);
            snd\_pcm\_writei(pcm, buffer, bytesRead/4);  // 将音频数据写入音频设备
        }
        fclose(file);
    } else {
        printf("无法打开音频文件\n");
    }

    // 关闭音频设备
    snd\_pcm\_drain(pcm);
    snd\_pcm\_close(pcm);

    return 0;
}


```

编译运行：



```
g++ audio.cpp -o audio -lasound
./audio

```

mp3与pcm格式转换：



```
# 可以用ffmpeg命令行工具
ffmpeg -i input.mp3 -f s16le -acodec pcm_s16le output.pcm
#-i input.mp3：指定输入的MP3文件。
#-f s16le：指定输出格式为16位有符号PCM数据。
#-acodec pcm\_s16le：选择PCM编码器，指定16位有符号的采样格式。

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6cbcd6c17cec4dba9bb3c0f895f02fa2.png)


以上。





