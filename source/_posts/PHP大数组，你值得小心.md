---
title: PHP大数组，你得小心
tags:
  - PHP
categories:
  - PHP
toc: false
description: 你有没有想过，存储在电脑上的文件在编程时读入内存的大小会改变吗？你有没有想过，一个长长的字符串被拆分成多段后，他们占用的总的空间是一样的吗？
date: 2019-06-06 16:48:01
---

### 关于内容存储
你有没有想过，存储在电脑上的文件在编程时读入内存的大小会改变吗？你有没有想过，一个长长的字符串被拆分成多段后，他们占用的总的空间是一样的吗？

### 源起
现在所在的项目需要将数据解析成一定的格式来进行操作，所有的原始数据总的也不超过100M，但是在实际的运行过程中，出现过很多次内存溢出致使代码停止运行，开始以为仅仅100来M的数据，把内存调整到1G总是足足有余。可是1G，2G，最后甚至调整到了3G才没有出现类似的故障，后来细查下来，原来PHP中相同的数据量使用数组和字符来存储数据时所占用的内存有这么大的差异，故此记录之。

### 实验
#### 先来个对比吧
![image.png](/images/2019/06/06/72418bd0-8845-11e9-a869-47a7845ed7c9.png)
#### 1.所有存入到一个字符串
```php

$begin	= (memory_get_usage())/1024/1024;
echo "begin:".$begin."\n";

$str	= "";
for($i=0;$i<200000;$i++)
{
    $temp	= md5($i);
    $str	.= $temp.$temp.$temp;
}

$end = (memory_get_usage())/1024/1024;
echo "end:".$end;

//结果如下
begin:0.36307525634766
end:20.363174438477

```

#### 2.存入一维数组
一维数组虽然和开始的字符串是同样多的内容，但是它所占用的内容已经多出17M，相应增加85%；
```php
$begin	= (memory_get_usage())/1024/1024;
echo "begin:".$begin."\n";

$str	= [];
for($i=0;$i<200000;$i++){

	    $temp   = md5($i);
	    array_push($str, $temp);
	    array_push($str, $temp);
	    array_push($str, $temp);
}


$end = (memory_get_usage())/1024/1024;
echo "end:".$end;

//结果如下
begin:0.36358642578125
end:37.044853210449
```


#### 3.存入二维数组
二维数组虽然和开始的字符串是同样多的内容，但是它所占用的内容已经多出50M，相应增加250%；
```php

$begin = (memory_get_usage())/1024/1024;
echo "begin:".$begin."\n";

$str = [];

for($i=0;$i<200000;$i++){

	    $temp	= md5($i);
	    $arr	= [$temp,$temp,$temp];
	    array_push($str, $arr);
}


$end = (memory_get_usage())/1024/1024;
echo "end:".$end;

//结果如下
begin:0.36331176757812
end:70.924461364746

```
### 是什么造成如上的原因呢？
。。。