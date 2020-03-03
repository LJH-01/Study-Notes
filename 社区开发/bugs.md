>## 查看服务的启动--------------ps aux | grep redis
>
>## mysql
>
>```shell
># 启动mysql服务
>service mysql start
>```
>
>## redis
>
>```shell
>以命令 redis-server /opt/redis-5.0.7/redis.conf  启动redis服务器
>以命令 redis-cli 启动客户端 ---必须先启动服务器，才能起客户端
>以命令 redis-cli shutdown 在客户端关闭后台服务器。
>```
>
>## kafka
>
>```shell
># 启动zookeeper  不能后台守护进程
>/opt/kafka_2.12-2.4.0/bin/zookeeper-server-start.sh /opt/kafka_2.12-2.4.0/config/zookeeper.properties
>后台启动的话 
>/opt/kafka_2.12-2.4.0/bin/zookeeper-server-start.sh  -daemon /opt/kafka_2.12-2.4.0/config/zookeeper.properties
># 启动kafka 服务器
>/opt/kafka_2.12-2.4.0/bin/kafka-server-start.sh /opt/kafka_2.12-2.4.0/config/server.properties
>后台启动
>nohup /opt/kafka_2.12-2.4.0/bin/kafka-server-start.sh /opt/kafka_2.12-2.4.0/config/server.properties 1>/dev/null 2>&1 
>```
>
>## ElasticSearch
>
>```shell
># 启动es
>/opt/elasticsearch-6.4.3/bin/elasticsearch
>```
>

-----------------

### kafka 安装

1. 下载安装包，解压

2. 配置config文件夹下的两个文件，zookeeper.properties。server.properties 都是只更改了日志路径。

3. 给data目录权限。

4. 启动zookeeper，到根目录下启动zookeeper

	```shell
	/opt/kafka_2.12-2.4.0/bin/zookeeper-server-start.sh /opt/kafka_2.12-2.4.0/config/zookeeper.properties
	```

5. 启动kafka的server

	```shell
	/opt/kafka_2.12-2.4.0/bin/kafka-server-start.sh /opt/kafka_2.12-2.4.0/config/server.properties
	```

6. 使用kafka创建一个topic

	```shell
	/opt/kafka_2.12-2.4.0/bin/kafka-topics.sh --create --bootstrap-server localhost:9092 -replication-factor 1 --partitions 1 --topic test
	# 查看topic是否创建
	/opt/kafka_2.12-2.4.0/bin/kafka-topics.sh --list --bootstrap-server localhost:9092
	```

7. 创建一个topic

	``` shell
	# 创建一个生产者
	tqr@TQR-ubuntu:/opt/kafka_2.12-2.4.0/bin$ ./kafka-console-producer.sh  --broker-list localhost:9092 --topic test
	# 可以发布消息了
	#创建消费者--新窗口
	tqr@TQR-ubuntu:/opt/kafka_2.12-2.4.0/bin$ ./kafka-console-consumer.sh  --bootstrap-server localhost:9092 --topic test --from-beginning
	```

8. 关闭的时候倒着关闭

## Redis 的安装和使用

1. 官网下载安装包

2. ```shell
	tar xzf redis-5.0.7.tar.gz # 解压
	cd redis-5.0.7                     # 进入根目录
	make									  # make，编译
	make install 						# 安装
	```

3. 修改 redis-5.0.7目录下的redis.conf; daemonize守护线程，默认是no 改为yes，不然一关服务器就关了。

4. 修改 redis-5.0.7/src 目录下的dump.rdb 访问权限，这里给的时766。

5. 将 redis-5.0.7目录下的redis.conf 复制一份到home 为了启动方便

6. ```java
	以命令 redis-server redis.conf 启动redis服务器
	以命令 redis-cli 启动客户端 ---必须先启动服务器，才能起客户端
	以命令 redis-cli shutdown 在客户端关闭后台服务器。
	```

------------

```console
flushdb - 删单个库
flushall - all
```

# IDEA

souf==System.out.printf("");

sout==System.out.println();

soutf == System.out.println("CommunityApplicationTests.testApplicationContext"); 输出当前类和方法名

总之 sout+... 

fori  === 自动写for循环

iter== 创建foreach循环。

psvm   == public static void main

----------------------

`ctrl`+`i`快速实现接口方法

