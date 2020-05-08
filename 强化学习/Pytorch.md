# Pytorch

> * torch 可以将 产生的 tensor 形式数据放在GPU中加速（前提是有合适的GPU），所以在神经网络中用Torch的tensor形式数据最好，numpy array 和tensor 可以方便的转换 
> * Torch 中的 Variable 就是一个存放会变化的值的地理位置. 里面的值（Torch 的Tensor）会不停的变化.
> * Variable 计算时, 它在背景幕布后面一步步默默地搭建着一个庞大的系统, 叫做计算图, computational graph. 将所有的计算步骤 (节点) 都连接起来, 最后进行误差反向传递的时候, 一次性将所有 variable 里面的修改幅度 (梯度) 都计算出来, 而 tensor 就没有这个能力
> * 使用veriable的时候，veriable.data-----转换成tensor 数据，veriable.data.numpy()-转化成numpy数据，matplotlib画图要转换成 numpy数据



# 关于强化学习训练中的术语：

1. batch_size：用minibatch方法时会定义batch_size，即把数据分几份后，每份的大小是多少。
2. iteration：用minibatch时就意味着train完一个batch
3. epoch：one forward pass and one backward pass of all the training examples, in the neural network terminology.即所有训练数据过一遍，如果数据有300条，你把数据分成了3个batch，batch_size是300 / 3 = 100，3个batch都跑完，即跑了三个iteration，就是一个epoch。
4. episode：one a sequence of states, actions and rewards, which ends with terminal state. 如果玩围棋，从头下到尾的一局棋就是一个episode。

# tqdm

Traceback (most recent call last):
  File "/home/tqr/PycharmProjects/SmallPedestrianGroup/test_train/train.py", line 156, in <module>
    main(sys_args)
  File "/home/tqr/PycharmProjects/SmallPedestrianGroup/test_train/train.py", line 120, in main
    trainer.optimize_batch(train_batches, episode)
  File "/home/tqr/PycharmProjects/SmallPedestrianGroup/crowd_nav/modules/trainer.py", line 52, in optimize_batch
    self.data_loader = DataLoader(self.memory, self.batch_size, shuffle=True)
  File "/home/tqr/.local/lib/python3.6/site-packages/torch/utils/data/dataloader.py", line 213, in __init__
    sampler = RandomSampler(dataset)
  File "/home/tqr/.local/lib/python3.6/site-packages/torch/utils/data/sampler.py", line 94, in __init__
    "value, but got num_samples={}".format(self.num_samples))
ValueError: num_samples should be a positive integer value, but got num_samples=0
100%|██████████| 5/5 [02:56<00:00, 35.31s/it]