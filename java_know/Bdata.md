## Hadoop

https://blog.csdn.net/zuochang_liu/article/list/1

![image](https://note.youdao.com/yws/public/resource/c5cd2ffd19c44ab4ff8fdf9c05f714e8/xmlnote/73D59BABAEF2440D9A78D80B65B94554/9476)

 [hadoop](http://blog.csdn.net/u011682879/article/details/55803847)

> Hadoop组件:

    1.HDFS
    2.YARN
    3.MAPREDUCE

> Hdfs和Hbase 压缩

    1. HDFS
        - 压缩流 CompressionCodec ... FileSystem.create(new Path("/user/txtx.gz"))  lzo(需要自己安装库)压缩比gzip(自带)更好,虽然压缩率不及gzip,但lzo支持split,可又分布式mapreduce执行任务,解压速度快.
        
        - 序列化文件
        
    2. HBase 无需压缩 本身数据是序列化的

> HDFS小文件存储

    1. 存储为HAR(Hadoop Archives),访问格式变为 har://URL
    2. 写入SequenceFile,文件名为key 内容为value
    3. 使用HBase方式存储
    4. 先合并小文件再上传到HDFS

```java
Mapper= {(total data size)/ (input split size)} If data size= 1 Tb and input split size= 100 MB Hence, Mapper= (1000*1000)/100= 10,000

right number of reducers is 0.95 or 1.75 multiplied by (<no. of nodes>*<no. of maximum container per node>)
```
    
> Jobtracker与namenode 在同一节点启动，SecondaryNameNode帮助NameNode合并日志，减少NameNode启动时间。

> Hadoop核心配置文件：1，core-site.xml；2，hdfs-site.xml；3，mapred-site.xml；4，yarn-site.xml

> mapreduce原理：分而治之。master将大数据集分发给各个分支节点共同处理，然后整合各个节点的中间结果。JobTracker调度工作，TaskTracker执行工作。

> mapreduce流程: map->combiner->partition->sort->copy->sort->grouping->reduce

> HDFS流程：
    1.write：client-->namenode-->记录一条数据位置（元数据）告诉client存在哪儿-->
    2.read：

> combiner：是在map端运行的计算任务，小型的reduce，减少map端的输出数据。优化传输。但是使用combiner需要map输出的结果和reduce输入输出的一样。

> partition：默认实现hashpartition，是map端将数据按照reduce个数取余，进行分区，不同的reduce来copy自己的数据。partition
的作用是将数据分到不同的reduce进行计算，加快计算效果。

> 数据倾斜：几个或一个reduce允许很慢，导致程序处理时间过长
    1.extends Partitioner自定义分区方式
    
> 二次排序（除了对key排序，还需要对value排序），需要实现的接口：
    1. WritableComparable
    2. Partition
    
> hadoop优化：

    1. 程序角度
        a. 避免不必要的reduce任务
        b. 给job增加combiner阶段
        c. 处理非Text对象时，可以使用Writable对象，如IntWriteable
        d. 对字符串操作，使用StringBuilder/StringBuffer
        
    2. Hadoop参数
        a. 心跳监测时间
        b. compress压缩方式
        c. 推测执行（speculative execution）默认true
        d. 失败容忍比例 默认0 失败重新尝试次数 默认4
        e. mapred.task.timeout 默认任务超时时间10分钟
    3. Linux系统参数
    
    
---
## Hive

> hive内部表外部表详解

        - https://blog.csdn.net/qq_36743482/article/details/78393678
        - 内部表：加载数据到hive所在的hdfs目录，删除时，元数据和数据文件都删除
        - 外部表：不加载数据到hive所在的hdfs目录，删除时，只删除表结构。
            ```sql
            -- 将数据加载到外部表
             create external table testljb(id int) partitioned by (age int) location 'hdfs_file_path';
            ```
    
        - 生产环境建议使用外部表
            - 1、因为外部表不会加载数据到hive，减少数据传输、数据还能共享。
            - 2、hive不会修改数据，所以无需担心数据的损坏
            - 3、删除表时，只删除表结构、不删除数据。

> hive分区表：Hive分区更方便于数据管理，常见的有时间分区和业务分区。 

    -  show partitions table_name;
    ```
    ok
    ds=2018-07-02
    ds=2018-07-03

    ```
    - 分区表、动态分区
    

> insert into :将一张表的数据写到另一张表、override write :覆盖之前的内容。

> hive的 order by、sort by、distribute by、cluster by
    
    - orderBy 全局排序，因此只有一个reduce，会导致计算时间过长
    - sortBy 只能保证在同一个reduce中的数据可以按指定字段排序
    - distributeBy 控制在map端如何拆分数据给reduce端的。hive会根据distribute by后面列，对应reduce的个数进行分发，默认是采用hash算法。sort by为每个reduce产生一个排序文件。在有些情况下，你需要控制某个特定行应该到哪个reducer，这通常是为了进行后续的聚集操作。distribute by刚好可以做这件事。因此，distribute by经常和sort by配合使用
    
    - clusterBy  除了具有distribute by的功能外还兼具sort by的功能。但是排序只能是倒叙排序，不能指定排序规则为ASC或者DESC


    
---   
## HBase

[HBase基础](https://blog.csdn.net/u010967382/article/details/37878701)
> rowkey行键：行健以字典序排列，设计时充分利用这个特点，将经常一起查询的行健设计在一起，例如时间戳结尾，用户名开头（位置相关性）

> 对于同类的列簇,可以加一个前缀,每个columnfamily底层是一个文件

---
## Flume

> flume的配置
    1.source
    2.channel
    3.sink
    
> flume 与 log4j
    1.log4j
        - log4j采集的日志带有session id ,session可以通过redis共享,保证集群日志同一个session落到不同的tomcat中,而session id一样
        - 稳定,不宕机
        - 和项目结合过于紧密
    2.flume
        - 可插拔性 灵活

---   
## Kafka

> Kafka存储结构/data/ :"topic_name-partition" eg: /wordcounttopic-0/00000.log,00000.log


---
## Spark

> 提交时的参数
    
> 结构流

> RDD、DataSet、DataFrame

    1. RDD
        - 优点：编译时异常检查（类型安全）、直接操作对象
        - 缺点：序列化反序列化性能开销、受JVM的GC影响
        
    2. spark2.0后   DataFrame = DataSet[Row]
        - 优点：运行快，不受JVM控制
        - 缺点：不是类型安全
        
    3. DataSet：Dataset和DataFrame拥有完全相同的成员函数，区别只是每一行的数据类型不同
        -优点：类型安全编译时就可以发现类型错误、DataFrame/DataSet均支持sqorkSQL
        -缺点：运行稍微慢

> spark任务运行流程：

    ```
    我们使用spark-submit提交一个Spark作业之后，这个作业就会启动一个对应的Driver进程。根据你使用的部署模式（deploy-mode）不同，Driver进程可能在本地启动，也可能在集群中某个工作节点上启动。Driver进程本身会根据我们设置的参数，占有一定数量的内存和CPU core。而Driver进程要做的第一件事情，就是向集群管理器（可以是Spark Standalone集群，也可以是其他的资源管理集群）申请运行Spark作业需要使用的资源，这里的资源指的就是Executor进程。YARN集群管理器会根据我们为Spark作业设置的资源参数，在各个工作节点上，启动一定数量的Executor进程，每个Executor进程都占有一定数量的内存和CPU core。
    
    在申请到了作业执行所需的资源之后，Driver进程就会开始调度和执行我们编写的作业代码了。Driver进程会将我们编写的Spark作业代码分拆为多个stage，每个stage执行一部分代码片段，并为每个stage创建一批task，然后将这些task分配到各个Executor进程中执行。
    
    task是最小的计算单元，负责执行一模一样的计算逻辑（也就是我们自己编写的某个代码片段）
    ，只是每个task处理的数据不同而已。一个stage的所有task都执行完毕之后，会在各个节点本地的磁盘文件中写入计算中间结果，然后Driver就会调度运行下一个stage。下一个stage的task的输入数据就是上一个stage输出的中间结果。如此循环往复，直到将我们自己编写的代码逻辑全部执行完，并且计算完所有的数据，得到我们想要的结果为止。
    ```
    
    1. client 提交任务给Master
    2. Master 找到Driver、driver向master或yarn资源管理器申请分配资源给Executor
    3. driver将应用转为RDD Graph，DAGScheduler将RDD Graph转为Stage的DAG(有向无环图)提交给TaskScheduler
    4. TaskScheduler提交任务给Executetor执行。

> spark shuffle :shuffle过程，简单来说，就是将分布在集群中多个节点上的同一个key，拉取到同一个节点上，进行聚合或join等操作。比如reduceByKey、join等算子，都会触发shuffle操作

    - repartition 
    - ByKey 除了countByKey、如：groupByKey、reduceByKey
    - join
    
> spark on hive 需要拷贝配置文件、需要关联一个mysql-connection-.jar

> spark内存溢出
    
    1. Driver端：SparkContext、DAGScheduler、对RDD的stage切分
        - 增大spark.driver.memory 默认1G
    2. Map：rdd创建过多
    
    3. 数据倾斜
    
    4. shuffle：默认HashPartitioner
    
> 数据倾斜的原有：
    1. 数据问题
        - key 本身分布不均匀，包括大量key为空
        - key 设置不合理
    2. spark问题
        - shuffle并发度不够
        - 计算方式有误
    3. 后果：
        - 拖慢运行速度
        - 将某个executor造成OOM

> 解决数据倾斜的方式：

    1. 数据问题时：
        -  sample函数进行抽样；找出异常的key
            
            无效的数据：
                1. 对于无效数据进行filter
            
            有效的关键数据：
                1. 隔离执行，将异常的key过滤出来单独处理，最后与正常数据的处理结果进行union操作。
                2. 对key先添加随机值，进行操作后，去掉随机值，再进行一次操作。
                3. 使用reduceByKey 代替 groupByKey(reduceByKey用于对每个key对应的多个value进行merge操作，最重要的是它能够在本地先进行merge操作，并且merge操作可以通过函数自定义.)
                4. 使用map join。
                
    2. spark问题：
        - 提高shuffle并发度
        - map join 替代reduce join：将小表（<1G）广播到executor
  

> checkpoint周期设置成batch周期的5至10倍。

> cahce() 本质调用persist() ;使用persist（）可以制定对应的缓存级别；

> 列式存储的 parquet文件格式

> [parquet+spark](https://www.slideshare.net/SparkSummit/spark-parquet-in-depth-spark-summit-east-talk-by-emily-curtin-and-robbie-strickland)

> [parquet详解](https://www.jianshu.com/p/47b39ae336d5)


---
## Flink

[Flink源码解析](https://www.jianshu.com/u/a598db7c39b2)

---
## Redis

> Sentinel(哨兵模式) :

    ![image](https://img-blog.csdn.net/20170405093045288?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc2hvdWh1emhlemhpc2hlbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
    
    - 在某几个redis服务器开启哨兵模式,可以不间断检查master与slave是否正常运行。通知、故障转移

> redis数据结构
    
    1. String   字符串
        - set
        - get
        - setnx
        - mset 批量设置多个string的值
        - incr(key)
        - incrby(key,integer)
        
    2. Hash     哈希
        - hset(key, field, value)：向名称为key的hash中添加元素field
        - hget(key, field)：返回名称为key的hash中field对应的value
        - hmget(key, (fields))：返回名称为key的hash中field i对应的value
        - hmset(key, (fields))：向名称为key的hash中添加元素field
        - hincrby(key, field, integer)：将名称为key的hash中field的value增加integer
hexists(key, field)：名称为key的hash中是否存在键为field的域

    3. List     列表
        - rpush(k,v)
        - lpush(k,v)
        - llen(k)
    
    4. Set      集合
        - sadd(k,member)
        - srem(k,member)
        
    5. SortSet  有序集合
    
> redis持久化的方式
    1. RDB 内存快照 间隔写入（顺序读写） 格式与redis一致 加载较快
    2. AOF 日志 追加日志（顺序读写）


---
## Elasticsearch

> 脑裂问题：
    1. master >= 3  最少投票数超过节点一半以上来解决
    2. master = 2 



- http://blog.csdn.net/xfg0218/article/details/52514585
- http://blog.csdn.net/high2011/article/details/51594928
- https://blog.csdn.net/albg_boy/article/details/78424509
