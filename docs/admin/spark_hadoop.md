<!-- # 管理员手册
为方便日后维护和迁移，在此记录一些jupyterhub和Spark的配置方法 -->
# 管理Spark和HDFS
## 启动和关闭
Spark和Hadoop的部署其实可配置的东西很复杂，但是多数教学用不到而且毕竟麻烦，所以我们只启动最简单的Spark Standalone和HDFS
```shell
$SPARK_HOME/sbin/start-all.sh # 启动spark
$SPARK_HOME/sbin/stop-all.sh # 关闭spark
$HADOOP_HOME/sbin/start-dfs.sh # 启动hdfs
$HADOOP_HOME/sbin/stop-dfs.sh # 关闭hdfs
# 如果环境变量没有找到上面的脚本，输入source /etc/profile
```
spark和hadoop配置成系统服务比较困难，目前加到了开机项，但不保证遇到一些特殊原因会自行关闭，如果发现连接不上，需要手动启动。

目前spark集群有一个master，一个worker节点，则启动时命令如下
```shell
$SPARK_HOME/sbin/start-master.sh
$SPARK_HOME/sbin/start-worker.sh 
```

Spark不能使用root启动，因为启动用户通过ssh连接master和worker，而root出于安全考虑被禁止ssh登录

## WebUI
HDFS 10.40.13.202:50070  
Spark Master节点 10.40.13.202:8080  
spark-shell/pyspark的应用监控：10.40.13.202:4040,4041,4042,...
为避免占用过多服务器资源，spark-defaults.conf中我们可以禁用spark-shell/pyspark的ui（`spark.ui.enabled false`）。

### 通过WebUI监控spark作业
在10.40.13.202:8080中可以看到当前spark中运行的application，可以手动结束占用资源过大的作业，也可以通过网页提供的链接访问具体某个作业的WebUI，如果网页访问失败，请注意网址域名是否变成了chenserver.chen.server，将其改为10.40.13.202重新访问即可。

另外，请通过`netstat -nltp`命令可以查看4040、4041、4042...这一族端口的开启情况，并结合8080网页的作业情况对比，是否有学生用户自行创建了新的spark cluster，如果有，请及时终止其进程。

## 安装配置
这部分网上的教程很多，主要就是这几个步骤

+ 拷贝ssh密钥配置免密
+ 下载软件
+ 修改环境变量配置JAVA_HOM/HADOOP_HOME/SPARK_HOME
+ 修改Spark和Hadoop自身配置
    - 对于spark主要是$SPARK_HOME/conf/spark-env.sh配置好一些变量
    - 对于hadoop是$HADOOP_HOME/etc/hadoop/，这里比较复杂需要参考网上教程，然后初始化namenode
+ 用安装目录sbin文件夹内的脚本启动服务
+ Jps查看已启动进程判断是否成功，HDFS是看到namenode、secondarynamenode、datanode各一个，Spark是看到Master和Worker各一个

环境变量修改/etc/profile直接为所有用户配置比较省事，但保险起见spark和hadoop自己的环境变量仍要写一遍。

<!-- ```shell
export JAVA_HOME="/usr/lib/jvm/java-1.8.0-openjdk"
export HADOOP_HOME="/usr/local/hadoop"
export SPARK_HOME="/usr/local/spark"
export SCALA_HOME="/usr/share/scala"
export CLASSPATH="$CLASSPATH:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar"
export HIVE_HOME="/usr/local/hive"
export HADOOP_CLASSPATH="$JAVA_HOME/lib/tools.jar:$HADOOP_CLASSPATH"
export PATH="$PATH:/usr/local/bin:/usr/local/sbin:/usr/bin:/usr/sbin:/root/bin:$JAVA_HOME/bin:/usr/local/anaconda3/bin:$HADOOP_HOME/bin:$SPARK_HOME/bin:$HIVE_HOME/bin"
export COURSIER_CACHE="/usr/share/coursier/cache" # 这个是Scala的第三方库下载位置
```-->

### 动态资源分配
Spark默认配置会给每个application的executer分配所有可用的cpu，直至application退出才释放。而我们的课程中学生多使用交互式spark而非作业提交，不终止notebook/session时application根本不会退出，在多人同时连接时其他人的程序会一直等待，因此我们需要设置每个excutor可使用的最大cpu核数，以及动态资源分配，以便在用户不运行新的交互式命令，系统自动释放对应的executor。详细配置请查阅[spark官方文档](https://spark.apache.org/docs/latest/job-scheduling.html)。

在spark-defaults.conf中添加如下设置
```shell
# 动态资源分配
spark.dynamicAllocation.enabled                         true
## 设置每个executer在闲置30s后释放
spark.dynamicAllocation.executorIdleTimeout             30s
## 设置每个executer的缓存在闲置两天后也释放
spark.dynamicAllocation.cachedExecutorIdleTimeout       2d
spark.dynamicAllocation.shuffleTracking.enabled         true
spark.shuffle.service.enabled                           true
# 每个executor启动时默认分配的cpu个数
spark.deploy.defaultCores                               2
# 最大分配的cpu个数
spark.cores.max                                         4
```


### 示例配置文件
spark-env.sh
```shell
SPARK_LOCAL_IP=10.40.13.202
SPARK_LOG_DIR=/data/spark/logs

SPARK_SSH_OPTS="-p 9933"
JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
SPARK_HOME=/usr/local/spark-3.3.0-bin-hadoop3
SCALA_HOME=/usr/share/scala-2.11

PYSPARK_PYTHON=/usr/local/anaconda3/envs/jupyter/bin/python
PYSPARK_DRIVER_PYTHON=/usr/local/anaconda3/envs/jupyter/bin/python
```
spark-defaults.conf
```shell
spark.master                    spark://10.40.13.202:7077
spark.dynamicAllocation.enabled                         true
spark.dynamicAllocation.executorIdleTimeout             30s
spark.dynamicAllocation.cachedExecutorIdleTimeout       2d
spark.dynamicAllocation.shuffleTracking.enabled         true
spark.shuffle.service.enabled                           true
spark.deploy.defaultCores                               2
spark.cores.max                                         4
```




## 关于Scala
目前jupyter并未安装scala的kernel

scala的配置相比python有一些复杂，且apache-toree文档不是很完善，目前没有安装，如果后续需要可以尝试
<!-- ~~尝试了apache-toree，无法启动kernel，提示说scala缺少类。~~

~~jupyterhub上目前的scala kernel是[almond](https://github.com/almond-sh/almond)，虽然可以正常使用，但按照官方文档的方式运行spark时，无法同时运行两个context，一个人启动kernel时，另一个人的作业完全无法进行，所以暂时不要使用。~~

~~Scala的包下载器coursier和cs都下载在/home/hadoop目录下，设置了COURSIER_CACHE，并修改这个文件夹使其对所有用户可读可写可执行，但sbt普通用户打包时仍提示权限不足~~ -->