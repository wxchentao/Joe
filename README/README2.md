# 基于云服务器创建个人网站

## 实验二

## 前提：购买了云服务器CentOS7.2

### 一、安装Apache Web服务器

#### （1）使用yum工具安装：

在root权限下指令yum install httpd

1.1:![Alt](https://github.com/wxchentao/Joe/blob/master/images/second/1.1.png)

1.2:![Alt](https://github.com/wxchentao/Joe/blob/master/images/second/1.2.png)

#### (2)安装完成以后，启动Apache Web服务器

指令：systemctl start httpd.service

1.3:![Alt](https://github.com/wxchentao/Joe/blob/master/images/second/1.3.png)

测试Apache服务器是否成功运行，找到腾讯云实例的公有IP地址(your_cvm_ip)，在你本地主机的浏览器上输入（我的是106.54.89.194

### 二、安装MySQL

#### （1）CentOS 7.2的yum源中并末包含MySQL，需要其他方式手动安装。因此，我们采用MySQL数据库的开源分支MariaDB作为替代。

安装MariaDB: yum install mariadb-server mariadb

2.1:![Alt](https://github.com/wxchentao/Joe/blob/master/images/second/2.1.png)

2.2:![Alt](https://github.com/wxchentao/Joe/blob/master/images/second/2.2.png)

#### （2）安装好之后，启动mariadb

指令：systemctl start mariadb

#### （3）运行简单的安全脚本以移除潜在的安全风险，启动交互脚本：

指令：mysql_secure_installation

2.3![Alt](https://github.com/wxchentao/Joe/blob/master/images/second/2.3.png)

设置相应的root访问密码以及相关的设置(都选择Y)。

2.4![Alt](https://github.com/wxchentao/Joe/blob/master/images/second/2.4.png)

#### （4）最后设置开机启动MariaDB：

指令：systemctl enable mariadb.service

2.5![Alt](https://github.com/wxchentao/Joe/blob/master/images/second/2.5.png)

### 三、安装PHP

#### （1）PHP 7.x包在许多仓库中都包含，这里我们使用Remi仓库，而Remi仓库依赖于EPEL仓库，因此首先启用这两个仓库

指令：

```中文
sudo yum install epel-release yum-utils
sudo yum install http://rpms.remirepo.net/enterprise/remi-release-7.rpm
```

3.1![Alt](https://github.com/wxchentao/Joe/blob/master/images/second/3.1.png)

3.2![Alt](https://github.com/wxchentao/Joe/blob/master/images/second/3.2.png)

3.3![Alt](https://github.com/wxchentao/Joe/blob/master/images/second/3.3.png)

#### （2）启用PHP7.2 Remi仓库

指令：yum-config-manager --enable remi-php72

3.4![Alt](https://github.com/wxchentao/Joe/blob/master/images/second/3.4.png)

#### （3）安装PHP以及PHP-MYSQL

指令： yum install php php-mysql

3.5![Alt](https://github.com/wxchentao/Joe/blob/master/images/second/3.5.png)

3.6![Alt](https://github.com/wxchentao/Joe/blob/master/images/second/3.6.png)

#### （4）查看安装的php版本：

指令：php -v

3.7![Alt](https://github.com/wxchentao/Joe/blob/master/images/second/3.7.png)

#### （5）安装以后重启Apache服务器支持PHP

指令：systemctl restart httpd.service

3.8![Alt](https://github.com/wxchentao/Joe/blob/master/images/second/3.8.png)

#### （6）安装PHP模块

为了更好的运行PHP，需要启动PHP附加模块

##### （i）使用如下命令可以查看可用模块：

指令：yum search php-

3.9![Alt](https://github.com/wxchentao/Joe/blob/master/images/second/3.9.png)

3.10![Alt](https://github.com/wxchentao/Joe/blob/master/images/second/3.10.png)

##### （ii）先行安装php-fpm(PHP FastCGI Process Manager)和php-gd(A module for PHP applications for using the gd graphics library)，WordPress使用php-gd进行图片的缩放。

指令：yum install php-fpm php-gd

3.11![Alt](https://github.com/wxchentao/Joe/blob/master/images/second/3.11.png)

##### （iii）重启Apache服务

指令：service httpd restart

3.12![Alt](https://github.com/wxchentao/Joe/blob/master/images/second/3.12.png)

### 四、测试PHP

#### （1）这里我们利用一个简单的信息显示页面（info.php）测试PHP。创建info.php并将其置于Web服务的根目录（/var/www/html/）

指令： vim /var/www/html/info.php

4.1![Alt](https://github.com/wxchentao/Joe/blob/master/images/second/4.1.png)

#### （2）该命令使用vim在/var/www/html/处创建一个空白文件info.php，我们添加如下内容：

<?php phpinfo(); ?>

4.2![Alt](https://github.com/wxchentao/Joe/blob/master/images/second/4.2.png)

#### （3）完成之后，使用刚才获取的cvm的IP地址，在你的本地主机的浏览器中输入:

网址：http://your_cvm_ip/info.php（改成你的公网IP地址

4.3![Alt](https://github.com/wxchentao/Joe/blob/master/images/second/4.3.png)

### 五、安装WordPress以及完成相关配置

#### （1）为WordPress创建一个MySQL数据库

##### （i）首先以root用户登录MySQL数据库：mysql -u root -p

5.1![Alt](https://github.com/wxchentao/Joe/blob/master/images/second/5.1.png)

##### （ii）首先为WordPress创建一个新的数据库：

指令：CREATE DATABASE wordpress;

##### （iii）接着为WordPress创建一个独立的MySQL用户：

指令：CREATE USER wordpressuser@localhost IDENTIFIED BY 'password';

“wordpressuser”和“password”使用你自定义的用户名和密码。

5.2![Alt](https://github.com/wxchentao/Joe/blob/master/images/second/5.2.png)

##### （iiii）授权给wordpressuser用户访问数据库的权限：

指令：GRANT ALL PRIVILEGES ON wordpress.* TO wordpressuser@localhost IDENTIFIED BY 'password';

5.3![Alt](https://github.com/wxchentao/Joe/blob/master/images/second/5.3.png)

##### （iiiii）随后刷新MySQL的权限：

指令：FLUSH PRIVILEGES;

##### （iiiiii）最后，退出MySQL的命令行模式：

指令：exit

5.4![Alt](https://github.com/wxchentao/Joe/blob/master/images/second/5.4.png)

#### （2）安装WordPress

##### （i）下载WordPress至当前用户的主目录：

指令：wget http://wordpress.org/latest.tar.gz

5.5![Alt](https://github.com/wxchentao/Joe/blob/master/images/second/5.5.png)

##### （ii）wget命令从WordPress官方网站下载最新的WordPress集成压缩包，解压该文件：

指令：tar xzvf latest.tar.gz

5.6![Alt](https://github.com/wxchentao/Joe/blob/master/images/second/5.6.png)

##### （iii）解压之后在主目录下产生一个wordpress文件夹。我们将该文件夹下的内容同步到Apache服务器的根目录下，使得wordpress的内容能够被访问。

指令：rsync -avP ~/wordpress/ /var/www/html/

5.7![Alt](https://github.com/wxchentao/Joe/blob/master/images/second/5.7.png)

##### （iiii）接着在Apache服务器目录下为wordpress创建一个文件夹来保存上传的文件：

指令：mkdir /var/www/html/wp-content/uploads8

5.8![Alt](https://github.com/wxchentao/Joe/blob/master/images/second/5.8.png)

##### （iiiii）对Apache服务器的目录以及wordpress相关文件夹设置访问权限：

指令：chown -R apache:apache /var/www/html/*

5.9![Alt](https://github.com/wxchentao/Joe/blob/master/images/second/5.9.png)

#### （3）配置WordPress

##### （i）大多数的WordPress配置可以通过其Web页面完成，首先通过命令行连接WordPress和MySQL。定位到wordpress所在文件夹：

指令：cd /var/www/html

##### （ii）WordPress的配置依赖于wp-config.php文件，当前该文件夹下并没有该文件，我们通过拷贝wp-config-sample.php文件来生成：

指令：cp wp-config-sample.php wp-config.php

5.10![Alt](https://github.com/wxchentao/Joe/blob/master/images/second/5.10.png)

##### （iii）通过nano超简单文本编辑器来修改配置，主要是MySQL相关配置：

指令：nano wp-config.php

将文件中的DB_NAME，DB_USER和DB_PASSWORD更改成之前为WordPress创建的数据库的相关信息，这三处信息是当前唯一需要修改的。

5.11![Alt](https://github.com/wxchentao/Joe/blob/master/images/second/5.11.png)

5.12![Alt](https://github.com/wxchentao/Joe/blob/master/images/second/5.12.png)

#### （4）通过Web界面进一步配置WordPress

##### （i）经过上述的安装和配置，WordPress运行的相关组件已经就绪，接下来通过WordPress提供的Web页面进一步配置。输入你的IP地址或者域名：

指令：http://server_domain_name_or_IP

5.13![Alt](https://github.com/wxchentao/Joe/blob/master/images/second/5.13.png)

##### （ii）设置网站的标题，用户名和密码以及电子邮件等，点击**Install WordPress**，弹出确认页面：

5.14![Alt](https://github.com/wxchentao/Joe/blob/master/images/second/5.14.png)

##### （iii）输入用户名和密码之后，会进入WordPress的控制面板，接下来，就可以开始个性化配置了！

5.15![Alt](https://github.com/wxchentao/Joe/blob/master/images/second/5.15.png)



## 参考文献

https://blog.csdn.net/llfjfz/article/details/95501675