`alt`+`insert`可以弹出窗口快速实现seter，geter方法，tostring方法等

`ctrl`+`shift`+`n` 快速搜索

`ctrl`+`shift`+上/下 ：移动一行代码

* 
* 使用spring Initializr 创建项目后，idea不能导入依赖包，原因是依赖的包在阿里云的镜像里面没有，最后将阿里云的镜像注释掉开始下载，所以还是不用国内的吧，虽然国外的慢但至少全面。
* 后来发现还是阿里云快，也不知道第一次是哪里搞错了。

---

## maven 依赖出错

1. 确保下载路径正确，现在的策略是不配置镜像，就使用国外镜像，因为使用阿里云的镜像速度并没有加快，相反严重怀疑地址有问题。我确信它是错误的，
2. 可以检查本的maven仓库即./m2文件夹，检查响应的依赖是否下载到本地。
3. 检查IDEA中的外部依赖看是否加入依赖。

出问题无外乎这几种问题之间的同步出了错误，我的错误是因为下载的时候换了镜像源，正常是不会出错的，但是我这次偏偏出错了，解决办法是首先发现了./m2里没有这个jar包然后删了重下，后来导入成功了，但是右侧maven窗口又红线，移除依赖在加入即可。

-----

## @Mapper @Repository

在dao中的 mapper 如果写了@ reporsitory注解则Myatis扫描不到，写上@Mapper注解Mybatis可以扫描到但是Spring出红线说找不到装配类但是并不影响运行，现在的解决方法是两个注解都写上。

## mapper.xml的问题

极易出错

## Thymeleaf

Thymeleaf 里 th:后面不能加空格

----------

ps 是瞬时的进程状态  -aux是选项

grep 是文本搜索命令 grep ‘test’ d*显示所有以d开头的文件中包含 test的行。

-----------

解压tar.gz

**tar**
 解包：tar xvf FileName.tar
 打包：tar cvf FileName.tar DirName
 （注：tar是打包，不是压缩！）
 ———————————————
**.gz**
 解压1：gunzip FileName.gz
 解压2：gzip -d FileName.gz
 压缩：gzip FileName
**.tar.gz**
 解压：tar zxvf FileName.tar.gz
 压缩：tar zcvf FileName.tar.gz DirName
 ———————————————
**.bz2**
 解压1：bzip2 -d FileName.bz2
 解压2：bunzip2 FileName.bz2
 压缩： bzip2 -z FileName
**.tar.bz2**
 解压：tar jxvf FileName.tar.bz2
 压缩：tar jcvf FileName.tar.bz2 DirName
 ———————————————
**.bz**
 解压1：bzip2 -d FileName.bz
 解压2：bunzip2 FileName.bz

**.tar.bz**
 解压：tar jxvf FileName.tar.bz
 ———————————————
**.Z**
 解压：uncompress FileName.Z
 压缩：compress FileName
**.tar.Z**
 解压：tar Zxvf FileName.tar.Z
 压缩：tar Zcvf FileName.tar.Z DirName
 ———————————————
**.tgz**
 解压：tar zxvf FileName.tgz

**.tar.tgz**
 解压：tar zxvf FileName.tar.tgz
 压缩：tar zcvf FileName.tar.tgz FileName
 ———————————————
**.zip**
 解压：unzip FileName.zip
 压缩：zip FileName.zip DirName
 ———————————————
**.rar**
 解压：rar a FileName.rar
 压缩：rar e FileName.rar
 ———————————————
**.lha**
 解压：lha -e FileName.lha
 压缩：lha -a FileName.lha FileName

---------------

ubuntu 环境变量文件 /etc/profile,

vim 撤销操作 u

将一个目录下的所有文件移动 mv *；

-------------

创建Dash图标：

```
sudo gedit ~/.local/share/applications/apache_jmeter.desktop
```

内容

```
[Desktop Entry]
Encoding=UTF-8
Version=1.0
Type=Application
Name=Apache JMeter (3.2 r1790748)
Icon=apache_jmeter.png
Path=/opt/apache-jmeter-3.2
Exec=java -jar /opt/jmeter/bin/ApacheJMeter.jar
StartupNotify=false
StartupWMClass=Apache JMeter
OnlyShowIn=Unity;
X-UnityGenerated=true
```