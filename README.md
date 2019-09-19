# 云计算开发基础

## 实验一

### 一、购买腾讯云服务器并登录

#### （1）购买腾讯云服务器

在腾讯实名认证完就可以享受学生特价优惠了，不过并非腾讯平台独有，阿里云和亚马逊也可以使用。

如果要一起购买域名的话，记得要进行网站备案。

1.1:![image](https://github.com/wxchentao/Joe/blob/master/images/first/1.1.png)

#### (2)使用WebShell登录已购买的云服务器实例

1.2:![Alt]https://github.com/wxchentao/Joe/blob/master/images/first/1.2.png

1.3:![Alt]https://github.com/wxchentao/Joe/blob/master/images/first/1.3.png

1.4:![Alt]https://github.com/wxchentao/Joe/blob/master/images/first/1.4.png

1.5:![Alt]https://github.com/wxchentao/Joe/blob/master/images/first/1.5.png

#### （3）下载安装Xshell，并使用Xshell登录腾讯云实例

下载过程很简单，上网搜用于个人研究的非商业途径就可以从官网免费下载简单安装Xshell。

打开Xshell，新建会话。

1.6:![Alt]https://github.com/wxchentao/Joe/blob/master/images/first/1.6.png

输入腾讯云服务器的公网ip和登录密码。

1.7:![Alt]https://github.com/wxchentao/Joe/blob/master/images/first/1.7.png

1.8:![Alt]https://github.com/wxchentao/Joe/blob/master/images/first/1.8.png

### 二、创建GitHub项目并在本地同步

#### （1）注册GitHub账号：https://github.com/

很简单，总之点开注册申请之后按着步骤走。

#### （2）在GitHub上创建云计算项目（CloudComputing）并在本地同步

先在网站上创立项目，填写名称。

2.1:![Alt]https://github.com/wxchentao/Joe/blob/master/images/first/2.1.png

在本地上下载安装好GIt。运行git-bash命令行。

2.2:![Alt]https://github.com/wxchentao/Joe/blob/master/images/first/2.2.png

执行ls -al ~/.ssh命令行，创建新的ssh key，再关联自己的GitHub邮箱

2.3![Alt]https://github.com/wxchentao/Joe/blob/master/images/first/2.3.png

直接回车后，在“C:\Users\UserName”文件夹下生成“id_rsa”和“id_rsa.pub”两个文件，分别对应私钥和公钥。随后复制“id_rsa.pub”的内容到GitHub网站的Settings–>SSH and GPG keys中：

2.4![Alt]https://github.com/wxchentao/Joe/blob/master/images/first/2.4.png

测试SSH Key是否配置成功

2.5![Alt]https://github.com/wxchentao/Joe/blob/master/images/first/2.5.png

使用GitBash配置自己的用户名和GitHub邮箱

2.6![Alt]https://github.com/wxchentao/Joe/blob/master/images/first/2.6.png

cd一处文件夹，然后git init将其初始化。随后拷贝GitHub网站中的项目网址

2.7![Alt]https://github.com/wxchentao/Joe/blob/master/images/first/2.7.png

添加远程代码仓库的URL，随后拉取数据

2.8![Alt]https://github.com/wxchentao/Joe/blob/master/images/first/2.8.png

touch README.md新建README文档指令，添加文件夹中的所有文件，提交文件：

2.9![Alt]https://github.com/wxchentao/Joe/blob/master/images/first/2.9.png

这里我的提交出了点问题，网上查询了解决方法之后成功

2.10![Alt]https://github.com/wxchentao/Joe/blob/master/images/first/2.10.png

2.11![Alt]https://github.com/wxchentao/Joe/blob/master/images/first/2.11.png

### 三、本地安装VMwareWorkstation和CentOS操作系统

#### （1）自行安装VMwareWorkStation

网上下载一个VM10以及CentOS7.6的镜像即可，按步骤next安装前者

#### （2）在VMwareWorkStation安装CentOS操作系统

先点开新建虚拟机，选择典型

3.1![Alt]https://github.com/wxchentao/Joe/blob/master/images/first/3.1.png

选择下载的CentOS7.6镜像（iso文件）

3.2![Alt]https://github.com/wxchentao/Joe/blob/master/images/first/3.2.png

选择Linux操作系统和对应版本

3.3![Alt]https://github.com/wxchentao/Joe/blob/master/images/first/3.3.png

选择安装位置

3.4![Alt]https://github.com/wxchentao/Joe/blob/master/images/first/3.4.png

随后一路next，配置好CentOS虚拟机后，我们开启虚拟机

3.5![Alt]https://github.com/wxchentao/Joe/blob/master/images/first/3.5.png

选择版本

3.6![Alt]https://github.com/wxchentao/Joe/blob/master/images/first/3.6.png

输入配置时的用户名和密码，就可以登录进行操作

3.7![Alt]https://github.com/wxchentao/Joe/blob/master/images/first/3.7.png

