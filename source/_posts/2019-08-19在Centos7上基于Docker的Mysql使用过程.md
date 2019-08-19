---
title: Centos7上基于Docker的Mysql使用过程
tags:
  - MySQL
  - Docker
categories:
  - Docker
  - MySQL
toc: false
date: 2019-08-19 16:12:43
---

### 1.启动
```shell

[root@dev001 vhost]# docker run --name mysql -p 3308:3306 -itd -e MYSQL_ROOT_PASSWORD=xx mysql:8.0 

67a5025a3a14ab815bd04e08a4c357d6e235db8f7c59f0432dc102ea2f2f2525

```
### 2.查看容器IP
```shell

[root@dev001 vhost]# docker inspect mysql
//找到networks字段
"Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "244a2f6355b609ede5c8311c530516b7493175881d7777c53f497d3fbc56110f",
                    "EndpointID": "90a9071b14f938083b807ae78d5bf88e292a3bc52286757929adedb0473d6693",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                }

```
### 3.在宿主机连接容器的mysql,发生错误
```shell
[root@dev001 vhost]# mysql -h172.17.0.2 -uroot -p

Authentication plugin 'caching_sha2_password' cannot be loaded: /usr/local/mysql/lib/plugin/caching_sha2_password.so: cannot open shared object file: No such file or directory

```
#### 错误原因：
在宿主机使用的mysql版本和容器内的mysql版本不一致，在mysql8中，mysql的密码的加密方式不一致，mysql8中的默认加密方式为：**caching_sha2_password**，而宿主机中找不到这种加密方式的库，所以报错。

#### 解决方式：
```shell
docker exec -it mysql2 /bin/bash # 进入mysql容器:
mysql:mysql -uroot -pmima #进入mysql
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '你的密码'; #修改密码和加密方式
flush privileges; #刷新权限
exit # 退出mysql
exit # 退出容器
```
### 4.使用宿主机映射的端口登录
启动容器的时候将宿主机的3307映射到了容器的3306，这个时候是可以使用宿主机的3307端口登录
```shell
[root@dev001 zhangweixi]# mysql -uroot -P 3307 -p
Enter password: 
ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: YES)

```
然而，却登录失败了，这是因为使用本机登录的时候，mysql将默认host为localhost,此时对应的加密方式还是caching_sha2_password

#### 5.其他
不要忘记了服务器的防火墙端口设置和阿里云的安全组端口设置
