







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍基于node-media-server搭建流媒体服务器示例。  
>  **学其所用，用其所学。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. node-media-server介绍](#smirk1_nodemediaserver_7)
	+ [:blush:2. 环境安装与配置](#blush2__23)
	+ [:satisfied:3. 应用示例](#satisfied3__31)




### 😏1. node-media-server介绍


`node-media-server` 是一个基于 `Node.js` 的流媒体服务器，它提供了构建和管理实时音视频流媒体应用程序所需的功能。它是一个开源项目，具有灵活性和可扩展性，适用于各种流媒体应用场景。


以下是一些 `node-media-server` 的特点和功能：



> 
> 1.RTMP支持：node-media-server 支持 RTMP（Real-Time Messaging Protocol）协议，用于接收和传输实时的音视频流。RTMP 适用于实时直播和互动应用等场景。
> 
> 
> 



> 
> 2.多路并发流支持：node-media-server 具有多路并发流处理能力，可以同时处理多个流媒体的接收、转码、推流和录制等操作。
> 
> 
> 



> 
> 3.高性能和低延迟：node-media-server 的设计注重高性能和低延迟，使其适用于实时应用场景，如实时直播、互动直播和视频聊天等。
> 
> 
> 



> 
> 4.支持多种编码格式：node-media-server 支持多种常用的音视频编码格式，如 H.264、AAC、VP8 等，使其能够处理不同类型的流媒体数据。
> 
> 
> 



> 
> 5.功能丰富的 API：node-media-server 提供了丰富的 API，方便开发人员进行配置和管理。你可以通过编写代码来定制和扩展服务器的功能。
> 
> 
> 



> 
> 6.高度可配置：node-media-server 具有灵活的配置选项，允许你根据特定需求进行定制。你可以配置服务器的端口、流媒体路径、认证方式等。
> 
> 
> 


### 😊2. 环境安装与配置



```
# 安装nodejs和ffmpeg
sudo apt install nodejs ffmpeg
# 安装node-media-server
npm install node-media-server

```

### 😆3. 应用示例


创建`app.js`，写入：



```
const NodeMediaServer= require('node-media-server');
const config = {
    rtmp: {
        port: 1935,
        chunk\_size: 60000,
        gop\_cache: true,
        ping: 60,
        ping\_timeout: 30
    },
    http: {
        port: 8000,
        allow\_origin: '\*',
    }
};
 
var nms = new NodeMediaServer(config)
nms.run();

```

运行该程序：`node app.js`


准备好一个mp4视频，用`ffmpeg`命令行推流（也可自己写程序）：



```
ffmpeg -re -i input.mp4 -c:v copy -c:a copy -f flv rtmp://localhost:1935/live/stream_name

```

最后效果示例，地址在`http://localhost:8000/admin/`：


![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/eba0d4c1b2e94ffb9c7ca615452d86b2.png)


另外，也可以用C++程序来实现视频推流，下面是一个示例：



```
// defer.h
#ifndef FFMPEG\_EXAMPLES\_DEFER\_H
#define FFMPEG\_EXAMPLES\_DEFER\_H

#include <utility>

template<typename F> class defer\_raii
{
public:
    // copy/move construction and any kind of assignment would lead to the cleanup function getting
    // called twice. We can't have that.
    defer\_raii(defer_raii&&)                 = delete;
    defer\_raii(const defer_raii&)            = delete;
    defer_raii& operator=(const defer_raii&) = delete;
    defer_raii& operator=(defer_raii&&)      = delete;

    // construct the object from the given callable
    template<typename FF> defer\_raii(FF&& f) : cleanup\_function(std::forward<FF>(f)) {}

    // when the object goes out of scope call the cleanup function
    ~defer\_raii() { cleanup\_function(); }

private:
    F cleanup_function;
};

template<typename F> defer_raii<F> defer\_func(F&& f) { return { std::forward<F>(f) }; }

#define DEFER\_ACTUALLY\_JOIN(x, y) x##y
#define DEFER\_JOIN(x, y) DEFER\_ACTUALLY\_JOIN(x, y)
#ifdef \_\_COUNTER\_\_
#define DEFER\_UNIQUE\_VARNAME(x) DEFER\_JOIN(x, \_\_COUNTER\_\_)
#else
#define DEFER\_UNIQUE\_VARNAME(x) DEFER\_JOIN(x, \_\_LINE\_\_)
#endif

#define defer(lambda\_\_) \
 [[maybe\_unused]] const auto& DEFER\_UNIQUE\_VARNAME(\_defer\_) = defer\_func([&]() { lambda\_\_; })

#endif // !FFMPEG\_EXAMPLES\_DEFER\_H

```


```
// main.cpp
extern "C" {
#include <libavutil/avutil.h>
#include <libavcodec/avcodec.h>
#include <libavformat/avformat.h>
#include <libavutil/time.h>
}

#define \_\_STDC\_CONSTANT\_MACROS
#include <stdint.h>

#include "defer.h"
// #include "logging.h"
#include <iostream>
#include <chrono>
#include <thread>
#include "fmt/format.h"

int main(int argc, char\* argv[])
{
    // Logger::init(argv[0]);

    if (argc < 3) {
        std::cout << "pushing <input\_video> <rtmp\_name>" << std::endl;
        return -1;
    }

    const char \* in_filename = argv[1];
    const char \* rtmp_name = argv[2];

    AVFormatContext \* decoder_fmt_ctx = nullptr;
    avformat\_open\_input(&decoder_fmt_ctx, in_filename, nullptr, nullptr);
    avformat\_find\_stream\_info(decoder_fmt_ctx, nullptr);

    int video_stream_idx = av\_find\_best\_stream(decoder_fmt_ctx, AVMEDIA_TYPE_VIDEO, -1, -1, nullptr, 0);
    // CHECK(video\_stream\_idx >= 0);

    // decoder
    auto decoder = avcodec\_find\_decoder(decoder_fmt_ctx->streams[video_stream_idx]->codecpar->codec_id);
    // CHECK\_NOTNULL(decoder);

    // decoder context
    AVCodecContext \* decoder_ctx = avcodec\_alloc\_context3(decoder);
    // CHECK\_NOTNULL(decoder\_ctx);

    avcodec\_parameters\_to\_context(decoder_ctx, decoder_fmt_ctx->streams[video_stream_idx]->codecpar);
    avcodec\_open2(decoder_ctx, decoder, nullptr);

    av\_dump\_format(decoder_fmt_ctx, 0, in_filename, 0);

    //
    // output
    //
    AVFormatContext \* encoder_fmt_ctx = nullptr;
    avformat\_alloc\_output\_context2(&encoder_fmt_ctx, nullptr, "flv", nullptr);
    avformat\_new\_stream(encoder_fmt_ctx, nullptr);

    // encoder
    auto encoder = avcodec\_find\_encoder\_by\_name("libx264");
    // CHECK\_NOTNULL(encoder);

    AVCodecContext \*encoder_ctx = avcodec\_alloc\_context3(encoder);
    // CHECK\_NOTNULL(encoder\_ctx);

    AVDictionary\* encoder_options = nullptr;
    av\_dict\_set(&encoder_options, "crf", "23", AV_DICT_DONT_OVERWRITE);
    av\_dict\_set(&encoder_options, "threads", "auto", AV_DICT_DONT_OVERWRITE);
    defer(av\_dict\_free(&encoder_options));

    // encoder codec params
    encoder_ctx->height = decoder_ctx->height;
    encoder_ctx->width = decoder_ctx->width;
    encoder_ctx->pix_fmt = decoder_ctx->pix_fmt;
    encoder_ctx->sample_aspect_ratio = decoder_ctx->sample_aspect_ratio;
    encoder_ctx->framerate = av\_guess\_frame\_rate(decoder_fmt_ctx, decoder_fmt_ctx->streams[video_stream_idx], nullptr);

    // time base
    encoder_ctx->time_base = av\_inv\_q(encoder_ctx->framerate);
    encoder_fmt_ctx->streams[0]->time_base = encoder_ctx->time_base;

    avcodec\_open2(encoder_ctx, encoder, &encoder_options);
    avcodec\_parameters\_from\_context(encoder_fmt_ctx->streams[0]->codecpar, encoder_ctx);

    if (!(encoder_fmt_ctx->oformat->flags & AVFMT_NOFILE)) {
        avio\_open(&encoder_fmt_ctx->pb, rtmp_name, AVIO_FLAG_WRITE);
    }

    avformat\_write\_header(encoder_fmt_ctx, nullptr);

    av\_dump\_format(encoder_fmt_ctx, 0, rtmp_name, 1);

    AVPacket \* in_packet = av\_packet\_alloc();
    defer(av\_packet\_free(&in_packet));
    AVPacket \* out_packet = av\_packet\_alloc();
    defer(av\_packet\_free(&out_packet));
    AVFrame \* in_frame = av\_frame\_alloc();
    defer(av\_frame\_free(&in_frame));

    int64\_t first_pts = AV_NOPTS_VALUE;

    while(av\_read\_frame(decoder_fmt_ctx, in_packet) >= 0) {
        if (in_packet->stream_index != video_stream_idx) {
            continue;
        }

        int ret = avcodec\_send\_packet(decoder_ctx, in_packet);
        while(ret >= 0) {
            av\_frame\_unref(in_frame);
            ret = avcodec\_receive\_frame(decoder_ctx, in_frame);
            if (ret == AVERROR(EAGAIN) || ret == AVERROR_EOF) {
                break;
            } else if (ret < 0) {
                std::cout << "[PUSHING] avcodec\_receive\_frame()";
                return ret;
            }

            in_frame->pict_type = AV_PICTURE_TYPE_NONE;

            // sleep @{
            first_pts = first_pts == AV_NOPTS_VALUE ? av\_gettime\_relative() : first_pts;

            int64\_t ts = av\_gettime\_relative() - first_pts;

            int64\_t pts_us = av\_rescale\_q(in_frame->pts, encoder_fmt_ctx->streams[0]->time_base, { 1, AV_TIME_BASE });
            int64\_t sleep_us = std::max<int64\_t>(0, pts_us - ts);

            std::cout << fmt::format("[PUSHING] pts = {:>6.3f}s, ts = {:>6.3f}s, sleep = {:>4d}ms, frame = {:>5d}, fps = {:>5.2f}",
                                     pts_us / 1000000.0, ts / 1000000.0, sleep_us / 1000,
                                     encoder_ctx->frame_number, encoder_ctx->frame_number / (ts / 1000000.0));
            av\_usleep(sleep_us);
            std::this_thread::sleep\_for(std::chrono::milliseconds(10));
            // @}

            ret = avcodec\_send\_frame(encoder_ctx, in_frame);
            while(ret >= 0) {
                av\_packet\_unref(out_packet);
                ret = avcodec\_receive\_packet(encoder_ctx, out_packet);

                if(ret == AVERROR(EAGAIN)) {
                    break;
                } else if(ret == AVERROR_EOF) {
                    std::cout << "[PUSHING] EOF";
                    break;
                } else if(ret < 0) {
                    std::cout << "[PUSHING] avcodec\_receive\_packet()";
                    return ret;
                }

                out_packet->stream_index = 0;
                av\_packet\_rescale\_ts(out_packet, decoder_fmt_ctx->streams[video_stream_idx]->time_base, encoder_fmt_ctx->streams[0]->time_base);

                if (av\_interleaved\_write\_frame(encoder_fmt_ctx, out_packet) != 0) {
                    std::cout <<"[PUSHING] av\_interleaved\_write\_frame()";
                    return -1;
                }
            }
        }
        av\_packet\_unref(in_packet);
    }

    avformat\_close\_input(&decoder_fmt_ctx);
    avformat\_free\_context(encoder_fmt_ctx);

    avcodec\_free\_context(&decoder_ctx);
    avcodec\_free\_context(&encoder_ctx);

    return 0;
}

```

编译与运行：



```
g++ -o main main.cpp -lavformat -lavcodec -lavutil -lfmt -D\_\_STDC\_CONSTANT\_MACROS
./main test.mp4 rtmp://127.0.0.1:1935/live/test
# 还不太完善，可改进

```

![请添加图片描述](https://img-blog.csdnimg.cn/5ea93bb657184b9eb8515cc76047c16a.png)


以上。





