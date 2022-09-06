# 文件路径记录
1. 服务器有三块物理硬盘，通过BIOS将两块400G的硬盘组成了虚拟磁盘阵列作为800G的系统盘，挂载点为`/`，另一块1T的作为数据盘，挂载点为`/data`，大规模数据请存储与1T的硬盘中，系统软件安装在系统盘
2. 为减少系统盘占用，管理员用户的家目录在`/home`，普通学生用户家目录在`/data/home`
3. java和scala由apt自动安装，目录分别在`/usr/lib/jvm/java-11-openjdk-amd64`和`/usr/share/scala-2.11`
4. spark目录在`/usr/local/spark-3.3.0-bin-hadoop3`
5. Anaconda在`/usr/local/anaconda3`，带有jupyterhub和pyspark的conda环境在`/usr/local/anaconda3/envs/jupyter`