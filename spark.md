# spark

## 安装scala
在官网下载源文件后，执行`sudo tar -zxvf scala-2.10.5.tgz -C /opt/scala/`

配置scala环境变量，打开`sudo gedit ~/.bashrc`，在末尾增加:
```
export SCALA_HOME=/opt/scala/scala-2.10.5
export PATH=${SCALA_HOME}/bin:$PATH
```
重启或者`source ~/.bashrc`，查看`scala -version`

## scala语法糖
=> 语法糖，用于创建函数的实例
[参考](https://stackoverflow.com/questions/6951895/what-does-and-mean-in-scala)

## 安装spark
在官网下载源文件后，执行`sudo tar -zxvf spark-1.6.3-bin-hadoop2.6.tgz -C /opt/spark/`

配置spark环境变量，打开`sudo gedit ~/.bashrc`，在末尾增加:
```
export SPARK_HOME=/opt/spark/spark-1.6.3-bin-hadoop2.6
export PATH=${SPARK_HOME}/bin:$PATH
```
重启或者`source ~/.bashrc`，查看`spark-shell`

## scala和java容器类转换
执行asJava时，提示value asjava is not a member of scala.collection.immutable.map，在调用前引用：
`import collection.JavaConverters._`

Scala和java之间有许多类似的容器类，可以用asJava和asScala进行转换操作

## json数据中出现’$’符号
spark sql在SELECT语句中出现’$’会报错，因此考虑将文本中的’$’符号删除
```
scala > “$$test in time”.replaceAll(“\\$”,””)
res7:String = test in time
```

## 保存数据hbase
saveAsHadoopDataset

当前采用saveAsHadoopDataset保存数据，虽然数据能够保存成功，但是在运行过程中会产生很多异常

[参考](https://archive.cloudera.com/cdh5/cdh/5/hbase-0.98.6-cdh5.3.9/book/ch15s15.html)

## 在集群上运行程序，找不到HBaseConfiguration
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

### mutable.MutableList生成的数据转换成dataframe，需要先引用toDF
```
import sqlContext.implicits._  ///是在调用toDF前引用，不是在文件的最前面
val fetchResDF = fetchRes.toDF()
println(fetchResDF.getClass())
fetchResDF.show()
```
[参考](https://stackoverflow.com/questions/39397652/convert-scala-list-to-dataframe-or-dataset)


## 更改spark的日志级别
`sc.setLogLevel(“WARN”)`

## 在程序中获取spark的context
```
val conf = new SparkConf().setMaster(“local”).setAppName(“Simple Application”)
val sc = new SparkContext(conf)
```

## a master url must be set in your configuration spark
Spark的HOST:PORT值找不到，在配置SparkSession时增加：
```
SparkSession spark = SparkSession.builder().appName("SomeAppName").config("spark.master", "local").getOrCreate();
```
[参考](https://stackoverflow.com/questions/38008330/spark-error-a-master-url-must-be-set-in-your-configuration-when-submitting-a)

## spark-submit配置优先级
SparkConf >flags passed to spark-submit > default file

## spark-submit deploy-mode
通常的部署策略是将你的程序提交到一个网关机器，在物理上和工作机器相连（比如在单独的EC2集群上的主节点），在这种情况下，client模式是合适的。

在client模式下，driver伴随着spark-submit线程同时启动，承担集群的客户角色。应用的输入和输出都跟console（工作台）关联，因此特别适合调用REPL（read-eval-print loop，读取-求值-输出-循环，也被称做交互式顶层构件，如Spark shell）的情景下。

相应地，如果你的应用从一个很远的机器提交到工作机器上（比如在你的笔记本上），通常使用cluster模式来减小drivers和executors之间的网络延迟。当前，standalone 模式不支持python应用的cluster模式。

## Spark访问neo4j
在sbt文件中添加依赖：
```
resolvers += "Spark Packages Repo" at "http://dl.bintray.com/spark-packages/maven"
libraryDependencies += "neo4j-contrib" % "neo4j-spark-connector" % "1.0.0-RC1"
```
执行spark-submit时添加依赖
```
spark-submit --packages neo4j-contrib:neo4j-spark-connector:1.0.0-RC1 SavedNeo4j.jar
```

### 在SparkConf的对象(以conf为例)中添加参数：
```
conf.set("spark.neo4j.bolt.url","bolt://localhost:7687")
conf.set("spark.neo4j.bolt.user","neo4j")
conf.set("spark.neo4j.bolt.password","xuchuyan")
```
调用Neo4jGraph的加载图函数：
```
val g = Neo4jGraph.loadGraph(sc,"node",Seq("relation"),"node")
```

### session.run方式，
```
//import org.neo4j.spark.Neo4jConfig
//val config =org.neo4j.spark.Neo4jConfig(sc.getConf)
//val driver = config.driver()
///import org.neo4j.driver.v1._.GraphDatabase
val driver =GraphDatabase.driver("bolt://localhost:7687",AuthTokens.basic("neo4j","xuchuyan"))
val session = driver.session()
val result = session.run("match(n) return n.name as name,n.originalId as originalId,n.oneId as oneId limit 10")
while(result.hasNext()){
println(result.next())
}
driver.close()
```
### tx.run方式
```
val tx = session.beginTransaction()
val result = tx.run("match(n) return n.name as name,n.originalId as originalId,n.oneId as oneId limit 10")
```
