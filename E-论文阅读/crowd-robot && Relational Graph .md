# crowd-robot && Relational Graph Learning

## Crowd-robot

### Attention-based

基于假说：

The interactions (i.e., spatial relations) between humans are important for robot navigation and for predicting future human motion。

两个创新：

1. attentation，（应用别人的）
2. local map 。（一定程度上反映human 之间影响）

![2019-12-25 21-45-56 的屏幕截图](/home/tqr/Study-Notes/E-论文阅读/image/2019-12-25 21-45-56 的屏幕截图-1577281942755.png)

![2019-12-25 21-47-39 的屏幕截图](/home/tqr/Study-Notes/E-论文阅读/image/2019-12-25 21-47-39 的屏幕截图.png)

ei = fe(s;wi;Mi;We);，ei 用来转换成attention score，

hi = yh(ei;Wh);  代表一对交互。



------------------

## Relational Graph

还是基于上一篇的假说，关注点主要在建模human之间的交互上。

主要贡献：使用GCN 建模了human之间的作用。

缺陷：没有考虑图的范围

### GCN

>A variant of GNNs is GCNshttps://zhuanlan.zhihu.com/p/71200936
>
>GCN，图卷积神经网络，跟CNN的作用类似，就是一个特征提取器，只不过它的对象是图数据。GCN精妙地设计了一种从图数据中提取特征的方法，从而让我们可以使用这些特征去对图数据进行**节点分类（node classification）、图分类（graph classification）、边预测（link prediction），还可以顺便得到图的嵌入表示（graph embedding），用途广泛。因此现在人们尝试让GCN到各个领域中发光发热。
>
>假设我们手头有一批图数据，其中有N个节点（node），每个节点都有自己的特征，我们设这些节点的特征组成一个N×D维的矩阵X，然后各个节点之间的关系也会形成一个N×N维的矩阵A，也称为邻接矩阵（adjacency matrix）。X和A便是我们模型的输入。
>
>GCN也是一个神经网络层，它的层与层之间的传播方式是：
>
>![img](/home/tqr/Study-Notes/E-论文阅读/image/v2-94c7d5014d9e9bcf81f630831cf9d9f0_hd.png)
>
>这个公式中：
>
>- A波浪=A+I，I是单位矩阵
>- D波浪是A波浪的度矩阵（degree matrix），公式为 ![[公式]](/home/tqr/Study-Notes/E-论文阅读/image/equation.svg) 
>- H是每一层的特征，对于输入层的话，H就是X
>- σ是非线性激活函数
>
>我们先不用考虑为什么要这样去设计一个公式。我们现在只用知道： ![[公式]](/home/tqr/Study-Notes/E-论文阅读/image/equation.svg) 这个部分，**是可以事先算好的**，因为D波浪由A计算而来，而A是我们的输入之一。
>

### 具体做法：

DRL 模型还和上次一样，

不同的是在S的表示上使用了GCM-----------进而价值预测和“human”动作预测也是基于此衍生而来。

#### Graph modle：

* 用agent为节点，节点之间的关系作为边，建立图。

* 节点状态通过MLP转化为同一纬度，的特征矩阵-----------------------X

* 节点之间的关系用一个近似函数得来，由边得到邻接矩阵-----------A
* 应用GCN通过L曾传播，得到新的-----------------------------------------X-----作为状态输入，此时状态中隐含了各个agent之间的相互作用。

在这个模型的基础上发展出来两个函数 ：

* Fv() 状态价值估计函数
* Fp() 预测human state，这里的价值函数是一个hand-craft function
* 然后做d步的预测，预测多步可以更好的估计转台价值。

训练过程是DQN的过程。

----------------------

----

-----

abstract：

我们的方法reason about 所有agent之间的关系，使用GCN 在agent的状态表示中编码高阶的交互，然后用他们做状态预测和价值估计。

因为可以对human做动作预测，我们可以做一个向前多步的计划而且考虑了人human croded 随时间的变化。

和 state-of-art 的方法做了比较，证明我们的方法更高效，导致更少的碰撞、震荡、冻结。

-----

introduction：

推断复杂动态系统成员之间的关系可以赋予agents决策能力，人群导航问题的挑战在于agent必须保持safe和符合社会规范。

social force 等尽管可以估计humans的轨迹，但是没有使用预测轨迹来inform navigation policy。

有些DRL 的方法呢 即没有利用human之间的interactions 也没有去近似他们。只是通过一些不可靠的假设来简化他们。例如，预测human以线性运动，或者只考虑human当前的状态而不是将他们的预测状态联合考虑进去。

GNN 是建模对象和他们之间关系的工具。他的变种之一：GCN 将nodes之间的关系定义为邻接矩阵。当nodes之间的relation被给出后，“wang”“Grover”等人提出学习这种对象之间的关系并且使用学到的“关注”来计算新的特征。受到这项工作的启发，我们提出了

relational graph learning model 。关系-图学习模型。它推演了agents之间的关系，然后使用GCN 来计算agents之间的交互。拿着这些预测到的human和robot的交互特征，我们联合地来做高效的机器人导航和humans动作预测。

