# Towards Optimally Decentralized Multi-Robot Collision Avoidance via Deep Reinforcement Learning

> （sensor-level探测器层面的方法）我们在一个完全去中心化的框架下，输入数据完全是从obboard 收集来的数据。

## Problem Formulation

1. 圈圈代表agent，半径R
2. ot = [ot z, ot g, ot v] 
3. at ∼ πθ(at | ot) ----------at is a velocity
4. Bk ------------is obstacles
5. L as the set of trajectories for all robots, which are subject tothe robot’s kinematic (e.g. non-holonomic) constraints, i.e.:
	L = {li, i = 1, ..., N |
	vt
	i ∼ πθ(at i | ot i),
	pt i = pt i−1 + Δt · vit,
	∀j ∈ [1, N], j = i : pt i − pt j > 2R
	∧ ∀k ∈ [1, M] : pt i − Bk > R
	∧ vit ≤ vimax}.
6. **使平均时间最小**

## Appraoch

部分可观测的马尔科夫模型(S, A, P, R, Ω, O)

S:状态空间

A：动作空间

P：状态转换模型

R：回报函数

Ω：观测空间

O：系统观测状态的概率分布。

因为每个agent的状态转换都发生在一个完全去中心化的方式下，因此 状态转换模型 P 不需要了

### 1 observation space

1. ot = [ot z, ot g, ot v]
2. ot z 包含机关扫描仪最后连续三帧的测量，角度180，范围为4米，512个距离值.实验中扫描仪装在robot的前部而不是中央。
3. otg 是局部极坐标系下的点。
4. otv 包括机器人的平移速度和旋转速度。用训练过程中累计的统计数据减去平均值，除以标准差来标准化。

### 2 Action space

1. 速度包括平移速度和旋转速度 at = [vt, wt]  v ∈ (0.0, 1.0)  w ∈ (−1.0, 1.0)

### 3Reward design

它的reward 是为了 ：

1. avoid collisions
2. 是平均导航时间最小

rti = (gr)t i + (cr)t i + (wr)t i.![2019-12-25 20-59-27 的屏幕截图](/home/tqr/Study-Notes/E-论文阅读/image/2019-12-25 20-59-27 的屏幕截图.png)

## B.Network architecture

![2019-12-25 21-04-08 的屏幕截图](/home/tqr/Study-Notes/E-论文阅读/image/2019-12-25 21-04-08 的屏幕截图.png)

### C.多场景训练

作者说DRL主要关注离散的动作空间，所以这里用了PPO。

πθ 使用从机器人仿真收集来的经验数据。训练过程是ppo的训练过程。

1）训练过程

2）训练场景：多场景的训练有可能提高鲁棒性和质量。

3）阶段训练：两阶段1：20robot noobstacles2：58，rich scenarios

--------------





# DeepMNavigate: Deep Reinforced Multi-Robot Navigation Unifying Local & Global Collision Avoidance

使用 motion information map ，包括 global 、lacal  map使得我们的方法可以利用全局的和局部的信息。

使用三层CNN 使用这些map作为输入，生成合适的动作。

* 和自己上一篇最大的不同是：ot =[ot z; ot g; ot v; ot M ] 多了一个otm

两个map：

* global map，全局坐标系下，每个anget的坐标
* lacal map 局部坐标系下的坐标

两者构建过程相同，以global map为例：

在 H * W 的范围，原点是(H/2 ; W/2 )



-----然后是使用PPO训练，因为存map太占空间，所以用稀疏矩阵来存储。即使这样训练中58个agent，450个动作，还是用了2.5G存储空间，