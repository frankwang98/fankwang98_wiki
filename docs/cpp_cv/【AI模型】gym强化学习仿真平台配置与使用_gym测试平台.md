







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍gym强化学习仿真平台配置与使用。  
>  **无专精则不能成，无涉猎则不能通。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. 项目介绍](#smirk1__7)
	+ [:blush:2. 环境配置](#blush2__29)
	+ [:satisfied:3. 使用说明](#satisfied3__49)




### 😏1. 项目介绍


项目Github地址：`https://github.com/openai/gym`


Doc：`https://gymnasium.farama.org/`


OpenAI Gym 是一个用于开发和比较强化学习算法的开源工具包。它提供了一系列标准化的环境场景和 API 接口，使得研究人员和开发者能够轻松地创建、测试和评估各种强化学习算法。


以下是 OpenAI Gym 的一些重要特点和组成部分：



> 
> 1.环境（Environments）：OpenAI Gym 包含了大量的环境场景，涵盖了从经典的控制任务到连续动作空间中的机器人控制等多种应用。例如，CartPole（倒立摆）、MountainCar（上山车）和Pong（乒乓球游戏）等。每个环境都提供了一组标准化的状态和动作空间，以及定义好的奖励机制。
> 
> 
> 



> 
> 2.动作空间（Action Spaces）：Gym 支持多种类型的动作空间，包括离散（Discrete）动作空间，如左/右移动或选择某个动作编号；以及连续（Continuous）动作空间，如在某个范围内选择一个实数值。
> 
> 
> 



> 
> 3.状态空间（Observation Spaces）：Gym 定义了标准的状态观测空间，以便智能代理从环境中获取感知信息。状态可以是离散的，也可以是连续的。
> 
> 
> 



> 
> 4.奖励（Rewards）：每次执行动作后，环境会给予智能代理一个奖励信号，以指导其学习。奖励可以是正数、负数或零，表明了智能代理对于特定状态和动作的性能好坏。
> 
> 
> 



> 
> 5.API 接口：Gym 提供了方便易用的 API 接口，使得研究人员和开发者能够与环境进行交互。这些接口包括 reset()（重置环境）、step()（执行动作并观察下一个状态和奖励）和 render()（可选的渲染环境）等。
> 
> 
> 



> 
> 6.应用广泛：OpenAI Gym 被广泛应用于强化学习的研究、教育和开发中。它提供了一个统一的接口和基准环境，使得不同算法和方法之间的比较更加公平和可靠。
> 
> 
> 


OpenAI Gym 的目标是为强化学习社区提供一个通用的平台，促进算法的创新、共享和发展。它已经成为许多强化学习学术论文和项目的标准工具。


### 😊2. 环境配置


下面进行环境配置：



```
# 安装依赖
sudo apt install -y libgl1-mesa-dev libgl1-mesa-glx libopenmpi-dev zlib1g-dev
# 最好在Linux或Mac使用
pip install gym

```

另外也可通过源码安装：



```
git clone https://github.com/openai/gym.git
cd gym
pip install -e .
# 验证
python -m gym.envs.classic_control.cartpole

```

### 😆3. 使用说明


Gym示例：



```
import gym

env = gym.make("CartPole-v1")
observation, info = env.reset(seed=42)

for _ in range(1000):
    action = env.action_space.sample()
    observation, reward, terminated, truncated, info = env.step(action)
    print("run step ...")

    if terminated or truncated:
        observation, info = env.reset()
env.close()

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6cbcd6c17cec4dba9bb3c0f895f02fa2.png)


以上。





