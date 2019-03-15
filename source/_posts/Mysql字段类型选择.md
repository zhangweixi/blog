---
title: Mysql字段类型选择
tags:
  - MySQL
categories:
  - MySQL
toc: true
description: >-
  不同的字段类型会占用不同的存储空间，如果字段类型选择正确了，从而可以减少磁盘，进而减少购买磁盘的钱,不同的字段类型在查询的时候会有不同的效率，因而选择合适的字段可以提高查询效。可是，如何选择合适的字段类型呢？
date: 2019-03-14 15:00:10
---

### 如何为数据选择合适的字段类型
#### 一、选择合适的字段类型有什么好处？
1. 不同的字段类型会占用不同的存储空间，如果字段类型选择正确了，从而可以减少磁盘，进而减少购买磁盘的钱
2. 不同的字段类型在查询的时候会有不同的效率，因而选择合适的字段可以提高查询效率

![image.png](/images/2019/03/14/d4149ed0-462b-11e9-b958-61cffeec0d3d.png)


#### 二、选择字段的原则
当一个字段可以使用多种类型时，那么应该使用什么样的原则最好呢？
选择的顺序如下：

- 数字
- 日期 或者 二进制
- 字符

![image.png](/images/2019/03/14/1cc6eeb0-462e-11e9-b958-61cffeec0d3d.png)

![image.png](/images/2019/03/14/72fbdf20-462e-11e9-b958-61cffeec0d3d.png)

![image.png](/images/2019/03/14/b13d8630-462e-11e9-b958-61cffeec0d3d.png)

![image.png](/images/2019/03/14/7c390670-462f-11e9-b958-61cffeec0d3d.png)

![image.png](/images/2019/03/14/a118e000-462f-11e9-b958-61cffeec0d3d.png)

![image.png](/images/2019/03/14/b3e11270-462f-11e9-b958-61cffeec0d3d.png)

![image.png](/images/2019/03/14/3759fe50-4630-11e9-b958-61cffeec0d3d.png)


**下表是每种类型占用空间列表：**

|类型|占用空间|示例|
|-|-|-|
|tinyint|1字节|常用与布尔类型，或者状态|
|smallint|2字节||
|mediumint|3字节||
|int|4字节||
|bigint|4字节||
|date|3字节||
|timestamp|4字节||
|datetime|8字节||



对于相同级别的数据类型，应该选择占用空间小的。

#### 三、事例证明