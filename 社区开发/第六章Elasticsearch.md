# Elasticserch 分布式搜索引擎

## 6.1 Elasticsearch 入门

1. Elasticsearch简介
	1. 一个分布式的（多台服务器）Restful风格（请求风格，普通httpxieyi访问）的搜索引擎
	2. 支持对各种类型的数据的检索（非结构化、结构化）
	3. 搜索速度块，可以提供实时的搜索服务（1.存数据-2.建索引-3.搜，ES可以实时）
	4. 便于水平扩展，每秒可以处理PB级海量数据。（加服务器）
2. Elasticsearch术语--（和数据库作对比）
	1. 索引（库 +6.0后对应表+）、类型（表+7.0废弃+）、文档（行-json）、字段（列）。
	2. 集群（多个服务器）、节点（某个服务器）、分片（对一索引存成多个分片）、副本（对分片的备份）。

https://dl.pstmn.io/download/latest/linux64

---

1. 镜像下载地址

	https://thans.cn/mirror/elasticsearch.html

2. 下载中文分词工具，解压到es的plugins文件夹下新建ik文件夹

3. es更改elasticsearch.yml 文件，设置数据存放路径和日志存放路径。配置jvm.options设置内存 

	> -Xms256m
	>
	> -Xmx512m

4. IKAnalyzer.cfg.xml文件可以添加新词汇

5. 在bin目录下直接./elasticsearch 即可启动。

6. 启动后

	> ```shell
	> curl -X GET "localhost:9200/_cat/health?v"   # 查看健康状态
	> curl -X GET "localhost:9200/_cat/nodes?v"    # 查看节点
	> curl -X GET "localhost:9200/_cat/indices?v"  # 查看索引
	> curl -X PUT "localhost:9200/test"                        # 创建索引  -- （可在执行查询看结果）--（无备份，yellow）
	> curl -X DELETE "localhost:9200/test"                 # 删除索引
	> ```

7. 安装postman,解压即可用。

	>localhost:9200/test/_search?q=content:运营-----搜索 content
	>
	>localhost:9200/test/_search
	>
	>body中
	>
	>```javas
	>{
	>	"query":{
	>		"multi_match":{
	>			"query":"互联网",
	>			"fields":["title","content"]
	>		}
	>	}
	>}
	>```
	>

## 6.2 Spring 整合 Elasticsearch

1. 引入依赖
	1. spring-boot-stater-data-elasticsearch
2. 配置Elasticsearch
	1. cluster-name 、cluster-node
3. Spring Data Elasticsearch
	1. ElasticsearchTemplate
	2. ElasticsearchRepository

-------

使用：

1. 在实体类上用注解做配置

