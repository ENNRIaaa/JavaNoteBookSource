# Redis入门

### 什么是NoSql？

NoSql = Not Only Sql

泛指非关系型数据库，随着互联网web2.0网站的兴起，传统的关系型数据库在处理web2.0网站，特别是超大规模和高并发的SNS类型的web2.0纯动态网站已经力不从心，出现了很多难以克服的问题，而费关系型数据库则由于其本身的特点得到了非常迅速的发展。NoSql数据库的产生就是为了解决大规模数据集合多重数据种类带来的调站，尤其是大数据应用难题。

很多数据类型，例如个人信息、社交网络、地理位置等，这些数据类型的存储不需要一个固定的格式，不需要多余的操作就可以横向扩展的。



### NoSql特点：

解耦！

1. 方便扩展（数据之间没有关系，易扩展）

2. 大数据量高性能（Redis一秒可写8W次，读取11W次）

3. 数据类型是多样型的（不需要事先设计数据库）

4. 传统的RDBMS和NoSql区别：

   ```
   传统RDBMS：
   - 结构化组织
   - SQL
   - 数据和关系都存在单独的表中
   - 严格的一致性
   - 基础的实务操作
   
   NoSql：
   - 不仅仅是数据
   - 没有固定的查询语言
   - 键值对、列存储、文档型存储、图形数据库
   - 最终一致性
   - CAP定理和BASE（异地多活）
   - 高性能、高可用、易扩展
   ```



### 3V+3高：

3V---海量Velume

​       多样Variety

​        实时Velocity

3高-----高并发

​       	高可扩

​       	高性能



### NoSql的四大分类：

- KV键值对：
  - 新浪：Redis
  - 美团：Redis + Tair
  - 阿里、百度：Redis + memcache
- 文档型数据库（BSON格式）：
  - MongoDB（基于分布式文件存储的数据库，使用C++编写，主要用来处理大量的文档。MongoDB是一个介于关系型数据库和非关系型数据库的中间产品。它是非关系型数据库中功能最丰富的，最像关系型数据库的）
  - ConthDB
- 列存储
  - HBase
  - 分布式文件系统
- 图形关系数据库
  - Neo4j



| **分类**              | **Examples举例**                                      | 典型应用场景                                                 | 数据模型                                        | 优点                                                         | 缺点                                                         |
| --------------------- | ----------------------------------------------------- | ------------------------------------------------------------ | ----------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **键值（key-value）** | Tokyo Cabinet/Tyrant， Redis， Voldemort， Oracle BDB | 内容缓存，主要用于处理大量数据的高访问负载，也用于一些日志系统等等。 | Key 指向 Value 的键值对，通常用hash table来实现 | 查找速度快                                                   | 数据无结构化，通常只被当作字符串或者二进制数据               |
| **列存储数据库**      | Cassandra， HBase， Riak                              | 分布式的文件系统                                             | 以列簇式存储，将同一列数据存在一起              | 查找速度快，可扩展性强，更容易进行分布式扩展                 | 功能相对局限                                                 |
| **文档型数据库**      | CouchDB， MongoDb                                     | Web应用（与Key-Value类似，Value是结构化的，不同的是数据库能够了解Value的内容） | Key-Value对应的键值对，Value为结构化数据        | 数据结构要求不严格，表结构可变，不需要像关系型数据库一样需要预先定义表结构 | 查询性能不高，而且缺乏统一的查询语法。                       |
| **图形(Graph)数据库** | Neo4J， InfoGrid， Infinite Graph                     | 社交网络，推荐系统等。专注于构建关系图谱                     | 图结构                                          | 利用图结构相关算法。比如最短路径寻址，N度关系查找等          | 很多时候需要对整个图做计算才能得出需要的信息，而且这种结构不太好做分布式的集群方案。 |



### Redis概述：

> Redis(Remote Dictionary Server) ，即远程字典服务，是一个开源的使用ANSI C语言编写、支持网络、可基于内存亦可持久化的日志型、Key-Value数据库，并提供多种语言的API。从2010年3月15日起，Redis的开发工作由VMware主持。从2013年5月开始，Redis的开发由Pivotal赞助。

#### Redis能干什么？

- 内存存储、持久化，内存中是断电即失，所以说持久化很重要(rdb、aof)
- 效率高，可用于高速缓存
- 发布订阅系统
- 地图信息分析
- 计时器、计数器（浏览量！）



#### 特性：

- 多样化数据类型
- 持久化
- 集群
- 事务
- ...



#### 官方地址：

[官网](https://redis.io/)

[中文官网](http://www.redis.cn/)

