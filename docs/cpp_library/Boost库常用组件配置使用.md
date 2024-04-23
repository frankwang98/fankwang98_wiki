### 😏1. Boost库介绍

项目Github地址：`https://github.com/boostorg/boost`

Boost库在线书籍：`https://wizardforcel.gitbooks.io/the-boost-cpp-libraries/content/0.html`

Boost是一个流行的、开源的C++库集合，提供了各种功能强大的库和工具，扩展了C++语言的能力，并为开发者提供了更高级别的抽象和工具。Boost库经过广泛的使用和测试，被认为是C++社区的事实标准之一。

Boost库包含了多个模块，每个模块都提供了不同领域的功能和工具，覆盖了诸如字符串操作、数据结构、算法、日期时间处理、文件系统、线程、网络、正则表达式等各个方面。以下是一些常用的Boost库：

> 1.Boost.Asio：提供了异步I/O操作的网络编程库，支持TCP、UDP、串口等网络协议。

> 2.Boost.Smart_Ptr：提供了智能指针类，如shared_ptr和weak_ptr，用于方便地进行内存管理。

> 3.Boost.Filesystem：提供了对文件系统的访问和操作，包括文件和目录的创建、删除、遍历等。

> 4.Boost.Regex：提供了正则表达式的功能，用于进行文本匹配和搜索操作。

> 5.Boost.Thread：提供了跨平台的多线程编程接口，简化了线程的创建、同步和通信等操作。

> 6.Boost.Serialization：提供了对象的序列化和反序列化功能，可以将对象以二进制或XML格式进行存储和传输。

除了以上列举的库之外，Boost还包含了许多其他功能丰富的库，如Boost.Math用于数学计算、Boost.Graph用于图论算法、Boost.Test用于单元测试等。Boost库通常以头文件方式提供，使用Boost只需包含相应的头文件，并链接对应的库文件。

Boost库的目标是提供高质量和高可移植性的C++代码，因此它的代码质量很高，并且支持各种主流操作系统和编译器。Boost库的开发是一个开放的社区驱动过程，接受用户的反馈和贡献，并定期发布新版本。

### 😊2. 环境配置

下面进行环境配置：

```sh
# apt安装常用模块
sudo apt-get install libboost-dev

# Boost.Geometry只在boost1.75以上支持
wget https://dl.bintray.com/boostorg/release/1.76.0/source/boost_1_76_0.tar.gz
tar -xzvf boost_1_76_0.tar.gz
cd boost_1_76_0
./bootstrap.sh --prefix=/usr/local
sudo ./b2 install
sudo apt install libboost-all-dev
# 验证高版本安装
ls /usr/local/include/boost/geometry/
```

### Boost.Thread使用示例

创建线程示例：

```cpp
#include <iostream>
#include <boost/thread.hpp>

// 线程函数
void threadFunction()
{
    // 输出线程相关信息
    std::cout << "Thread ID: " << boost::this_thread::get_id() << std::endl;
    std::cout << "Hello from a thread!" << std::endl;
}

int main()
{
    // 创建线程并启动
    boost::thread threadObj(threadFunction);
    // 多个线程类似

    // 等待线程结束
    threadObj.join();

    // 输出主线程相关信息
    std::cout << "Thread ID: " << boost::this_thread::get_id() << std::endl;
    std::cout << "Main thread exiting..." << std::endl;

    return 0;
}

```

编译运行：

```sh
g++ -o main main.cpp -lboost_thread -lpthread
./main
Thread ID: 7f65d8552700
Hello from a thread!
Thread ID: 7f65d8553740
Main thread exiting...

```

### Boost.Serialization使用示例

