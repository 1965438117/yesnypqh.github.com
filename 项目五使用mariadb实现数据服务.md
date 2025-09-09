项目五使用mariadb实现数据服务

l 安装mariadb-server服务器并配置配置文件

编辑MariaDB数据库安装源信息（vim /etc/yum.repos.d/MariaDB.repo，没有该文件需要先创建）：

[mariadb]

name=MariaDB

baseurl=https://mirrors.aliyun.com/mariadb/yum/10.4/centos8-amd64/

gpgkry=https://mirrors.aliyun.com/mariadb/yum/RPM-GPG-KEY-MariaDB

gpgcheck=1

或

[mariadb]

name = MariaDB

baseurl = http://mirrors.ustc.edu.cn/mariadb/yum/10.4/centos7-amd64/

gpgkey=http://mirrors.ustc.edu.cn/mariadb/yum/RPM-GPG-KEY-MariaDB

gpgcheck=1

————————————————

如图所示：

yum install -y mariadb-server 

![img](file:///C:\Users\ysnyp\AppData\Local\Temp\ksohtml12824\wps1.jpg) 

![img](file:///C:\Users\ysnyp\AppData\Local\Temp\ksohtml12824\wps2.jpg) 

![img](file:///C:\Users\ysnyp\AppData\Local\Temp\ksohtml12824\wps3.jpg) 

开启mariadb数据库服务同时加入开机自启动，查看是否加入开机自启动以及mariadb当前状态

systemctl start mariadb

systemctl enable mariadb

systemctl list-unit-files|grep mariadb

systemctl status mariadb

![img](file:///C:\Users\ysnyp\AppData\Local\Temp\ksohtml12824\wps4.jpg) 

首次登录直接mysql即可，登录以后设置密码。

mysql

set password=password('1234');w

update mysql.user set authentication_string=password('123456') 

where User="root" and Host="localhost";

![img](file:///C:\Users\ysnyp\AppData\Local\Temp\ksohtml12824\wps5.jpg) 

使用mariadb客户端管理数据库:

mysql -u root -pcentos@mariadb#123

create database if not exists firstdb;

show databases;

use firstdb;

create table if not exists test_table(

id int (11),

name varchar(20),

sex enum('0','1','2'),

primary key(id)

);

修改数据库编码格式为utf-8，test为数据库名

ALTER DATABASE test CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci ;

修改数据表格式为utf-8，test_table为数据表名

alter table test_table  convert to character set utf8mb4 collate utf8mb4_bin；

show tables;

![img](file:///C:\Users\ysnyp\AppData\Local\Temp\ksohtml12824\wps6.jpg) 

命令:CREATE USER 'username'@'host' IDENTIFIED BY 'password';

例：CREATE USER 'root'@'%' IDENTIFIED BY '';

教材实例（运行报错）：

CREATE USER root@localhost IDENTIFIED VIA unix_socket OR mysql_native_password USING 'invalid';

CREATE USER mysql@localhost IDENTIFIED VIA unix_socket OR mysql_native_password USING

'invalid';

上述设置以后无密码也可以登录mariadb，set设置密码后root也可无密码登录。

 

任务二使用phpadmin管理mariadb

安装apache服务，查看状态，安装php

yum install -y httpd

systemctl status httpd

systemctl enable httpd

yum install -y php php-mysqlnd php-json

 

![img](file:///C:\Users\ysnyp\AppData\Local\Temp\ksohtml12824\wps7.jpg) 

查看httpd、php、mysql版本信息

http -v

php -v

mysql -v

![img](file:///C:\Users\ysnyp\AppData\Local\Temp\ksohtml12824\wps8.jpg) 

安装wget，使用wget从phpMyAdmin官网获取下载程序

yum install -y wget

wget http://files.phpmyadmin.net/phpMyAdmin/5.0.1/phpMyAdmin-5.0.1-all-languages.tar.gz

![img](file:///C:\Users\ysnyp\AppData\Local\Temp\ksohtml12824\wps9.jpg) 

tar -zxvf phpMyAdmin-5.0.1-all-languages.tar.gz -C /var/www/

cd  /var/www

mv phpMyAdmin-5.0.1-all-languages.tar.gz  phpmyadmin

![img](file:///C:\Users\ysnyp\AppData\Local\Temp\ksohtml12824\wps10.jpg) 

chown -R apache:apache /var/www/phpmyadmin

![img](file:///C:\Users\ysnyp\AppData\Local\Temp\ksohtml12824\wps11.jpg) 

vim /etc/httpd/conf/httpd.conf

![img](file:///C:\Users\ysnyp\AppData\Local\Temp\ksohtml12824\wps12.jpg) 

vim /etc/httpd/conf.d/welcome.conf

![img](file:///C:\Users\ysnyp\AppData\Local\Temp\ksohtml12824\wps13.jpg) 

l phpmyadmin管理数据库

![img](file:///C:\Users\ysnyp\AppData\Local\Temp\ksohtml12824\wps14.jpg) 

![img](file:///C:\Users\ysnyp\AppData\Local\Temp\ksohtml12824\wps15.jpg) 

![img](file:///C:\Users\ysnyp\AppData\Local\Temp\ksohtml12824\wps16.jpg) 

![img](file:///C:\Users\ysnyp\AppData\Local\Temp\ksohtml12824\wps17.jpg) 

![img](file:///C:\Users\ysnyp\AppData\Local\Temp\ksohtml12824\wps18.jpg) 

![img](file:///C:\Users\ysnyp\AppData\Local\Temp\ksohtml12824\wps19.jpg) 

![img](file:///C:\Users\ysnyp\AppData\Local\Temp\ksohtml12824\wps20.jpg) 

![img](file:///C:\Users\ysnyp\AppData\Local\Temp\ksohtml12824\wps21.jpg) 

![img](file:///C:\Users\ysnyp\AppData\Local\Temp\ksohtml12824\wps22.jpg) 

![img](file:///C:\Users\ysnyp\AppData\Local\Temp\ksohtml12824\wps23.jpg) 

在Mariadb服务器上输入以下命令赋予root用户所有权限并允许所有地址链接

grant all on *.* to root@'%' identified by '1234';

![img](file:///C:\Users\ysnyp\AppData\Local\Temp\ksohtml12824\wps24.jpg) 

l workbench 使用

![img](file:///C:\Users\ysnyp\AppData\Local\Temp\ksohtml12824\wps25.jpg) 

![img](file:///C:\Users\ysnyp\AppData\Local\Temp\ksohtml12824\wps26.jpg) 

![img](file:///C:\Users\ysnyp\AppData\Local\Temp\ksohtml12824\wps27.jpg) 

![img](file:///C:\Users\ysnyp\AppData\Local\Temp\ksohtml12824\wps28.jpg) 

![img](file:///C:\Users\ysnyp\AppData\Local\Temp\ksohtml12824\wps29.jpg) 

![img](file:///C:\Users\ysnyp\AppData\Local\Temp\ksohtml12824\wps30.jpg) 

 

 

 

![img](file:///C:\Users\ysnyp\AppData\Local\Temp\ksohtml12824\wps31.jpg) 

 

 

 ![img](file:///C:\Users\ysnyp\AppData\Local\Temp\ksohtml12824\wps32.jpg)

![img](file:///C:\Users\ysnyp\AppData\Local\Temp\ksohtml12824\wps33.jpg) 

![img](file:///C:\Users\ysnyp\AppData\Local\Temp\ksohtml12824\wps34.jpg) 

![img](file:///C:\Users\ysnyp\AppData\Local\Temp\ksohtml12824\wps35.jpg) 

![img](file:///C:\Users\ysnyp\AppData\Local\Temp\ksohtml12824\wps36.jpg) 

​                                        

![img](file:///C:\Users\ysnyp\AppData\Local\Temp\ksohtml12824\wps37.jpg)  

l 主从集群实现Mariadb的高可用

编辑配置文件内容如图所示： vim /etc/my.cnf  

![img](file:///C:\Users\ysnyp\AppData\Local\Temp\ksohtml12824\wps38.jpg) 

创建同步账号：

create user if not exists 'replication_user'@'%' identified by '1234';

grant replication slave on *.* to 'replication_user'@'%';

   

![img](file:///C:\Users\ysnyp\AppData\Local\Temp\ksohtml12824\wps39.jpg) 

获取主节点二进制文件位置和偏移量

![img](file:///C:\Users\ysnyp\AppData\Local\Temp\ksohtml12824\wps40.jpg) 

创建第二台数据库服务器

配置好配置文件，内容如下：

![img](file:///C:\Users\ysnyp\AppData\Local\Temp\ksohtml12824\wps41.jpg) 

连接主服务器

 change master to master_host='192.168.2.11', master_user='replication_user', master_password='1234', master_port=3306, master_log_file='db-cluster-mariadb-bin.000001',master_log_pos=341,master_connect_retry=10;

![img](file:///C:\Users\ysnyp\AppData\Local\Temp\ksohtml12824\wps42.jpg) 

启动主从服务器同步服务

start slave；

![img](file:///C:\Users\ysnyp\AppData\Local\Temp\ksohtml12824\wps43.jpg) 

验证主从集群服务状态

![img](file:///C:\Users\ysnyp\AppData\Local\Temp\ksohtml12824\wps44.jpg)  

在主服务器上添加数据

![img](file:///C:\Users\ysnyp\AppData\Local\Temp\ksohtml12824\wps45.jpg) 

在从服务器上验证数据是否同步

![img](file:///C:\Users\ysnyp\AppData\Local\Temp\ksohtml12824\wps46.jpg) 

​                                                                