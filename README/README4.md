# Ceph的安装与实践

## 实验四

## 前提：VMware 12pro以上，centos7镜像

### 一、完成Ceph基于4个节点的安装（VMWareWorkstation 4个VM实例）

#### （1）新建虚拟机节点ceph-admin

1.1:![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/1.1.png)

1.2：![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/1.2.png)

1.3：![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/1.3.png)

1.4：![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/1.4.png)

1.5：![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/1.5.png)

1.6：![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/1.6.png)

1.7：![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/1.7.png)

1.8：![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/1.8.png)

1.9：![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/1.9.png)

1.10：![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/1.10.png)

1.11：![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/1.11.png)

1.12：![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/1.12.png)

选择你的cento7镜像

1.13：![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/1.13.png)

新建完了以后开启虚拟机，它会开始安装

1.14：![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/1.14.png)

这边显示的是图形界面的安装，但是只是为了方便演示，之后使用的是最小化安装

1.15：![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/1.15.png)

1.16：![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/1.16.png)

1.17：![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/1.17.png)

1.18：![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/1.18.png)

1.19：![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/1.19.png)

1.20：![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/1.20.png)

1.22：![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/1.22.png)

这就是安装好的界面了

#### （2）接下来这是安装VMtools（可做可不做）

2.1：![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/2.1.png)

双击光驱

2.2：![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/2.2.png)

打开终端挂载盘

2.3：![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/2.3.png)

2.4：![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/2.4.png)

复制到自己的/tmp目录下解压

2.5：![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/2.5.png)

运行解压后的文件里的安装程序

2.7：![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/2.7.png)

2.8：![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/2.8.png)

#### （3）配置完毕后，选择虚拟机克隆

克隆出node1，node2，node3三台新的虚拟机

以上我们就准备好了四台实验用的VM实例

### 二、配置所有节点

#### （1）创建一个Ceph用户

在所有节点上创建一个名为“ **cephuser** ” 的新用户。

#### （1）使用如下命令查看操作系统内核信息：

```
useradd -d /home/cephuser -m cephuser
passwd cephuser
```

3.1:![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/3.1.png)

创建新用户后，我们需要为“ cephuser”配置sudo。他必须能够以root用户身份运行命令，并且无需密码即可获得root用户特权。

```
echo "cephuser ALL = (root) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/cephuserchmod 0440 /etc/sudoers.d/cephusersed -i s'/Defaults requiretty/#Defaults requiretty'/g /etc/sudoers
```

3.2:![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/3.2.png)

#### （2）安装和配置NTP

安装NTP以同步所有节点上的日期和时间。运行ntpdate命令通过NTP协议设置日期和时间，我们将使用us pool NTP服务器。然后启动并启用NTP服务器在引导时运行。

```
yum install -y ntp ntpdate ntp-docntpdate 0.us.pool.ntp.orghwclock --systohcsystemctl enable ntpd.servicesystemctl start ntpd.service
```

 3.3：![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/3.3.png)

 3.4：![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/3.4.png)

#### （3）安装Open-vm-tools

如果要在VMware内部运行所有节点，则需要安装此虚拟化实用程序。否则，请跳过此步骤。

```
yum install -y open-vm-tools
```

 3.5：![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/3.5.png)

#### （4）禁用SELinux

通过使用sed流编辑器编辑SELinux配置文件，在所有节点上禁用SELinux。

```
sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config
```

3.6：![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/3.6.png)

#### （5）配置主机文件

先查看自己的虚拟网络编辑器的IP范围

3.7：![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/3.7.png)

3.8：![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/3.8.png)

用ip addr查看nat动态分配给每台节点的ip地址，这将成为接下来填写配置的依据

3.9：![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/3.9.png)

使用vim编辑器在所有节点上编辑/ etc / hosts文件，并添加带有所有集群节点的IP地址和主机名的行。

```
vim /etc/hosts
```

粘贴以下配置：

3.10：![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/3.10.png)

测试网络，ping主机ping外网和ping集群节点

3.11：![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/3.11.png)

3.12：![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/3.12.png)

到这里初步配置就完成了

### 三、配置SSH服务器

#### （1）配置**ceph-admin节点**

admin节点用于配置监视节点和osd节点。登录到**ceph** -admin节点并成为“ **cephuser** ”。

```
ssh root@ceph-admin
su - cephuser
```

4.1：![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/4.1.png)

admin节点用于安装和配置所有群集节点，因此ceph-admin节点上的用户必须具有无需密码即可连接到所有节点的特权。我们必须在“ ceph-admin”节点上为“ cephuser”配置无密码的SSH访问。

为“ **cephuser** ” 生成ssh密钥。

```
ssh-keygen
```

将密码短语留空/空白。

4.2：![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/4.2.png)

接下来，为ssh配置创建配置文件。

```
vim ~/.ssh/config
```

粘贴以下配置：

```
Host ceph-admin        
		Hostname ceph-admin        
		User cephuser 
Host node1        
		Hostname node1        
		User cephuser 
Host node2
		Hostname node2        
		User cephuser 
Host node3        
		Hostname node3
        User cephuser 
```

4.3：![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/4.3.png)

4.4：![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/4.4.png)

更改配置文件的权限。

```
chmod 644 ~/.ssh/config
```

4.5：![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/4.5.png)

这里其他no route是因为我没开机其他节点，总之其他节点的配置也是一样的

#### （2）使用ssh-copy-id命令将SSH密钥添加到所有节点。

```
ssh-keyscan node1 node2 node3 >> ~/.ssh/known_hosts
ssh-copy-id node1
ssh-copy-id node2
ssh-copy-id node3
```

4.6：![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/4.6.png)

完成后，请尝试从ceph-admin节点访问osd1服务器。

```
ssh osd1
```

