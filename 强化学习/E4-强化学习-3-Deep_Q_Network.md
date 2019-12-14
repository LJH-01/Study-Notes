# E4-强化学习-3-Deep Q Network

`强化学习`

Deep Q Network 是一种融合了神经网络和Q Learning的方法。

---

传统的表格形式的强化学习有这样一个瓶颈. 当问题太复杂, 状态很多 ，如果全用表格来存储它们, 我们的计算机内存不够, 而且每次搜索对应的状态也是一件很耗时的事.

利用神经网络生成Q值，再用Q learning 的原则，

---

DQN两大利器

---

Experience replay 和 Fixed Q-targets.

有了这两种提升手段, DQN 才能在一些游戏中超越人类.

DQN算法更新（Tensorflow）

---

![9198e2cf4968e0946aba4bceba187455.jpg](image/9198e2cf4968e0946aba4bceba187455.jpg)

也就是个 Q learning 主框架上加了些装饰.

这些装饰包括:

* 记忆库 (用于重复学习)

* 神经网络计算 Q 值

* 暂时冻结 q_target 参数 (切断相关性)

---

DQN神经网络（Tensorflow）

---

TensorFlow™是一个基于数据流编程（dataflow programming）的符号数学系统，被广泛应用于各类机器学习（machine learning）算法的编程实现，其前身是谷歌的神经网络算法库DistBelief

使用Tensorflow搭建神经网络。

推荐搭建两个神经网络， target_net 用于预测 q_target 值, 他不会及时更新参数. eval_net 用于预测 q_eval, 这个神经网络拥有最新的神经网络参数. 不过这两个神经网络结构是完全一样的, 只是里面的参数不一样.

---

DQN思维决策（Tensorflow）

---

DQN 的精髓部分之一: 记录下所有经历过的步, 这些步可以进行反复的学习, 所以是一种 off-policy 方法

---

OpenAI gym环境库（选学教程）

---

手动编环境是一件很耗时间的事情, 所以如果有能力使用别人已经编好的环境, 可以节约我们很多时间. OpenAI gym 就是这样一个模块, 他提供了我们很多优秀的模拟环境.

---

Duble DQN （Tensorflow）（选学教程）

---

DQN 基于 Q-learning, Q-Learning 中有 Qmax, Qmax 会导致 Q现实 当中的过估计 (overestimate). 而 Double DQN 就是用来解决过估计的.

我们知道 DQN 的神经网络部分可以看成一个 最新的神经网络 + 老神经网络, 他们有相同的结构, 但内部的参数更新却有时差

---

Prioritized Experience Replay (DQN) (Tensorflow)（选学教程）

---

---

Dueling DQN（Tensor flow）（选学教程）

---
