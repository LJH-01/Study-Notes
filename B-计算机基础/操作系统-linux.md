# 操作系统-linux

[https://man.linuxde.net/](https://man.linuxde.net/)专门查询Linux命令的网站

[https://www.cnblogs.com/fnlingnzb-learner/p/5831284.html](https://www.cnblogs.com/fnlingnzb-learner/p/5831284.html)//常用命令大全

[https://blog.csdn.net/vblittleboy/article/details/8103264](https://blog.csdn.net/vblittleboy/article/details/8103264)//chomd 777

系统信息 

arch  显示机器的处理器架构(1) 

date  显示系统日期 

关机 (系统的关机、重启以及登出 ) 

shutdown -h now 关闭系统(1) 

shutdown -r now 重启(1) 

reboot 重启(2) 

logout 注销 

文件和目录 

cd /home 进入 '/ home' 目录' 

cd .. 返回上一级目录 

pwd 显示工作路径 

ls 查看目录中的文件 

tree 显示文件和目录由根目录开始的树形结构(1) 

lstree 显示文件和目录由根目录开始的树形结构(2) 

mkdir dir1 创建一个叫做 'dir1' 的目录' 

rm -f file1 删除一个叫做 'file1' 的文件' 

rmdir dir1 删除一个叫做 'dir1' 的目录' 

mv dir1 new_dir 重命名/移动 一个目录 

cp file1 file2 复制一个文件 

文件搜索 

find /home/user1 -name \*.bin 在目录 '/ home/user1' 中搜索带有'.bin' 结尾的文件 

whereis halt 显示一个二进制文件、源码或man的位置 

挂载一个文件系统 （mount/umount）

mount /dev/hda2 /mnt/hda2 挂载一个叫做hda2的盘 - 确定目录 '/ mnt/hda2' 已经存在 

umount /dev/hda2 卸载一个叫做hda2的盘 - 先从挂载点 '/ mnt/hda2' 退出 

磁盘空间 (df/du)

df -h 显示已经挂载的分区列表 

du -sh dir1 估算目录 'dir1' 已经使用的磁盘空间' 

du -sk * | sort -rn 以容量大小为依据依次显示文件和目录的大小 

用户和群组 (group/user)

groupadd group_name 创建一个新用户组 

groupdel group_name 删除一个用户组 

useradd user1 创建一个新用户 

userdel -r user1 删除一个用户 ( '-r' 排除主目录) 

passwd 修改口令 

文件的权限 - 使用 "+" 设置权限，使用 "-" 用于取消 ----------chmod

ls -lh 显示权限 

ls /tmp | pr -T5 -W$COLUMNS 将终端划分成5栏显示 

chmod ugo+rwx directory1 设置目录的所有人(u)、群组(g)以及其他人(o)以读（r ）、写(w)和执行(x)的权限 

chmod go-rwx directory1 删除群组(g)与其他人(o)对目录的读写执行权限 

chmod 7 含义：rwx分别对应读写执行，4，2，1--7就是全部权限

文件的特殊属性 - 使用 "+" 设置权限，使用 "-" 用于取消 -----------chattr

chattr +a file1 只允许以追加方式读写文件 

chattr +i file1 设置成不可变的文件，不能被删除、修改、重命名或者链接 

lsattr 显示特殊的属性 

打包和压缩文件 ---------bizp2,bunzip2/gzip,guzip/rar a ,rar x/ tar

tar只是一个打包工具，不负责压缩, 压缩是由gzip与bzip2来实现的。.bz2和.gz的区别在于前者比后者压缩率更高，后者比前者花费更少的时间。gzip的压缩速度是最快的

bunzip2 file1.bz2 解压一个叫做 'file1.bz2'的文件 

bzip2 file1 压缩一个叫做 'file1' 的文件 

gunzip file1.gz 解压一个叫做 'file1.gz'的文件 

gzip file1 压缩一个叫做 'file1'的文件 

gzip -9 file1 最大程度压缩 

tar -cvf archive.tar file1 创建一个非压缩的 tarball 

RPM 包 - （Fedora, Redhat及类似系统） (rpm-软件包管理器)

YUM 软件包升级器 - （Fedora, RedHat及类似系统） 

DEB 包 (Debian, Ubuntu 以及类似系统) 

APT 软件工具 (Debian, Ubuntu 以及类似系统) 

rpm -ivh package.rpm 安装一个rpm包 

yum install package_name 下载并安装一个rpm包 

dpkg -i package.deb 安装/更新一个 deb 包 

apt-get install package_name 安装/更新一个 deb 包 

文本处理 

cat主要三大功能：

1.一次显示整个文件。 cat filename

2.从键盘创建一个文件。 cat > filename  只能创建新文件,不能编辑已有文件.

3.将几个文件合并为一个文件： $cat file1 file2 > file

grep命令是一种强大的文本搜索工具，它能使用正则表达式搜索文本

sed 是 stream editor 的缩写，中文称之为“流编辑器”。是一个面向行处理的工具，它以“行”为处理单位，针对每一行进行处理，处理后的结果会输出到标准输出（STDOUT）。它不会对读取的文件做任何贸然的修改，而是将内容都输出到标准输出中。

echo命令, 在shell编程中极为常用, 在终端下打印变量value的时候也是常常用到的,主要功能是在显示器上显示一段文字，一般起到一个提示的作用。

paste该命令主要用来将多个文件的内容合并，与cut命令完成的功能刚好相反。

paste file1 file2 合并两个文件或两栏的内容 

sort file1 file2 排序两个文件的内容 

comm命令可以用于两个文件之间的比较

comm -1 file1 file2 比较两个文件的内容只删除 'file1' 所包含的内容 

comm -2 file1 file2 比较两个文件的内容只删除 'file2' 所包含的内容 

comm -3 file1 file2 比较两个文件的内容只删除两个文件共有的部分 

备份 

dump -0aj -f /tmp/home0.bak /home 制作一个 '/home' 目录的完整备份 

dump -1aj -f /tmp/home0.bak /home 制作一个 '/home' 目录的交互式备份 

网络 - （以太网和WIFI无线） 

ifconfig eth0 显示一个以太网卡的配置 

---

自旋锁与互斥锁有点类似，只是自旋锁不会引起调用者睡眠，如果自旋锁已经被别的执行单元保持，调用者就一直循环在那里看是否该自旋锁的保持者已经释放了锁，"自旋"一词就是因此而得名。

Linux线程

LINUX线程的实现就是基于核心轻量级进程的”一对一”线程模型，一个线程实体对应一个核心轻量级进程，而线程之间的管理在核外函数库中实现。

进程和线程

（1）进程是资源的分配和调度的一个独立单元，而线程是CPU调度的基本单元

（2）同一个进程中可以包括多个线程，并且线程共享整个进程的资源（寄存器、堆栈、上下文），一个进程至少包括一个线程。

（3）进程的创建调用fork或者vfork，而线程的创建调用pthread_create，进程结束后它拥有的所有线程都将销毁，而线程的结束不会影响同个进程中的其他线程的结束

（4）线程是轻两级的进程，它的创建和销毁所需要的时间比进程小很多，所有操作系统中的执行功能都是创建线程去完成的

（5）线程中执行时一般都要进行同步和互斥，因为他们共享同一进程的所有资源

（6）线程有自己的私有属性PCB，线程id，寄存器、硬件上下文，而进程也有自己的私有属性进程控制块TCB，这些私有属性是不被共享的，用来标示一个进程或一个线程的标志

---

内存碎片

内部碎片就是已经被分配出去（能明确指出属于哪个进程）却不能被利用的内存空间；

外部碎片指的是还没有被分配出去（不属于任何进程），但由于太小了无法分配给申请内存空间的新进程的内存空闲区域。

---

进程同步——睡眠理发师----读者写者----哲学家进餐

---

产生死锁的必要条件

互斥条件：进程要求对所分配的资源进行排它性控制，即在一段时间内某资源仅为一进程所占用。

请求和保持条件：当进程因请求资源而阻塞时，对已获得的资源保持不放。

不剥夺条件：进程已获得的资源在未使用完之前，不能剥夺，只能在使用完时由自己释放。

环路等待条件：在发生死锁时，必然存在一个进程--资源的环形链。

解决死锁的方法

资源一次性分配：一次性分配所有资源，这样就不会再有请求了：（破坏请求条件）

只要有一个资源得不到分配，也不给这个进程分配其他的资源：（破坏请求保持条件）

可剥夺资源：即当某进程获得了部分资源，但得不到其它资源，则释放已占有的资源（破坏不可剥夺条件）

资源有序分配法：系统给每类资源赋予一个编号，每一个进程按编号递增的顺序请求资源，释放则相反（破坏环路等待条件）

---

使用apt-get更新包数据库                   `sudo apt-get update`

使用apt-get升级已安装的软件包         s`udo apt-get upgrade`

`区别：`

apt-get update:仅更新包的数据库。例如，如果安装了XYX软件包版本1.3，则在apt-get update之后，数据库将知道有更新的版本1.4可用。

apt-get upgrade:当您在apt-get update之后进行apt-get升级时，它会将已安装的软件包升级（或更新，无论您喜欢哪个术语）到新版本。

这就是为什么更新Ubuntu的最快和最方便的方法是使用此命令： 
```
sudo apt-get update && sudo apt-get upgrade -y
```

```
使用apt-get安装新软件包----------重复无害，install一个已经安装的软件包无害
```

如果知道包的名称，可以使用以下命令轻松安装它：`sudo apt-get install <package_name>`

使用apt-get删除已安装的软件包：sudo apt-get remove <package>

``卸载软件包的另一种方法是使用清除：`sudo apt-get purge <package_name>```

#### `有什么区别？`

``````````````

```

```

````* `apt-get remove只删除包的二进制文件。它不会触及配置文件`
* `apt-get purge删除与包相关的所有内容，包括配置文件`
```因此，如果您“删除”特定软件并再次安装，您的系统将具有相同的配置文件。当然，再次安装时，系统会要求您覆盖现有的配置文件。`

```当你搞砸了程序的配置时，清除特别有用。您希望从系统中完全擦除其痕迹，并且可能重新开始。`

```大多数情况下，简单删除对于卸载软件包来说已经足够了。`

``### `如何使用apt-get清理系统`

```哦，是的！您还可以使用apt-get清理系统并释放一些磁盘空间。`

```您可以使用以下命令清除检索到的包文件的本地存储库：`

`````
sudo apt-get clean
```

```另一种方法是使用autoclean。与上面的clean命令不同，autoclean只删除那些现在有更新版本的检索包文件，它们将不再使用。`

`````
sudo apt-get autoclean
```

```另一种释放磁盘空间的方法是使用autoremove。它会删除自动安装的lib和软件包，以满足已安装软件包的依赖关系。如果删除了包，则这些自动安装的包在系统中是无用的。此命令删除此类包。`

`````
sudo apt-get autoremove
```

```这是清理Linux系统的命令行方式。如果您更喜欢GUI，这里有一些Linux的CCleaner替代品，您可以在基于Ubuntu和Ubuntu的Linux发行版上使用它们。`

``

``### 