4.7：![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/4.7.png)

### 五、配置防火墙

#### （1）我们将使用防火墙保护系统。

在此步骤中，我们将在所有节点上启用firewald，然后打开ceph-admon，ceph-mon和ceph-osd所需的端口。

登录到ceph-admin节点并启动firewalld。

```
ssh root@ceph-adminsystemctl start firewalldsystemctl enable firewalld
```

5.1![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/5.1.png)

#### （2）打开端口80、2003和4505-4506，然后重新加载防火墙。

```
sudo firewall-cmd --zone=public --add-port=80/tcp --permanent
sudo firewall-cmd --zone=public --add-port=2003/tcp --permanent
sudo firewall-cmd --zone=public --add-port=4505-4506/tcp --permanent
sudo firewall-cmd --reload
```

5.2![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/5.2.png)

#### （3）在Ceph监视节点上打开新端口，然后重新加载防火墙。

```
sudo firewall-cmd --zone=public --add-port=6789/tcp --permanent
sudo firewall-cmd --reload
```

5.3![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/5.3.png)

#### （4）最后，打开每个osd节点上的端口6800-7300-osd1，osd2和os3。

从ceph-admin节点登录到每个osd节点。

```
ssh osd1sudo systemctl start firewalld
sudo systemctl enable firewalld
```

打开端口并重新加载防火墙。

```
sudo firewall-cmd --zone=public --add-port=6800-7300/tcp --permanent
sudo firewall-cmd --reload
```

防火墙配置完成。

5.4![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/5.4.png)

#### （5）配置Ceph OSD节点

先ssh上各个节点，创建一个目录给osd守护进程使用，接下来会用到，这里只展示一个osd1

5.5![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/5.5.png)

### 六、构建Ceph集群

#### （1）在这一步中，我们将在ceph-admin节点的所有节点上安装Ceph。

登录到ceph-admin节点。

```
ssh root@ceph-admin
su - cephuser
```

#### （2）在ceph-admin节点上安装ceph-deploy

添加Ceph存储库，并使用yum命令安装Ceph部署工具' **ceph-deploy** '。

```
sudo rpm -Uhv http://download.ceph.com/rpm-jewel/el7/noarch/ceph-release-1-1.el7.noarch.rpm
sudo yum update -y && sudo yum install ceph-deploy -y
```

6.1![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/6.1.png)

6.2![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/6.2.png)

#### （3）创建新的集群配置

创建新的群集目录。

```
mkdir clustercd cluster/
```

6.3![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/6.3.png)

ERROR——？！！！查个教程

6.4![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/6.4.png)

被认为是ssh的问题，添加了这一行指令

6.5![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/6.5.png)

再安装了openssh

6.6![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/6.6.png)

我选择重来一遍配置ssh

6.7![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/6.7.png).

查了一下这是解决方案，给cephuser赋予对cluster文件的权限

6.8![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/6.8.png)

再次执行成功了，该命令将在集群目录中生成Ceph集群配置文件'ceph.conf'。

6.9![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/6.9.png)

用vim编辑ceph.conf文件。

```
vim ceph.conf
```

在[global]块下，在下面粘贴配置。

```
# Your network address
public network = 192.168.220.0/24#用你自己的子网地址
osd pool default size = 2
```

6.10![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/6.10.png)

6.11![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/6.11.png)

#### （4）在所有节点上安装Ceph

现在，从ceph-admin节点在所有其他节点上安装Ceph。这可以通过单个命令完成。

```
ceph-deploy install ceph-admin node1 node2 node3 node4
```

7.2![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/7.2.png)

7.3![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/7.3.png)

该命令将在所有节点上自动安装Ceph：node1-3和ceph-admin-安装将花费一些时间。

7.4![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/7.4.png)

现在将ceph-mon部署在node1节点上。

```
ceph-deploy mon create-initial
```

7.5![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/7.5.png)

该命令将创建监视键，并使用“ ceph”命令检查并获取键。

```
ceph-deploy gatherkeys mon1
```

7.6![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/7.6.png)

我 报 错 了，它说找不到这个文件

7.7![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/7.7.png)

我到node1上一看，发现创了，但是文件名不对，所以找不着，网上说是因为主机名错误

7.8![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/7.8.png)

我一查node1的主机名，果不其然

7.9![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/7.9.png)

赶紧改成node1

7.10![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/7.10.png)

这是接踵而至的错误

7.11![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/7.11.png)

根据教程我在node上执行了这个命令

7.12![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/7.12.png)

终于成功创建以下密钥环了

7.13![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/7.13.png)

7.14![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/7.14.png)

#### （5）将OSDS添加到群集

之前创建过osd，现在依旧以osd1为例

从管理节点执行ceph-deploy来准备OSD

7.16![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/7.16.png)

激活OSD

7.17![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/7.17.png)

如果出现permission denied的错误，记得登陆节点赋予ceph对osd文件夹的修改权限

7.20![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/7.20.png)

用ceph-deploy把配置文件和admin密钥复制到管理节点和ceph节点，这样，每次执行ceph命令行时就无需指定monitor地址和ceph.client.admin.keyring了

7.18![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/7.18.png)

确保对ceph.client.admin.keyring有正确的操作权限

7.19![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/7.19.png)

检查集群的健康状况

7.21![Alt](https://github.com/wxchentao/Joe/blob/master/images/fourth/7.21.png)

此时全部节点和操作已经配置完成，至此，整个ceph的安装过程完成。



### 参考文献：

1.https://www.howtoforge.com/tutorial/how-to-build-a-ceph-cluster-on-centos-7/#step-configure-the-ssh-server

2.http://blog.sina.com.cn/s/blog_c5daaa110102xs73.html

3.云计算原理与实践P146-149

4.https://blog.whsir.com/post-4604.html