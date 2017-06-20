---
title: 服务器常用服务 Mysql、FTP 、Nginx等环境安装（一）
date: 2017-06-19 13:51:43
categories: 服务器
tags: [服务器,linux,Mysql]
---
之前的阿里云试用服务器要到期了，刚好阿里云又搞活动，买了台凑合用，又要安装一整套运行环境，在这里整理下一些基础服务的安装配置。

> 系统 Centos 7 64位  
> 以下命令在root下执行：  

### Mysql


检查 MySQL 是否已安装  
>`rpm -qa | grep mysql`

如果有，就先全部卸载，命令如下：  
>`yum -y remove mysql`

然后下载mysql的repo源  
>`wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm`

安装mysql-community-release-el7-5.noarch.rpm包
>`rpm -ivh mysql-community-release-el7-5.noarch.rpm`

安装这个包后，会获得两个mysql的yum repo源：  
 `/etc/yum.repos.d/mysql-community.repo`  
`/etc/yum.repos.d/mysql-community-source.repo`
下面就可以直接用yum安装Mysql  
>`yum install mysql-server`  

安装好后首先重置密码  
>`mysql -u root`  

如果报错 ‘ERROR 2002 (HY000): Can’t connect to local MySQL server through socket ‘/var/lib/mysql/mysql.sock’  
原因是/var/lib/mysql的访问权限问题。下面的命令把/var/lib/mysql的拥有者改为当前用户

> `chown -R user:user /var/lib/mysql`

然后，重启服务：  
>`service mysqld restart`  

登录重置密码：  
>`mysql -u root -p`  
>`mysql > use mysql;`
>`mysql > update user set password=password('123456') where user='root';`

必要时加入以下命令行，为root添加远程连接的能力。链接密码为 “root”（不包括双引号）  
>`mysql> GRANT ALL PRIVILEGES ON *.* TO root@"%" IDENTIFIED BY "root";`  

重启mysql服务后才生效  

>`service mysqld restart`  

查看并修改数据库编码为utf8  
>`mysql > show variables like "%char%";`  

![图片](/upload/mysql.png)  
编辑Mysql的配置文件来修改编码格式
>`vim /etc/my.cnf`  

在[client]下添加  
>`default_character_set=utf8`  

在[mysqld]下添加  
>`character_set_server = utf8`

保存退出后重启mysqld：  
>`service mysqld restart`

完成以上的操作就OK了。
先写到这里，下面再写FTP，Nginx，Git等的常用软件安装。
