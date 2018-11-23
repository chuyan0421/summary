# spark

## spark中访问hbase
### 保存数据
saveAsHadoopDataset

当前采用saveAsHadoopDataset保存数据，虽然数据能够保存成功，但是在运行过程中会产生很多异常

[参考](https://archive.cloudera.com/cdh5/cdh/5/hbase-0.98.6-cdh5.3.9/book/ch15s15.html)

### 在集群上运行程序，找不到HBaseConfiguration
java.lang.NoClassDefFoundError: org/apache/hadoop/hbase/HBaseConfiguration

找不到HBaseConfiguration这个类，因为我们是通过spark-submit来启动程序的，所以要在spark-env.sh的配置文件中加上Hbase的lib
```
export SPARK_CLASSPATH=$SPARK_CLASSPATH:/usr/local/hbase/hbase-1.2.0/lib/*
```
更改完毕后，执行`source spark-env.sh`

或者在运行的时候添加到--jars后面（建议采取这种方式）
```
spark-submit --master local --jars /usr/hdp/2.5.5.0-157/hbase/lib/hbase-common.jar,/usr/hdp/2.5.5.0-157/hbase/lib/hbase-client.jar,/usr/hdp/2.5.5.0-157/hbase/lib/hbase-server.jar,/usr/hdp/2.5.5.0-157/hbase/lib/hbase-protocol.jar,/usr/hdp/2.5.5.0-157/hbase/lib/htrace-core-3.1.0-incubating.jar,/usr/hdp/2.5.5.0-157/hbase/lib/guava-12.0.1.jar  OneId.jar
```

[参考](https://blog.csdn.net/mtj66/article/details/52366336)（参考文档中因为通过hadoop启动，所以要在HADOOP_CLASSPATH下更改）

### 创建Hbase表时显示已存在，list显示却没有
执行`zkCLI.sh`

`ls /hbase/table`发现不为空，将相应的表痕迹删除即可

`rmr /hbase/table/”table_name” (将”table_name”替换成相应的表名即可)`
[参考](https://blog.csdn.net/huashao0602/article/details/77050929)

## Hive操作
```
>hive
>show databases;
>use xxDatabase;
>show tables;
>describe xxDatabase.xxTable;
```

## 在spark中访问hive
### hiveContext
返回回来的数据带不同的格式，（sc是SparkContext的引用）
```
val hiveContext = new org.apache.spark.sql.hive.HiveContext(sc)
hiveContext.setConf("hive.metastore.uris", "thrift://METASTORE:9083")
hiveContext.sql("use dtplatform")
val isalesData=hc.sql("select user_id,email,phone from dtplatform.dim_us_user")
```

### jdbc
返回回来的数据是string格式

进入spark的bin文件下，执行
```
./beeline
!connect jdbc:hive2://192.168.33.25:10000
```
输入用户名和密码之后即可进入，采用hive的语句即可调用远程的hive数据库
[参考]
(https://stackoverflow.com/questions/39997224/how-to-connect-to-remote-hive-server-from-spark)

## mutable.MutableList生成的数据转换成dataframe，需要先引用toDF
```
import sqlContext.implicits._  ///是在调用toDF前引用，不是在文件的最前面
val fetchResDF = fetchRes.toDF()
println(fetchResDF.getClass())
fetchResDF.show()
```
[参考](https://stackoverflow.com/questions/39397652/convert-scala-list-to-dataframe-or-dataset)