```cpp
#include <iostream>
#include <fstream>
#include <boost/archive/text_oarchive.hpp>
#include <boost/archive/text_iarchive.hpp>

// 要进行序列化和反序列化的示例类
class MyClass
{
public:
    int data;
    double d;
    std::string str;

    // 声明 Boost 序列化函数为友元函数
    friend class boost::serialization::access;

    // Boost 序列化函数（将对象转换为字节流）
    template<class Archive>
    void serialize(Archive & ar, const unsigned int version)
    {
        ar & data;
        ar & d;
        ar & str;
    }
};

int main()
{
    // 创建一个 MyClass 对象并设置数据
    MyClass obj;
    obj.data = 42;
    obj.d = 1.005;
    obj.str = "hello";

    // 将对象序列化到文件
    std::ofstream outputFile("data.txt");
    boost::archive::text_oarchive outputArchive(outputFile);
    outputArchive << obj;
    outputFile.close();

    // 从文件中反序列化对象
    std::ifstream inputFile("data.txt");
    boost::archive::text_iarchive inputArchive(inputFile);
    MyClass restoredObj;
    inputArchive >> restoredObj;
    inputFile.close();

    // 输出反序列化后的对象数据
    std::cout << "Restored data: " << restoredObj.data << std::endl;
    std::cout << "Restored d: " << restoredObj.d << std::endl;
    std::cout << "Restored str: " << restoredObj.str << std::endl;

    return 0;
}

```

编译运行：

```sh
g++ -o main main.cpp -lboost_serialization && ./main
Restored data: 42
Restored d: 1.005
Restored str: hello

```

### Boost.Math使用示例

```cpp
#include <iostream>
#include <boost/math/constants/constants.hpp>
#include <boost/math/special_functions/bessel.hpp>

int main()
{
    // 计算圆周率
    double pi = boost::math::constants::pi<double>();
    std::cout << "Pi: " << pi << std::endl;

    // 贝塞尔函数
    double besselJ0 = boost::math::cyl_bessel_j(0, 2.0);
    std::cout << "Bessel J0(2.0): " << besselJ0 << std::endl;

    return 0;
}

```

编译运行：

```sh
g++ -o main main.cpp -lboost_math_c99 -lboost_math_c99f && ./main
Pi: 3.14159
Bessel J0(2.0): 0.223891

```

### Boost.Time使用示例

```cpp
#include <iostream>
#include <boost/date_time/posix_time/posix_time.hpp>

long GetTime();

int main()
{
    // 获取当前系统时间
    boost::posix_time::ptime now = boost::posix_time::second_clock::local_time();
    std::cout << "Current system time: " << now << std::endl;
   
    // 格式化输出当前系统时间
    std::string formattedTime = boost::posix_time::to_simple_string(now);
    std::cout << "Formatted current system time: " << formattedTime << std::endl;
   
    // 日期增减
    boost::posix_time::ptime tomorrow = now + boost::gregorian::days(1);
    std::cout << "Tomorrow: " << tomorrow << std::endl;
   
    // 时间增减
    boost::posix_time::ptime nextHour = now + boost::posix_time::hours(1);
    std::cout << "Next hour: " << nextHour << std::endl;

    // 时间差计算
    boost::posix_time::time_duration diff = nextHour - now;
    std::cout << "Difference between now and next hour: " << diff.total_seconds() << " seconds" << std::endl;

    // 获取当前系统时间，精确到毫秒
    boost::posix_time::ptime now_ms = boost::posix_time::microsec_clock::local_time();
    // 将时间转换为毫秒
    boost::posix_time::time_duration duration = now_ms.time_of_day();
    long milliseconds = duration.total_milliseconds();
    // 输出毫秒级时间
    std::cout << "Current system milliseconds: " << milliseconds << std::endl;

    long t1 = GetTime();
    sleep(1);
    long t2 = GetTime();
    // 输出时间差
    std::cout << "This program cost: " << t2 - t1 << std::endl;

    return 0;
}

long GetTime()
{
    boost::posix_time::ptime now_ms = boost::posix_time::microsec_clock::local_time();
    boost::posix_time::time_duration duration = now_ms.time_of_day();
    long milliseconds = duration.total_milliseconds();
    return milliseconds;
}

```

编译运行：

```sh
g++ -o main main.cpp -lboost_date_time && ./main
28 16:52:31
Tomorrow: 2023-Jul-29 16:52:31
Next hour: 2023-Jul-28 17:52:31
Difference between now and next hour: 3600 seconds
Current system milliseconds: 60751420
This program cost: 1000

```

### Boost.Geometry使用示例

```cpp
// 计算两点间距离 -lboost_system -lboost_geometry
#include <iostream>
#include <boost/geometry.hpp>
#include <boost/geometry/geometries/register/point.hpp>

namespace bg = boost::geometry;

// 定义一个 Point 结构体，并注册为 Boost.Geometry 的点类型
struct Point
{
    double x, y;
};

BOOST_GEOMETRY_REGISTER_POINT_2D(Point, double, bg::cs::cartesian, x, y)

int main()
{
    // 创建两个点
    Point p1{0.0, 0.0};
    Point p2{1.0, 1.0};

    // 计算两个点之间的欧几里得距离
    double distance = bg::distance(p1, p2);

    std::cout << "Distance between points: " << distance << std::endl;

    return 0;
}

```


