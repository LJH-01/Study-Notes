Python3

Python 是一个高层次的结合了解释性、编译性、交互性和面向对象的脚本语言。

可扩展性 -- 如果你需要你的一段关键代码运行得更快或者希望某些算法不公开，你可以把你的部分程序用 C 或 C++ 编写，然后在你的 Python 程序中使用它们。

默认情况下，Python 3 源码文件以 **UTF-8** 编码，所有字符串都是 unicode 字符串。



python 中只有int，float，bool和复数



Python中的字符串不可变，索引从左往右从0起始，从右往左从-1起始。没有char，char是长度为1的string，

print(str[a:b]),a和b指的都是索引。

## import 与 from...import

在 python 用 import 或者 from...import 来导入相应的模块。

将整个模块(somemodule)导入，格式为： import somemodule

从某个模块中导入某个函数,格式为： from somemodule import somefunction

从某个模块中导入多个函数,格式为： from somemodule import firstfunc, secondfunc, thirdfunc

将某个模块中的全部函数导入，格式为： from somemodule import *





## 标准数据类型

Python3 中有六个标准的数据类型：

- Number（数字）
- String（字符串）
- List（列表）
- Tuple（元组）
- Set（集合）
- Dictionary（字典）

Python3 的六个标准数据类型中：

- **不可变数据（3 个）：**Number（数字）、String（字符串）、Tuple（元组）；
- **可变数据（3 个）：**List（列表）、Dictionary（字典）、Set（集合）。



数值的除法包含两个运算符：/ 返回一个浮点数，// 返回一个整数。



列表是写在方括号 [] 之间、用逗号分隔开的元素列表。列表同样可以被索引和截取，列表被截取后返回一个包含所需元素的新列表。变量[头下标:尾下标] ，结果包含左侧，不包含右侧--字符串也一样，列表中的元素是可变的



Tuple（元组）

元组tuple和列表类似，不同之处再与阿元组的元素不能修改，元组写在小括号里，用逗号隔开，元组中的类型可以不相同。

集合（set）是由一个或数个形态各异的大小整体组成的



Dictionary（字典）存储键值对

-----------Python3 解释器----

