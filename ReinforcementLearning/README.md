# Reinforcement Learning

**什么是强化学习**

> Reinforcement Learning is learning what to do -- **how to map situations to actions** -- so as to maximize a numerical reward signal.
>
> **trial-and-error search and delayed reward **-- are the two important distinguishing features of reinforcement learning.
>
> 强化学习是定义在一类问题上，而不是方法上，所有能解决那类问题的方法都可以称作强化学习方法。
>
> 强化学习定义在什么的问题上呢？  a learning agent interacting with its environment to achieve a goal.



在 DRL 中，深度学习那一块可以看作特征提取工具。

几个重要概念：

* state： The state is sufficient statistic of the environment and thereby comprises all the necessary information for the action to take the best action.



## 强化学习

**三大类算法**

* value based
* policy based
* model based



**三类问题**

* prediction
* control
* planning



**算法派系**

* Q-Learning
  * Q-Learning
  * Deep Q-Learning
  * Double Deep Q-Learning
  * Prioritized Experience Replay
* Policy-Based
  * Actor-Critic
  * REINFORCE
  * A3C
  * DPG
  * DDPG
  * ​






## DRL 面临的问题

* 监督信号只有一个 reward，而且十分稀疏
* agent 的 observation 是时序相关的，并不是 iid 的。
  * 这个问题是这样：传统的 RL 算法，都是看到一个 obs，然后直接就更新参数，但是 DL 需要训练数据是 IID 的。用传统 RL 的训练方法显然是不行的啦，所以搞出了 experience replay 方法。
  * 为什么 DL 需要的训练数据是 IID 的呢？ 可能的原因是：因为我们用 mini-batch 训练方法，一个 mini-batch 的梯度应该是 整个 batch 的无偏估计，数据 IID 的话，是 无偏，但是如果数据不是 IID 的话，那就不是 无偏了。






## Glossary

* prediction problem : 也叫做 policy evaluation。给定一个 policy， 计算 state-value function 或 action-value function 的值。
* control problem ： 寻找最优 policy
* Planning：根据 model 构建一个 value function 或者 policy。（model已知哦）
* on-policy： evaluate or improve the behavior policy
* off-policy ：从 behavior policy 形成的 traces 中学习 自己的最优 policy
* model-free： agent 直接从 experience 中学习，model未知（不知道 状态转移矩阵）
* on-line mode：training algorithms are executed on data acquired in sequence。
* off-line mode：也叫 batch mode，并不是看到一个样本就训练。
* episodic：环境有个终止状态（而且一定会到达这个终止状态）
* non-episodic： 环境没有终止状态（或者不会到达一个终止状态）（MC的方法歇菜）