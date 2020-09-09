---
title: 记录一次线上环境配置以及测试
toc: true
mathjax: true
date: 2020-09-08 21:37:44
password:
summary:
tags:
    - mysql
    - ssh
categories:
    - linux
---
&nbsp;&nbsp;&nbsp;&nbsp;最近几天再忙学校自动填表的线上项目部署，遇到了不少问题，为防止以后忘记，在此特地记录一下。
<!-- more -->
### mysql插入操作报错
报错信息为
```shell
$:incorrect value .......... for column 1
```
&nbsp;&nbsp;&nbsp;&nbsp;中间省略号为一串乱码，一般这种问题就和编码相关，去查了一下，mysql默认编码为latin1,插入中文数据会报错，需要对表和数据库的编码进行修改。\
&nbsp;&nbsp;&nbsp;&nbsp;去stackoverflow上面查询对应的解决方案
```mysql
#For database
ALTER DATABASE
    databasename
    CHARACTER SET = utf8mb4
    COLLATE = utf8mb4_unicode_ci;

#For table
ALTER TABLE
    tablename
    CONVERT TO CHARACTER SET utfmb4
    COLLATE utf8mb4_unicode_ci;

#For column
ALTER TABLE
    table_name
    CHANGE column_name column_name
    VARCHAR(191)
    CHARACTER SET utf8mb4
    COLLATE utf8mb4_unicode_ci;
```
&nbsp;&nbsp;&nbsp;&nbsp;由于历史原因，mysql的unicode编码为3个字节，所以得将字符编码设置为utf8mb4(话说我本地的mysql好像版本新一些8.0，不用改编码也能跑,centos7给👴爬)。

&nbsp;&nbsp;&nbsp;&nbsp;这里出现了一个COLLATE字段，可以简单理解为字段组织排序的方式，ci尾缀代表case insensitive。更详细的可以参考[掘金的这篇博客](https://juejin.im/post/6844903726499512334)

&nbsp;&nbsp;&nbsp;&nbsp;修改字符集之后插入成功

### ssh远程连接+debug过程
&nbsp;&nbsp;&nbsp;&nbsp;按理来说这个其实比较简单，就是按照
```bash
$:ssh username@ip_address
```
初次输入密码，在本地生成公钥私钥文件
```shell
ssh-keygen -t rsa -b 4096
```
更多参数可以查手册，默认是2048位的。之后通过scp命令传输文件将公钥传输到.ssh文件夹下面，并且传输内容到authorized_keys中。
```bash
scp local/path/.ssh/id_rsa.pub username@ip_address: remote/path/.ssh/uploaded_file_name
```
On remote side
```bash
cat ~/.ssh/uploaded_file >> ~/.ssh/authorized_keys
chmod 700 ~/.ssh/
chmod 600 ~/.ssh/*
```
chmod stands for change mod, which means changes to different access level.\
&nbsp;&nbsp;&nbsp;&nbsp;突然写起英文2333，这上面的步骤都很简单，但是在我原来的PC上都不管用，最后的原因是因为我的$HOME变量中含有中文。所以问题的解决方案就是修改中文用户名为英文，具体过程就不再啰嗦了。\
&nbsp;&nbsp;&nbsp;&nbsp;大致就是注销当前账号，使用administrator登录，修改用户文件夹和注册表中的路径名字，在切换回来。就可以不用使用密码远程连接主机了。
### 额外啰嗦
为了使主机更加安全，可以禁用密码验证，修改/etc/ssh/sshd_config文件，将password authentication修改为false,记得修改之前备份文件.
```bash
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak
#centos7使用这中格式的指令
systemctl restart sshd 
```
本来还想把cron定时运行脚本以及grep查找指令也写一下，时间太晚了就先写这么多了。
