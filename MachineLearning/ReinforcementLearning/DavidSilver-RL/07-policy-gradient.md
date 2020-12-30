# Policy Gradient

* Value Based: a policy was generated directly from the value function, e.g. using $\epsilon-greedy$
  * Value Based 方法其实是两步走：policy evaluation, policy improvement
* Policy Based: we will directly parametrise the policy, $\pi_\theta(s,a)=\mathbb P(a|s, \theta)$
  * Policy Based 方法一步走：直接 policy improvement
  
如何评估一个policy $\pi_\theta$ 的好坏呢?
* episode environments, 我们可以使用 start value。$J(\theta) = V^{\pi_\theta}(s1) = \mathbb E_\theta[v1]$
* continuing environments, 我们可以使用 average value。$J_{avV}(\theta)=\sum_sd^{\pi_\theta}(s)V^{\pi_\theta}(s)$
* 或者使用average reward per time-step，$J_{avR}(\theta)=\sum_sd^{\pi_\theta}(s)\sum_a\pi_\theta(s,a)\mathcal R_s^a$

Policy based reinforcement learning is an **optimisation** problem
* Find $\theta$ that maximises $J(\theta)$
* Value based 并不是个优化问题？而是 policy evaluation & policy improvement

One-Step MDP，Policy Gradient推倒
$$
J(\theta)=\mathbb E_{\pi_\theta}[r] 
= \sum_s d(s)\sum_a \pi_\theta(s,a)\mathcal R_s^a
$$

$$
\begin{aligned}
\nabla_\theta J(\theta) &= \sum_s d(s)\sum_a\pi_\theta(s,a) \nabla_\theta \log\pi_\theta(s,a)\mathcal R_s^a \\\\
&=\mathbb E_{(s,a)\sim \pi_\theta}\Biggr[\nabla_\theta \log\pi_\theta(s,a)\mathcal R_s^a\Biggr]
\end{aligned}
$$

如何将 One-Step MDP PG 算法扩展到 MDP 上？
$$
\mathbb E_{\tau \sim \pi_\theta}\Biggr[\nabla_\theta \log\pi_\theta(s,a) Q^{\pi_\theta}(s,a)\Biggr]
$$

# Reinforce
只能用在 terminate MDP 中。使用 return 来作为 $Q^{\pi_\theta}(s,a)$ 的无偏估计

# Actor-Critic
使用一个 q-value net 来估计 $Q^{\pi_\theta}(s,a)$

# 
