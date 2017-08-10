# DRL tutorial icml2016  David Silver

**RL 的三种方法**

* value-based
* policy-based
* model-based



**RL is a general-purpose framework for decision-making**

* RL is for an `agent` with the capacity to `act`
* Each `action` influences the agent's future `state`
* Success is measured by a scalar `reward` signal
* Goal: `select actions to maximize future reward`



**DL is general-purpose framework for representation learning**

* Given an `objective`
* Learn `representation` that is required to achieve objective
* Directly from `raw inputs`
* Using minimal domain knowledge



**AI = RL + DL**

* RL defines the objective (maximize future reward?)
* DL gives the mechanism (build the representation)
* RL+DL = general intelligence



## DL

**weight sharing**

* over time (RNN)
* over space (CNN)



## RL

**An RL agent may include one or more of these components**

* Policy : agent's behavior function
* Value function : how good is each state and/or action
* Model: agent's representation of the environment

**Policy**

* deterministic policy: 
* stochastic policy: 

**Value Function**

* value function is a prediction of future reward

  * "How much reward will i get from action a in state s"
* Q-value function gives expected total reward
  * from state `s` and action `a`
  * under policy $\pi$
  * with discount factor $\gamma$


$$
Q^{\pi}(a|s) = \mathbb E\Biggr[r_{t+1}+\gamma r_{t+2}+...\Bigr|s,a\Biggr]
$$

* Value functions decompose into a Bellman equation


$$
Q^{\pi}(a|s) = \mathbb E\Biggr[r_{t+1}+\gamma Q^\pi(a'|s')\Bigr|s,a\Biggr]
$$
**Model**

* build a model of the environment
* Plan (e.g. by lookahead search) using model



## how to combine DL and RL

**Use deep neural networks to represent**

* Value function
* Policy
* Model



**Optimize loss function by SGD**



## Value-based DRL

* Represent value function by `Q-network` with weight `w`

如果 action 的离散的：

* Q(a|s;w), 输入 state，输出 n 个值 表示每个 action 的 q-value

如果 action 是连续的：

* Q(s,a;w), 输入 state，action， 输入 相应哦 q-value



**Q-learning**

* 最优的 Q-values 应该服从 Bellman equation

$$
Q^*(s,a) = \mathbb E_{s'}\Biggr[r+\gamma \max_{a'}Q^*(s',a')\Bigr|s,a\Biggr]
$$

* 右边那项作为 target
* 使用 随即梯度下降来 最小化 MSE loss


$$
I=\Biggr(r+\gamma \max_{a'}Q^*(s',a';w)-Q(s,a;w)\Biggr)^2
$$

* q-value 向 target 靠近



**Q-learning ...**

* DQN
* Double DQN
* Prioritised replay
* Dueling network
* Gorila



**Asynchronous Reinforcement Learning**

* Exploits multi-threading of standard CPU
* Execute many instances of agent in parallel
* Network parameters shared between threads
* Parallelism **decorrelates** data
  * Viable alternative to experience replay
  * Parallelism decorrelates data
* Similar speedup to Gorila - on a single machine



## Policy-Based DRL

* 用神经网络来表示 policy ， policy-network

$$
a = \pi(a|s;\mu) 
$$

$$
a=\pi(s;\mu)
$$

* 用 total discounted reward 作为 objective function



$$
L(\mu) = \mathbb E\Biggr[r_1+\gamma r_2+...\Bigr|\pi(\star;\mu)\Biggr]
$$

* 调整 $\mu$ 来获得 更多的 reward



* 策略梯度
  * stochastic policy
    * action 是 随机的


$$
\frac{\partial L(\mu)}{\partial\mu} =\mathbb E\Biggr[\frac{\partial\log\pi(a|s;\mu)}{\partial \mu}Q^\pi(s,a)\Biggr]
$$



* 
  * deterministic
    * action 是 连续的


$$
\frac{\partial L(\mu)}{\partial\mu} =\mathbb E\Biggr[\frac{\partial\log\pi(a|s;\mu)}{\partial a}\frac{\partial a}{\partial \mu}\Biggr]
$$


**Algorithms**

* DDPG
* DPG
* A3C



**DRL with Continuous Actions (high-dimensional)**

* 没法简单的计算 $\max\limits_aQ(s,a)$, 因为是连续的空间，没法简单的求全局最大值
  * Actor-critic 方法没有使用 $\max$
* Q-values 对 a 是可导的
  * Deterministic policy gradients exploit knowledge of $\frac{\partial Q}{\partial a}$
* ​



## Model Based

**Learning Models of the Environment**

**What if we have a perfect model? e.g. game rules are known**



**Alpha Go**





## 参考资料

[http://techtalks.tv/talks/deep-reinforcement-learning/62360/](http://techtalks.tv/talks/deep-reinforcement-learning/62360/)

