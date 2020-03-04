---
title: 基于Docker的Mysql使用过程（二）——修改配置
tags:
  - MySQL
  - Docker
categories:
  - MySQL
  - Docker
toc: false
description: <img src="/images/2019/08/20/1bf96c40-c303-11e9-9b99-a370fc44353a.png"></img>
date: 2019-08-20 10:38:15
---

## 目标
- 修改容器的数据目录
- 修改配置文件
- [参考官方文档](https://github.com/docker-library/docs/tree/master/mysql)

## 一.添加额为的配置文件
#### 1.1、容器配置信息
容器自身的配置文件为/etc/mysql/my.cnf,如果要想添加自己的配置，有两种办法，一是修改my.cnf,二是新增配置项到/etc/mysql/conf.d目录下面，my.cnf会将该目录下的所有文件都包含进去。所以现在在宿主机建立一个配置文件，然后映射到该目录下。新增配置分为复制文件和文件映射，为了以后修改方便，采用文件映射的方式。

*++包含这种思想是一种非常好的模式，通常要想去修改某个已有的东西会比较麻烦，但是如果只是新增或者删除一个东西则会变得非常简单++*

#### 1.2、新增配置，生成binlog
新建配置文件/xxx/my.cnf
![image.png](/images/2019/08/20/89336190-c31b-11e9-ad11-1174ecaccd6e.png)

## 二.修改默认的数据目录
#### 2.1查找信息
登录容器查找MySQL的data目录，可以看见当前的目录是：/var/lib/mysql

![image.png](/images/2019/08/20/8f3b0ac0-c2f8-11e9-acf4-e97ea1e6a762.png)

## 三、执行命令，查看解决。
#### 3.1 将数据目录和文件目录一起映射
![image.png](/images/2019/08/20/57604aa0-c327-11e9-ad11-1174ecaccd6e.png)
#### 3.2 查看结果
![image.png](/images/2019/08/20/7b45e9b0-c2fb-11e9-acf4-e97ea1e6a762.png)