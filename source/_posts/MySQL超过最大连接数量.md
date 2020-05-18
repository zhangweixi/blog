---
title: MySQL超过最大连接数量
tags:
  - MySQL
categories:
  - MySQL
toc: false
date: 2020-05-12 16:08:41
---

### 报错
```sql
SQLSTATE[HY000] [1129] Host '172.20.0.1' is blocked because of many connection errors; unblock with 'mysqladmin flush-hosts' (SQL: select * from `user` where `openid` = oQxMh5b_yYB6YjK74C5l73T5vGZ0 limit 1)
```
### 环境
docker mysql8
几乎处于无人使用的状态


### 解决
```sql
flush hosts;
```

### 为什么会发生这样的问题呢？
