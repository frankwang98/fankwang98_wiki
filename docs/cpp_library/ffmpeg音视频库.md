> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍FFmpeg音视频库介绍与使用示例。  
>  **无专精则不能成，无涉猎则不能通。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞

### 😏1. FFmpeg音视频库介绍

ffmpeg官网：`http://www.ffmpeg.org/`

`FFmpeg`是一款开源的音视频库，提供了处理音视频文件、转码、解码、编码、播放等功能。它是一个完整的跨平台解决方案，支持多种音视频格式，并提供多种API和工具来处理音视频数据。

FFmpeg框架的基本组成包括：`AVFormet`封装模块、`AVCodec`编解码模块、`AVFilter`滤镜模块、`AVDevice`、`AVUtil`等模块库。

下面简单介绍一些FFmpeg库的基础知识：

> 1.编码器与解码器：FFmpeg提供了多种编码器和解码器来处理不同的音视频格式，例如H.264、MPEG-4、AAC等。可以使用avcodec_find_encoder和avcodec_find_decoder函数查找可用的编码器和解码器，并使用avcodec_open2函数打开需要使用的编码器或解码器。

> 2.格式封装与解封装：FFmpeg可以处理多种音视频文件格式，例如MP4、AVI、WAV等。它使用封装格式来将音视频流打包到一个容器中。常见的封装格式有MP4、AVI、FLV、MKV等。可以使用avformat_open_input函数打开音视频文件，并使用av_read_frame函数读取文件中的音视频数据。

> 3.帧与数据包：在FFmpeg中，音视频数据被组织成帧和数据包。音频数据通常被组织成PCM数据，每个样本对应一帧数据；而视频数据则被组织成一系列关键帧和非关键帧。

> 4.协议：FFmpeg可以处理不同的音视频流传输协议，例如RTSP、RTMP、HTTP等。可以使用avformat_open_input函数打开网络音视频流，并使用av_read_frame函数读取数据包。

### 😊2. 环境配置

下面进行环境配置：

```bash
# apt安装
sudo apt install ffmpeg
ffmpeg -version
# 也可选择源码安装

# windows可从官网下载

```

编译示例：

```bash
g++ -o main main.cpp -lavformat -lavcodec -lavutil

```

CMakeLists.txt示例：

```bash
cmake_minimum_required(VERSION 3.19)
project(ffmpeg_demo)

set(CMAKE_CXX_STANDARD 14)

set(FFMPEG_ROOT "D:/develop/ffmpeg611_mingw/mingw64")
include_directories(${FFMPEG_ROOT}/include)
link_directories(${FFMPEG_ROOT}/lib)
set(FFMPEG_LIBS avcodec avformat avutil avdevice avfilter postproc swresample swscale)
#link_directories(${FFMPEG_ROOT}/bin)
#set(FFMPEG_LIBS avcodec-60 avformat-60 avutil-58 swscale-7)

add_executable(ffmpeg_demo main.cpp)
target_link_libraries(ffmpeg_demo ${FFMPEG_LIBS})

```

### 😆3. 应用实例

#### 视频

采集摄像头：

```bash
ffmpeg -f video4linux2 -s  640x480 -pixel_format yuyv422  -i /dev/video0  out.mp4 -loglevel debug
ffmpeg -f x11grab -framerate 25 -video_size 1280*720 -i :0.0 out.mp4	# 采集x11桌面

```

视频格式转换：

```bash
ffmpeg -i input.avi output.mp4

```

帧率转换：

```bash
ffmpeg -i input.avi -r 24 output.mp4

```

多路视频拼接命令行：

