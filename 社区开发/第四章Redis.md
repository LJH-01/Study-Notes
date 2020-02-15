# Redis   一站式高性能存储方案

## 4.1Redis入门

1. Redis是一款基于键值对的NoSQL数据库，它的值支持多种数据结构：

	字符串（String） 哈希（hashs）列表（lists）集合（sets）有序集合（sorted sets）等。

2. Redis将所有的数据都存放在内存中，所以他的读写性能十分惊人，同时，Redis还可以将内存中的数据以快照或日志的形式保存到硬盘上，以保证数据的安全性

3. Redis典型的应用场景包括：缓存，排行榜，计数器，社交网络，消息队列等。

>NoSQL 数据库：除了关系型数据库以外的数据库
>
>Not Only SQL，redis key是string，vaule可以是很多
>
>快照：RDD的方式直接把内存复制到硬盘；快照不适合实时做，适合隔一段时间左一次
>
>日志的方式：AOF 实时，不断追加，占空间，存硬盘时需要重新执行一遍
>
>1-性能好  2-支持数据类型多 3-使用简单
>
>用途，缓存，排行榜的缓存热门，计数器改变，社交网络点赞等，也可消息队列，不是专门但简单也可。后面将卡夫卡

1. Redis 主要学怎么控制数据，也就是命令

	>对字符串操作 ：Set，get，存取，incr 加一 ，decr减一
	>
	>哈希：hset  heget 
	>
	>列表：列表看成横向的容器，可以从两边存取，可以做栈和队列 lpush ，llen，lindex, lrange,rpop,
	>
	>集合：sadd, scard, spop(随机弹出，抽奖) smembers
	>
	>有序集合：zadd, zcard, zscore, zrank, zrange
	>
	>全局命令：keys \*,  keys test\*,  type ,exists,  del,  expire(指定过期),

## 4.2 Spring整合Redis

1. 引入依赖
	
	1. Spring-boot-starter-data-redis
2. 配置Redis
	1. 配置数据库参数
	
	2. 编写配置类，构造RedisTemplate(Spring-Boot 配好了，我们重新配置)
	
		```java
		package com.goodcoder.community.config;
		
		import org.springframework.context.annotation.Bean;
		import org.springframework.context.annotation.Configuration;
		import org.springframework.data.redis.connection.RedisConnectionFactory;
		import org.springframework.data.redis.core.RedisTemplate;
		import org.springframework.data.redis.serializer.RedisSerializer;
		
		/**
		 * TQR 2020/2/13
		 */
		@Configuration
		public class RedisConfig {
		
		    @Bean
		    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory factory) {
		        // 先将连接工厂注入
		
		        // 实例化redis模板
		        RedisTemplate<String,Object> template = new RedisTemplate<>();
		        template.setConnectionFactory(factory);
		
		        // 设置key的序列化方式
		        template.setKeySerializer(RedisSerializer.string());
		        // 设置value的序列化方式
		        template.setValueSerializer(RedisSerializer.json());
		        // 设置hash的key的序列化方式
		        template.setHashKeySerializer(RedisSerializer.string());
		        // 设置hash的alue的序列化方式
		        template.setHashValueSerializer(RedisSerializer.json());
		
		        // 使配置生效
		        template.afterPropertiesSet();
		        return template;
		    }
		
		}
		```
3. 访问Redis
	1. redisTemplate.opsForValue()
	2. redisTemplate.opsForHash()
	3. redisTemplate.opsForList()
	4. redisTemplate.opsForSet()
	5. resisTemplate.opsForZSet()
	6. …………………………

4. ```java
	注入redisTemplate.
	绑定 rediskey的操作。简化。
	public void testBoundOpreate(){
	        String rediskey = "test:student";
	        
	        redisTemplate.opsForValue().increment(rediskey);
	        redisTemplate.opsForValue().increment(rediskey);
	        redisTemplate.opsForValue().increment(rediskey);
	
	        BoundValueOperations operations = redisTemplate.boundValueOps(rediskey);
	        operations.increment();
	        operations.increment();
	        operations.increment();
	    }
	```

5. Redis 事务

	>* 将事务放到一个队列中，提交时统一执行，所以事务中间做查询不会返回结果。
	>
	>* 也支持编程式事务和声明式事务。因为Redis事务的特殊性所以常用编程式事务。将一小段代码括起来
	>
	>* 编程式事务演示：
	>
	>	```java
	>	@Test
	>	    public void testTransactional(){
	>	        // 放在execute 里执行，里面有一个回调函数，SessionCallback()
	>	        // 回调函数中一个方法，execute 会将 RedisOperations 对象传入
	>	        //redisOperations.multi(); // 开始事务
	>	        
	>	        Object obj = redisTemplate.execute(new SessionCallback() {
	>	            @Override
	>	            public Object execute(RedisOperations redisOperations) throws DataAccessException {
	>	                String redisKey = "test:tx";
	>	                
	>	                redisOperations.multi();    ///////////// 开始事务
	>	                
	>	                redisOperations.opsForSet().add(redisKey,"zhangsan","lisi","wangwu");
	>	                System.out.println(redisOperations.opsForSet().members(redisKey)); // 无效操作
	>	                
	>	                return redisOperations.exec();//////////// 结束事务
	>	            }
	>	        });
	>	        System.out.println(obj);
	>	    }
	>	```
	>
	>	

## 4.3 点赞--数据存Redis 提升性能

1. 点赞
	1. 支持对帖子、评论点赞
	2. 第一次点赞，第二次取消点赞
2. 首页点赞数量
	1. 统计帖子的点赞数量
3. 详情页点赞数量
	1. 统计点赞数量
	2. 显示点赞状态

## 4.4 我收到的赞

1. 重构点赞功能
	1. 以用户为key，记录点赞的数量
	2. increment(key) decrement(key)
2. 开发个人主页
	1. 以用户为key，查询点赞数量

## 4.5 关注和取消关注

1. 需求
	1. 开发关注、取消关注功能。
	2. 统计用户的关注数，粉丝数。
2. 关键
	1. 若A关注了B，则A是B的Followe(粉丝),B是A的Followee(目标)
	2. 关注的目标可以是用户、帖子、题目等，在实现时将这些目标抽象为实体。

## 4.6 关注列表、粉丝列表

1. 业务层
	1. 查询某个用户关注的人，支持分页
	2. 查询某个用户的粉丝，支持分页
2. 表现层
	1. 处理“查询关注的人”，“查询粉丝”的请求
	2. 编写“查询关注的人”，“查询粉丝”模板

## 4.7 使用Redis 优化登录模块

1. 使用Redis存储验证码
	1. 验证码需要频繁的刷新和访问，对性能要求较高
	2. 验证码不需要永久保存，通常在很短的时间后就会失效
	3. 分布式部署时存在Session共享的问题。
2. 使用Redis存储登录凭证
	1. 处理每次请求时，都要查询用户的登录凭证，访问的频率非常高
3. 使用Redis缓存用户信息
	1. 处理每次请求时，都要根据凭证查询用户信息，访问的频率非常高。