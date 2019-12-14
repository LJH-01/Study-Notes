# Pytorch

----torch 可以将 产生的 tensor 形式数据放在GPU中加速（前提是有合适的GPU），所以在神经网络中用Torch的tensor形式数据最好，numpy array 和tensor 可以方便的转换 

----Torch 中的 Variable 就是一个存放会变化的值的地理位置. 里面的值（Torch 的Tensor）会不停的变化.

-----Variable 计算时, 它在背景幕布后面一步步默默地搭建着一个庞大的系统, 叫做计算图, computational graph. 将所有的计算步骤 (节点) 都连接起来, 最后进行误差反向传递的时候, 一次性将所有 variable 里面的修改幅度 (梯度) 都计算出来, 而 tensor 就没有这个能力

----使用veriable的时候，veriable.data-----转换成tensor 数据，veriable.data.numpy()---转化成numpy数据，matplotlib画图要转换成 numpy数据

----激励函数 是为了解决我们日常生活中 不能用线性方程所概括的问题 .就是让神经网络可以描述非线性问题的步骤