```bash
# 两路视频横向拼接
ffmpeg -i out1.mp4 -i out2.mp4 -filter_complex "[0:v]pad=iw*2:ih*1[a];[a][1:v]overlay=w" out.mp4
# 两路视频纵向拼接
ffmpeg -i out1.mp4 -i out2.mp4 -filter_complex "[0:v]pad=iw:ih*2[a];[a][1:v]overlay=0:h" out.mp4
# 三路视频横向拼接
ffmpeg -i out1.mp4 -i out2.mp4 -i out3.mp4 -filter_complex "[0:v]pad=iw*3:ih*1[a];[a][1:v]overlay=w[b];[b][2:v]overlay=2.0*w" out.mp4 
# 三路视频纵向拼接
ffmpeg -i out1.mp4 -i out2.mp4 -i out3.mp4 -filter_complex "[0:v]pad=iw:ih*3[a];[a][1:v]overlay=0:h[b];[b][2:v]overlay=0:2.0*h" out.mp4
# 四路视频2X2排列
ffmpeg -i out1.mp4 -i out2.mp4 -i out3.mp4 -i out4.mp4 -filter_complex "[0:v]pad=iw*2:ih*2[a];[a][1:v]overlay=w[b];[b][2:v]overlay=0:h[c];[c][3:v]overlay=w:h" out.mp4

```

视频推流：

```bash
ffmpeg -r 100 -i /dev/video0 -f flv udp://192.168.111.121:9666
ffplay -max_delay 10 udp://192.168.111.121:9666
# 原始音视频推流
ffmpeg  -f alsa -ar 11025 -ac 2 -i hw:0 -i /dev/video0 -f flv udp://192.168.111.121:9666

```

#### 音频

音频格式转换：

```bash
ffmpeg64 -i null.ape -ar 44100 -ac 2 -ab 16k -vol 50 -f mp3 null.mp3
注：-i代表输入参数
          -acodec aac（音频编码用AAC） 
          -ar 设置音频采样频率
          -ac  设置音频通道数
          -ab 设定声音比特率
          -vol <百分比> 设定音量

# 或创建list包含所需音频
ffmpeg -safe 0 -f concat -i list.txt -c copy out.wav

```

音频拼接：

```bash
# 新建列表，包含所需音频
touch list.txt
file '1.mp3'
file '2.mp3'
ffmpeg -f concat -i list.txt -c copy out.mp3

```

音频录制：

```bash
ffmpeg -y -f alsa -i hw:0 -t 00:00:03 -ar 8000 -ac 1 out.mp3

```

音频推流：

```bash
ffmpeg  -f alsa -i hw:0 -ar 11025 -ac 1 -f flv udp://192.168.111.121:9666

```

其他参考：

```bash
视频播放器的实现：http://t.csdn.cn/N6Vuo

音视频播放器实现：http://t.csdn.cn/zJuXn

通过 opencv 读取摄像头：http://t.csdn.cn/mGCog

推送摄像头 rtsp 流：http://t.csdn.cn/YrLMm

```

#### C++示例

