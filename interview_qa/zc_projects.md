## 基本架构

![image](https://note.youdao.com/yws/public/resource/c5cd2ffd19c44ab4ff8fdf9c05f714e8/xmlnote/67A604F99ECA4816B929832E338B5D81/8858)

![image](https://note.youdao.com/yws/public/resource/c5cd2ffd19c44ab4ff8fdf9c05f714e8/xmlnote/8C3EB979E25B4F6C9BEC01D8F840590A/9185)

![image](https://note.youdao.com/yws/public/resource/c5cd2ffd19c44ab4ff8fdf9c05f714e8/xmlnote/997BD9D35C4D40ED880AA248B710167D/9211)

## 省平台
* 1. 技术:
    > Hadoop,flume,Spark,Kafka,Redis,elasticsearch

    >采集数据的流程、什么数据、怎么分析，分析后的结果有什么商业价值
有多少节点、节点配置、每天业务量多大、最大qps(每秒查询数)

* 2. 要点:
    > 每天数据量:

        > 数据源：
          1. 前置机(kafka获取省厅数据--->hbase)
          2. 区县平台(oracle---sqoop---Hbase)
          3. 数据来源:结构化数据,半结构/非结构数据,实时数据
        > 数据量:
          1. 5-8 GB/day过车数据；各个区县宁波，金华，台州...通行记录数据通过区县的前置机(在他们内网的linux机器)java程序执行传递到 kafka broker到hbase；
          2. 超限重量 = 当前总重*0.95 ? 限重标准
          3. 车轴数决定货车的限重标准: 2轴18吨;3轴25吨;6+轴50吨
          4. Oracle区县平台-->kattle抽取---->hbase存储大数据-->    (数据处理) 过车数据,案件数据-->elasticsearch存储3个节点；
          5. 案件 case_table 字段很多;包含各类型信息,比如站点,处罚文字笔录 ,处罚法律条文
          6. car_recod 过车数据 每天大概70万-80万条 2GB
            字段:
              省级
              市级
              县级
              车牌号
              站点
              车轴
              车速
              总重
              超重
          5. hbase--->elasticsearch：hook函数进行，同步es到hbase（旧的遗留同步数据会出现很多问题，新的架构使用mapreduce/hive进行处理写入mysql进行报表查询）
          6. 过车实时数据直接kafka--->spark streaming进行处理-->存储到redis

    > 模块

        - 数据统计报表模块
        - 实时展示模块
        - 黑名单
        - 一次违法判断
        - 趋势分析

* 3. 说明:

    > Spark:

        > 关于spark节点挂掉后,kafka还在生产数据,这些数据应该不会丢失,只要当spark启动后连接到kafka的topic,就可以接着消费.还有可以分topic 类似快车道topic(加大partitions,consumer消费数据时的吞吐量越大),慢车道topic,进行切换;
        > kafka数据流入spark后，spark如何保证语义

    [spark如何保证语义](https://spark.apache.org/docs/2.2.0/streaming-kafka-0-8-integration.html)

        > spark1.3之前:使用WAL，读取zookeeper的信息,跟新zookeeper的offsets
        > spark1.3之后:createDirectStream spark driver查询Kafka的最后偏移量,executor根据offset读取kafka的数据，只要Kafka可靠，spark就没问题
        > 简而言之，我们可以说我们可以依赖于故障重启的checkpoint，但我们不能依赖于升级重启的checkpoint。
        > 如果多次写入产生相同的数据，则此输出操作是幂等的。saveAsTextFile是典型的幂等更新; 具有唯一键的消息可以写入数据库而无需重复。这种方法将为我们提供等效的一次性语义。请注意，虽然它通常用于仅映射程序，但它需要在Kafka DStream上进行一些设置。
        > 设置enable.auto.commit为false。默认情况下，Kafka DStream会在收到数据后立即提交消费者抵消。我们想推迟批处理完全处理的这个动作。
        > 打开Spark Streaming的检查点以存储Kafka偏移量。但是，如果应用程序代码发生更改，则检查点数据不可重用。这导致了第二种选择：
        > 输出后提交卡夫卡补偿。Kafka提供了一个commitAsyncAPI，HasOffsetRanges该类可用于从初始RDD中提取偏移量：


    [关于checkpoint 升级解决方案](http://why-not-learn-something.blogspot.com/2016/08/upgrading-running-spark-streaming.html)

    > Kafka(queuing / publish-subscribe)队列 / 发布-订阅

        > kafka仅仅保证同一partition的消息顺序;如果需要强行排序，请确保将数据固定到单个分区.
        > 每天10GB 实时车辆检测超重数据 spark-streaming 计算来自哪里的数据呢?kafka topic写入需要进行规则过滤
        
        >kafka与flume
            
            1.
            flume:专用的工具对于日志,文件的抽取
            kafka:通用型工具,消息订阅发布,streaming处理
            2.
            flume:没有副本
            kafka:有副本
            
        > at-most-once至多一次
            1.First, set ‘enable.auto.commit’ to true.
            2.Also, set ‘auto.commit.interval.ms’ to a lower timeframe.
            3.Make sure, don’t make calls to consumer.commitSync(); from the consumer. Moreover,  Kafka would auto-commit offset at the specified interval, with this configuration of the consumer.
            
        > at-least-once至少一次
            1.First, set ‘enable.auto.commit’ to false  or Also, set ‘enable.auto.commit’ to true with ‘auto.commit.interval.ms’ to a higher number.
            2.By making the following call consumer.commitSync(), Consumer should now then take control of the message offset commits to Kafka;
            ```
            // Below call is important to control the offset commit. Do this call after you finish processing the business process.
            //在代码需要手动commit
           consumer.commitSync();
            ```
        
        >exactly-once仅仅一次
            1.At first, set enable.auto.commit = false.
            2.After processing the message, don’t make calls to consumer.commitSync().
            3.Moreover, by making a ‘subscribe’ call, Register consumer to a topic.
            4.In order to start reading from a specific offset of that topic/partition, implement a ConsumerRebalanceListener. Also, perform consumer.seek(topicPartition, offset), within the listener.
            5.As a safety net, implement idempotent.
            
        >性能调优:
            在延迟和吞吐量之间的平衡
            1.生产者
                -压缩
                -发送批量大小 batch.size 数据给到broker在发送之前的大小;至少内存有默认16384byte=16M
                -同步or异步
            2.消费者
                -提取的量大小 checkpoint interval间隔时间
                
            3.生产环境的服务器配置
                1.num.replica.fetchers
                    这个参数定义了将从前导复制到追随者的数据的线程数量。根据线程的可用性，我们可以修改这个参数的值。如果我们有可用的线程，那么让复制fetchers的数量可以并行完成复制是很重要的。
                2.replica.fetch.max.bytes
                    这个参数就是我们想要从每个fetchrequest中获取多少数据。为这个参数增加值是很好的，这样它就可以帮助在flowers中快速创建副本。
                3.replica.socket.receive.buffer.bytes
                    如果创建副本可用线程较少，我们可以增加缓冲区的大小。此外，如果复制线程与传入消息速率相比速度较慢，这将有助于保存更多数据。
                4.num.partitions
                    当卡夫卡在活动，我们应该注意这个配置。我们可以拥有并行级别并并行写入数据，这将自动增加吞吐量。
                5.num.io.threads
                    线程的数量必须取决于磁盘的数量。

    > Elasticsearch:

        > 分布式存储数据,历史数据统计查询,页面展示检索,月度季度报表；3个集群节点 1 header,2 leader


    > hbase:

        > 数据使用者使用HBase随机读取/访问HDFS中的数据。HBase位于Hadoop文件系统之上，提供读写访问。而hadoop-hive只能扫描所有。
        
        > 主要组成
            1.Zookeeper
            2.HMaster server
            3.Regin server
        

    > Sqoop
    
        > 非常适合在JDBC兼容数据库和Hadoop环境之间发送数据。Sqoop是为那些需要几个简单的CLI选项的用户构建的，这些用户可以将选择的数据库表导入到Hadoop中，执行大数据集分析运行mapreduce，这种分析由于资源限制而无法通过该数据库系统完成，然后将结果导回到该数据库中或其他）。当需要在数据库提取和Hadoop加载之间进行一些额外的自定义处理时，Sqoop就不足了，在这种情况下，可能首选Apache Spark的JDBC实用程序。 sqoop是apache旗下一款“Hadoop和关系数据库服务器之间传送数据”的工具。导入，导出命令翻译成mapreduce来实现，主要针对inputformat和outputformat进行定制。可以通过命令sheng gJava
        导入数据：MySQL，Oracle导入数据到Hadoop的HDFS、HIVE、HBASE等数据存储系统；
        导出数据：从Hadoop的文件系统中导出数据到关系数据库
        


    > Apache Kafka非常适用于接近实时的场景，大容量或多地点项目。它可以解决升级问题，只需其他解决方案的一小部分成本，并且具有开放源代码场景的灵活性。
    

    > MapReduce

        > 车辆人员企业黑名单:离线处理，每个月数据进行匹配，可以手动执行，也可以自动执行写入到oracle

    > oracel		地级列表 元数据

    > sqoop(kattle)

        > sqoop每次都会调用mapreduce ETL（Extract-Transform-Load的缩写，即数据抽取、转换、装载的过程）抽取到

    > 硬件:
        > 前置机 监控抓拍

    > echarts

        > 数据可视化




## 综合管理平台
* 1. 技术:
    > SpringCloud
        1. 基于oauth2的统一认证授权
            https://github.com/wiselyman/uaa-zuul
        2.
            https://github.com/jarvisqi/spring-cloud-microservice

    
    基本服务
        > register - 服务注册与发现
        > config - 同步配置
        > monitor - 监控
        > gateway - 代理所有微服务网关接口
        > auth - oauth2认证:采用jdbc 数据库的方式存储客户端的信息；采用redis存储令牌信息。
        > zipkin - 分布式跟踪
    业务服务
        > dynamic
        > statistics
        > system
    > docker集成发布
* 2.
* 3.
* oauth2.0
```
（A）用户打开客户端以后，客户端要求用户给予授权。

（B）用户同意给予客户端授权。

（C）客户端使用上一步获得的授权，向认证服务器申请令牌。

（D）认证服务器对客户端进行认证以后，确认无误，同意发放令牌。

（E）客户端使用令牌，向资源服务器申请获取资源。

（F）资源服务器确认令牌无误，同意向客户端开放资源。
```
```json
     HTTP/1.1 200 OK
     Content-Type: application/json;charset=UTF-8
     Cache-Control: no-store
     Pragma: no-cache
     {
       "access_token":"2YotnFZFEjr1zCsicMWpAA",//授权令牌
       "token_type":"example",
       "expires_in":3600,
       "refresh_token":"tGzv3JOkF0XG5Qx2TlKWIA",//刷新令牌
       "example_parameter":"example_value"
     }
```















## WCM日志分析系统
* 1. 技术:
    > Hadoop,flume,zookeeper
* 2.
* 3.