```cpp
// 点集转线
#include <iostream>
#include <vector>
#include <boost/geometry.hpp>
#include <boost/geometry/geometries/point_xy.hpp>
#include <boost/geometry/geometries/linestring.hpp>

namespace bg = boost::geometry;

typedef bg::model::d2::point_xy<double> Point;
typedef bg::model::linestring<Point> LineString;

int main()
{
    // 创建点集
    std::vector<Point> points;
    points.push_back(Point(0, 0));
    points.push_back(Point(1, 1));
    points.push_back(Point(2, 2));
    points.push_back(Point(3, 3));

    // 将点集转换为线
    LineString line;
    bg::assign_points(line, points);

    // 输出线的坐标
    std::cout << "Line coordinates: ";
    for (auto it = boost::begin(line); it != boost::end(line); ++it)
    {
        std::cout << bg::get<0>(*it) << " " << bg::get<1>(*it) << ", ";
    }
    std::cout << std::endl;

    return 0;
}

```


```cpp
// 面要素转线要素
#include <iostream>
#include <vector>
#include <boost/geometry.hpp>
#include <boost/geometry/geometries/polygon.hpp>
#include <boost/geometry/geometries/linestring.hpp>

namespace bg = boost::geometry;

typedef bg::model::polygon<bg::model::d2::point_xy<double>> Polygon;
typedef bg::model::linestring<bg::model::d2::point_xy<double>> LineString;

void polygonToLineString(const Polygon& polygon, LineString& lineString)
{
    const auto& outerRing = bg::exterior_ring(polygon);
    bg::append(lineString, outerRing);

    for (const auto& innerRing : bg::interior_rings(polygon))
    {
        bg::append(lineString, innerRing);
    }
}

int main()
{
    // 创建一个多边形
    Polygon polygon;
    bg::read_wkt("POLYGON((0 0,0 10,10 10,10 0,0 0),(2 2,2 8,8 8,8 2,2 2))", polygon);

    // 将多边形转换为线
    LineString lineString;
    polygonToLineString(polygon, lineString);

    // 输出线上的点
    std::cout << "Line points: ";
    for (const auto& point : lineString)
    {
        std::cout << "(" << bg::get<0>(point) << " " << bg::get<1>(point) << "), ";
    }
    std::cout << std::endl;

    return 0;
}

```


```cpp
// 线要素转点要素
#include <iostream>
#include <vector>
#include <boost/geometry.hpp>
#include <boost/geometry/geometries/point.hpp>
#include <boost/geometry/geometries/linestring.hpp>

namespace bg = boost::geometry;

typedef bg::model::point<double, 2, bg::cs::cartesian> Point;
typedef bg::model::linestring<Point> LineString;

void lineStringToPoints(const LineString& lineString, std::vector<Point>& points)
{
    for (const auto& point : lineString)
    {
        points.push_back(point);
    }
}

int main()
{
    // 创建一个线要素
    LineString lineString;
    lineString.push_back(Point(0, 0));
    lineString.push_back(Point(1, 1));
    lineString.push_back(Point(2, 2));

    // 将线要素转换为点
    std::vector<Point> points;
    lineStringToPoints(lineString, points);

    // 输出点的坐标
    std::cout << "Point coordinates: ";
    for (const auto& point : points)
    {
        std::cout << "(" << bg::get<0>(point) << " " << bg::get<1>(point) << "), ";
    }
    std::cout << std::endl;

    return 0;
}

```

### Boost.PropertyTree

INI配置文件解析示例：

```cpp
#include <iostream>
#include <boost/property_tree/ptree.hpp>
#include <boost/property_tree/ini_parser.hpp>

using namespace std;

/*
[Section1]
Username = john
Password = secret

[Section2]
Port = 8080
*/

int main() {
    // 创建一个property_tree对象
    boost::property_tree::ptree pt;
    // 使用ini_parser库加载INI文件
    boost::property_tree::ini_parser::read_ini("./data/data.ini", pt);

    // 读取配置项的值
    std::string username = pt.get<std::string>("Section1.Username");
    std::string password = pt.get<std::string>("Section1.Password");
    int port = pt.get<int>("Section2.Port");

    // 打印读取的值
    std::cout << "Username: " << username << std::endl;
    std::cout << "Password: " << password << std::endl;
    std::cout << "Port: " << port << std::endl;

    return 0;
}

```

