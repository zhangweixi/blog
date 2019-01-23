----
title: Linux安装munin
description: 服务器监控软件munin安装过程中的一点问题
comments: true
date: 2019/1/20 20:46:25
tags: 
- linux
categories:
- Linux
----


>  本文参考博客Ron Ngai的安装博客几指引下操作的，[博客地址](https://www.cnblogs.com/rond/p/3757804.html)
>  安装系统：centos7

#### 1.安装客户端
```bash
[root@home] yum install munin-node
```


这个客户端不是任何人都能否来采集信息的，所以配置一个允许来采集的服务器的ip,编辑文件,由于是在一台服务器上，所以是127.0.0.1，如果不是一台，那么自行配置为你的服务端ip地址


```bash
[root@home] vim /etc/munin/munin-node.conf

allow ^127\.0\.0\.1$
allow ^::1$
```

配置好了，那么就启动呗，操作命令有：

```bash
 #service munin-node start        #启动<br/>
 #service munin-node stop        #停止<br/>
 #service munin-node restart     #重启<br/>
```


#### 2.安装服务端程序
```bash

[root@home] yum install munin

```
安装完成之后，会产生如下文件：

```bash

[root@home] ll   /usr/bin | grep munin

-rwxr-xr-x    1 root root        4258 Nov 24 15:54 munin-check
-rwxr-xr-x    1 root root         654 Nov 24 15:54 munin-cron
-r-xr-xr-x    1 root root        3351 Nov 24 15:53 munindoc
```

&nbsp;
说明：这个软件并不是常驻内存的，所以每次运行都需要执行一个命令，而按作者原本的意思是软件安装完成后会自动在系统的定时任务里面添加一条记录，但是失败的话就不一定了，定时任务如下：
&nbsp;


```bash
1 7 * * * curl http://www.xx.com/api/speed/admin/create_paper
* * * * * su munin /usr/bin/munin-cron #munin新增的任务
```


但是:如果没有呢，那很有可能是失败了，这个时候去手动执行一次这个命令，注意不要用root用户直接执行，要使用munin用户执行，如果有问题的话会提示错误的，不管他是否工作，咱们先做一个配置，配置服务端在正常的情况下去采集某台服务器的信息:

```bash
[root@home]  vim /etc/munin.conf

[local.127-0-0-1] #括号里面的名词可以随便写，那只是作为一个区分各个被监控主机的名字,下面的ip改成您的ip
    address 127.0.0.1
    use_node_name yes
```


如果出错，安装一下这个软件试试：
```bash
[root@home] yum install zlib
```


#### 3.验证结果
如果现在一切没有问题的话，那么就会有结果产生了，产生的结果保存在【/var/www/html/munin】,如果里面有很多html文件的话，那么就算成功了，如果没有呢，那么再次手动执行一下服务端程序，再看看

产生结果后，这个时候只需要将这个目录配置成可以用web访问的方式就可以了，具体可参看lnmp环境搭建和虚拟机配置