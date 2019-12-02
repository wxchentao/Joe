# Docker基础实验

## 实验三

## 前提：购买了云服务器CentOS7.2

### 一、掌握Docker基础知识

#### （1）使用yum工具安装：

Modernize your applications, accelerate innovation
Securely build, share and run modern applications anywhere

1.1:![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/1.1.png)

### 二、安装Docker

#### （1）使用如下命令查看操作系统内核信息：

```
uname -r
```

2.1:![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/2.1.png)

顺带看一下Linux的版本号：

2.2:![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/2.2.png)

CentOS 7的应用程序库可能不是最新的，因此首先更新应用程序数据库

2.3![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/2.3.png)

#### （2）安装

接下来添加Docker的官方仓库，下载最新的Docker并安装：

```
curl -fsSL https://get.docker.com/ | sh
```

2.4![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/2.4.png)

#### （3）安装完成之后启动Docker守护进程，即Docker服务：

指令：sudo systemctl start docker

2.5![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/2.5.png)

验证Docker是否成功启动，得到类似如下图的输出：

2.6![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/2.6.png)

最后，确保Docker当服务器启动时自启动：

```
sudo systemctl enable docker
```

2.7![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/2.7.png)

此外，还可以查看一下Docker的版本信息：

2.8![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/2.8.png)

参考：https://blog.csdn.net/llfjfz/article/details/97964215

### 三、Docker的基本操作

#### （1）Docker的命令使用

查看docker所有的命令，键入：
docker

3.1![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/3.1.png)

查看当前系统docker的相关信息：

3.2![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/3.2.png)

可见当前并未安装任何镜像（Images），运行任何容器（Containers）。

#### （2）加载Docker镜像

Docker镜像是容器运行的基础，默认情况下，将从Docker Hub拉取镜像。首先使用search命令查询Docker Hub中的可用镜像，这里以查询可用的CentOS镜像为例，命令从Docker Hub拉取centos镜像的相关信息，并返回可用镜像的列表，输出结果类似于：

3.3![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/3.3.png)

接下来拉取官方版本(OFFICIAL)的镜像：

3.4![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/3.4.png)

一旦镜像下载完成，可以基于该镜像运行容器，使用run命令：

3.5![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/3.5.png)

查看一下当前系统中存在的镜像：

3.6![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/3.6.png)

#### （3）运行Docker容器

以上述的CentOS镜像为例运行其容器，使用-it参数进入交互shell模式：

3.7![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/3.7.png)

进行container内部shell，可以在此shell运行任何命令，比如安装Apache Web服务器：

3.8![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/3.8.png)

现在此容器已经安装了Apache Web服务器。注意：所有对于容器的更改只保存在当前运行的容器中，并未写入镜像。

#### （4）创建新的镜像

在前序操作的基础上，本小节将创建新的镜像，即提交更改到新的镜像。首先从容器的交互shell退出并保存状态，使用exit命令

我们首先使用如下命令查看本地中的容器：

3.9![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/3.9.png)

可以发现刚才运行的ID为9e77的容器也在列表之中。
现在使用commit命令来提交更改到新的镜像中，即创建新的镜像。命令格式

3.10![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/3.10.png)

这种提交类似于git协议的提交，同样这里提交的镜像只保存在本地。后续可以提交到远程镜像仓库，比如Docker Hub。
再次使用镜像查看命令：

3.11![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/3.11.png)

可以看到刚才创建的新镜像“centos:apache_web”，并且从大小（SIZE）来看(346MB)，区别于我们原始从Docker Hub拉取的CentOS的官方镜像(202MB)。

接下来要为新建的镜像打上标签（Tag），否则后续推送镜像到Docker Hub的时候将出现“ denied: requested access to the resource is denied”的错误。关于这个错误的解答详见[stackoverflow](https://stackoverflow.com/questions/41984399/denied-requested-access-to-the-resource-is-denied-docker)。
Tag命令的语法：

docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]

3.12![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/3.12.png)

完成之后，同样查看已存在的镜像：

3.13![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/3.13.png)

完成之后，同样查看已存在的镜像：

#### （5）推送镜像到远程镜像仓库

可以把本地镜像推送到远程镜像仓库，最为著名的就是Docker官方的Docker Hub。当然比如阿里也提供容器仓库，同时也可以自己构建镜像仓库。这里以Docker Hub为例介绍如何实现镜像推送。首先要到[Docker Hub](https://hub.docker.com/)上进行注册，然后这里我们使用shell登录：

3.14![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/3.14.png)

输入密码。用户名和密码都正确，随后会显示登录成功。
使用如下命令推送新创建的镜像：

3.15![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/3.15.png)

这里显示被拒绝了？？登陆网站去创建一下仓库

3.16![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/3.16.png)

再推一次，这里推送成功了

3.17![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/3.17.png)

3.18![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/3.18.png)

参考文献：https://blog.csdn.net/llfjfz/article/details/98596243

#### （6）在Docker的CentOS容器实例中安装WordPress

先将容器运行起来

3.1![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/images/3.1.png)

发现这个容器好像有点不对劲，查看一下已有镜像

3.2![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/images/3.2.png)

好像没有空的镜像，选择重新拉取一个版本为7的centos镜像（现在latest已经是8版本

3.3![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/images/3.3.png)

3.4![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/images/3.4.png)

3.5![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/images/3.5.png)

再度运行，发现错误还是一样

3.6![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/images/3.6.png)

无奈，遂去网上查询后决定修改一下配置文件，再重启服务

3.7![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/images/3.7.png)

3.8![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/images/3.8.png)

3.9![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/images/3.9.png)

再度运行一次后，容器内终于可以正常上网。根据安装WordPress的步骤，我们先安装Apache服务器

3.10![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/images/3.10.png)

3.11![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/images/3.11.png)

因为centos的七版本有些特殊，新建容器的指令和基础命令行要如下形式

3.12![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/images/3.12.png)

成功创建后进入容器

3.13![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/images/3.13.png)

将服务设为自启

3.14![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/images/3.14.png)

为了让投射端口可以看见，进入控制台开放所有端口

3.15![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/images/3.15.png)

安装数据库（以前实验有详解就不复述

3.16![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/images/3.16.png)

3.17![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/images/3.17.png)

启动数据库服务

3.18![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/images/3.18.png)

安装php

3.19![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/images/3.19.png)

3.20![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/images/3.20.png)

3.21![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/images/3.21.png)

3.22![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/images/3.22.png)

3.23![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/images/3.23.png)

3.24![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/images/3.24.png)

3.25![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/images/3.25.png)

设置数据库连接

3.26![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/images/3.26.png)

安装WordPress

3.29![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/images/3.29.png)

3.30![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/images/3.30.png)

使用8888端口查看网站，成功显示WordPress初始界面，就可以搭建新的网站了

3.31![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/images/3.31.png)

将完成的镜像推送

3.28![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/images/3.28.png)

3.33![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/images/3.33.png)

### 四、使用Dockerfile

#### （1）先下载好所需要的文件

4.1![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/4.1.png)

4.2![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/4.2.png)

编译dockerfile文档

4.5![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/4.5.png)

4.6![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/4.6.png)

4.7![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/4.7.png)

#### （2）然后使用build命令

4.3![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/4.3.png)

呃，报错了！在编译的时候不知道出了什么问题了

4.4![Alt](https://github.com/wxchentao/Joe/blob/master/images/third/4.4.png)

拜拜，晚安。dockerfile真的太难太麻烦了。



### 参考文献：林立老师的CSDN和诸多无用教程