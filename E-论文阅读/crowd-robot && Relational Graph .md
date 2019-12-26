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

对我有用的：1.