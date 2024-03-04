







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍CI/CD持续集成与部署C++示例。  
>  **学其所用，用其所学。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. CI/CD介绍](#smirk1_CICD_7)
	+ [:blush:2. GitHub Actions示例](#blush2_GitHub_Actions_25)
	+ [:satisfied:3. GitLab CI/CD示例](#satisfied3_GitLab_CICD_57)




### 😏1. CI/CD介绍


`CI/CD`（持续集成/持续交付）是一种软件开发实践和方法论，旨在通过自动化和持续性地集成、构建、测试和交付软件来提高开发团队的效率和软件质量。它的目标是使软件开发流程更加敏捷、可靠和可持续。


`CI/CD` 通常包括以下两个主要概念：



> 
> 1.持续集成（Continuous Integration）：持续集成是指开发人员将代码频繁地合并到共享代码库（如版本控制系统）中，并通过自动化构建和测试来验证代码的正确性。每当有新的代码提交时，持续集成服务器会自动触发构建过程，运行测试套件，并提供即时的反馈。这有助于发现和解决问题，避免在开发周期后期的集成问题。
> 
> 
> 



> 
> 2.持续交付/持续部署（Continuous Delivery/Continuous Deployment）：持续交付和持续部署是指在通过持续集成验证后，自动将应用程序交付给生产环境或部署到目标服务器的过程。持续交付意味着构建、测试和打包过程自动化，并生成可交付的软件包，但最终的部署仍然需要手动触发。持续部署则更进一步，将软件的部署过程也自动化，从而实现完全自动化的软件交付和部署。
> 
> 
> 


CI/CD 的主要优势包括：


* 加速软件交付：通过自动化和并行化的流程，减少手动操作和部署时间，加快软件交付速度。
* 提高软件质量：通过持续集成和自动化测试，及时发现和解决问题，提高软件质量和稳定性。
* 降低风险：由于频繁的集成和测试，可以快速发现和解决潜在问题，减少集成和部署过程中的风险。
* 增强团队协作：通过共享代码库和自动化流程，促进团队成员之间的协作和沟通，减少集成冲突。
* 可追溯性和可重复性：所有构建和部署过程都被记录下来，使得可以追溯到特定版本的软件，同时也可以重复执行相同的流程。


CI/CD 工具和平台提供了一组功能和功能集，用于自动化构建、测试和部署流程。一些常见的 CI/CD 工具包括 `Jenkins`、`GitLab CI/CD`、`Travis CI`、`CircleCI` 和 `GitHub Actions`。


### 😊2. GitHub Actions示例


在项目中创建`.github/workflows/cpp.yml`和`main.cpp`，一个最简的示例如下：



```
name: C++ CI

on:
  push:
    branches:
      - main  # 当 main 分支有代码推送时触发工作流
  pull\_request:
    branches:
      - main  # 当有针对 main 分支的 PR 时触发工作流

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2  # 检出代码

    - name: Set up C++ compiler
      run: sudo apt-get update && sudo apt-get install -y g++  # 设置 C++ 编译环境

    - name: Build
      run: g++ main.cpp -o main  # 执行编译命令

    - name: Test
      run: ./main # run

```

### 😆3. GitLab CI/CD示例


在项目中创建.gitlab-ci.yml和main.cpp，最简示例如下：



```
stages:
  - build
  - test

build:
  stage: build
  image: ubuntu:20.04
  before\_script:
    - apt-get update && apt-get install -y g++
  script:
    - g++ main.cpp -o main && ./main

test:
  stage: test
  script:
    - echo "Running tests..."


```

![请添加图片描述](https://img-blog.csdnimg.cn/5ea93bb657184b9eb8515cc76047c16a.png)


以上。





