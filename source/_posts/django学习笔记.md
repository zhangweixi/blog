---
title: django学习笔记
tags:
  - python
  - django
originContent: ''
categories:
  - Python
toc: false
date: 2019-02-26 13:32:30
---

## 时间
1. 将数据库的DateTimeField字段的日期格式化为:"2019-01-01 13:04:45",\
```python

#字段.strftime("%Y-%m-%d %H:%M:%S")，如果有个产品模型
goods = Goods.objects().get(id=1)
goods.created_at = goods.created_at.strftime("%Y-%m-%d %H:%M:%S")

```
2. 