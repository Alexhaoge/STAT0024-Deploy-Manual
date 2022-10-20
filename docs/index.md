# {{config.site_name}}
本服务器（{{config.extra.site_ip}}）是统计与数据科学学院的“分布式存储与计算”和“数据库课程”的上机实验用服务器，使用前请阅读此说明。

## 注意事项
虽然服务器硬件配置很好，但考虑到分布式存储与计算课程选课人数非常多，服务器在高峰期的负载会非常大，因此请务必:  

1. **所有spark作业请连接到本地已有的master(spark://{{config.extra.site_ip}}:7077)，不要新建local集群**  
2. 在使用spark-shell后**及时`Ctrl+D`退出**  
3. 在使用pyspark后及时**输入`exit()`退出**  
4. 在使用notebook结束后，及时**在jupyterlab中结束kernel**  
5. 避免长期存储特别大的文件（GB级）  
6. 避免运行内存负载特别大的程序（10GB以上）  

如不遵守上述原则，系统/管理员可能会终止你的作业导致数据丢失，若长时间严重占用系统资源，则可能会暂时禁用你的jupyterhub和系统用户访问权限。

课程结束一个月后我们将删除学生账号，届时个人目录下的文件会无法恢复，请及时备份个人文件。

## 目录
+ 分布式存储与计算
    + [JupyterHub使用说明](dsc/jupyter/)
    + [Anaconda使用说明](dsc/conda/)
    + [PySpark使用说明 - 怎样在JupyterHub中使用PySpark](dsc/pyspark/)
    + [Scala使用说明 - 如何启动Scala的Spark环境](dsc/scala/)
    <!-- + [HDFS使用说明 - 为什么我读不了数据集](dsc/hdfs/) -->
+ 数据库
    + [MySQL学生使用说明](mysql/)
+ 管理手册（学生用户请忽略）
    + [Spark&Hadoop](admin/spark_hadoop/)
    + [JupyterHub](admin/jupyter/)
    + [Rstudio](admin/rstudio/)
    + [MySQL](admin/mysql.md)
    + [SSH和Linux服务器使用说明](admin/sshlinux/)
    + [文件路径记录](admin/fs.md)
+ 归档

## 一些参考资料
+ 林子雨 - 子雨大数据之Spark入门教程（Scala版）  
    [http://dblab.xmu.edu.cn/blog/spark/](http://dblab.xmu.edu.cn/blog/spark/)  
+ 林子雨 - 子雨大数据之Spark入门教程(Python版)  
    [http://dblab.xmu.edu.cn/blog/1709-2/](http://dblab.xmu.edu.cn/blog/1709-2/)  
+ Spark官方文档  
    [https://spark.apache.org/docs/3.3.0/](https://spark.apache.org/docs/3.3.0/)  
    多看官方文档，比网上的博客更权威和详细