2. 定义Reporsitory接口

	```java
	discussPostReporsitory.save(discussMapper.selectDiscussPostById(291)); // 插入一条数据
	
	discussPostReporsitory.saveAll(discussMapper.selectDiscussPosts(101,0,100)); // 批量插入
	
	// 更新--就是覆盖
	public void testUpdate(){
	        DiscussPost post = discussMapper.selectDiscussPostById(231);
	        post.setContent("我是新人，使劲灌水");
	        discussPostReporsitory.save(post);
	    }
	
	// 删除
	discussPostReporsitory.deleteById(231); // 删除一个，
	 discussPostReporsitory.deleteAll();// 全部删除，危险
	
	//搜索的结果可以高亮显示匹配，原理是es可以给匹配到的字段前后添加标签，我们需要在css上给他们指定颜色即可
	    // 使用reporsitory 搜索---没法处理高亮
	public void testSearchByRepository() {
	        SearchQuery searchQuery = new NativeSearchQueryBuilder()
	                .withQuery(QueryBuilders.multiMatchQuery("互联网寒冬", "title", "content")) // 构造查询条件
	                .withSort(SortBuilders.fieldSort("type").order(SortOrder.DESC))// 构建排序条件
	                .withSort(SortBuilders.fieldSort("score").order(SortOrder.DESC))// 构建排序条件
	                .withSort(SortBuilders.fieldSort("createTime").order(SortOrder.DESC))
	                .withPageable(PageRequest.of(0, 10))
	                .withHighlightFields(
	                        new HighlightBuilder.Field("title").preTags("<em>").postTags("</em>"),
	                        new HighlightBuilder.Field("content").preTags("<em>").postTags("</em>")
	                ).build();
	        // elasticTemplate.queryForPage(searchQuery,class,SearchResultMapper)
	        // 底层获取到了高亮现实的值，但是没有返回
	
	        Page<DiscussPost> page = discussReporsitory.search(searchQuery);
	        System.out.println(page.getTotalElements());
	        System.out.println(page.getTotalPages());
	        System.out.println(page.getNumber());
	        System.out.println(page.getSize());
	
	        for(DiscussPost post :page){
	            System.out.println(post);
	        }
	    }
	
	//使用ElasticsearchTemplate  自己处理高亮
	@Test
	    public void testSearchByTemplate(){
	        SearchQuery searchQuery = new NativeSearchQueryBuilder()
	                .withQuery(QueryBuilders.multiMatchQuery("互联网寒冬", "title", "content")) // 构造查询条件
	                .withSort(SortBuilders.fieldSort("type").order(SortOrder.DESC))// 构建排序条件
	                .withSort(SortBuilders.fieldSort("score").order(SortOrder.DESC))// 构建排序条件
	                .withSort(SortBuilders.fieldSort("createTime").order(SortOrder.DESC))
	                .withPageable(PageRequest.of(0, 10))
	                .withHighlightFields(
	                        new HighlightBuilder.Field("title").preTags("<em>").postTags("</em>"),
	                        new HighlightBuilder.Field("content").preTags("<em>").postTags("</em>")
	                ).build();
	
	        Page<DiscussPost> page = elasticTemplate.queryForPage(searchQuery, DiscussPost.class, new SearchResultMapper() {
	            @Override
	            public <T> AggregatedPage<T> mapResults(SearchResponse response, Class<T> aClass, Pageable pageable) {
	                SearchHits hits = response.getHits();
	                if(hits.getTotalHits() <=0){
	                    return null;
	                }
	                List<DiscussPost> list = new ArrayList<>();
	                for(SearchHit hit : hits){
	                    DiscussPost discussPost = new DiscussPost();
	
	                    String id = hit.getSourceAsMap().get("id").toString();
	                    discussPost.setId(Integer.valueOf(id));
	
	                    String userId = hit.getSourceAsMap().get("userId").toString();
	                    discussPost.setUserId(Integer.valueOf(userId));
	
	                    String title = hit.getSourceAsMap().get("title").toString();
	                    discussPost.setTitle(title);
	
	                    String content = hit.getSourceAsMap().get("content").toString();
	                    discussPost.setContent(content);
	
	                    String status = hit.getSourceAsMap().get("status").toString();
	                    discussPost.setStatus(Integer.valueOf(status));
	
	                    String createTime = hit.getSourceAsMap().get("createTime").toString();
	                    discussPost.setCreateTime(new Date(Long.valueOf(createTime)));
	
	                    String commentCount = hit.getSourceAsMap().get("commentCount").toString();
	                    discussPost.setCommentCount(Integer.valueOf(commentCount));
	
	                    // 处理高亮显示的结果
	                    HighlightField titleField = hit.getHighlightFields().get("title");
	                    if(titleField != null){
	                        discussPost.setTitle(titleField.getFragments()[0].toString());
	                    }
	                    HighlightField contentField = hit.getHighlightFields().get("content");
	                    if(contentField != null){
	                        discussPost.setTitle(contentField.getFragments()[0].toString());
	                    }
	
	                    list.add(discussPost);
	                }
	
	                return new AggregatedPageImpl(list,pageable,
	                        hits.getTotalHits(),response.getAggregations(),response.getScrollId(),hits.getMaxScore());
	            }
	
	            @Override
	            public <T> T mapSearchHit(SearchHit searchHit, Class<T> aClass) {
	                return null;
	            }
	        });
	        System.out.println(page.getTotalElements());
	        System.out.println(page.getTotalPages());
	        System.out.println(page.getNumber());
	        System.out.println(page.getSize());
	
	        for(DiscussPost post :page){
	            System.out.println(post);
	        }
	    }
	```

	

## 6.3 开发社区搜索功能

1.  搜索服务
	1. 将帖子保存至Elasticsearch服务器。
	2. 从Elasticsearch服务器删除帖子
	3. 从Elasticsearch服务器搜索帖子
2. 发布事件
	1. 发布帖子，将帖子异步地提交到Elasticsearch服务器
	2. 增加评论时，将帖子异步di提交到Elasticsearch服务器
	3. 在消费组件中增加一个方法，消费帖子发布事件。
3. 显示结果
	1. 在控制器中处理搜索请求，在HTML上显示搜索结果