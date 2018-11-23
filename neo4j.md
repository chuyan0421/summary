# Neo4j
## 介绍
Node：节点，A node in Neo4j is a node as described in the property graph model, with properties and labels

Relationships：关系，A relationship in Neo4j is a relationship as described in the property graph model, with a relationship type and properties.

Properties：属性

Labels：同一个node的标签不唯一，（首字母大写）

Traversal：遍历

Schema：模式可选择

Indexes：索引用于加快查询节点的速度
## 图数据库比较
在做增量式运算时，要把之间运行的图数据保存在图形数据库中，当有新数据加入进来的时候，将新数据和原本的数据导入graphx进行计算。

因此跟graphx相关的概念集中在图计算，而非图存储和查询领域。所以经常浏览db-engines的朋友们不难发现，图数据库列表里就没有graphx这一项。在比较图存储和图查询性能时，比较集合多是neo4j、orientdb、titan、arangodb等图数据库系统。而比较图计算时，比较集合多是graphlab、giraph、graphX。

orientdb 和neo4j提供graphx的接口

[orientdb 与 neo4j比较](https://db-engines.com/en/system/Neo4j%3BOrientDB)

[Titan与 neo4j比较](https://db-engines.com/en/system/Neo4j%3BTitan)

[Arangodb 与 neo4j比较](https://www.arangodb.com/why-arangodb/arangodb-vs-neo4j/)

## TinkerPop介绍
使用tinkerpop的gremlin graph traversal语言可以操作图数据库（oltp）和图计算引擎（olap）

[参考](https://stackoverflow.com/questions/35146651/what-is-the-difference-between-titan-and-spark-graphx-and-which-one-is-the-prefe/35164382)

tinkerpop的SparkGraphComputer的优点有二：

使用SparkGraphComputer可以直接对tinkerpop兼容的图数据库（如titan）进行图引擎算法，带来图数据库和图引擎的兼容性

SparkGraphComputer有更好的编程模型来开发定制化的图形算法，除非熟悉罐装算法（canned algorithms），否则只能放弃graphx的pregel API来定制化算法

## 启动
在neo4j安装目录的bin文件夹下执行
```
neo4j start
cypher-shell
```

## 在spark-shell运行时，报错:Unsupported authentication token, scheme 'none' is only allowed when auth is disabled.
没有授权
```
spark-shell --conf spark.neo4j.bolt.password=”password” --packages neo4j-contrib:neo4j-spark-connector:2.1.0-M4 (把”password”改为设置的密码)
```

## 导入数据到neo4j
有以下方式，[参考](http://paradoxlife.me/how-to-insert-bulk-data-into-neo4j)

create

load csv

batch inserter

batch import

neo4j-import

在neo4j browser上每次只能执行一个指令，调用neo4j/bin下的`neo4j-shell -file /point/to/loadCsv `可以执行对应的文件的多个语句，每个语句之间用分号“;”隔开

loadCsv文件的内容为：
```
MATCH (n) DETACH DELETE n;
LOAD CSV FROM 'file:///combineVertices.csv' AS line
CREATE (:node { name: line[1], originalId: line[2], hash: line[0], oneId: line[3] });
LOAD CSV FROM 'file:///combineEdges.csv' AS line2
MATCH(a:node {hash:line2[0]}),(b:node {hash:line2[2]})
CREATE (a) -[:relation {role:line2[1]}] -> (b);
/////CREATE CONSTRAINT ON (n:node) ASSERT n.hash IS UNIQUE
```
## 出现connect refuse的错误，
要更改neo4j/conf/neo4j.conf文件关于neo4j shell的配置
```
dbms.shell.enabled=true
dbms.shell.host=127.0.0.1
dbms.shell.port=1337
```
注意：根据neo4j的版本不同，语句会不一样
[参考1](https://stackoverflow.com/questions/29628204/trying-to-execute-a-list-of-cypher-statements-in-neo4j-via-the-admin-interface/29628996)
[参考2](https://stackoverflow.com/questions/35316074/cannot-connect-to-remote-neo4j-server-using-neo4j-shell)

## 访问远程neo4j
在修改./conf/neo4j.conf文件，找到下面两行，解注释掉：
```
dbms.connectors.default_listen_address=0.0.0.0
dbms.connector.bolt.listen_address=:7687
```
保存后重启neo4j


## 清空库
```
match (n)
detach delete n
```

## ORDER输出结果排序
```
MATCH (a:Person)-[:ACTED_IN]->(m:Movie)RETURN a,count(*) AS appearances
ORDER BY appearances DESC LIMIT 10;
```

## COLLECT集合
```
MATCH (m:Movie)<-[:ACTED_IN]-(a:Person)
RETURN m.title AS movie, collect(a.name) AS cast, count(*) AS actors
```

## UNION联合
```
MATCH (actor:Person)-[r:ACTED_IN]->(movie:Movie)
RETURN actor.name AS name, type(r) AS acted_in, movie.title AS title
UNION
MATCH (director:Person)-[r:DIRECTED]->(movie:Movie)
RETURN director.name AS name, type(r) AS acted_in, movie.title AS title
```

## WITH链式执行
```
MATCH (person:Person)-[:ACTED_IN]->(m:Movie)
WITH person, count(*) AS appearances, collect(m.title) AS movies
WHERE appearances > 1
RETURN person.name, appearances, movies
```

## 在已存在的节点之间添加关系，通过index的方式实现，
[参考](https://stackoverflow.com/questions/20456002/adding-relationship-to-existing-nodes-with-cypher)
