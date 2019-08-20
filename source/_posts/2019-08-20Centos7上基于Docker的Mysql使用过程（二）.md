---
title: Centos7上基于Docker的Mysql使用过程（二）——修改数据目录
tags:
  - Docker
  - MySQL
categories:
  - MySQL
  - Docker
toc: false
date: 2019-08-20 10:38:15
---

## 目标
修改容器的数据目录

## 参考
[https://github.com/docker-library/docs/tree/master/mysql](https://github.com/docker-library/docs/tree/master/mysql)

## 查找信息
登录容器查找MySQL的data目录，可以看见当前的目录是：/var/lib/mysql

![image.png](/images/2019/08/20/8f3b0ac0-c2f8-11e9-acf4-e97ea1e6a762.png)
## 将目录映射到宿主机目录
```shell

[root@dev001 zhangweixi]# docker run -itd \
--name test-mysql \
-p     3309:3306 \
-e     MYSQL_ROOT_PASSWORD=root \
-v     /data/docker-mysql/test-mysql:/var/lib/mysql  \
mysql:8.0

```
查看目录，修改成功。

![image.png](/images/2019/08/20/7b45e9b0-c2fb-11e9-acf4-e97ea1e6a762.png)