XML配置文件解析示例：

```cpp
#include <iostream>
#include <boost/property_tree/ptree.hpp>
#include <boost/property_tree/xml_parser.hpp>

/*
<root>
 <title>Sample Book</title>
 <year>2023</year>
 <author>John Doe</author>
</root>
*/

int main() {
    // 创建一个property_tree对象
    boost::property_tree::ptree pt;

    try {
        // 使用xml_parser库加载XML文件
        boost::property_tree::read_xml("./data/data.xml", pt);

        // 读取XML节点的值
        std::string title = pt.get<std::string>("root.title");
        int year = pt.get<int>("root.year");
        std::string author = pt.get<std::string>("root.author");

        // 打印读取的值
        std::cout << "Title: " << title << std::endl;
        std::cout << "Year: " << year << std::endl;
        std::cout << "Author: " << author << std::endl;
    } catch (const boost::property_tree::ptree_error& e) {
        // 处理解析错误
        std::cerr << "XML parsing error: " << e.what() << std::endl;
    }

    return 0;
}

```

JSON配置文件解析示例：

```cpp
#include <iostream>
#include <boost/property_tree/ptree.hpp>
#include <boost/property_tree/json_parser.hpp>

/*
{
 "name": "John Doe",
 "age": 30,
 "address": {
 "city": "New York",
 "street": "123 Main St"
 }
}
*/

int main() {
    // 创建一个property_tree对象
    boost::property_tree::ptree pt;

    try {
        // 使用json_parser库加载JSON文件
        boost::property_tree::read_json("./data/data.json", pt);

        // 读取JSON节点的值
        std::string name = pt.get<std::string>("name");
        int age = pt.get<int>("age");
        std::string city = pt.get<std::string>("address.city");
        std::string street = pt.get<std::string>("address.street");

        // 打印读取的值
        std::cout << "Name: " << name << std::endl;
        std::cout << "Age: " << age << std::endl;
        std::cout << "City: " << city << std::endl;
        std::cout << "Street: " << street << std::endl;
    } catch (const boost::property_tree::ptree_error& e) {
        // 处理解析错误
        std::cerr << "JSON parsing error: " << e.what() << std::endl;
    }

    return 0;
}

```

### Boost.InterProcess

共享内存读写示例

```cpp
#include <boost/interprocess/shared_memory_object.hpp>
#include <boost/interprocess/mapped_region.hpp>
#include <iostream>

using namespace boost::interprocess;

int main()
{
    // 创建或打开共享内存对象
    shared_memory_object shm(open_or_create, "my_shared_memory", read_write);

    // 设置共享内存对象的大小
    shm.truncate(1024);

    // 映射共享内存到当前进程的地址空间
    mapped_region region(shm, read_write);

    // 获取共享内存的首地址
    void* addr = region.get_address();

    // 写入数据到共享内存
    const char* str = "Hello, Boost.Interprocess!";
    std::strcpy(static_cast<char*>(addr), str);

    // 从共享内存读取数据
    char buffer[1024];
    std::strcpy(buffer, static_cast<char*>(addr));

    // 输出读取到的数据
    std::cout << "Message from shared memory: " << buffer << std::endl;

    // 删除共享内存对象
    shared_memory_object::remove("my_shared_memory");

    return 0;
}

```

### Boost.Asio

http服务端示例：

