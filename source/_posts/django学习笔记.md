---
title: django学习笔记
tags:
  - python
  - django
categories:
  - Python
toc: true
description: |-
  本文主要整理在使用django中遇到的一些问题<br/>
  1.将数据库的DateTimeField字段的日期格式转化为 "xxxx-xx-xx xx:xx:xx"
date: 2019-02-26 13:32:30
---

## 一、时间
1. 将数据库的DateTimeField字段的日期格式化为:"2019-01-01 13:04:45"
```python
#字段.strftime("%Y-%m-%d %H:%M:%S")，如果有个产品模型
goods = GoodsModel.objects().get(id=1)
goods.created_at = goods.created_at.strftime("%Y-%m-%d %H:%M:%S")

```
### 二、数据库
#### 2.1. django模型中有表的字段和Python关键字冲突时的处理方式?