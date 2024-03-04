







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍Golang环境配置与示例。  
>  **学其所用，用其所学。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. Golang介绍](#smirk1_Golang_7)
	+ [:blush:2. 环境安装与配置](#blush2__25)
	+ [:satisfied:3. 应用示例](#satisfied3__37)
	+ - [最简示例：](#_38)
		- [格式化字符串示例：](#_54)
		- [变量声明示例：](#_74)
		- [常量声明示例：](#_91)
		- [函数调用示例：](#_110)
		- [数据声明示例：](#_150)
		- [指针使用示例：](#_180)
		- [递归函数示例（阶乘）](#_206)
		- [类型转换示例：](#_226)
		- [并发（轻量级线程）：](#_263)
		- [UDP网络示例：](#UDP_286)
		- [json解析示例：](#json_383)




### 😏1. Golang介绍


`Go`（也被称为 `Golang`）是一种开源的静态类型编程语言，由 Google 开发并于2009年首次公开发布。Go 语言的设计目标是提供一种简单、高效、可靠的编程语言，适用于构建大型项目的并发和网络应用。


以下是 Go 语言的一些特点和优势：



> 
> 1.简洁易读：Go 语言采用简洁的语法和结构，注重代码的可读性。它摈弃了一些复杂的特性和冗余的语法，使得代码更易理解和维护。
> 
> 
> 



> 
> 2.并发支持：Go 语言天生支持并发编程。它引入了 goroutine 和 channel 的概念，使得编写并发程序变得简单和高效。通过 goroutine，可以轻松地创建并发执行的任务，并通过 channel 在不同的 goroutine 之间进行通信和数据同步。
> 
> 
> 



> 
> 3.高性能：Go 语言通过使用垃圾回收器、即时编译技术和轻量级线程（goroutine）等特性，实现了出色的运行时性能。它在处理并发和网络任务时非常高效，适用于构建高性能的分布式系统。
> 
> 
> 



> 
> 4.内置工具和库：Go 语言附带了丰富的标准库，内置了许多常用的功能和工具。例如，网络编程、加密解密、文件操作、测试框架等。这些库使得开发者可以更轻松地构建各种类型的应用程序。
> 
> 
> 



> 
> 5.跨平台支持：Go 语言的编译器和运行时支持跨多个平台，包括 Windows、macOS、Linux 等。这使得开发者可以方便地在不同的操作系统上开发和部署应用程序。
> 
> 
> 



> 
> 6.静态类型检查：Go 语言是一种静态类型语言，它在编译阶段进行强类型检查，可以帮助开发者在编码过程中捕获潜在的错误。这有助于提高代码质量和可靠性。
> 
> 
> 



> 
> 7.社区支持：Go 语言拥有庞大而活跃的开发者社区，社区成员分享着大量的代码库、文档和教程。开发者可以从社区中获取支持和帮助，加快开发进度。
> 
> 
> 


### 😊2. 环境安装与配置



```
# Ubuntu
# apt安装
sudo apt install golang-go
# 版本验证
go version
# 编译运行（2种方法）
go run hello.go
go build hello.go && ./hello

```

### 😆3. 应用示例


#### 最简示例：



```
// hello.go
package main // 包声明

import "fmt" // 引入包

/\* 入口函数 \*/
func main() { // 需要注意的是 { 不能单独放在一行
	/\* 第一个go语句 \*/
	fmt.Println("Hello, World!") // 一行写一个go语句，不需要;
	fmt.Println("Hello, Go!")
	fmt.Println("Google" + " Go!")
	/\* 注释与C++形式一样 单行注释// 多行注释/\* \*/
}

```

#### 格式化字符串示例：



```
package main

import (
	"fmt"
)

func main() {
	// %d 表示整型数字，%s 表示字符串
	var favnumber=6
	var favfruit="apple"
	var fav="I love number = %d and love fruit is = %s"
	/\* 格式化字符串 Sprintf Printf \*/
	// var favorite=fmt.Sprintf(fav,favnumber,favfruit)
	// fmt.Println(favorite)
	fmt.Printf(fav,favnumber,favfruit)
}

```

#### 变量声明示例：



```
package main
import "fmt"
func main() {
    var a string = "Hello" // 声明变量并初始化
    fmt.Println(a)

    var b, c int = 1, 2
	var d bool // 未初始化默认零值
    fmt.Println(b, c, d)

	result := b + c // 这种不带声明格式的只能在函数体中出现（简短声明）
	fmt.Println(result)
}

```

#### 常量声明示例：



```
package main

import "fmt"

func main() {
   const LENGTH int = 10
   const WIDTH int = 5  
   var area int
   const a, b, c = 1, false, "str" //多重赋值

   area = LENGTH \* WIDTH
   fmt.Printf("面积为 : %d", area)
   println()
   println(a, b, c)  
}

```

#### 函数调用示例：



```
package main

import "fmt"

func main() {
   /\* 定义局部变量 \*/
   var a int = 100
   var b int = 200
   var ret int

   /\* 调用函数并返回最大值 \*/
   ret = max(a, b)

   fmt.Printf("最大值是 : %d\n", ret )

   c, d := swap("Google", "Go")
   fmt.Println(c, d)
}

/\* 返回两个数的最大值 \*/
func max(num1, num2 int) int {
   /\* 定义局部变量 \*/
   var result int

   if (num1 > num2) {
      result = num1
   } else {
      result = num2
   }
   return result
}

/\* 交换两个string \*/
func swap(x, y string) (string, string) {
	return y, x
}

```

#### 数据声明示例：



```
package main

import "fmt"

func main() {
   var i,j,k int
   /\* 声明数组的同时快速初始化数组 \*/
   balance := [5]float32{1000.0, 2.0, 3.4, 7.0, 50.0}
   /\* 输出数组元素 \*/
   for i = 0; i < 5; i++ {
      fmt.Printf("balance[%d] = %f\n", i, balance[i] )
   }
   
   /\* 数组长度不确定 \*/
   balance2 := [...]float32{1000.0, 2.0, 3.4, 7.0, 50.0}
   /\* 输出每个数组元素的值 \*/
   for j = 0; j < 5; j++ {
      fmt.Printf("balance2[%d] = %f\n", j, balance2[j] )
   }

   /\* 将索引为 1 和 3 的元素初始化 \*/
   balance3 := [5]float32{1:2.0, 3:7.0}  
   for k = 0; k < 5; k++ {
      fmt.Printf("balance3[%d] = %f\n", k, balance3[k] )
   }
}

```

#### 指针使用示例：



```
package main

import "fmt"

func main() {
   var a int= 20 // 声明实际变量
   var ip \*int   // 声明指针变量

   ip = &a  // 指针变量的存储地址

   fmt.Printf("a 变量的地址是: %x\n", &a  )

   /\* 指针变量的存储地址 \*/
   fmt.Printf("ip 变量储存的指针地址: %x\n", ip )

   /\* 使用指针访问值 \*/
   fmt.Printf("\*ip 变量的值: %d\n", \*ip )

   /\* 空指针 \*/
   var ptr \*int
   fmt.Printf("ptr 的值为 : %x\n", ptr)
}

```

#### 递归函数示例（阶乘）



```
package main

import "fmt"

func Factorial(n uint64)(result uint64) {
    if (n > 0) {
        result = n \* Factorial(n-1)
        return result
    }
    return 1
}

func main() {  
    var i int = 6
    fmt.Printf("%d 的阶乘是 %d\n", i, Factorial(uint64(i)))
}

```

#### 类型转换示例：



```
package main

import (
	"fmt"
	"strconv"
)

func main() {
	/\* 数值类型转换 \*/
	var sum int = 17
	var count int = 5
	var mean float32
   
	mean = float32(sum)/float32(count)
	fmt.Printf("mean 的值为: %f\n",mean)

	/\* 字符串类型转换 \*/
	num := 123
    str := strconv.Itoa(num)
    fmt.Printf("整数 %d 转换为字符串为：'%s'\n", num, str)

	str1 := "3.14"
    num1, err := strconv.ParseFloat(str1, 64)
    if err != nil {
        fmt.Println("转换错误:", err)
    } else {
        fmt.Printf("字符串 '%s' 转为浮点型为：%f\n", str1, num1)
    }

	num2 := 3.14
    str2 := strconv.FormatFloat(num2, 'f', 2, 64)
    fmt.Printf("浮点数 %f 转为字符串为：'%s'\n", num2, str2)
}

```

#### 并发（轻量级线程）：



```
package main

import (
	"fmt"
	"time"
)

func say(s string) {
	for i := 0; i < 5; i++ {
		time.Sleep(100 \* time.Millisecond)
		fmt.Println(s)
	}
}

func main() {
	go say("world") // goroutine轻量级线程，不需要显式地创建和管理线程
	say("hello")
}

```

#### UDP网络示例：


客户端：



```
package main

import (
	"fmt"
	"net"
)

func main() {
	// 创建一个 UDP 地址结构
	serverAddr, err := net.ResolveUDPAddr("udp", "127.0.0.1:8888")
	if err != nil {
		fmt.Println("Error: ", err)
		return
	}

	// 创建一个 UDP 连接
	conn, err := net.DialUDP("udp", nil, serverAddr)
	if err != nil {
		fmt.Println("Error: ", err)
		return
	}
	defer conn.Close()

	// 向服务器发送消息
	msg := []byte("Hello, Server!")
	\_, err = conn.Write(msg)
	if err != nil {
		fmt.Println("Error: ", err)
		return
	}

	// 接收服务器的响应消息
	buf := make([]byte, 1024)
	n, \_, err := conn.ReadFromUDP(buf)
	if err != nil {
		fmt.Println("Error: ", err)
		return
	}

	// 将服务器的响应消息打印到控制台
	fmt.Println("Received Message:", string(buf[:n]))
}

```

服务端：



```
package main

import (
	"fmt"
	"net"
)

func main() {
	// 创建一个 UDP 地址结构
	addr, err := net.ResolveUDPAddr("udp", ":8888")
	if err != nil {
		fmt.Println("Error: ", err)
		return
	}

	// 创建一个 UDP 连接监听器
	conn, err := net.ListenUDP("udp", addr)
	if err != nil {
		fmt.Println("Error: ", err)
		return
	}
	defer conn.Close()

	fmt.Println("UDP Server Running...")

	for {
		// 接收来自客户端的消息
		buf := make([]byte, 1024)
		n, remoteAddr, err := conn.ReadFromUDP(buf)
		if err != nil {
			fmt.Println("Error: ", err)
			continue
		}

		// 将接收到的消息打印到控制台
		fmt.Printf("Received message from %s: %s\n", remoteAddr.String(), string(buf[:n]))

		// 向客户端发送响应
		reply := []byte("Hi, client!")
		\_, err = conn.WriteToUDP(reply, remoteAddr)
		if err != nil {
			fmt.Println("Error: ", err)
			continue
		}
	}
}

```

#### json解析示例：



```
package main

import (
	"encoding/json"
	"fmt"
)

type Person struct {
	Name string `json:"name"`
	Age  int    `json:"age"`
}

func main() {
	/\* 解析json \*/
	// 要解析的 JSON 数据
	jsonData := []byte(`{"name":"Alice","age":25}`)

	// 创建一个 Person 类型的变量
	var p Person

	// 解析 JSON 数据到 Person 变量
	err := json.Unmarshal(jsonData, &p)
	if err != nil {
		fmt.Println("解析 JSON 出错:", err)
		return
	}

	// 输出解析结果
	fmt.Println("Name:", p.Name)
	fmt.Println("Age:", p.Age)


	/\* 写入json \*/
	// // 创建一个 Person 对象
	// p := Person{Name: "Bob", Age: 30}

	// // 将 Person 对象转换为 JSON 格式
	// jsonData, err := json.Marshal(p)
	// if err != nil {
	// fmt.Println("生成 JSON 出错:", err)
	// return
	// }

	// // 输出生成的 JSON 数据
	// fmt.Println(string(jsonData)) 
}

```

![请添加图片描述](https://img-blog.csdnimg.cn/5ea93bb657184b9eb8515cc76047c16a.png)


以上。





