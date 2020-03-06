---
title: STM32模板文件构建
tags:
  - STM32
categories:
  - 嵌入式
toc: false
date: 2020-03-04 22:43:23
---

在学习STM32的时候，对一个新手很困难的一个问题就是如何构建一个模板目录，而一般教程则会告诉你，下载什么东西，然后将什么目录的文件拷贝到你的项目的哪个目录中去。比如：
#### 例：如何构建STM32F407的工程目录
1. 访问：www.stmcu.org。
2. 右上角搜索框输入 stm32cubef4，点击搜索在弹出的搜索结果页中，文档栏第一个有stm32cubef4，点击进去即可找到下载页面。
3. 新建一个工程目录，如图![image.png](/images/2020/03/04/a892cd30-5e27-11ea-98c4-f73a60ff1a18.png)
4. 解压stm32cubef4,将
```shell
STM32Cube_FW_F4_V1.24.1\Drivers\STM32F4xx_HAL_Driver\Src-->FWLIB\Src
STM32Cube_FW_F4_V1.24.1\Drivers\STM32F4xx_HAL_Driver\Inc-->FWLIB\Inc

STM32Cube_FW_F4_V1.24.1\Drivers\CMSIS\Device\ST\STM32F4xx\Source\Templates\arm\startup_stm32f407xx.s,
STM32Cube_FW_F4_V1.24.1\Drivers\CMSIS\Include\
[
	cmsis_armcc.h，
	cmsis_armclang.h ，
	cmsis_compiler.h ，
	cmsis_version.h ，
	mpu_armv7.h ，
	core_cm4.h
]->CORE\

```
#### 如果遇到新的板子怎么办
还是模拟这个路径去寻找吗？如果不对呢？如何获得权威的信息来源呢？

#### 正确方式
1. 到官网下载相应的扩展软件包，如STM32CubeF4，搜出来会很多，找到正确的
2. 解压后内部有一个Documentation文件夹，内部有一个文档，文档里面说明了一个简单的列子，根据说明打开这个工程，看看这里面是如何构建的一个工程![image.png](/images/2020/03/05/dbe13780-5e31-11ea-98c4-f73a60ff1a18.png) 
3. 如果重新编译后在这个目录找不到相应的头文件，按下面操作，一定就能够找到了![image.png](/images/2020/03/05/60eb71c0-5e32-11ea-98c4-f73a60ff1a18.png)

至此，一个完整的构建模板的流程至此完毕