```cpp
#include <boost/beast.hpp>
#include <boost/asio.hpp>
#include <iostream>

namespace beast = boost::beast;
namespace http = beast::http;
namespace net = boost::asio;
using tcp = net::ip::tcp;

void handle_request(http::request<http::string_body>& request, http::response<http::string_body>& response) {
    // 处理请求并生成响应
    response.version(request.version());
    response.result(http::status::ok);
    response.set(http::field::server, "Boost Beast HTTP Server");
    response.body() = "Hello, World!";
    response.prepare_payload();
}

int main() {
    try {
        // 创建 IO 上下文和解析器
        net::io_context io_context;

        // 创建监听端口
        tcp::acceptor acceptor(io_context, tcp::endpoint(tcp::v4(), 8881));

        while (true) {
            // 接受连接
            tcp::socket socket(io_context);
            acceptor.accept(socket);

            // 读取请求
            beast::flat_buffer buffer;
            http::request<http::string_body> request;
            http::read(socket, buffer, request);

            // 处理请求并生成响应
            http::response<http::string_body> response;
            handle_request(request, response);

            // 发送响应
            http::write(socket, response);
        }
    } catch (const std::exception& e) {
        std::cerr << "Error: " << e.what() << std::endl;
        return 1;
    }

    return 0;
}

```

http客户端示例：

```cpp
#include <boost/beast.hpp>
#include <boost/asio.hpp>
#include <iostream>

namespace beast = boost::beast;
namespace http = beast::http;
namespace net = boost::asio;
using tcp = net::ip::tcp;

int main() {
    try {
        // 创建 IO 上下文和解析器
        net::io_context io_context;
        tcp::resolver resolver(io_context);
        beast::tcp_stream stream(io_context);

        // 解析主机和端口
        auto const results = resolver.resolve("localhost", "8881");

        // 连接到服务器
        stream.connect(results);

        // 创建 HTTP 请求
        http::request<http::string_body> request(http::verb::get, "/", 11);
        request.set(http::field::host, "localhost");
        request.set(http::field::user_agent, "Boost Beast");

        // 发送请求
        http::write(stream, request);

        // 接收并打印响应
        beast::flat_buffer buffer;
        http::response<http::string_body> response;
        http::read(stream, buffer, response);

        std::cout << "Response code: " << response.result_int() << std::endl;
        std::cout << "Response body: " << response.body() << std::endl;
    } catch (const std::exception& e) {
        std::cerr << "Error: " << e.what() << std::endl;
        return 1;
    }

    return 0;
}

```

编译运行：

```
g++ -o server server.cpp -lboost_system -lboost_thread -lpthread
g++ -o client client.cpp -lboost_system -lboost_thread -lpthread

```

TCP服务端示例：

```cpp
#include <iostream>
#include <boost/asio.hpp>

using boost::asio::ip::tcp;

int main()
{
    try
    {
        boost::asio::io_context io_context;

        // 创建一个 TCP acceptor，监听指定端口
        tcp::acceptor acceptor(io_context, tcp::endpoint(tcp::v4(), 8081));

        // 等待并接受连接
        tcp::socket socket(io_context);
        acceptor.accept(socket);

        // 接收客户端的消息
        char response[1024];
        size_t bytesRead = socket.read_some(boost::asio::buffer(response));

        std::cout << "Response from client: ";
        std::cout.write(response, bytesRead);

        // 处理连接请求
        std::string message = "Hello, Boost.Asio!";
        boost::system::error_code ignored_error;
        boost::asio::write(socket, boost::asio::buffer(message), ignored_error);
    }
    catch (std::exception& e)
    {
        std::cerr << "Exception: " << e.what() << std::endl;
    }

    return 0;
}

```

编译运行：

```
g++ -o server server.cpp -lboost_system -lpthread
./server

```

TCP客户端示例：

```cpp
#include <iostream>
#include <boost/asio.hpp>

using boost::asio::ip::tcp;

int main() {
  try {
    // 创建IO上下文对象
    boost::asio::io_context io_context;

    // 创建socket对象
    tcp::socket socket(io_context);

    // 解析服务器地址和端口
    tcp::resolver resolver(io_context);
    tcp::resolver::results_type endpoints = resolver.resolve("127.0.0.1", "8081");

    // 连接到服务器
    boost::asio::connect(socket, endpoints);

    // 发送数据给服务器
    std::string request = "Hello, server!";
    boost::asio::write(socket, boost::asio::buffer(request));

    // 接收服务器的响应
    char response[1024];
    size_t bytesRead = socket.read_some(boost::asio::buffer(response));

    // 显示服务器的响应
    std::cout << "Response from server: ";
    std::cout.write(response, bytesRead);
    std::cout << std::endl;

  } catch (std::exception& e) {
    std::cerr << "Exception: " << e.what() << std::endl;
  }

  return 0;
}

```

编译运行：

