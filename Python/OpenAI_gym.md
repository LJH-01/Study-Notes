# OpenAIgym

## 使用自己的环境：

1. 继承gym.Env，重写一些方法，编写自己的环境

2. 建立一个环境文件夹env,将定义好的环境放入其中，并在该目录下再新建一个文件\_\_init\_\_.py，文件内容如下：

	````python
	from gym.envs.registration import register
	
	register(
		id = 'env_name-v0' # 环境名,版本号v0必须有
		entry_point = 'env.myenv:MyEnv' # 文件夹名.文件名:类名
		# 根据需要定义其他参数
	)
	````

3. 使用的时候

	```python
	env = gym.make('id')
	env.resert()
	env.render()
	env.step()
	env.render()
	env.colse()
	```
