## iceoryx介绍
Iceoryx是一种高性能、实时通信中间件，专门设计用于处理大规模、实时数据交换的场景。Iceoryx旨在提供低延迟、高吞吐量和可靠性，适用于各种实时系统和嵌入式应用。

官网：`https://iceoryx.io/v1.0.1/`

Github地址：`https://github.com/eclipse-iceoryx/iceoryx`

以下是Iceoryx的一些关键特点和优势：

>1.低延迟和高吞吐量：Iceoryx采用零拷贝和共享内存的方式进行数据传输，减少了数据复制的开销，从而实现低延迟和高吞吐量。

>2.实时性能：Iceoryx专注于实时通信需求，具有可预测的性能表现，适用于需要严格实时性能的系统。

>3.轻量级设计：Iceoryx采用模块化的设计，只包含必要的功能模块，使其更加轻量级并且易于集成到现有系统中。

>4.支持多种通信模式：Iceoryx支持发布-订阅、请求-响应等多种通信模式，可以灵活地满足不同应用场景的通信需求。

>5.跨平台兼容：Iceoryx支持多种操作系统和架构，可以在不同平台上运行，并提供一致的接口和性能。

Iceoryx广泛应用于自动驾驶、机器人技术、工业自动化等领域，为实时数据交换提供了可靠的解决方案。通过使用Iceoryx，开发人员可以更轻松地构建高性能、实时的通信系统，满足对实时性能有严格要求的应用需求。

## 环境配置
参考：`https://iceoryx.io/v1.0.1/getting-started/installation/`

Ubuntu下环境配置：
```sh
sudo apt install gcc g++ cmake libacl1-dev libncurses5-dev pkg-config
git clone https://github.com/eclipse-iceoryx/iceoryx.git
cd iceoryx
cmake -Bbuild -Hiceoryx_meta
cmake -Bbuild -Hiceoryx_meta -DCMAKE_PREFIX_PATH=$(PWD)/build/dependencies/
cmake --build build
sudo cmake --build build --target install
```

## 基础示例

iox-roudi是Iceoryx中的一个重要组件，负责管理Iceoryx运行时的核心功能。RouDi 是 Runtime Discovery（运行时发现）和 Routing（路由）的缩写，它提供了一种轻量级的进程间通信机制，用于实现不同模块之间的通信和数据交换。

需要首先启动：`iox-roudi`

发布订阅示例：
```cpp
// pub.cpp
#include "topic_data.hpp"
#include "iox/signal_watcher.hpp"
#include "iceoryx_posh/popo/publisher.hpp"
#include "iceoryx_posh/runtime/posh_runtime.hpp"
#include <iostream>

int main()
{
    // initialize runtime
    constexpr char APP_NAME[] = "iox-cpp-publisher-helloworld";
    iox::runtime::PoshRuntime::initRuntime(APP_NAME);

    // create publisher
    iox::popo::Publisher<RadarObject> publisher({"Radar", "FrontLeft", "Object"});

    double ct = 0.0;
    // wait for term
    while (!iox::hasTerminationRequested())
    {
        ++ct;

        // Retrieve a sample from shared memory
        auto loanResult = publisher.loan();
        // publish
        if (loanResult.has_value())
        {
            auto& sample = loanResult.value();
            // Sample can be held until ready to publish
            sample->x = ct;
            sample->y = ct;
            sample->z = ct;
            sample.publish();
        }
        else
        {
            auto error = loanResult.error();
            // Do something with error
            std::cerr << "Unable to loan sample, error code: " << error << std::endl;
        }

        // msg
        std::cout << APP_NAME << " sent value: " << ct << std::endl;
        std::this_thread::sleep_for(std::chrono::seconds(1));
    }

    return 0;
}
```

```cpp
// sub.cpp
#include "topic_data.hpp"
#include "iceoryx_posh/popo/subscriber.hpp"
#include "iceoryx_posh/runtime/posh_runtime.hpp"
#include "iox/signal_watcher.hpp"
#include <iostream>

int main()
{
    // initialize runtime
    constexpr char APP_NAME[] = "iox-cpp-subscriber-helloworld";
    iox::runtime::PoshRuntime::initRuntime(APP_NAME);

    // initialize subscriber
    iox::popo::Subscriber<RadarObject> subscriber({"Radar", "FrontLeft", "Object"});

    // run until interrupted by Ctrl-C
    while (!iox::hasTerminationRequested())
    {
        // receive
        auto takeResult = subscriber.take();
        if (takeResult.has_value())
        {
            std::cout << APP_NAME << " got value: " << takeResult.value()->x << std::endl;
        }
        else
        {
            // error
            if (takeResult.error() == iox::popo::ChunkReceiveResult::NO_CHUNK_AVAILABLE)
            {
                std::cout << "No chunk available." << std::endl;
            }
            else
            {
                std::cout << "Error receiving chunk." << std::endl;
            }
        }

        // wait
        std::this_thread::sleep_for(std::chrono::milliseconds(100));
    }

    return (EXIT_SUCCESS);
}
```

```sh
# CMakeLists.txt
cmake_minimum_required(VERSION 3.5)
project(IceoryxExample)

set(CMAKE_CXX_STANDARD 11)

# 查找Iceoryx的包
include(GNUInstallDirs)
find_package(iceoryx_platform REQUIRED)
find_package(iceoryx_posh CONFIG REQUIRED)
find_package(iceoryx_hoofs CONFIG REQUIRED)

include(IceoryxPackageHelper)
include(IceoryxPlatform)
include(IceoryxPlatformSettings)

iox_add_executable(
    TARGET  pub_helloworld
    FILES   ./pub.cpp
    LIBS    iceoryx_posh::iceoryx_posh
)

iox_add_executable(
    TARGET  sub_helloworld
    FILES   ./sub.cpp
    LIBS    iceoryx_posh::iceoryx_posh
)
```