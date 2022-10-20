# Rstudio Server 使用说明
服务上安装了Rstuio Server，地址为[http://{{config.extra.site_ip}}:8787](http://{{config.extra.site_ip}}:8787)。用户名和密码与linux/JupyterHub相同

鉴于资源有限，不建议对学生开放。我们设置了用户组限制，必须在rstudio-users用户组才能登陆，通过如下命令将用户添加至用户组
```shell
adduser 用户名 rstudio-users
```

在Rstuio界面可以通过`install.packages`安装R包至个人目录，如果是一些常用的比较大的包，建议通过ssh命令行，使用root权限安装到系统目录供所有用户使用。一些R包安装过程中需要对应的linux库支持，按照R的错误提示，使用apt命令安装即可。