# 杂项

### 自定义jupyterhub的html
~~为了方便前台修改密码和激活用户，我重写了jupyterhub的html文件，存储在`/usr/local/anaconda3/envs/jupyterhub/share/jupyterhub/templates/`，主要是修改了`admin.html`和`home.html`，如果升级jupyterhub将会覆盖这两个文件，所以升级前应做好备份。~~

## 使用sbt打包Scala程序后spark-submit
https://spark.apache.org/docs/3.3.0/quick-start.html#self-contained-applications  
目前sbt虽然安装了但是访问权限有问题可能还无法使用，如果有同学愿意帮忙配置请联系老师或管理员。