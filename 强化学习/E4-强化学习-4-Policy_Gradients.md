# E4-强化学习-4-Policy Gradients

`强化学习`

Policy Gradients(梯度原则)

---

Policy Gradients 直接输出动作的最大好处就是, 它能在一个连续区间内挑选动作

Policy Gradients 算法更新（Tensorflow）

---

Policy gradient 是 RL 中另外一个大家族, 他不像 Value-based 方法 (Q learning, Sarsa), 但他也要接受环境信息 (observation), 不同的是他要输出不是 action 的 value, 而是具体的那一个 action, 这样 policy gradient 就跳过了 value 这个阶段. 而且个人认为 Policy gradient 最大的一个优势是: 输出的这个 action 可以是一个连续的值, 之前我们说到的 value-based 方法输出的都是不连续的值, 然后再选择值最大的 action. 而 policy gradient 可以在一个连续分布上选取 action.

Policy Gradients 思维决策（Tensorflow）

---