```
g++ -o client client.cpp -lboost_system -lpthread
./client

```

UDP服务端示例：

```cpp
#include <iostream>
#include <boost/asio.hpp>

using boost::asio::ip::udp;

int main()
{
    boost::asio::io_context io_context;

    // 创建UDP端点并绑定到特定端口
    udp::socket socket(io_context, udp::endpoint(udp::v4(), 8888));

    // 接收缓冲区
    char recv_buffer[1024];

    while (true) {
        udp::endpoint remote_endpoint;

        // 接收数据
        size_t bytes_received = socket.receive_from(boost::asio::buffer(recv_buffer), remote_endpoint);

        // 打印接收到的数据
        std::cout.write(recv_buffer, bytes_received);
        std::cout << std::endl;

        // 发送回复
        std::string message = "Hello from server!";
        socket.send_to(boost::asio::buffer(message), remote_endpoint);
    }

    return 0;
}

```

UDP客户端示例：

```cpp
#include <iostream>
#include <boost/asio.hpp>

using boost::asio::ip::udp;

int main()
{
    boost::asio::io_context io_context;

    // 创建UDP端点并绑定到任意端口
    udp::socket socket(io_context, udp::endpoint(udp::v4(), 0));

    // 远程服务器端点
    udp::endpoint remote_endpoint(boost::asio::ip::address::from_string("127.0.0.1"), 8888);

    // 发送数据
    std::string message = "Hello from client!";
    socket.send_to(boost::asio::buffer(message), remote_endpoint);

    // 接收缓冲区
    char recv_buffer[1024];

    // 接收回复
    udp::endpoint sender_endpoint;
    size_t bytes_received = socket.receive_from(boost::asio::buffer(recv_buffer), sender_endpoint);

    // 打印回复数据
    std::cout.write(recv_buffer, bytes_received);
    std::cout << std::endl;

    return 0;
}

```

websocket服务端示例：

```cpp
#include <iostream>
#include <string>
#include <boost/asio.hpp>
#include <boost/beast.hpp>
#include <thread>

namespace asio = boost::asio;
namespace beast = boost::beast;
using tcp = asio::ip::tcp;

void SendMessages(beast::websocket::stream<tcp::socket>& ws) {
    try {
        // Send a message every 1 seconds
        while (true) {
            std::this_thread::sleep_for(std::chrono::seconds(1));
            ws.write(asio::buffer("Server: Sending a message from the server!"));
        }
    } catch (const std::exception &e) {
        std::cerr << "Exception in SendMessages: " << e.what() << std::endl;
    }
}

int main() {
    try {
        asio::io_context io_context;

        // Create and bind the acceptor
        tcp::acceptor acceptor(io_context, {tcp::v4(), 8881});

        while (true) {
            // Accept connection
            tcp::socket socket(io_context);
            acceptor.accept(socket);

            // WebSocket upgrade
            beast::websocket::stream<tcp::socket> ws(std::move(socket));
            ws.accept();

            // Start a thread to send messages to the client
            std::thread senderThread(SendMessages, std::ref(ws));

            // Read WebSocket message
            beast::flat_buffer buffer;
            ws.read(buffer);

            // Print received message
            std::cout << "Received: " << beast::buffers_to_string(buffer.data()) << std::endl;

            // Echo the message back to the client
            ws.text(ws.got_text());
            ws.write(buffer.data());

            // Join the sender thread
            senderThread.join();
        }
    } catch (const std::exception &e) {
        std::cerr << "Exception: " << e.what() << std::endl;
    }

    return 0;
}

```

websocket客户端示例：

