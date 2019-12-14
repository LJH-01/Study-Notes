# linux

**卸载软件**
### 删除软件及其配置文件
sudo apt-get --purge remove <package>
### 删除没用的依赖包
sudo apt-get autoremove <package>
### 此时dpkg的列表中有“rc”状态的软件包，可以执行如下命令做最后清理：
sudo dpkg -l |grep ^rc|awk '{print $2}' |sudo xargs dpkg -P

---

**安装软件**

sudo apt install packge-name

sudo apt -f install packge-name  # 修复安装，如果遇到安装失败，或者报错可以尝试用这个命令

sudo apt-cache depends packge-name # 查看软件的依赖包

sudo apt-cache packge-name  # 搜索包

---

**wps中文输入光标不跟随解决方案**

[https://www.bbsmax.com/A/MAzAn7ZOJ9/](https://www.bbsmax.com/A/MAzAn7ZOJ9/)

sudo apt-get install qt4-qtconfig 

sudo apt-get install ibus-qt4

---

Linux查看进程的命令：

**ps命令——查看静态的进程统计信息**（一般结合选项使用 ps aux 或 ps -elf 命令）

建议使用 ps -elf 查询，输出的信息更详细些，包括 PPID (对应的父进程 的PID 号)

**top 命令——查看进程动态信息**（以全屏交互式的界面显示进程排名，及时跟踪系统资源占用情况

`终止进程执行`使用

kill 命令终止进程的命令格式： `kill PID号` 如果无法响应终止信号，可以结合 -9 选项： kill -9 PID号（-9表示强制终止进程，但强制终止会导致程序运行的部分数据丢失，应谨慎使用）

---

ubuntu删除无用图标

1.看/usr/share/applications下是否有xxx.desktop

2.可以到～/.local/share/applications下看是否有xxx.desktop

---

### 卸载IDEA 

* 删除主程序目录，也就是我们本文上面讲的解压出来的 `idea-IU-141.1532.4`。
* 如果不想保留你的配置文件，还可以删除配置目录，目录所在位置：ls -a  ;     sudo rm -rf ./Idea...

---

安装和破解 IDEA  截至 2019年11月30日，目前找到的破解包最多能破到 idea2019.2.4

1.下载  tar.gz包。

2.解压 tar.gz包 ------------ -------`sudo tar -zxvf ideaIU-2019.2.4.tar.gz`

`3.放  /opt 目录下----------------sudo mv ideaIU-2019.2.4 /opt`````````````

`4.赋予权限  ---------------------sudo chmod 755 -R `ideaIU-2019.3/``

5.下载 破解包，见于我的网盘  JetbrainsCrack.jar

6.我将这个jar包放进 我的ieda安装目录的bin目录下面，（实际上放到哪里都可以，放这里最好了，至少不会误删）/opt/ideaIU-2019.2.4/idea-IU-192.7142.36/bin

7. bin目录下 ./idea.sh 启动idea，一路按照自己意愿来选即可，（后期都可以改）关键一点！！！：在激活步骤选择 试用30天的选项

8. 进入idea 修改配置文件：点击 "Help" -> "Edit Custom VM Options ..."，如果提示是否要创建文件，请点"是 |Yes"。在打开的 vmoptions 编辑窗口末行添加：`-javaagent:JetbrainsCrack.jar文件的绝对路径`，

如我的路径: `````-javaagent:/opt/ideaIU-2019.2.4/idea-IU-192.7142.36/bin/JetbrainsCrack.jar`

mac: -javaagent:/Users/neo/JetbrainsCrack.jar

linux: -javaagent:/home/neo/JetbrainsCrack.jar

windows: -javaagent:C:\Users\neo\JetbrainsCrack.jar

`9.重启idea  进入后 点击：`Help" -> “Register”，选择 License server 方式，地址填入：`http://jetbrains-license-server` （应该会自动填上） 看到 Licensed to 用户名，即激活成功

安装过程一路顺利是最好，万一遇到“意外”，分享几个我踩过的坑：

1。真的安装错了，一塌糊涂，决定要重装了：几个步骤：

* 删掉安装目录，例如我是在 /opt 文件夹下，  就 在/opt文件夹下   sudo rm -rf  ideaIU-2019.2.4/

* 删掉图标：到这两个目录下找图标删除之：/usr/share/applications下是否有xxx.desktop       .local/share/applications下看是否有xxx.desktop

* 删掉配置文件： 在主目录下 ls -a  显示隐藏文件夹，找到  类似   .IntelliJIdea2019.2 这样的文件夹，删除掉,这样彻底将idea删除干净。

---
