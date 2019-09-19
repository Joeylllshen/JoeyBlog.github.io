---
layout: post
title:  "linux下vsftp搭建ftp"
date:   2019-09-19 
categories: ['Sample Post', 'update']
tags: sample demo samuel-jackson-ipsum
permalink: sample-post-2
hero: images/image.jpg
hero_high: images/image-high.jpg
hero_low: images/image-low.jpg
hero_alt: Demo Image
---

### vsftp安装

1.检查是否安装了vsftpd软件

```
#rpm -qa|grep vsftp
```

2.若没有安装，则下载vsftpd-2.2.2-24.el6.x86_64.rpm包，进行安装

```
 #cd /home/rpm
 #rpm -ivh --nodeps vsftpd-2.2.2-24.el6.x86_64.rpm
```

3.启动服务，出现以下错误，原因是openssl的rpm包太旧了。

![启动服务错误](https://img-blog.csdn.net/20180604185820882?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N5MTA4NDQ2Mjk5Mw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

4.更新openssl的rpm包，出现以下错误，原因是openssl的rpm包已经安装了一个版本，所有产生了冲突

![这里写图片描述](https://img-blog.csdn.net/2018060419042994?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N5MTA4NDQ2Mjk5Mw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
解决方式：使用更新命令，而不是安装命令

 `#rpm -Uvh openssl-1.0.1e-57.el6.x86_64.rpm --nodeps`

5.使用“启动命令”
![这里写图片描述](https://img-blog.csdn.net/20180604191106571?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N5MTA4NDQ2Mjk5Mw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

 启动ftp命令`#service vsftpd start`
 停止ftp命令`#service vsftpd stop`
 重启ftp命令`#service vsftpd restart`

### vsftpd的配置
ftp的配置文件主要有三个，位于/etc/vsftpd/目录下，分别是：

    ftpusers 该文件用来指定那些用户不能访问ftp服务器。
    user_list 该文件用来指示的默认账户在默认情况下也不能访问ftp
    vsftpd.conf vsftpd的主配置文件

### 匿名用户操作

​	切换目录`cd /etc/vsftpd/`

​	修改vsftpd.conf文件`vi vsftpd.conf`

​	去掉以下两行前面的’#'号

```
  anon_upload_enable=YES
  anon_mkdir_write_enable=YES
```

在浏览器中输入地址

![img](https://img-blog.csdn.net/20180604193551245?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N5MTA4NDQ2Mjk5Mw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

在文件夹地址栏中输入地址

![img](https://img-blog.csdn.net/20180604193914771?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N5MTA4NDQ2Mjk5Mw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

### 非匿名用户操作

创建账号和密码

![img](https://img-blog.csdn.net/20180604194751530?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N5MTA4NDQ2Mjk5Mw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

在文件夹浏览器中输入地址`ftp://vsftp所在机器ip`；空白处右键，选择登陆

![img](https://img-blog.csdn.net/20180604195116441?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N5MTA4NDQ2Mjk5Mw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

### 问题



如何修改 vsftpd 的默认根目录 /var/ftp/ 到另一个目录?
默认配置下，匿名用户登录 vsftpd 服务后的根目录是 /var/ftp/； 系统用户登录 vsftpd 服务后的根目录是系统用户的家目录。

若要修改登录 vsftpd 服务后的根目录，只要修改 /etc/vsftpd/vsftpd.conf 文件即可。 假设要把 vsftpd 服务的登录根目录调整为 /vae/www/html，可加入如下三行：

```shell
local_root=/var/www/html 
 chroot_local_user=YES 
 anon_root=/var/www/html
```

注解：local_root 针对系统用户；anon_root 针对匿名用户。 编辑 vsftpd.conf 文件后，保存退出，参照如下指令重新启动服务： service vsftpd restart。之后 ftp 客户端登陆 vsftpd 服务后，其根目录是 /var/www/html。

扩展链接：
https://www.linuxidc.com/Linux/2017-06/144900.htm


--------------------- 