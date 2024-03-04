







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍基于SpringBoot创建Web页面并热更新。  
>  **学其所用，用其所学。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. SpringBoot介绍](#smirk1_SpringBoot_7)
	+ [:blush:2. 环境安装与配置](#blush2__23)
	+ [:satisfied:3. 应用示例](#satisfied3__41)
	+ [:satisfied:4. 开发环境热更新](#satisfied4__69)




### 😏1. SpringBoot介绍


官网：`https://spring.io/`


`Spring Boot` 是一个用于快速开发单个微服务的框架，它基于 `Spring` 框架，简化了 Spring 应用的初始化过程和开发流程。Spring Boot 提供了一套默认的配置，使得开发人员可以快速搭建和运行基于 Spring 的应用程序。


Spring Boot 的特点包括：



> 
> 1.简化配置：Spring Boot 提供了约定优于配置的理念，大部分的应用都可以使用默认的配置，减少了开发人员对配置文件进行繁琐设置的需求。
> 
> 
> 



> 
> 2.内嵌容器：Spring Boot 支持内嵌 Tomcat、Jetty、Undertow 等 Servlet 容器，可直接通过 main 方法启动应用，无需额外部署。
> 
> 
> 



> 
> 3.自动化配置：Spring Boot 可以根据项目的依赖和环境自动配置 Spring 应用程序，大大减少了开发人员的工作量。
> 
> 
> 



> 
> 4.独立运行：Spring Boot 应用程序可以作为独立的 Java 程序运行，不需要外部部署容器。
> 
> 
> 



> 
> 5.集成测试：Spring Boot 内建了对单元测试和集成测试的支持，提供了方便的测试工具。
> 
> 
> 


### 😊2. 环境安装与配置


在IDEA社区版中创建`SpringBoot`项目，可以安装`Spring Boot Helper`插件，code可以用：



```
I1VGAYWU90-eyJsaWNlbnNlSWQiOiJJMVZHQVlXVTkwIiwibGljZW5zZWVOYW1lIjoic2lnbnVwIHNjb290ZXIiLCJhc3NpZ25lZU5hbWUiOiIiLCJhc3NpZ25lZUVtYWlsIjoiIiwibGljZW5zZVJlc3RyaWN0aW9uIjoiIiwiY2hlY2tDb25jdXJyZW50VXNlIjpmYWxzZSwicHJvZHVjdHMiOlt7ImNvZGUiOiJQU1BSSU5HQk9PVElERUEiLCJmYWxsYmFja0RhdGUiOiIyMDI1LTA4LTAxIiwicGFpZFVwVG8iOiIyMDI1LTA4LTAxIiwiZXh0ZW5kZWQiOmZhbHNlfV0sIm1ldGFkYXRhIjoiMDEyMDIyMDkwMlBTQU4wMDAwMDUiLCJoYXNoIjoiVFJJQUw6LTkyNjI5NTY5MiIsImdyYWNlUGVyaW9kRGF5cyI6NywiYXV0b1Byb2xvbmdhdGVkIjpmYWxzZSwiaXNBdXRvUHJvbG9uZ2F0ZWQiOmZhbHNlfQ==-dXDw3NXs9u2WveCuTUBmSo6rW6aL6x4BAubU3MvgG1ZxywEH+CMrfRjkHsCqobws/zuaegUkJ9anYcZ3Udkm3xVDDKkb0Vy7xevzhhajbFPH41JRNiySLGcVkjVfUjFigoY1ZBrpvsJ421nfKhsr8Wj1mCYh5O9JTjKRoOB0+s1Yd72ETgvl9YTt3/maE9sRONPW2/3aN0gjtwfPdfTnWk+Cn2+JAsmtlloPD2kwUNjD0ddWpfdFnNvvOP4OhDdNE9tlNmcWOjQs5YRVjwl4UNQiv6szb4j89Mkb8puQ0G3wkhmaMypnUIEEBUBly4FVngj3KHoZnyed0U7j1JWemQ==-MIIETDCCAjSgAwIBAgIBDTANBgkqhkiG9w0BAQsFADAYMRYwFAYDVQQDDA1KZXRQcm9maWxlIENBMB4XDTIwMTAxOTA5MDU1M1oXDTIyMTAyMTA5MDU1M1owHzEdMBsGA1UEAwwUcHJvZDJ5LWZyb20tMjAyMDEwMTkwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQCUlaUFc1wf+CfY9wzFWEL2euKQ5nswqb57V8QZG7d7RoR6rwYUIXseTOAFq210oMEe++LCjzKDuqwDfsyhgDNTgZBPAaC4vUU2oy+XR+Fq8nBixWIsH668HeOnRK6RRhsr0rJzRB95aZ3EAPzBuQ2qPaNGm17pAX0Rd6MPRgjp75IWwI9eA6aMEdPQEVN7uyOtM5zSsjoj79Lbu1fjShOnQZuJcsV8tqnayeFkNzv2LTOlofU/Tbx502Ro073gGjoeRzNvrynAP03pL486P3KCAyiNPhDs2z8/COMrxRlZW5mfzo0xsK0dQGNH3UoG/9RVwHG4eS8LFpMTR9oetHZBAgMBAAGjgZkwgZYwCQYDVR0TBAIwADAdBgNVHQ4EFgQUJNoRIpb1hUHAk0foMSNM9MCEAv8wSAYDVR0jBEEwP4AUo562SGdCEjZBvW3gubSgUouX8bOhHKQaMBgxFjAUBgNVBAMMDUpldFByb2ZpbGUgQ0GCCQDSbLGDsoN54TATBgNVHSUEDDAKBggrBgEFBQcDATALBgNVHQ8EBAMCBaAwDQYJKoZIhvcNAQELBQADggIBABqRoNGxAQct9dQUFK8xqhiZaYPd30TlmCmSAaGJ0eBpvkVeqA2jGYhAQRqFiAlFC63JKvWvRZO1iRuWCEfUMkdqQ9VQPXziE/BlsOIgrL6RlJfuFcEZ8TK3syIfIGQZNCxYhLLUuet2HE6LJYPQ5c0jH4kDooRpcVZ4rBxNwddpctUO2te9UU5/FjhioZQsPvd92qOTsV+8Cyl2fvNhNKD1Uu9ff5AkVIQn4JU23ozdB/R5oUlebwaTE6WZNBs+TA/qPj+5/we9NH71WRB0hqUoLI2AKKyiPw++FtN4Su1vsdDlrAzDj9ILjpjJKA1ImuVcG329/WTYIKysZ1CWK3zATg9BeCUPAV1pQy8ToXOq+RSYen6winZ2OO93eyHv2Iw

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/a2605443a70e464badc7a0d9f287e7dc.png)


然后新建项目就有`Spring Initializr`，界面如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/5ffbea9143654d8b81f581c9c651f9ac.png)


选择`Spring Web`依赖：


![在这里插入图片描述](https://img-blog.csdnimg.cn/993541642d7143cd882d162854ccc288.png)


然后`Maven`就会自动安装SpringBoot的依赖。


### 😆3. 应用示例


下面就开始创建一个简单的`Web`页面：


新建一个`controller`包和类：


![在这里插入图片描述](https://img-blog.csdnimg.cn/5a854a12e2d84cac95a412c81bd7f490.png)


DemoController.java



```
package com.example.java\_springboot.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class DemoController {

// http://localhost:8080/hello 协议+地址+请求页面
    @GetMapping("/hello")
    public String hello() {
        return "你好，世界";
    }
}

```

然后运行项目，就可以打开地址`http://localhost:8080/hello`显示了。


![在这里插入图片描述](https://img-blog.csdnimg.cn/e657a2422fd04c4fb3fe8a58bafd5511.png)


### 😆4. 开发环境热更新


热更新之后，每次改了web的页面，就不用重启项目，IDEA将自动重启刷新。


要实现热更新，首先在`pom.xml`增加依赖：



```
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-devtools</artifactId>
	<optional>true</optional>
</dependency>

```

在配置application.properties里新增，设置好监视的目录：



```
spring.devtools.restart.enabled=true
spring.devtools.restart.additional-paths=src/main/java

```

然后在`设置-编译器`中，勾选`“自动构建项目”`：


![在这里插入图片描述](https://img-blog.csdnimg.cn/55a2dcfda3e642f8a39ef70aabb9ae45.png)


在`设置-高级设置`中，勾选编译器的`“允许自动make启动”`：


![在这里插入图片描述](https://img-blog.csdnimg.cn/2f48ad5311d5465c87b3d82be9ceb1cb.png)


这样设置好之后，就可以更改代码并随时刷新Web页面了。


![请添加图片描述](https://img-blog.csdnimg.cn/5ea93bb657184b9eb8515cc76047c16a.png)


以上。





