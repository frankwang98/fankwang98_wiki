## jemalloc介绍
jemalloc 是一个开源的通用内存分配器，专门用于动态内存管理。它最初由 Facebook 的工程师 Jason Evans 开发，并已被许多大型互联网公司广泛使用，包括 Facebook、Netflix、Mozilla 等。

github: `https://github.com/jemalloc/jemalloc`

wiki: `http://jemalloc.net/`

以下是 jemalloc 的一些主要特点和优势：

高性能：jemalloc 设计了许多优化策略，以提高内存分配和释放的性能。它采用了多种技术，包括线程本地缓存、分离的内存分配器、延迟分配等，以实现高效的内存管理。

可伸缩性：jemalloc 被设计为高度可伸缩的内存分配器，适用于多线程和多核环境。它使用了多种技术来减少锁竞争，提高并发性能。

低碎片化：jemalloc 使用了多种算法和技术来减少内存碎片化，并提高内存利用率。它采用了分离的内存分配器和延迟分配策略，以减少内存碎片化的影响。

可定制性：jemalloc 提供了丰富的配置选项和调优参数，允许用户根据自己的需求进行定制和优化。用户可以通过调整参数来改变内存分配器的行为，以满足不同应用场景的需求。

跨平台：jemalloc 支持多种操作系统和架构，包括 Linux、FreeBSD、macOS 等，可以在各种环境下使用。

总的来说，jemalloc 是一个性能优异、可伸缩、低碎片化的通用内存分配器，适用于构建高性能和可伸缩的应用程序。它已经被许多大型互联网公司广泛应用，并且在开源社区中得到了广泛认可和使用。

## 环境配置

apt安装：
```sh
sudo apt install -y libjemalloc-dev
```

源码安装：
```sh
git clone https://github.com/jemalloc/jemalloc.git
./autogen.sh
configure
make
sudo make install
```

## 应用示例
内存分配和释放示例：

```cpp
#include <jemalloc/jemalloc.h>
#include <iostream>

int main() {
    // 分配内存
    size_t size = 1024; // 分配 1024 字节的内存
    void* ptr = je_malloc(size); // 使用 jemalloc 进行内存分配

    if (ptr == nullptr) {
        std::cerr << "Memory allocation failed!" << std::endl;
        return 1;
    }

    std::cout << "Memory allocated successfully!" << std::endl;

    // 使用分配的内存
    // 这里可以对分配的内存进行读写操作

    // 释放内存
    je_free(ptr); // 使用 jemalloc 进行内存释放

    std::cout << "Memory freed successfully!" << std::endl;

    return 0;
}
```