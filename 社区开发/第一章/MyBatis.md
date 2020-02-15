# MyBatis

安装MySQL

启动mysql服务：service mysql start

关闭mysql服务：service mysql stop

查看mysql状态：service mysql status

---------

## Mybatis

http://www.mybatis.org/mybatis-3 mybatis的网站

http://www.mybatis.org/spring Spring整合mybatis

### 核心组件

* SqlSessionFactory: 用于船舰SqlSession的工厂类
* SqlSession: MyBatis的核心组件，用于项数据库执行SQL
* 主配置文件：xml配置文件，可以对MyBatis的底层行为作出详细的配置

------------

在SpringBoot中，上面不需要写代码，下面俩才需要写代码。

* Mapper接口：就是DAO接口，在MyBatis中习惯性的称之为Mapper。
* Mapper映射器：用于编写SQL，并将SQL和实体类映射的组件，采用XML、注解均可实现。

### 使用MyBatis对用户表进行CRUD操作。

1. 使用Maven Repository搜索响应的包添加到pom.xml文件中。
2. 配置mysql，原本需要写xml配置，在SpringBoot框架下可以直接在application.propreties里配置。可以去springBoot官网上找配置模板。

-----

1. 在entity中写实体类，添加geter和seter方法

2. 在dao写mapper接口

3. 在resources中写mapper（这跟application.properties文件配置有关）

	```xml
	?xml version="1.0" encoding="UTF-8" ?>
	<!DOCTYPE mapper
	        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
	        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
	<mapper namespace="com.goodcoder.community.dao.UserMapper"> # 这里写dao的全限定名
	    <select id="selectBlog" resultType="Blog">
	    select * from Blog where id = #{id}
	  </select>
	</mapper>
	```

4. 写完写测试方法，验证正确性。