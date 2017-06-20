---
title: 服务器常用Mysql、FTP 、Nginx等环境安装（二）
date: 2017-06-20 14:39:08
categories: 服务器
tags: [服务器,linux,FTP,Nginx]
---
上一章配好了Mysql，这章就主要配置一下FTP和Nginx这两个Web服务常用的环境了。  
<!--more-->
> 系统 Centos 7 64位  
> 以下命令在root下执行：  

### FTP  

我的阿里云装的是vsftpd，一款在Linux发行版中最受推崇的FTP服务器程序。  
惯例用yum安装vsftp
>`yum -y install vsftpd`  

装好后配置vsftpd服务器  
修改配置文件 /etc/vsftpd/vsftpd.conf
>`vim /etc/vsftpd/vsftpd.conf`

修改/增加以下字段
>`#anonymous_enable=YES`  
>`userlist_deny=NO`  
>`#根目录位置设置`  
>`local_root=/var/public_root`

增加FTP帐户  
这里设置的账户名为“test”,密码为“123456”
>`useradd test -s /sbin/nologin`
>`passwd test`

编辑user_list文件，增加test用户访问FTP

>`vi /etc/vsftpd/user_list`  

建立之前设置的Ftp根目录，并设置访问权限  

>`mkdir /var/public_root`
>`chown -R test /var/public_root`
>`chmod -R 755 /var/public_root`

启动vsftpd服务  
>`service vsftpd start`

设置开机启动vsftpd ftp服务   
>`chkconfig vsftpd on`

OK,配置完成，可以通过ftp来登录验证一下是否成功，也可以服务器上安装一个ftp客户端本地调试看看。

### Nginx  

直接通过配置epel yum 源来安装  
>`wget http://dl.Fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm`  
>`rpm -ivh epel-release-latest-7.noarch.rpm`  
>`yum install nginx -y`  

启动nginx  

>`systemctl start nginx`

设置开机启动

>`systemctl enable nginx`

配置文件路径  

>`vim /etc/nginx/nginx.conf`

#开启 gzip 压缩（web服务器优化必开）  
>gzip on;  
gzip_disable "MSIE [1-6].";  
gzip_types text/plain text/javascript text/css application/json application/x-javascript application/javascript image/jpeg image/gif image/png;  
#nginx 传输文件大小，默认为1M  
client_max_body_size 20m;  
client_header_buffer_size 32k;  
large_client_header_buffers 4 32k;  
#设定请求缓冲  
client_header_buffer_size 128k;  
large_client_header_buffers 4 128k;  

Nginx基本配置完成，配好对应的站点，WEB服务就可以正常使用了。
