







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍Rust环境配置与语法基础。  
>  **学其所用，用其所学。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. Rust介绍](#smirk1_Rust_7)
	+ [:blush:2. 环境安装与配置](#blush2__25)
	+ [:satisfied:3. 应用示例](#satisfied3__56)




### 😏1. Rust介绍


`Rust`是一种创新型的系统编程语言，由Mozilla研发。它将C++的高性能和控制力与安全性、并发性和现代语言设计相结合。


官网：`https://www.rust-lang.org/`


Rust具有以下特点：



> 
> 1.零成本抽象: Rust允许编写高层次的抽象代码，同时不会对性能产生影响；
> 
> 
> 



> 
> 2.安全保障: Rust通过语言级别的静态内存管理和所有权模型来避免常见的内存安全问题；
> 
> 
> 



> 
> 3.并发支持: Rust提供了多线程编程的支持，并且可以避免锁的使用和线程竞争问题；
> 
> 
> 



> 
> 4.高性能: Rust通过内联汇编、去除垃圾回收等技术实现了C++级别的性能；
> 
> 
> 



> 
> 5.生态丰富: Rust生态系统中有大量优秀的第三方库支持，可以满足不同领域的需求；
> 
> 
> 


综上，Rust是一种用于构建高性能、可靠和安全的系统级应用程序的语言。它旨在成为下一个系统编程语言的首选。目前，Rust已经被广泛应用于各种领域，包括Web开发、游戏开发、网络应用和嵌入式设备等。


### 😊2. 环境安装与配置


Rust安装：



```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
# 安装完成后，rustup会显示完整的安装路径，包括cargo（Rust的构建工具）和rustc（Rust编译器）
# 生效环境
source $HOME/.cargo/env
# 查看版本号
rustc --version
cargo --version

```

国内源配置（在`.cargo`创建`config`，将配置写入，否则`cargo build`会出错）：



```
[source.crates-io]
registry = "https://github.com/rust-lang/crates.io-index"

replace-with = 'tuna'
[source.tuna]
registry = "https://mirrors.tuna.tsinghua.edu.cn/git/crates.io-index.git"

#replace-with = 'ustc'
#[source.ustc]
#registry = "git://mirrors.ustc.edu.cn/crates.io-index"

[net]
git-fetch-with-cli = true

```

### 😆3. 应用示例


helloworld示例：



```
cargo new hello-rust
# 生成目录如下
hello-rust
|- Cargo.toml # 编译文件
|- src
  |- main.rs # 源文件
cargo run

```

添加ferris-says依赖示例：



```
cargo add ferris-says
# main.rs写入
cargo build
cargo run

```


```
use ferris\_says::say; // from the previous step
use std::io::{stdout, BufWriter};

fn main() {
    let stdout = stdout();
    let message = String::from("Hello fellow Rustaceans!");
    let width = message.chars().count();

    let mut writer = BufWriter::new(stdout.lock());
    say(&message, width, &mut writer).unwrap();
}

```

![请添加图片描述](https://img-blog.csdnimg.cn/5ea93bb657184b9eb8515cc76047c16a.png)


以上。





