# graphx
## 介绍
限制计算类型，图平行（graph-parallel，如Pregel，PowerGraph）系统，在进行图像算法时优于数据平行系统（data-parallel如Hadoop MapReduce），但不适合从数据库中提取图，消除同个范围内的顶点，
数据平行系统会产生复杂的join操作和过多的数据转换

GraphX通过统一的抽象使得同样的数据可以通过图像或者表格展示而不需要数据转移和复制。

GraphX提供标准的数据平行操作（map，reduce，filter，join），也提供图平行操作（subgraph，mrTriplets）
每个点（vertex）和边（edge）都有属性（properties），属性是可查看的（如用户轮廓，时间戳，权重），模型参数和算法状态也是可查看的（如PageRank，latent factor，信息）

结构和属性的隔离是GraphX系统的重要部分

数据平行的计算通过在独立的资源内计算独立的数据来获得平行性，相应的，图平行的计算获得平行性的方法是依据处理资源将图（依赖的）数据分块，然后通过交互计算和通信来解决依懒性（沿着边）

图平行抽象的方法定位的视角是很窄的（the narrow view of computation），因此在建立和变换图结构，在不同的独立图之间跨越操作这些表达时是不适合的。比如有一个已经存在图（消费者关系），把外部数据（销售信息）集成进来

GraphX用64位的整形作为顶点id（不要求是连续的），这个id可以直接获取（如用户ids），也可以通过hash函数来转换（如页面url）

triplets，((i, j), (Pv(i), Pe(i, j), Pv(j)))，边和两个点及3者的属性组合在一起，在图操作中有广泛的用途，与filter一起可以提取跨越两个区域的边，也可以把两个有不同兴趣的用户联系起来。

mapV，mapE改变顶点和边的属性而不改变图的结构

leftjoin，在原本的图的点属性中包含了新的图信息，通过提取该信息并组成新的图

mrTriplets，是triplets，map，reduce的组合，

RDD，resilient distributed dataset 弹性分布式数据集


## 使用saveAsTextfile会生成多个文件
使用saveAsTextfile函数保存节点和边信息时，在hadoop会出现多个文件
分布式计算时会把文件保存成个多个partitions
想保存成一份，在调用saveAsTextfile前把所有数据放在1个partition上，但这样会牺牲速度
```
data.coalesce(1,true).saveAsTextFile()
```
或者将生成的多分文件通过hadoop自带的指令合在一起
```
hadoop fs -getmerge /output/dir/on/hdfs/ /desired/local/output/file.txt
hadoop fs -cat /some/where/on/hdfs/job-output/part-r-* > TheCombinedResultOfTheJob.txt
```

## 使用saveAsTextfile生成csv文件中有括号
```
graph.vertices.map(x=>x._1+","+ x._2).saveAsTextFile()
```