```cpp
#include <iostream>
#include <string>
#include <boost/asio.hpp>
#include <boost/beast.hpp>

namespace asio = boost::asio;
namespace beast = boost::beast;
using tcp = asio::ip::tcp;

int main() {
    try {
        asio::io_context io_context;

        // Resolve the host name
        tcp::resolver resolver(io_context);
        auto const results = resolver.resolve("localhost", "8881");

        // Connect to the server
        tcp::socket socket(io_context);
        asio::connect(socket, results);

        // WebSocket upgrade
        beast::websocket::stream<tcp::socket> ws(std::move(socket));
        ws.handshake("localhost", "/");

        // Receive and print the initial message from the server
        beast::flat_buffer buffer;
        ws.read(buffer);
        std::cout << "Received: " << beast::buffers_to_string(buffer.data()) << std::endl;

        // Start a thread to continuously receive messages from the server
        std::thread receiverThread([&ws] {
            try {
                while (true) {
                    beast::flat_buffer buffer;
                    ws.read(buffer);
                    std::cout << "Received: " << beast::buffers_to_string(buffer.data()) << std::endl;
                }
            } catch (const std::exception &e) {
                std::cerr << "Exception in receiverThread: " << e.what() << std::endl;
            }
        });

        // Send a WebSocket message
        ws.text(true);
        ws.write(asio::buffer("Client: Hello, Server!"));

        // Join the receiver thread
        receiverThread.join();

        // Close the WebSocket connection
        ws.close(beast::websocket::close_code::normal);
    } catch (const std::exception &e) {
        std::cerr << "Exception: " << e.what() << std::endl;
    }

    return 0;
}

```

服务端示例：

```cpp
#include <iostream>
#include <string>
#include <boost/beast/core.hpp>
#include <boost/beast/websocket.hpp>
#include <boost/asio/ip/tcp.hpp>
#include <boost/asio/io_context.hpp>

namespace beast = boost::beast;
namespace websocket = beast::websocket;
using tcp = boost::asio::ip::tcp;

void runWebSocketServer(unsigned short port) {
    try {
        boost::asio::io_context io_context;

        tcp::acceptor acceptor(io_context, tcp::endpoint(tcp::v4(), port));
        tcp::socket socket(io_context);
        acceptor.accept(socket);

        websocket::stream<tcp::socket> ws(std::move(socket));
        ws.accept();

        while (true) {
            beast::flat_buffer buffer;
            ws.read(buffer);

            std::string message(beast::buffers_to_string(buffer.data()));
            std::cout << "Received message: " << message << " port: " << port << std::endl;

            ws.text(ws.got_text());
            ws.write(boost::asio::buffer("Response: " + message));
        }
    } catch (const std::exception& e) {
        std::cerr << "Error: " << e.what() << std::endl;
    }
}

int main() {
    std::cout << "WebSocket server is running..." << std::endl;

    unsigned short port = 9002;
    runWebSocketServer(port);

    // 可在多线程中开启多个端口
    // unsigned short port2 = 9003;
    // runWebSocketServer(port2);

    return 0;
}

```

客户端示例：

```cpp
#include <iostream>
#include <chrono>
#include <thread>
#include <boost/beast/core.hpp>
#include <boost/beast/websocket.hpp>
#include <boost/asio/ip/tcp.hpp>
#include <boost/asio/connect.hpp>
#include <boost/asio/io_context.hpp>

namespace beast = boost::beast;
namespace websocket = beast::websocket;
using tcp = boost::asio::ip::tcp;

void runWebSocketClient(const std::string& serverAddress, unsigned short port) {
    try {
        boost::asio::io_context io_context;
        tcp::resolver resolver(io_context);
        websocket::stream<tcp::socket> ws(io_context);

        auto const results = resolver.resolve(serverAddress, std::to_string(port));
        boost::asio::connect(ws.next_layer(), results.begin(), results.end());
        ws.handshake(serverAddress, "/");

        while (true) {
            std::string message = "Hello, WebSocket!";
            ws.write(boost::asio::buffer(message));

            beast::flat_buffer buffer;
            ws.read(buffer);
            std::cout << "Server response: " << beast::buffers_to_string(buffer.data()) << " port: " << port << std::endl;

            std::this_thread::sleep_for(std::chrono::seconds(1));
        }

        // ws.close(websocket::close_code::normal);
    } catch (const std::exception& e) {
        std::cerr << "Error: " << e.what() << std::endl;
    }
}

int main() {
    std::cout << "WebSocket client example" << std::endl;

    std::string serverAddress = "localhost";
    unsigned short port = 9002;
    runWebSocketClient(serverAddress, port);

    // 可在多线程中开启多个端口
    // unsigned short port2 = 9003;
    // runWebSocketClient(serverAddress, port2);

    return 0;
}

```

编译：

```
g++ server.cpp -o server -lboost_system -lboost_thread -lpthread
g++ client.cpp -o client -lboost_system -lboost_thread -lpthread

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6cbcd6c17cec4dba9bb3c0f895f02fa2.png)


以上。





