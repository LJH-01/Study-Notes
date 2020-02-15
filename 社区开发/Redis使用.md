ps aux | grep redis ------查看redis服务的启动

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