计划和预测multi-agents 的状态是许多问题的基础，包括多个无合作agent的决策问题，我们用relational graph learning 的方法应用于model-based RL，来解决这个问题，它可以预测human 将来的动作，并且同时对robot导航。



related work 

1. rule-based 方法 RVO、ORCA、IGP 严重依赖 手工模型。
2. Social-LSTM 使用LSTM 预测human的动作，但是，预测动作并不能直接产生动作策略。而且以来大量数据集和数据集的好坏
3. 'Cheng' 等人采用DRL，上一篇通过 attentive pooling 有了提高，但是限制在只是部分的建模了crowd interactions。
4. LeTS-Drive 的方法中使用CNN 粗粒度的建模了intentions和interactions

Relational reasonging。

对象的动作变迁可以被编码都GNN 的节点更新函数中。

对象的关系变化可以被编码进边的更新函数中。

但多数现在的GNNs系统需要已知的和固定的图输入，

所以推理实体间的关系并且基于推理学习很重要。

我们的模型在每一个步长动态的猜测 crowded-robot的关系图，学习每个agnet的介于这张图的状态表示。

---

MPC 模型预测控制  是一类控制算法，利用系统模型去预测状态变化，然后接着计划控制。传统的MPC 经常假设有已知的环境---不真实，

MCTS （Monte-Carl Tree Search) 已被用于复杂的搜索问题的即时决策，通过评估虚拟的轨迹的价值。更近来，model-based 的RL先得到一个对于世界的预测模型然后使用这个模型去做决策。我们的预测模型realational graph 采用生数据作为输入，预测human之间的多重交互轨迹。

我们是第一个将relational learning 和model-based DRL 相结合的。

![image-20191229162953281](/home/tqr/Study-Notes/E-论文阅读/image/image-20191229162953281.png)

Approach

先讨论我们怎样用RL 的方法建模导航过程，

然后介绍我们的对于人群交互建模的关系-图学习模型

然后，我们么展示这个模型如何能够被合并进一个简化了的 Monte-Carlo Tree Search 来进行d-step的规划。

RL-

输入的是{robot的状态，其他人i的观测状态，j，k……}  输出的是动作。reward定义。 我们使用一个model-based 的方法来学习一个状态转换模型来预测human的动作。



B---Model Predictive Relational Graph Learning 

将crowd建模为一个graph来说明他们之间的联系，使用GCN来计算human和robot的状态代表。使用这个graph模型作为基础，我们再延伸建立两个模组，fv（）估计当前状态对价值，fp（）预测下一步状态对预测模块

Relational graph learning 

最大的挑战在于学习到一个好的编码了所有agents交互的表达形式。

将crowd和robot 建模为图 G = （V，E） v是节点，E 是边，eij 隐含了agenti 对agentj的注意，或者说agengj对agenti的重要性。

这种成对的关系，由一个近似方程而来，当所有的agents之间的关系都得到后，一个图网络 “繁殖”信息从节点到节点，计算出所有agent的状态的表示。 然后 我们的方法不仅学到了robot对agent的注意力，还需到了humans -humans的attention。除了学到了robot的状态表示还通过信息传递学到了其他agents的状态表示。



这个学习过程包括两个部分，关系推断和交互建模。

关系推断：使用两层 MLPs fr(),fh() 将robot和human不同纬度的状态表示嵌入一个固定长度的向量，。这样产生了一个 特征矩阵 X 。使用近似函数产生成对的关系值，然后产生 邻接矩阵 A = softmax（XWaXt）由此我们得到了特征矩阵X和邻接矩阵A。

交互建模：

有X 和A 使用GCN 来计算成对交互的特征，

H0 = X， 然后在GCN 中 H(l+1) = s(AH(l)W(l))+H(l) ，经过L层的传递，

得到矩阵  Z = H(L)  这时每一行代表了每个agent的编码了局部交互的状态表示。

-------

价值估计模组------------用来估计robot的状态价值，由前面的状态表示 + value network  预测状态价值。

状态预测模组---------用来预测将来的human states ，假设robot的动作会被很好的执行，然后下一个状态可以被计算出来。状态预测模组包含两部分，前面的状态表示，+ motion prediction（动作预测）fm（） 预测他们下一步的状态。 即 当前状态，+ 预测动作==下一步状态，这里的动作预测的reward 使用一个手工的函数。



C  d-step 规划。DRL 解决了短视，然而因为估值函数不完美，和未知的人类行为策略，还是会产生困于局部最优。我们通过预测未来的状态，可以模拟d步，这带来了更好的对状态的估值。同时，通过控制d的深度，可以在计算量上做一个权衡。

与自己前一篇文章相比，不需要去查询环境或者使用现成的轨迹预测去获得human的下一个状态。我们直接用我们的状态预测模组和价值估计模组进行d步的计算，然后选择使d步价值最大的动作。

直觉上来说 价值估计函数 估计了进入一个状态的好坏，使用这个估计，我们递归地搜索接下来的k个状态。当agent进入一个未知的环境中时，多步的估计会得到一个更好的状态估计。