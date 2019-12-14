# E4-强化学习-5-Actor Critic

`强化学习`

Actor Critic 演员评判家。他合并了以值为基础（Q learning）和以动作概率为基础（比如 Policy Gradients-梯度原则）两类强化学习算法。

---

原来 Actor-Critic 的 Actor 的前生是Policy Gradients, 这能让它毫不费力地在连续动作中选取合适的动作, Actor Critic 中的 Critic 的前生是 Q-learning 或者其他的 以值为基础的学习法 , 能进行单步更新, 而传统的 Policy Gradients 则是回合更新, 这降低了学习效率.

总结：Actor 前身 Policy Gradients 吸取了它可以在连续动作总选取何时动作的优点。

Critic的前身Q-learning等，吸取了他单步更新的优点。

Actor Critic（Tensor flow）

---

结合了 Policy Gradient (Actor) 和 Function Approximation (Critic) 的方法. Actor 基于概率选行为, Critic 基于 Actor 的行为评判行为的得分, Actor 根据 Critic 的评分修改选行为的概率.

Deep Deterministic Policy Gradient（DDPG）（Tensorflow）

---

它吸收了 [Actor-Critic](https://morvanzhou.github.io/tutorials/machine-learning/ML-intro/4-08-AC/) 让 [Policy gradient](https://morvanzhou.github.io/tutorials/machine-learning/ML-intro/4-07-PG/) 单步更新的精华, 而且还吸收让计算机学会玩游戏的 [DQN](https://morvanzhou.github.io/tutorials/machine-learning/ML-intro/4-06-DQN/) 的精华, 合并成了一种新算法, 叫做 Deep Deterministic Policy Gradient

一句话概括 DDPG: Google DeepMind 提出的一种使用 Actor Critic 结构, 但是输出的不是行为的概率, 而是具体的行为, 用于连续动作 (continuous action) 的预测. DDPG 结合了之前获得成功的 DQN 结构, 提高了 Actor Critic 的稳定性和收敛性.

Asynchronous Advantage Actor-Critic （A3C）

---

一种有效利用计算资源, 并且能提升训练效用的算法, Asynchronous Advantage Actor-Critic, 简称 A3C.

如果使用 A3C 的方法, 我们可以给他们安排去不同的核, 并行运算. 实验结果就是, 这样的计算方式往往比传统的方式快上好多倍

Asynchronous Advantage Actor-Critic（A3C）（Tensorflow）

---

一句话概括 A3C: Google DeepMind 提出的一种解决 Actor-Critic 不收敛问题的算法. 它会创建多个并行的环境, 让多个拥有副结构的 agent 同时在这些并行环境上更新主结构中的参数. 并行中的 agent 们互不干扰, 而主结构的参数更新受到副结构提交更新的不连续性干扰, 所以更新的相关性被降低, 收敛性提高.

A3C 的算法实际上就是将 Actor-Critic 放在了多个线程中进行同步训练. 可以想象成几个人同时在玩一样的游戏, 而他们玩游戏的经验都会同步上传到一个中央大脑. 然后他们又从中央大脑中获取最新的玩游戏方法.

Distributed Proximal Policy Optimization（ 分布式近端策略优化）（DPPO）（Tensor flow）

---

一句话概括 PPO: OpenAI 提出的一种解决 Policy Gradient 不好确定 Learning rate (或者 Step size) 的问题. 因为如果 step size 过大, 学出来的 Policy 会一直乱动, 不会收敛, 但如果 Step Size 太小, 对于完成训练, 我们会等到绝望. PPO 利用 New Policy 和 Old Policy 的比例, 限制了 New Policy 的更新幅度, 让 Policy Gradient 对稍微大点的 Step size 不那么敏感.

PPO 是基于 Actor-Critic 算法
