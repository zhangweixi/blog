---
title: 基于Docker的Mysql使用过程（三）——数据迁移
tags:
  - MySQL
  - Docker
categories:
  - Docker
  - MySQL
toc: false
description: |-
  <p style="color:red;">核心内容：1.迁移数据库，2.修改sql_mode,3.修改默认加密方式</p>
  <img src="/images/2019/08/21/592fe600-c3e1-11e9-ad11-1174ecaccd6e.png"></img>
date: 2019-08-20 13:57:38
---

## 情况说明
目前要做的是数据迁移，而不是数据备份，即迁移成功后当前的Mysql就不再使用了，这样势必会影响到业务正常的运行。但是影响的程度也有多种选择：
1. 当前业务任然可以使用查询业务
2. 完全停止所有业务（最坏情况）
3. 业务照常进行，再迁移成功后立刻将应用切换的新的库。（*暂时不知是否能够保证在数据不发生错误的情况下进行*）
## 具体操作
1. 锁定MySQL，禁止修改数据
登录MySQL终端，将表加上读锁，使得当前的系统只能查询，而不能修改数据，特别注意，不要退出终端，退出后相当于释放了锁。
```SQL

FLUSH TABLES WITH READ LOCK;

```
2. 使用mysqldump导出数据
```shell
mysqldump --databases youdbname -u user -ppassword > /../db.sql
```

3.在新的系统中恢复数据
将db.sql拷贝到新系统，登录新的SQL客户端，导入数据，在此之前，你应当提前创建好数据库名称。
```SQL
SOURCE /../db.sql
```

4. 切换系统，释放锁
将系的数据库配置切换到新的系统，然后将加锁的老MySQL系统解锁。
```sql
UNLOCK TABLES;
```
## 特别注意事项
在实施正式系统的数据迁移之前，务必先部署一个测试系统，等在测试系统中对迁移的流程走过一遍，并且系统能够正常运行之后，才迁移正式的系统的数据。不然当你将正式系统切换到新的数据库后，发现某些配置不对，版本不同造成的问题会使你措手不及，惊慌失措。
比如在本次中就遇几个问题：
#### 1.sql_mode问题，遇到报错如下
```shell
SQLSTATE[42000]: Syntax error or access violation: 1231 Variable 'sql_mode' can't be set to the value of 'NO_AUTO_CREATE_USER'")
```
- 解决方式1：
将新的MySQL设置为：
```shell
sql_model=STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_	BY_ZERO,NO_ENGINE_SUBSTITUTION
```
- 设置之后发现还是不行，后经查阅，将应用系统改为非严格模式：strict=false
#### 2.现在位于Linux上的问题解决了，可是当我在Windows上连接时，却发现又产生新的账号问题；
```shell
SQLSTATE[HY000] [2054] The server requested authentication method unknown to the client
```
很奇葩，windows上用cmd的客户端可以登录，但是系统却无法登陆，此时系统用户的加密方式已经改为mysql_native_password,但是任然报错。解决此问题的方式是，在MySQL配置文件中修改默认的认证加密方式，问题迎刃而解，配置如下：
```shell
default_authentication_plugin=mysql_native_password
```
![image.png](/images/2019/08/21/592fe600-c3e1-11e9-ad11-1174ecaccd6e.png)