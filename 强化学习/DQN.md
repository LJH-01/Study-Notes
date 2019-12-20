# DQN

### 1.简介

> * 融合了神经网络和Q-learning
> * 两种方式
> 	1. 将 S+A 作为输入，返回Q
> 	2. 将 S 作为输入，返回所有 [Q]
> * 基于第二种方法： 
> 	1. 在S1通过nn预测Q估计采取A到达S2
> 	2. 在S2根据nn得到对于S3的最大估计，乘以衰减γ，得到Q现实
> 	3. 以学习率α乘以误差，反向传播更新nn
> * 两大利器 Experience reply 和 Fixed Q-target
> 	1. Experience reply：每次 DQN 更新的时候, 随机抽取一些之前的经历进行学习. 这种做法打乱了经历之间的相关性, 使得神经网络更新更有效率.
> 	2.  Fixed Q-targets 也是一种打乱相关性的机理,使用 fixed Q-targets, 需要在 DQN 中使用到两个结构相同但参数不同的神经网络, 预测 Q 估计 的神经网络具备最新的参数, 而预测 Q 现实 的神经网络使用的参数则是很久以前的. 

### 2.搭建练习-平衡车

**gym 模块在pytorch中也可以使用**

1. 神经网络部分：用来实现两个网络：一个是现实网络 (Target Net), 一个是估计网络 (Eval Net).

```python 
class Net(nn.Module):
    def __init__(self, ):
        super(Net, self).__init__()
        self.fc1 = nn.Linear(N_STATES, 10)		# N_STATES 是输入的观测值
        self.fc1.weight.data.normal_(0, 0.1)   	# initialization 权值初始化
        self.out = nn.Linear(10, N_ACTIONS)		# 输出动作
        self.out.weight.data.normal_(0, 0.1)   	# initialization

    def forward(self, x):
        x = self.fc1(x)	
        x = F.relu(x)
        actions_value = self.out(x)
        return actions_value
```

2. DQN体系：我们有两个 net, 有选动作机制, 有存经历机制, 有学习机制.

```python 
# 简化版
class DQN(object):
    def __init__(self):
        # 建立 target net 和 eval net 还有 memory

    def choose_action(self, x):
        # 根据环境观测值选择动作的机制
        return action

    def store_transition(self, s, a, r, s_):
        # 存储记忆

    def learn(self):
        # target 网络更新
        # 学习记忆库中的记忆
```

3. 具体实现

```python
class DQN(object):
    def __init__(self):
        self.eval_net, self.target_net = Net(), Net()

        self.learn_step_counter = 0     # 用于 target 更新计时
        self.memory_counter = 0         # 记忆库记数
        self.memory = np.zeros((MEMORY_CAPACITY, N_STATES * 2 + 2))     # 初始化记忆库
        self.optimizer = torch.optim.Adam(self.eval_net.parameters(), lr=LR)    # torch 的优化器
        self.loss_func = nn.MSELoss()   # 误差公式

    def choose_action(self, x):
        x = torch.unsqueeze(torch.FloatTensor(x), 0)
        # 这里只输入一个 sample
        if np.random.uniform() < EPSILON:   # 选最优动作
            actions_value = self.eval_net.forward(x)
            action = torch.max(actions_value, 1)[1].data.numpy()[0, 0]     # return the argmax
        else:   # 选随机动作
            action = np.random.randint(0, N_ACTIONS)
        return action

    def store_transition(self, s, a, r, s_):
        transition = np.hstack((s, [a, r], s_))
        # 如果记忆库满了, 就覆盖老数据
        index = self.memory_counter % MEMORY_CAPACITY
        self.memory[index, :] = transition
        self.memory_counter += 1

    def learn(self):
        # target net 参数更新
        if self.learn_step_counter % TARGET_REPLACE_ITER == 0:
            self.target_net.load_state_dict(self.eval_net.state_dict())
        self.learn_step_counter += 1

        # 抽取记忆库中的批数据
        sample_index = np.random.choice(MEMORY_CAPACITY, BATCH_SIZE)
        b_memory = self.memory[sample_index, :]
        b_s = torch.FloatTensor(b_memory[:, :N_STATES])
        b_a = torch.LongTensor(b_memory[:, N_STATES:N_STATES+1].astype(int))
        b_r = torch.FloatTensor(b_memory[:, N_STATES+1:N_STATES+2])
        b_s_ = torch.FloatTensor(b_memory[:, -N_STATES:])

        # 针对做过的动作b_a, 来选 q_eval 的值, (q_eval 原本有所有动作的值)
        q_eval = self.eval_net(b_s).gather(1, b_a)  # shape (batch, 1)
        q_next = self.target_net(b_s_).detach()     # q_next 不进行反向传递误差, 所以 detach
        q_target = b_r + GAMMA * q_next.max(1)[0]   # shape (batch, 1)
        loss = self.loss_func(q_eval, q_target)

        # 计算, 更新 eval net
        self.optimizer.zero_grad()
        loss.backward()
        self.optimizer.step()
```

4. 训练过程

```python
dqn = DQN() # 定义 DQN 系统

for i_episode in range(400):
    s = env.reset()
    while True:
        env.render()    # 显示实验动画
        a = dqn.choose_action(s)

        # 选动作, 得到环境反馈
        s_, r, done, info = env.step(a)

        # 修改 reward, 使 DQN 快速学习
        x, x_dot, theta, theta_dot = s_
        r1 = (env.x_threshold - abs(x)) / env.x_threshold - 0.8
        r2 = (env.theta_threshold_radians - abs(theta)) / env.theta_threshold_radians - 0.5
        r = r1 + r2

        # 存记忆
        dqn.store_transition(s, a, r, s_)

        if dqn.memory_counter > MEMORY_CAPACITY:
            dqn.learn() # 记忆库满了就进行学习

        if done:    # 如果回合结束, 进入下回合
            break

        s = s_
```



