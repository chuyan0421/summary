# 开发环境安装

## 安装java
在官网下载源文件后，执行`sudo tar -zxvf jdk-8u112-linux-x64.tar.gz -C /usr/lib/java/`

配置JAVA环境变量，打开'sudo gedit ~/.bashrc'，在末尾增加:
```
export JAVA_HOME=/usr/lib/java/jdk1.8.0_112
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:${JRE_HOME}/bin:$PATH
```
重启或者`source ~/.bashrc`，查看版本`java -version`

## 安装scala
在官网下载源文件后，执行`sudo tar -zxvf scala-2.10.5.tgz -C /opt/scala/`

配置scala环境变量，打开`sudo gedit ~/.bashrc`，在末尾增加:
```
export SCALA_HOME=/opt/scala/scala-2.10.5
export PATH=${SCALA_HOME}/bin:$PATH
```
重启或者`source ~/.bashrc`，查看`scala -version`

## 安装spark
在官网下载源文件后，执行`sudo tar -zxvf spark-1.6.3-bin-hadoop2.6.tgz -C /opt/spark/`

配置spark环境变量，打开`sudo gedit ~/.bashrc`，在末尾增加:
```
export SPARK_HOME=/opt/spark/spark-1.6.3-bin-hadoop2.6
export PATH=${SPARK_HOME}/bin:$PATH
```
重启或者`source ~/.bashrc`，查看`spark-shell`

## 安装hadoop
下载源文件后，执行`sudo tar -zxvf hadoop-2.6.4.tar.gz -C /usr/local/hadoop/`

配置hadoop环境变量，打开`sudo gedit ~/.bashrc`，在末尾增加:
```
export HADOOP_HOME=/usr/local/hadoop/hadoop-2.6.4
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export HADOOP_COMMON_HOME=$HADOOP_HOME
export HADOOP_HDFS_HOME=$HADOOP_HOME
export YARN_HOME=$HADOOP_HOME
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
export PATH=$PATH:$HADOOP_HOME/sbin:$HADOOP_HOME/bin
export HADOOP_INSTALL=$HADOOP_HOME
```
配置hadoop
### 1更改hadoop-2.6.4/etc/hadoop/hadoop-env.sh文件
export JAVA_HOME=/usr/lib/java/jdk1.8.0_112 （系统对应的jdk路径）

### 2更改hadoop-2.6.4/etc/hadoop/core-site.xml文件
```
<configuration>
    <property>
        <name>fs.default.name</name>
        <value>hdfs://localhost:8020</value>
    </property>
</configuration>
```
### 3更改hadoop-2.6.4/etc/hadoop/hdfs-site.xml文件
```
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
</configuration>
```
### 4从mapred-site.xml.template复制成mapred-site.xml再更改
```
<configuration>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
</configuration>
```
### 5更改hadoop-2.6.4/etc/hadoop/yarn-site.xml文件
```
<configuration>
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
</configuration>
```
[参考1](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/SingleCluster.html)
[参考2](https://www.yiibai.com/hbase/hbase_installation.html)

## 验证hadoop安装
```
cd ~
hdfs namenode -format
start-dfs.sh
```
访问http://localhost:50070
```
start-yarn.sh
```
访问http://localhost:8088/  （不会马上显示网页，需要耐心等待）


### 执行start-dfs.sh时报错：Unable to load native-hadoop library for your platform
在hadoop-2.6.4/etc/hadoop/hadoop-env.sh文件中添加
`export HADOOP_OPTS="$HADOOP_OPTS -Djava.library.path=$HADOOP_HOME/lib/native"`

### 执行start-dfs.sh时报错：localhost: ssh: connect to host localhost port 22: Connection refused
重装openssh
```
sudo apt-get remove openssh-client openssh-server
sudo apt-get install openssh-client openssh-server
```
在hadoop-2.6.4/etc/hadoop/hadoop-env.sh文件中添加
`export HADOOP_SSH_OPTS="-p 22"``

## 安装hbase
下载源文件后，执行`sudo tar -zvxf hbase-1.2.0-bin.tar.gz -C /usr/local/hbase/`

### 1更改hbase-1.2.0/conf/hbase-env.sh文件
`export JAVA_HOME=/usr/lib/java/jdk1.8.0_112 （系统对应的jdk路径）`

### 2更改hbase-1.2.0/conf/hbase-site.sh文件
```
<configuration>
   <property>
      <name>hbase.rootdir</name>
      <value>hdfs://localhost:8020/hbase</value>
   </property>
   <property>
      <name>hbase.cluster.distributed</name>
      <value>true</value>
   </property>
</configuration>
```

### 3验证
启动hbase，执行指令
```
bin/start-hbase.sh
hadoop fs -ls /hbase
```

## zookeeper安装
下载源文件后，执行`sudo tar -zxvf zookeeper-3.4.10.tar.gz -C /usr/local/hadoop/``
配置spark环境变量，打开`sudo gedit ~/.bashrc`，在末尾增加:
```
export ZOOKEEPER_HOME=/usr/local/zookeeper/zookeeper-3.4.10
export PATH=$PATH:$ZOOKEEPER_HOME/bin
source ~/.bashrc
```
在conf目录下执行： `cp zoo_sample.cfg zoo.cfg`

更改zoo.cfg内容：`dataDir=/home/yanzi/zookeeper`

启动zookeeper：`zkServer.sh start`

## kafka安装
下载源文件后，执行`sudo tar -zxvf kafka-0.10.2.2.tgz -C /usr/local/kafka/``
配置spark环境变量，打开`sudo gedit ~/.bashrc`，在末尾增加:
```
export KAFKA_HOME=/usr/local/kafka/kafka_2.10-0.10.2.2
export PATH=$PATH:$KAFKA_HOME/bin
source ~/.bashrc
```
更改config/server.properties内容：
```
broker.id=1
log.dir=/home/yanzi/kafka/logs-1
```

进入到kafka的路径下：`bin/zookeeper-server-start.sh -daemon config/zookeeper.properties `

[参考](https://blog.csdn.net/unknownli/article/details/45340107)

## 虚拟机作为服务器
[参考](https://blog.csdn.net/hk2291976/article/details/52983451)

## 更改win10下host文件
[参考](https://jingyan.baidu.com/article/624e7459b194f134e8ba5a8e.html)
