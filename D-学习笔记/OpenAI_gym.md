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

## python \_\_ init\_\_.py 文件的使用

1. 作用

	1. Python中package的标识不能删除
	2. 定义\_\_all\_\_用来模糊导入
	3. 编写Python 通常不建议在\_\_init\_\_中编写培养透红模块，保持尽量简单

2. __init__.py 文件的作用是将文件夹变为一个Python模块,Python 中的每个模块的包中，都有__init__.py 文件。通常__init__.py 文件为空，但是我们还可以为它增加其他的功能。我们在导入一个包时，实际上是导入了它的__init__.py文件。这样我们可以在__init__.py文件中批量导入我们所需要的模块，而不再需要一个一个的导入。

3. 主要控制包的导入行为，为什么需要__init__.py 
	__init__.py文件用于组织包（package）。这里首先需要明确包（package）的概念。什么是包（package）？简单来说，包是含有python模块的文件夹。一个python模块（module）为一个py文件，里面写有函数和类。包（package）是为了更好的管理模块（module）,相当于多个模块的父节点。

	当文件夹下有__init__.py时，表示当前文件夹是一个package，其下的多个module统一构成一个整体。这些module都可以通过同一个package引入代码中。

	__init__.py文件怎么写 
	可以什么都不写，但如果想使用`from package1 import *`这种写法的话，需要在__init__.py中加上：

	```pyton
	__all__ = ['file1','file2'] #package1下有file1.py,file2.py
	
	当我们导入这个包的时候，__init__.py文件自动运行
	```

## python logging模块使用

### 1 简单使用

默认情况下，logging模块将日志打印到屏幕上(stdout)，日志级别为WARNING(即只有日志级别高于WARNING的日志信息才会输出)，日志格式如下图所示：

