---
title: EGG实践笔记
tags: []
originContent: ''
categories: []
toc: false
date: 2020-05-18 14:47:08
---

1. 进制数据库字段将驼峰自动转换为下划线,设置：underscored:false
2. 数据库进制自动转换为复数：freezeTableName： true
3. 自定义数据库名称：tableName: xxx
4. 调用service的方式为ctx.service.xxxx.method(),这里的xxxx是某个服务的文件名字，而不是服务的类名