```cpp
extern "C" {
#include <libavformat/avformat.h>
#include <libavutil/avutil.h>
}
#include <map>

int main(int argc, char *argv[])
{
    if (argc < 3) {
        printf("remux <input> <output>n");
        return -1;
    }

    const char *in_filename  = argv[1];
    const char *out_filename = argv[2];

    // input
    AVFormatContext *decoder_fmt_ctx = nullptr;
    if (avformat_open_input(&decoder_fmt_ctx, in_filename, nullptr, nullptr) < 0) {
        fprintf(stderr, "failed to open the %s file.n", in_filename);
        return -1;
    }

    if (avformat_find_stream_info(decoder_fmt_ctx, nullptr) < 0) {
        fprintf(stderr, "not find stream info.n");
        return -1;
    }

    av_dump_format(decoder_fmt_ctx, 0, in_filename, 0);

    // output
    AVFormatContext *encoder_fmt_ctx = nullptr;
    if (avformat_alloc_output_context2(&encoder_fmt_ctx, nullptr, nullptr, out_filename) < 0) {
        fprintf(stderr, "failed to alloc output-context memory.n");
        return -1;
    }

    // map streams
    //
    // a media file always contains several streams, like video and audio streams.
    std::map<unsigned int, int> stream_mapping{};
    int stream_idx = 0;
    for (unsigned int i = 0; i < decoder_fmt_ctx->nb_streams; i++) {
        AVCodecParameters *decode_params = decoder_fmt_ctx->streams[i]->codecpar;
        if (decode_params->codec_type != AVMEDIA_TYPE_VIDEO &&
            decode_params->codec_type != AVMEDIA_TYPE_AUDIO &&
            decode_params->codec_type != AVMEDIA_TYPE_SUBTITLE) {
            stream_mapping[i] = -1;
            continue;
        }

        stream_mapping[i] = stream_idx++;

        AVStream *encode_stream = avformat_new_stream(encoder_fmt_ctx, nullptr);
        if (encode_stream == nullptr) {
            fprintf(stderr, "failed to create a stream for output.n");
            return -1;
        }

        if (avcodec_parameters_copy(encode_stream->codecpar, decode_params) < 0) {
            fprintf(stderr, "failed to copy parameters.n");
            return -1;
        }
    }

    // open the output file
    if (!(encoder_fmt_ctx->oformat->flags & AVFMT_NOFILE)) {
        if (avio_open(&encoder_fmt_ctx->pb, out_filename, AVIO_FLAG_WRITE) < 0) {
            printf("can not open the output file : %s.n", out_filename);
            return -1;
        }
    }

    // header
    if (avformat_write_header(encoder_fmt_ctx, nullptr) < 0) {
        fprintf(stderr, "can not write header to the output file.n");
        return -1;
    }

    av_dump_format(encoder_fmt_ctx, 0, out_filename, 1);

    // copy streams
    AVPacket *packet     = av_packet_alloc();
    int64_t frame_number = 0;
    while (av_read_frame(decoder_fmt_ctx, packet) >= 0) {
        if (stream_mapping[packet->stream_index] < 0) {
            // the packet is reference-counted.
            // the ffmpeg is a C library, we need unreference the buffer manually
            av_packet_unref(packet);
            continue;
        }

        // convert the timestamps from the time base of input stream to the time base of output stream
        //
        // In simple terms, a time base is a rescaling of the unit second, like 1/100s, 1001/24000s, and
        // each stream has its own time base. The timestamps of the packets are based on the time base of
        // their streams, timestamps of the same value have different meanings depending on the time unit.
        //
        // It is not necessary in this example since the time base of input and output streams is the same.
        av_packet_rescale_ts(packet, decoder_fmt_ctx->streams[packet->stream_index]->time_base,
                             encoder_fmt_ctx->streams[stream_mapping[packet->stream_index]]->time_base);

        printf(
                " -- %s] packet = %6ld, pts: %6ld, dts: %6ld, duration: %5ldn",
                av_get_media_type_string(encoder_fmt_ctx->streams[packet->stream_index]->codecpar->codec_type),
                frame_number++, packet->pts, packet->dts, packet->duration);

        packet->stream_index = stream_mapping[packet->stream_index];
        // write the packet to the output file
        // this function will take the owership of the packet, and packet will be blank
        if (av_interleaved_write_frame(encoder_fmt_ctx, packet) != 0) {
            fprintf(stderr, "failed to write frame.n");
            return -1;
        }
    }

    // write the trailer to the output file and close it
    av_write_trailer(encoder_fmt_ctx);
    if (encoder_fmt_ctx && !(encoder_fmt_ctx->oformat->flags & AVFMT_NOFILE))
        avio_closep(&encoder_fmt_ctx->pb);

    // release the resources
    // let's not think about the abnormal exit for simplicity
    av_packet_free(&packet);
    avformat_close_input(&decoder_fmt_ctx);
    avformat_free_context(encoder_fmt_ctx);

    return 0;
}

```

### 😆4. 视频播放器示例

视频播放器项目Github地址：`https://github.com/pockethook/player.git`

视频播放器项目主要使用`FFmpeg`做视频编解码，用`SDL`做渲染。

编译运行：

```bash
make
./player xxx.mp4

```

以上。
