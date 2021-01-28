---
title: Linux便签
tags:
  - linux
categories:
  - Linux
toc: false
date: 2020-05-29 16:29:44
---

#### 使用PHP调用shell输出到指定文件

```php
<?php
$cmd = "ls / > outfile 2>&1";
exec($cmd);
```