![https://upload-images.jianshu.io/upload_images/477558-da69f58ffd67989c.png](/home/tqr/Study-Notes/D-学习笔记/image/477558-da69f58ffd67989c.png)

![2020-01-06 10-17-22 的屏幕截图](/home/tqr/Study-Notes/D-学习笔记/image/2020-01-06 10-17-22 的屏幕截图.png) 

### 2.配置日志级别

```python
#!/usr/local/bin/python
# -*- coding:utf-8 -*-
import logging

# 通过下面的方式进行简单配置输出方式与日志级别
logging.basicConfig(filename='logger.log', level=logging.INFO)

logging.debug('debug message')
logging.info('info message')
logging.warn('warn message')
logging.error('error message')
logging.critical('critical message')
```

标准输出(屏幕)未显示任何信息，发现当前工作目录下生成了logger.log，内容如下：

> INFO:root:info message
>  WARNING:root:warn message
>  ERROR:root:error message
>  CRITICAL:root:critical message

### 3.几个重要概念

* Logger 记录器，应用程序代码能直接使用的接口。
* Handler 处理器，将（记录器产生的）日志记录发送至合适的目的地。
* Filter 过滤器，提供了更好的粒度控制，它可以决定输出哪些日志记录。
* Formatter 格式化器，指明了最终输出中日志记录的布局

#### Logger 记录器

Logger是一个树形层级结构，在使用接口debug，info，warn，error，critical之前必须创建Logger实例，即创建一个记录器，如果没有显式的进行创建，则默认创建一个root logger，并应用默认的日志级别(WARN)，处理器Handler(StreamHandler，即将日志信息打印输出在标准输出上)，和格式化器Formatter(默认的格式即为第一个简单使用程序中输出的格式)。

> 创建方法: `logger = logging.getLogger(logger_name)`

创建Logger实例后，可以使用以下方法进行日志级别设置，增加处理器Handler。

- logger.setLevel(logging.ERROR)  # 设置日志级别为ERROR，即只有日志级别大于等于ERROR的日志才会输出
- logger.addHandler(handler_name)  # 为Logger实例增加一个处理器
- logger.removeHandler(handler_name)   # 为Logger实例删除一个处理器

#### Handler 处理器

Handler处理器类型有很多种，比较常用的有三个，**StreamHandler**，**FileHandler**，**NullHandler**，详情可以访问[Python logging.handlers](https://links.jianshu.com/go?to=http%3A%2F%2Fpython.usyiyi.cn%2Fpython_278%2Flibrary%2Flogging.handlers.html%23)

创建StreamHandler之后，可以通过使用以下方法设置日志级别，设置格式化器Formatter，增加或删除过滤器Filter。

- ch.setLevel(logging.WARN)  # 指定日志级别，低于WARN级别的日志将被忽略
- ch.setFormatter(formatter_name)  # 设置一个格式化器formatter
- ch.addFilter(filter_name)  # 增加一个过滤器，可以增加多个
- ch.removeFilter(filter_name)  # 删除一个过滤器

##### StreamHandler

> 创建方法: `sh = logging.StreamHandler(stream=None)`

##### FileHandler

> 创建方法: `fh = logging.FileHandler(filename, mode='a', encoding=None, delay=False)`

##### NullHandler

NullHandler类位于核心logging包，不做任何的格式化或者输出。
 本质上它是个“什么都不做”的handler，由库开发者使用。

#### Formatter 格式化器

使用Formatter对象设置日志信息最后的规则、结构和内容，默认的时间格式为%Y-%m-%d %H:%M:%S。

> 创建方法: `formatter = logging.Formatter(fmt=None, datefmt=None)`

其中，fmt是消息的格式化字符串，datefmt是日期字符串。如果不指明fmt，将使用'%(message)s'。如果不指明datefmt，将使用ISO8601日期格式。

#### Filter 过滤器

Handlers和Loggers可以使用Filters来完成比级别更复杂的过滤。Filter基类只允许特定Logger层次以下的事件。例如用‘A.B’初始化的Filter允许Logger ‘A.B’, ‘A.B.C’, ‘A.B.C.D’, ‘A.B.D’等记录的事件，logger‘A.BB’, ‘B.A.B’ 等就不行。 如果用空字符串来初始化，所有的事件都接受。

> 创建方法: `filter = logging.Filter(name='')`

以下是相关概念总结:

> 熟悉了这些概念之后，有另外一个比较重要的事情必须清楚，即**Logger是一个树形层级结构**;
>  Logger可以包含一个或多个Handler和Filter，即Logger与Handler或Fitler是一对多的关系;
>  一个Logger实例可以新增多个Handler，一个Handler可以新增多个格式化器或多个过滤器，而且日志级别将会继承。

![img](https:////upload-images.jianshu.io/upload_images/477558-467ca16aae66770f.jpg?imageMogr2/auto-orient/strip|imageView2/2/w/459)

element_relation.jpg

### Logging工作流程

#### logging模块使用过程

1. 第一次导入logging模块或使用reload函数重新导入logging模块，logging模块中的代码将被执行，这个过程中将产生logging日志系统的默认配置。
2. 自定义配置(可选)。logging标准模块支持三种配置方式: dictConfig，fileConfig，listen。其中，dictConfig是通过一个字典进行配置Logger，Handler，Filter，Formatter；fileConfig则是通过一个文件进行配置；而listen则监听一个网络端口，通过接收网络数据来进行配置。当然，除了以上集体化配置外，也可以直接调用Logger，Handler等对象中的方法在代码中来显式配置。
3. 使用logging模块的全局作用域中的getLogger函数来得到一个Logger对象实例(其参数即是一个字符串，表示Logger对象实例的名字，即通过该名字来得到相应的Logger对象实例)。
4. 使用Logger对象中的debug，info，error，warn，critical等方法记录日志信息。

#### logging模块处理流程

![img](https:////upload-images.jianshu.io/upload_images/477558-a099cc71d0a4c453.png?imageMogr2/auto-orient/strip|imageView2/2/w/955)

logging_flow.png

1. 判断日志的等级是否大于Logger对象的等级，如果大于，则往下执行，否则，流程结束。
2. 产生日志。第一步，判断是否有异常，如果有，则添加异常信息。第二步，处理日志记录方法(如debug，info等)中的占位符，即一般的字符串格式化处理。
3. 使用注册到Logger对象中的Filters进行过滤。如果有多个过滤器，则依次过滤；只要有一个过滤器返回假，则过滤结束，且该日志信息将丢弃，不再处理，而处理流程也至此结束。否则，处理流程往下执行。
4. 在当前Logger对象中查找Handlers，如果找不到任何Handler，则往上到该Logger对象的父Logger中查找；如果找到一个或多个Handler，则依次用Handler来处理日志信息。但在每个Handler处理日志信息过程中，会首先判断日志信息的等级是否大于该Handler的等级，如果大于，则往下执行(由Logger对象进入Handler对象中)，否则，处理流程结束。
5. 执行Handler对象中的filter方法，该方法会依次执行注册到该Handler对象中的Filter。如果有一个Filter判断该日志信息为假，则此后的所有Filter都不再执行，而直接将该日志信息丢弃，处理流程结束。
6. 使用Formatter类格式化最终的输出结果。 注：Formatter同上述第2步的字符串格式化不同，它会添加额外的信息，比如日志产生的时间，产生日志的源代码所在的源文件的路径等等。
7. 真正地输出日志信息(到网络，文件，终端，邮件等)。至于输出到哪个目的地，由Handler的种类来决定。

> 注：以上内容摘抄自第三条参考资料，内容略有改动，转载特此声明。

### 再看日志配置

#### 配置方式

- 显式创建记录器Logger、处理器Handler和格式化器Formatter，并进行相关设置；
- 通过简单方式进行配置，使用[basicConfig()](https://links.jianshu.com/go?to=http%3A%2F%2Fpython.usyiyi.cn%2Fpython_278%2Flibrary%2Flogging.html%23logging.basicConfig)函数直接进行配置；
- 通过配置文件进行配置，使用[fileConfig()](https://links.jianshu.com/go?to=http%3A%2F%2Fpython.usyiyi.cn%2Fpython_278%2Flibrary%2Flogging.config.html%23logging.config.fileConfig)函数读取配置文件；
- 通过配置字典进行配置，使用[dictConfig()](https://links.jianshu.com/go?to=http%3A%2F%2Fpython.usyiyi.cn%2Fpython_278%2Flibrary%2Flogging.config.html%23logging.config.dictConfig)函数读取配置信息；
- 通过网络进行配置，使用[listen()](https://links.jianshu.com/go?to=http%3A%2F%2Fpython.usyiyi.cn%2Fpython_278%2Flibrary%2Flogging.config.html%23logging.config.listen)函数进行网络配置。

#### basicConfig关键字参数

| 关键字   |                             描述                             |
| -------- | :----------------------------------------------------------: |
| filename | 创建一个FileHandler，使用指定的文件名，而不是使用StreamHandler。 |
| filemode | 如果指明了文件名，指明打开文件的模式（如果没有指明filemode，默认为'a'）。 |
| format   |               handler使用指明的格式化字符串。                |
| datefmt  |                  使用指明的日期／时间格式。                  |
| level    |                     指明根logger的级别。                     |
| stream   | 使用指明的流来初始化StreamHandler。该参数与'filename'不兼容，如果两个都有，'stream'被忽略。 |

##### 有用的format格式

| 格式           |          描述          |
| -------------- | :--------------------: |
| %(levelno)s    |   打印日志级别的数值   |
| %(levelname)s  |    打印日志级别名称    |
| %(pathname)s   | 打印当前执行程序的路径 |
| %(filename)s   |  打印当前执行程序名称  |
| %(funcName)s   |   打印日志的当前函数   |
| %(lineno)d     |   打印日志的当前行号   |
| %(asctime)s    |     打印日志的时间     |
| %(thread)d     |       打印线程id       |
| %(threadName)s |      打印线程名称      |
| %(process)d    |       打印进程ID       |
| %(message)s    |      打印日志信息      |

##### 有用的datefmt格式

参考[time.strftime](https://links.jianshu.com/go?to=https%3A%2F%2Fdocs.python.org%2F2%2Flibrary%2Ftime.html%3Fhighlight%3Dstrftime%23time.strftime)

#### 配置示例

##### 显式配置

使用程序logger.py如下:

```
# -*- encoding:utf-8 -*-
import logging

# create logger
logger_name = "example"
logger = logging.getLogger(logger_name)
logger.setLevel(logging.DEBUG)

# create file handler
log_path = "./log.log"
fh = logging.FileHandler(log_path)
fh.setLevel(logging.WARN)

# create formatter
fmt = "%(asctime)-15s %(levelname)s %(filename)s %(lineno)d %(process)d %(message)s"
datefmt = "%a %d %b %Y %H:%M:%S"
formatter = logging.Formatter(fmt, datefmt)

# add handler and formatter to logger
fh.setFormatter(formatter)
logger.addHandler(fh)

# print log info
logger.debug('debug message')
logger.info('info message')
logger.warn('warn message')
logger.error('error message')
logger.critical('critical message')```

##### 文件配置
配置文件logging.conf如下:
​```[loggers]
keys=root,example01

[logger_root]
level=DEBUG
handlers=hand01,hand02

[logger_example01]
handlers=hand01,hand02
qualname=example01
propagate=0

[handlers]
keys=hand01,hand02

[handler_hand01]
class=StreamHandler
level=INFO
formatter=form02
args=(sys.stderr,)

[handler_hand02]
class=FileHandler
level=DEBUG
formatter=form01
args=('log.log', 'a')

[formatters]
keys=form01,form02

[formatter_form01]
format=%(asctime)s %(filename)s[line:%(lineno)d] %(levelname)s %(message)s```

使用程序logger.py如下:
​```#!/usr/bin/python
# -*- encoding:utf-8 -*-
import logging
import logging.config

logging.config.fileConfig("./logging.conf")

# create logger
logger_name = "example"
logger = logging.getLogger(logger_name)

logger.debug('debug message')
logger.info('info message')
logger.warn('warn message')
logger.error('error message')
logger.critical('critical message')```

##### 字典配置
有兴趣的童靴可以使用```logging.config.dictConfig(config)```编写一个示例程序发给我，以提供给我进行完善本文。

##### 监听配置
有兴趣的童靴可以使用```logging.config.listen(port=DEFAULT_LOGGING_CONFIG_PORT)```编写一个示例程序发给我，以提供给我进行完善本文。

更多详细内容参考[logging.config日志配置](http://python.usyiyi.cn/python_278/library/logging.config.html#module-logging.config)

### 参考资料
* [英文Python logging HOWTO](https://docs.python.org/2/howto/logging.html#logging-basic-tutorial)
* [中文Python 日志 HOWTO](http://python.usyiyi.cn/python_278/howto/logging.html#logging-basic-tutorial)
* [Python日志系统Logging](http://www.52ij.com/jishu/666.html)
* [logging模块学习笔记：basicConfig配置文件](http://www.cnblogs.com/bjdxy/archive/2013/04/12/3016820.html)
* 其他一些前辈博客相关文章
```

# matplotlib 绘图

https://blog.csdn.net/matrix_laboratory/article/details/50698239

# python 正负无穷大

Python中可以用如下方式表示正负无穷：

```
float("inf"), 
float("-inf")
```

# python argparse模块

argparse是python用于解析命令行参数和选项的标准模块，作用是用于解析命令行参数

使用方法：

  1、导入argparse模块  import argparse

  2、创建argparse对象  parser = argparse.ArgumentParser()

  3、添加命令行相关参数、选项  parser.add_argument("...")

  4、解析   parser.parse_args()



```
parser.add_argument('--c', action='store_true', default=false)
python test.py --c ---------- c是true（因为action）
python test.py     ---------- c是false（default）
```

-----

# os 和sys的区别

➤os

 

  os: This module provides a portable way of using operating system dependent functionality.

  这个模块提供了一种方便的**使用操作系统函数的方法。**

➤sys

   sys: This module provides access to some variables used or maintained  by the interpreter and to functions that interact strongly with the  interpreter.

  这个模块可供访问由解释器使用或维护的变量和与**解释器进行交互的函数。**

➤总结

  os模块负责程序与操作系统的交互，提供了访问操作系统底层的接口;sys模块负责程序与python解释器的交互，提供了一系列的函数和变量，用于操控python的运行时环境。

------

# shutil 主要用作文件拷贝用

## python 括号

( )小括号表示元组，元组时不可变的类型

[ ] 中括号表示list 列表类型数据

{ } 大括号表示字典类型数据 



# 关于pycharm导入包的问题

如果步是在pycharm中建立的包，那么之前的导入方法都适用，很顺畅。

在pycharm中，建的文件不会默认添加到包搜索路径中， 这样一来结果是在python console中可以运行，即直接点击run 即可运行，但是通过命令行运行会报错说找不到模块

因为pydev在运行时会把当前工程的所有文件夹路径都作为包的搜索路径，而命令行默认只是搜索当前路径。

那么在当前文件夹以外的文件自然找不到。