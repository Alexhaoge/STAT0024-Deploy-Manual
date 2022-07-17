# 计算实验环境使用说明
本服务器（10.40.13.202）是统计与数据科学学院的“分布式存储与计算”和“数据库课程”的上机实验用服务器，使用前请阅读此说明。
## 注意事项
1. 服务器**磁盘空间有限**，请尽量将文件放在个人家目录下`~/`，不要放在根目录下`/`（因为根目录挂载的磁盘分区只有50GB空间，家目录`/home`挂载了单独一个1个TB的磁盘分区，使用命令`df -h`可以查看磁盘使用情况）
2. 服务器内存大致为32GB，**请注意内存使用**，使用率超过90%会导致整个服务器的卡**顿甚至宕机**！
3. 服务器有一个四核的Intel Xeon CPU，足以满足教学中大多数实验的要求，但对于需要长时间作业的任务（如网格搜索），请尽量控制运行时间或在班级群中提前告知其他人。

## 目录
+ 分布式存储与计算
    + [JupyterHub使用说明](dsc/jupyter/)
    + [Anaconda使用说明](dsc/conda/)
    + [PySpark使用说明 - 怎样在JupyterHub中使用PySpark](dsc/pyspark/)
    + [Scala使用说明 - 如何启动Scala的Spark环境](dsc/scala/)
    + [HDFS使用说明 - 为什么我读不了数据集](dsc/hdfs/)
    + [SSH和Linux服务器使用说明](dsc/sshlinux/)
    + [管理员使用说明](dsc/admin/)
+ 数据库
    + [MySQL使用说明](mysql/)

## 一些参考资料
+ 林子雨 - 子雨大数据之Spark入门教程（Scala版）  
    [http://dblab.xmu.edu.cn/blog/spark/](http://dblab.xmu.edu.cn/blog/spark/)  
+ 林子雨 - 子雨大数据之Spark入门教程(Python版)  
    [http://dblab.xmu.edu.cn/blog/1709-2/](http://dblab.xmu.edu.cn/blog/1709-2/)  
+ Spark官方文档  
    [https://spark.apache.org/docs/3.0.1/](https://spark.apache.org/docs/3.0.1/)  
    多看官方文档，比网上的博客更权威和详细