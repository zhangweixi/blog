---
title: vmware安装vmware-tools
tags:
  - linux
categories:
  - Linux
toc: false
date: 2020-03-29 19:18:37
---

废话少说，直接开始
### 1.找到包含vmware-tools的软件包
很多教程一来就是如何如何安装，问题是现在没有这个软件呀，怎么安装？
打开vmware这个软件的安装目录，搜索该目录下有个叫linux.iso的文件，这家伙里面就包括了所需要的安装脚步
![image.png](/images/2020/03/29/0e062d10-71b0-11ea-9dbd-c1b3cb8786bf.png)

### 2.把这个包弄到虚拟机里面去
既然是iso文件，那就需要用虚拟光驱了
![image.png](/images/2020/03/29/cdcb0a80-71b0-11ea-9dbd-c1b3cb8786bf.png)
### 3.执行安装文件
正式安装之前，强烈建议你批量安装这些工具，免得安装过程中缺少依赖不断报错
```shell
yum -y install perl gcc gcc-c++ make cmake kernel kernel-headers kernel-devel net-tools
```

### 3.配置共享文件
后面的看[这里](https://www.cnblogs.com/wendyw/p/9719872.html)吧