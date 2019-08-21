---
title: 详解MySQL的SQL Modes
tags:
  - MySQL
categories:
  - MySQL
toc: false
date: 2019-08-21 16:06:46
---

> 段落引用根据MySQL手册翻译而来
> [https://dev.mysql.com/doc/refman/8.0/en/sql-mode.html](https://dev.mysql.com/doc/refman/8.0/en/sql-mode.html)
## MODELES集合
```SHELL
ANSI = REAL_AS_FLOAT, PIPES_AS_CONCAT, ANSI_QUOTES, IGNORE_SPACE, and ONLY_FULL_GROUP_BY
TRADITIONAL = STRICT_TRANS_TABLES, STRICT_ALL_TABLES, NO_ZERO_IN_DATE, NO_ZERO_DATE, ERROR_FOR_DIVISION_BY_ZERO, and NO_ENGINE_SUBSTITUTION 
```
## 所有的SQL MODELS列表
1. ALLOW_INVALID_DATES
mysql会对日期进行合法性检查，对应的字段为date,datetime。当添加上此项的时候，只会检查月份在1-12之间，日期在1-31之间，如果你插入一个无效的日期，将会对其进行改变，如：2004-04-31（4月份不存在31号）自动改变成0000-00-00，无任何报错，如果是严格模式的话，将直接报错。
2. ANSI_QUOTES
当设置了这个后，双引号【“】将被当做【`】来使用，即当做关键的标识符
```SQL
SELECT "TEST"; # 错
SELECT `TEST`; # 错
SELECT 'TEST'; # 对
```
3. ERROR_FOR_DIVISION_BY_ZERO
被0除是否报错，但是实际测试下来，没有发现在什么情况下报错。
```SQL
SET SQL_MODE=""; 
SELECT 10/0; # 结果为null
```
4. HIGH_NOT_PRECEDENCE
对于这一项，没有找到实际的用途，只发现了区别
```SQL
SET sql_mode = '';
SELECT NOT 1 BETWEEN -5 AND 5; # 可以理解为判断这句话：1不是在-5和5之间，错误，所以结果0
-> 0
SELECT NOT 10 BETWEEN -5 AND 5; # 可以理解为判断这句话：10不是在-5和5之间，正确，所以结果1
-> 1

SET sql_mode = 'HIGH_NOT_PRECEDENCE';
# 可以理解为判断这句话：1不是在-5和5之间，这是错误的，但是结果却是正确的
SELECT NOT 1 BETWEEN -5 AND 5; 
-> 1
SELECT NOT 10 BETWEEN -5 AND 5; 
-> 1
```
5. IGNORE_SPACE
在使用函数的时候忽略函数名和左括号【（】之间的空格。
```sql
mysql> set sql_mode = "STRICT_TRANS_TABLES";
Query OK, 0 rows affected, 1 warning (0.01 sec)

mysql> select max (age) from user;
ERROR 1630 (42000): FUNCTION test.max does not exist. Check the 'Function Name Parsing and Resolution' section in the Reference Manual


mysql> set sql_mode = "IGNORE_SPACE";
Query OK, 0 rows affected (0.00 sec)

mysql> select max (age) from user;
+-----------+
| max (age) |
+-----------+
|         3 |
+-----------+
1 row in set (0.00 sec)

```
