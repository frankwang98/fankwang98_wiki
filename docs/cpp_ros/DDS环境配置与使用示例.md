








#### 文章目录


* + [1. FastDDS介绍](#1_FastDDS_1)
	+ [2. 环境安装](#2__24)
	+ [3. 应用示例](#3__69)
	+ - [官方示例](#_70)
		- [创建发布和订阅示例](#_95)




### 1. FastDDS介绍


官方地址：`https://www.eprosima.com/index.php/company-all/news/146-fast-rtps-is-now-fast-dds`


API地址：`https://fast-dds.docs.eprosima.com/en/latest/`


`FastDDS`的前身是`Fast-RTPS`，实现了许多 DDS 规范。它是一种高性能的实时发布订阅框架。


`FastDDS`（Fast Data Distribution Service）是一种高性能、可扩展的数据分发服务，它实现了 `OMG DDS`（Object Management Group Data Distribution Service）标准。它是一个开源项目，旨在提供实时数据通信和消息传递的解决方案。


`FastDDS` 的主要特点和功能包括：



> 
> 1.高性能：Fast DDS 使用基于发布-订阅模式的数据分发机制，支持快速、可靠的数据交换。它的设计目标是提供低延迟和高吞吐量的数据传输，以满足实时性要求高的应用场景。
> 
> 
> 



> 
> 2.可扩展性：Fast DDS 具有良好的可扩展性，可以适应不同规模和复杂度的系统。它支持多种通信模式和拓扑结构，并提供灵活的配置选项，以满足各种应用需求。
> 
> 
> 



> 
> 3.安全性：Fast DDS 提供了可靠的数据传输和身份验证机制，以确保数据的机密性和完整性。它支持加密和访问控制，保护敏感数据不受未授权方访问。
> 
> 
> 



> 
> 4.多语言支持：Fast DDS 支持多种编程语言，包括 C++、Java、Python 等，使得开发人员可以在不同的编程环境中使用 Fast DDS 进行开发。
> 
> 
> 



> 
> 5.高度可定制：Fast DDS 提供了丰富的配置选项和可扩展的插件机制，使用户能够根据具体需求进行自定义扩展和功能增强。
> 
> 
> 


`FastDDS` 在实时数据通信领域具有广泛的应用，特别适用于分布式系统、实时控制和监控系统、机器人技术、物联网等领域。它为开发人员提供了一个可靠、高性能的数据分发平台，简化了实时数据交换的开发和集成过程。


### 2. 环境安装


FastDDS有`bin、source、docker image`三种安装方式。


这里采用`bin`安装，版本2.8.1。


下载地址：`https://www.eprosima.com/index.php/component/ars/repository/eprosima-fast-dds/eprosima-fast-dds-2-8-1`


![在这里插入图片描述](https://img-blog.csdnimg.cn/aff8eb872a134f119676f030a806f0d8.png)  
 安装包里，`install.sh`会自动安装各种依赖，然后进入src目录下，分别构建以下库：


* foonathan_memory_vendor，一个 STL 兼容的 C++ 内存分配器 库。
* fastcdr，一个根据 CDR 标准进行数据序列化的 C++ 库。
* fastrtps，eProsima Fast DDS库的核心库。
* fastddsgen，一个使用 IDL 文件中定义的数据类型生成源代码的 Java 应用程序。


![在这里插入图片描述](https://img-blog.csdnimg.cn/8e7a9c490dfe40c18ecaae3b7bf04644.png)


执行`install.h`需要`cmake 3.11`以上的版本，如果版本低的话需要先升级cmake：`http://t.csdn.cn/LezV9`



```
# 下载cmake
wget https://cmake.org/files/v3.22/cmake-3.22.1.tar.gz
sudo tar -xvzf cmake-3.22.1.tar.gz -C /usr/share
cd /usr/share/cmake-3.22.1
# 安装cmake
sudo chmod 777 ./configure
sudo ./configure
sudo make
sudo make install
sudo update-alternatives --install /usr/bin/cmake cmake /usr/local/bin/cmake 1 --force
cmake --version

```

安装fastdds：



```
sudo ./install.sh
# 安装了：git、build-essential、cmake、libssl-dev、libasio-dev、libtinyxml2-dev、openjdk-8-jre-headless、foonathan_memory_vendor、fastcdr、fastrtps(Fast DDS)、fastddsgen。
# 如果要测试FastDDS中的examples，需要在install.sh脚本脚本中打开该选项，默认为OFF。

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/3cf7816792a34a2e8b2cfba387ffaf85.png)


安装包也提供了`./uninstall.sh`脚本，可随时卸载。


参考：`https://www.jianshu.com/p/b9eb5dd9559f`


### 3. 应用示例


#### 官方示例



```
# 下载示例
git clone https://ghproxy.com/https://github.com/wanghuohuo0716/fastdds_helloworld.git
cd fastdds_helloworld
mkdir -p include/idl_generate/
cd idl/
fastddsgen -d ../include/idl_generate/  HelloWorld.idl	# -d选项指示生成的头文件保存目录
# 根据IDL文件生成接口文件后，同一个终端内接着编译FastDDS程序。
cd ..
mkdir build && cd build
cmake .. 
make

```

运行Publisher和Subscriber节点：



```
cd build/
./DDSHelloWorldPublisher
./DDSHelloWorldSubscriber

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/518375772e3d4151b701a16a614a45ca.png)


#### 创建发布和订阅示例



```
mkdir ddstest
touch HelloWorld.idl
touch HelloWorldPublisher.cpp
touch HelloWorldSubscriber.cpp
touch CMakeLists.txt

```

HelloWorld.idl



```
module HelloWorldModule {

struct HelloWorld
{
    unsigned long index;
    string message;
};

};

```

HelloWorldPublisher.cpp



```
/**
 * @file HelloWorldPublisher.cpp
 *
 */

#include "./build/HelloWorld.h"
#include "./build/HelloWorldPubSubTypes.h"

#include <fastdds/dds/domain/DomainParticipantFactory.hpp>
#include <fastdds/dds/domain/DomainParticipant.hpp>
#include <fastdds/dds/topic/TypeSupport.hpp>
#include <fastdds/dds/publisher/Publisher.hpp>
#include <fastdds/dds/publisher/DataWriter.hpp>
#include <fastdds/dds/publisher/DataWriterListener.hpp>

using namespace eprosima::fastdds::dds;

class HelloWorldPublisher
{
private:
    HelloWorld hello_;

    DomainParticipant *participant_;

    Publisher *publisher_;

    Topic *topic_;

    DataWriter *writer_;

    TypeSupport type_;

    class PubListener : public DataWriterListener
    {
    public:
        PubListener()
            : matched_(0)
        {
        }

        ~PubListener() override
        {
        }

        void on_publication_matched(
            DataWriter *,
            const PublicationMatchedStatus &info) override
        {
            if (info.current_count_change == 1)
            {
                matched_ = info.total_count;
                std::cout << "Publisher matched." << std::endl;
            }
            else if (info.current_count_change == -1)
            {
                matched_ = info.total_count;
                std::cout << "Publisher unmatched." << std::endl;
            }
            else
            {
                std::cout << info.current_count_change << " is not a valid value for PublicationMatchedStatus current count change." << std::endl;
            }
        }

        std::atomic_int matched_;

    } listener_;

public:
    HelloWorldPublisher()
        : participant_(nullptr), publisher_(nullptr), topic_(nullptr), writer_(nullptr), type_(new HelloWorldPubSubType())
    {
    }

    virtual ~HelloWorldPublisher()
    {
        if (writer_ != nullptr)
        {
            publisher_->delete_datawriter(writer_);
        }
        if (publisher_ != nullptr)
        {
            participant_->delete_publisher(publisher_);
        }
        if (topic_ != nullptr)
        {
            participant_->delete_topic(topic_);
        }
        DomainParticipantFactory::get_instance()->delete_participant(participant_);
    }

    // Initialize the publisher
    bool init()
    {
        hello_.index(0);
        hello_.message("HelloWorld, this is FastDDS."); // define message

        DomainParticipantQos participantQos;
        participantQos.name("Participant_publisher");
        participant_ = DomainParticipantFactory::get_instance()->create_participant(0, participantQos);

        if (participant_ == nullptr)
        {
            return false;
        }

        // Register the Type
        type_.register_type(participant_);

        // Create the publications Topic
        topic_ = participant_->create_topic("HelloWorldTopic", "HelloWorld", TOPIC_QOS_DEFAULT);

        if (topic_ == nullptr)
        {
            return false;
        }

        // Create the Publisher
        publisher_ = participant_->create_publisher(PUBLISHER_QOS_DEFAULT, nullptr);

        if (publisher_ == nullptr)
        {
            return false;
        }

        // Create the DataWriter
        writer_ = publisher_->create_datawriter(topic_, DATAWRITER_QOS_DEFAULT, &listener_);

        if (writer_ == nullptr)
        {
            return false;
        }
        return true;
    }

    // Send a publication
    bool publish()
    {
        if (listener_.matched_ > 0)
        {
            hello_.index(hello_.index() + 1);
            writer_->write(&hello_);
            return true;
        }
        return false;
    }

    // Run the Publisher
    void run(uint32_t samples)
    {
        uint32_t samples_sent = 0;
        while (samples_sent < samples)
        {
            if (publish())
            {
                samples_sent++;
                std::cout << "Message: " << hello_.message() << " with index: " << hello_.index() << " SENT" << std::endl;
            }
            std::this_thread::sleep_for(std::chrono::milliseconds(1000));
        }
    }
};

int main(int argc, char **argv)
{
    std::cout << "Starting publisher." << std::endl;
    int samples = 10; // pub count

    HelloWorldPublisher *mypub = new HelloWorldPublisher();
    if (mypub->init())
    {
        mypub->run(static_cast<uint32_t>(samples));
    }

    delete mypub;
    return 0;
}

```

HelloWorldSubscriber.cpp



```
/**
 * @file HelloWorldSubscriber.cpp
 *
 */
#include "./build/HelloWorld.h"
#include "./build/HelloWorldPubSubTypes.h"

#include <fastdds/dds/domain/DomainParticipantFactory.hpp>
#include <fastdds/dds/domain/DomainParticipant.hpp>
#include <fastdds/dds/topic/TypeSupport.hpp>
#include <fastdds/dds/subscriber/Subscriber.hpp>
#include <fastdds/dds/subscriber/DataReader.hpp>
#include <fastdds/dds/subscriber/DataReaderListener.hpp>
#include <fastdds/dds/subscriber/qos/DataReaderQos.hpp>
#include <fastdds/dds/subscriber/SampleInfo.hpp>

using namespace eprosima::fastdds::dds;

class HelloWorldSubscriber
{
private:
    DomainParticipant *participant_;

    Subscriber *subscriber_;

    DataReader *reader_;

    Topic *topic_;

    TypeSupport type_;

    class SubListener : public DataReaderListener
    {
    public:
        SubListener()
            : samples_(0)
        {
        }

        ~SubListener() override
        {
        }

        void on_subscription_matched(
            DataReader *,
            const SubscriptionMatchedStatus &info) override
        {
            if (info.current_count_change == 1)
            {
                std::cout << "Subscriber matched." << std::endl;
            }
            else if (info.current_count_change == -1)
            {
                std::cout << "Subscriber unmatched." << std::endl;
            }
            else
            {
                std::cout << info.current_count_change
                          << " is not a valid value for SubscriptionMatchedStatus current count change" << std::endl;
            }
        }

        void on_data_available(
            DataReader *reader) override
        {
            SampleInfo info;
            if (reader->take_next_sample(&hello_, &info) == ReturnCode_t::RETCODE_OK)
            {
                if (info.valid_data)
                {
                    samples_++;
                    std::cout << "Message: " << hello_.message() << " with index: " << hello_.index()
                              << " RECEIVED." << std::endl;
                }
            }
        }

        HelloWorld hello_;

        std::atomic_int samples_;

    } listener_;

public:
    HelloWorldSubscriber()
        : participant_(nullptr), subscriber_(nullptr), topic_(nullptr), reader_(nullptr), type_(new HelloWorldPubSubType())
    {
    }

    virtual ~HelloWorldSubscriber()
    {
        if (reader_ != nullptr)
        {
            subscriber_->delete_datareader(reader_);
        }
        if (topic_ != nullptr)
        {
            participant_->delete_topic(topic_);
        }
        if (subscriber_ != nullptr)
        {
            participant_->delete_subscriber(subscriber_);
        }
        DomainParticipantFactory::get_instance()->delete_participant(participant_);
    }

    // Initialize the subscriber
    bool init()
    {
        DomainParticipantQos participantQos;
        participantQos.name("Participant_subscriber");
        participant_ = DomainParticipantFactory::get_instance()->create_participant(0, participantQos);

        if (participant_ == nullptr)
        {
            return false;
        }

        // Register the Type
        type_.register_type(participant_);

        // Create the subscriptions Topic
        topic_ = participant_->create_topic("HelloWorldTopic", "HelloWorld", TOPIC_QOS_DEFAULT);

        if (topic_ == nullptr)
        {
            return false;
        }

        // Create the Subscriber
        subscriber_ = participant_->create_subscriber(SUBSCRIBER_QOS_DEFAULT, nullptr);

        if (subscriber_ == nullptr)
        {
            return false;
        }

        // Create the DataReader
        reader_ = subscriber_->create_datareader(topic_, DATAREADER_QOS_DEFAULT, &listener_);

        if (reader_ == nullptr)
        {
            return false;
        }

        return true;
    }

    // Run the Subscriber
    void run(uint32_t samples)
    {
        while (listener_.samples_ < samples)
        {
            std::this_thread::sleep_for(std::chrono::milliseconds(100));
        }
    }
};

int main(int argc, char **argv)
{
    std::cout << "Starting subscriber." << std::endl;
    int samples = 10; // sub count

    HelloWorldSubscriber *mysub = new HelloWorldSubscriber();
    if (mysub->init())
    {
        /* instant sub */
        while (1)
        {
            mysub->run(static_cast<uint32_t>(samples));
        }
    }

    delete mysub;
    return 0;
}

```

CMakeLists.txt



```
cmake_minimum_required(VERSION 3.5)
project(HelloWorldExample)

set(CMAKE_CXX_STANDARD 11)

find_package(fastcdr REQUIRED)
find_package(fastrtps REQUIRED)

# generate idl_gen
file(GLOB IDL_SOURCES "${CMAKE_CURRENT_SOURCE_DIR}/*.idl")

foreach(IDL_FILE ${IDL_SOURCES})
    get_filename_component(IDL_BASE_NAME ${IDL_FILE} NAME_WE)
    set(GENERATED_SOURCES "${CMAKE_CURRENT_BINARY_DIR}/${IDL_BASE_NAME}.cxx" "${CMAKE_CURRENT_BINARY_DIR}/${IDL_BASE_NAME}.h")
    add_custom_command(
        OUTPUT ${GENERATED_SOURCES}
        COMMAND fastddsgen -d ./ ${IDL_FILE}
        DEPENDS ${IDL_FILE}
        WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
        COMMENT "Generating C++ files from ${IDL_FILE}"
    )
    list(APPEND GENERATED_CPP_SOURCES ${GENERATED_SOURCES})
endforeach()

include_directories(${CMAKE_CURRENT_BINARY_DIR})

# generate lib
file(GLOB DDS_HELLOWORLD_SOURCES_CXX "./build/*.cxx")
add_library(HelloWorld_IDL_lib ${DDS_HELLOWORLD_SOURCES_CXX})

add_executable(HelloWorldPublisher HelloWorldPublisher.cpp ${GENERATED_CPP_SOURCES})
target_link_libraries(HelloWorldPublisher HelloWorld_IDL_lib fastcdr fastrtps)

add_executable(HelloWorldSubscriber HelloWorldSubscriber.cpp ${GENERATED_CPP_SOURCES})
target_link_libraries(HelloWorldSubscriber HelloWorld_IDL_lib fastcdr fastrtps)

```


```
# 编译运行
mkdir build && cd build
cmake ..
make
./HelloWorldPublisher
./HelloWorldSubscriber

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/7e2ecf43282140fe834d8462ae44d076.png#pic_center)


以上。



### 😏1. opendds项目介绍


项目Github地址：`https://github.com/OpenDDS/OpenDDS`


官网：`https://opendds.org/`


`OpenDDS`（`Open Data Distribution Service`）是一个开源的、高性能的实时数据分发和通信框架，符合`OMG`（`Object Management Group`）发布的`Data Distribution Service（DDS）`标准。它提供了分布式系统中实时通信和数据交换的基础设施，支持发布者-订阅者模型，使分布式应用程序能够可靠地交换数据。


以下是OpenDDS的一些主要特点和功能：



> 
> 1.数据分发：OpenDDS提供了可靠的数据分发机制，可以在分布式系统中高效地传输数据。它支持灵活的QoS（Quality of Service）策略，可以根据应用程序的需求配置数据交换的可靠性、传输速率、延迟、带宽等参数。
> 
> 
> 



> 
> 2.发布者-订阅者模型：OpenDDS基于发布者-订阅者模型，发布者将数据发布到特定的主题（Topic），而订阅者通过订阅相应的主题来接收数据。这种模型使得多个应用程序能够以异步、解耦的方式进行实时数据交换。
> 
> 
> 



> 
> 3.多种数据类型支持：OpenDDS支持多种数据类型的交换，包括结构体、数组、枚举和序列等。它使用IDL（Interface Definition Language）来定义数据类型，并自动生成相应的代码和类型支持。
> 
> 
> 



> 
> 4.可扩展性：OpenDDS具有良好的可扩展性，可以处理大规模分布式系统中的复杂通信需求。它支持动态发现和自适应性，可以自动发现和适应系统中的节点和资源变化。
> 
> 
> 



> 
> 5.平台支持：OpenDDS可在多个平台上运行，包括Linux、Windows和macOS等。它提供了对不同操作系统和网络协议的支持，并且可以与其他编程语言（如C++、Java和Python）进行集成。
> 
> 
> 



> 
> 6.社区支持：OpenDDS是一个活跃的开源项目，拥有一个积极的社区，提供了广泛的文档、示例代码和讨论论坛，以帮助开发人员学习和使用OpenDDS。
> 
> 
> 


`OpenDDS`是一个功能强大的实时数据分发和通信框架，适用于构建要求高性能、可靠性和实时性的分布式应用程序。它提供了丰富的功能和配置选项，可以根据应用程序的需求进行灵活的配置和定制。


### 😊2. 环境配置


下面进行环境配置：



```
sudo apt-get install build-essential libace-dev libssl-dev
# 下载对应版本
https://opendds.org/downloads.html
# configure会下载ACE+TAO网络包，如果访问github慢，可以在configure的878和886行添加镜像源https://mirror.ghproxy.com/
./configure
# 编译
make

```

### 😆3. 使用说明


官方示例：



```
source setenv.sh
cd DevGuideExamples/DCPS/Messenger
./run_test.pl

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6cbcd6c17cec4dba9bb3c0f895f02fa2.png)


以上。







