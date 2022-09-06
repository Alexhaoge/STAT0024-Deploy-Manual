# Python和Anaconda
服务器安装了Anaconda3，管理所有Python相关的环境。**建议不要用系统自带的Python，尽可能使用conda的jupyterhub环境下的Python**
## Anaconda使用
conda应该在所有用户的环境变量中，可以直接调用，若出现`conda：未找到命令`之类的问题，输入`bash`或`source /etc/profile`即可解决。

conda管理python包的原理是创建虚拟的"conda环境"，不同的conda环境是彼此独立的，使用conda或者pip安装在“base”环境中的包，在“my”环境是看不到的。

首次使用conda需要初始化，输入
```shell
conda init bash
```
然后重新登陆，即可激活和切换环境
```shell
conda activate 环境名 # 激活或切换环境，不输环境名默认进入base
conda deacivate 环境名 # 退出环境
```

**常见问题**：大家可能会遇到提示说conda没有init，那种情况一般是命令行没进入bash而是在linux最原始的sh，此时anaconda根本没有载入，输入bash即可解决。

## conda环境
Anaconda用conda管理多个python和其他语言的环境，服务器上的conda环境有base和jupyterhub两个，base是默认的，但我们主要的pyspark和其他包都安装在jupyterhub这个环境。记得通过conda activate切换环境，不然可能会遇到缺少python库的错误。

通过`conda env list`可以看到所有的conda环境。

### PySpark所在的conda环境
PySpark和一些常用的科学计算库（如Pandas/NumPy/SciPy/Scikit-Learn）均在名为`jupyter`的conda环境，如果是ssh或者jupyterlab的命令行界面使用，需要先切换conda环境。**默认的Base下因为python版本问题无法正常使用pyspark！**
```bash
conda activate jupyter
```

### 创建自己的conda环境
现有的“jupyter”环境已经安装了课程需要的所有包，**同学们使用这个环境就够了，原则上不要安装自己的conda环境**

**创建环境会很占用磁盘**，如果只是安装一两个常用包，请联系老师或平台管理员安装

如果确实需要自己创建conda环境，**请一定要将存储在个人家目录下(一般是/data/home/stxxxxxxx目录下)，不要在家目录之外的目录存储！**，如果未来服务器空间不足，可能会删除普通用户的conda环境。
```shell
conda create -p 创建的环境的存储路径 python=3.7 # 选择python版本，如果需要连接pyspark需要使用3.10
conda activate 创建环境名 # 进入环境
```