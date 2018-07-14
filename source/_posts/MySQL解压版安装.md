---
title: MySQL解压版安装
author: HongChangCui
date: 2017-02-05 20:05:33
categories: 数据库
tags: 
  - MySQL	
cover_picture: mysql.jpg
---
MySQL安装文件分为两种，一种是msi格式的，一种是zip格式的。如果是msi格式的可以直接点击安装;zip格式是自己解压，解压缩之后其实MySQL就可以使用了，但是要进行配置。
<!--more-->
## 安装配置
### 配置环境变量
我的电脑--》属性--》环境--》高级变量，点击系统变量Path,在其后添加Mysql bin文件夹的路径。如下图：
![MySQL环境变量配置][1]
### 修改配置文件
可以修改默认的配置文件如，my-default.ini，或者自己重写一个配置文件如，my.ini。

需要修改或者添加的配置项如下：
```
[mysqld] 
basedir=C:\Program Files\MySQL\MySQL Server 5.6（mysql所在目录） 
datadir=C:\Program Files\MySQL\MySQL Server 5.6\data （mysql所在目录\data）
```
*注：MySQL5.7+版本，会发现因根目录下，缺少data文件夹的情况，输入如下DOS命令：*
```
mysqld --initialize-insecure --user=mysql
```
然后回车；去目录下查看，已经自动创建好data文件夹。

### 安装MySQL服务
DOS命令mysqld -install，然后可以启动MySQL服务net start mysql。默认没有密码，输入mysql可直接进入。

## 设置/修改密码
```
mysqladmin -u root -p[oldpass] password newpass
```
*注意oldpass(老密码)可选，如果root默认密码为空，则不需要输入，如果需要更改老密码，请注意老密码与-p之间不要有空格，否则会报错，另外password和newpass(新密码)之间以空格分隔。*

输入上述命令会出现Enter passwrod: 不用输入，直接回车即可。如下图：
![MySQL 修改密码][2]


[1]: https://www.github.com/ChangHub/BlogImages/raw/master/MySQL%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F%E9%85%8D%E7%BD%AE.jpg "MySQL环境变量配置"
[2]: https://www.github.com/ChangHub/BlogImages/raw/master/MySQL%E4%BF%AE%E6%94%B9%E5%AF%86%E7%A0%81.jpg "MySQL修改密码"
