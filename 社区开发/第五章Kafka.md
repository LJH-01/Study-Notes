# 第五章-Kafka 构建TB级异步消息系统

## 1.阻塞队列---Kafka是阻塞队列框架

1. BlokingQueue：

	1. 解决线程通信的问题
	2. 阻塞方法：put, take.

	![image-20200215004417061](/home/tqr/Study-Notes/社区开发/image/image-20200215004417061.png)

2. 生产者消费者模式

	1. 生产者：产生数据的线程。
	2. 消费者：使用数据的线程。

3. 实现类

	1. ArrayBlokingQueue--数组
	2. LinkedBlokingQueue--链表
	3. PriorityBlokingQueue--优先队列, SynchronousQueue,DelayQueue--同步，等。

---

阻塞队列就是在生产者消费者之间做一个缓冲。

---------

演示

```java
public class BlockingQueueTests {
    public static void main(String[] args) {
        BlockingQueue queue = new ArrayBlockingQueue(10);
        new Thread(new Producer(queue)).start();
        new Thread(new Consumer(queue)).start();
        new Thread(new Consumer(queue)).start();
        new Thread(new Consumer(queue)).start();
    }
}
class Producer implements Runnable{

    private BlockingQueue<Integer> queue;

    public Producer(BlockingQueue queue){
        this.queue=queue;
    }

    @Override
    public void run() {
        try{
            for (int i = 0; i < 100; i++) {
                Thread.sleep(20);
                queue.put(i);
                System.out.println(Thread.currentThread().getName() + " 生产："+i+"  当前队列大小是：" +queue.size());

            }
        }catch (Exception e){
            e.printStackTrace();
        }
    }
}

class Consumer implements Runnable{

    private BlockingQueue<Integer> queue;

    public Consumer(BlockingQueue queue){
        this.queue=queue;
    }

    @Override
    public void run() {
        try{
            while(true){
                Thread.sleep(new Random().nextInt(1000));
                int c = queue.take();
                System.out.println(Thread.currentThread().getName() + " 消费："+c +"  当前队列大小是："+queue.size());
            }
        }catch (Exception e){
            e.printStackTrace();
        }
    }
}
```

## 2. Kafka 入门

1. Kafka简介
	1. Kafka是一个分布式的流媒体平台
	2. 应用：消息系统、日志收集、用户行为追踪、流式处理。

<img src="/home/tqr/Study-Notes/社区开发/image/image-20200215113413326.png" alt="image-20200215113413326" style="zoom: 67%;" />

2. Kafka特点
	1. 高吞吐量、消息持久化、高可靠性、高扩展性。

3. Kafka术语
	1. Broker（服务器，集群中的每一台服务器都是）   ,    Zookeeper（用来管理集群）
	2. Topic（阻塞队列分为点对点，和发布订阅两种方式，kafka采用第二种，生产者发布的位置就叫topic，可以看作一个文件夹）      ,   Partition （一个topic分为多个partition）   ,     Offset(在partition中的索引)
	3. Leader Replica（副本，做备份，主副本，负责相应）   ,     Follower Replica（从副本，只是备份，不负责做相应）

---------------

项目中主要用消息系统

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

## 5.4 发送系统通知---基于事件

1.  触发事件
	1. 评论后，发布通知
	2. 点赞后，发布通知
	3. 关注后，发布通知
2. 处理事件
	1. 封装事件对象
	2. 开发事件的生产者
	3. 开发事件的消费者

<img src="/home/tqr/Study-Notes/社区开发/image/image-20200215161707770.png" alt="image-20200215161707770" style="zoom:50%;" />

先封装event，然后封装两个类，eventProducer 和 eventConsumer。然后让eventProducer.fireEvent()

## 5.5 显示系统通知

1. 通知列表
	1. 显示评论，点赞，关注三种类型的通知
2. 通知详情
	1. 分页显示某一类主题所包含的通知
3. 未读消息
	1. 在页面头部显示所有的未读消息的数量

----------------

消息队列的用处：

### 1异步处理

场景说明：用户注册后，需要发注册邮件和注册短信。

### 2应用解耦

场景说明：用户下单后，订单系统需要通知库存系统。

### 3流量削锋

应用场景：秒杀活动，一般会因为流量过大，导致流量暴增，应用挂掉。为解决这个问题，一般需要在应用前端加入消息队列。

### 5消息通讯