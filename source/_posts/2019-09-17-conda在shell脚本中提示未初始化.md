---
title: conda在shell脚本中提示未初始化
tags:
  - python
categories:
  - Python
toc: false
description: >-
  用户首次使用conda时，需要先进行初始化，初始化后必须重新执行.bashrc文件才能立刻生效，当在shell脚本中激活环境是，必须将.bashrc文件中关于初始化conda的代码拷贝到shell文件头，才能在shell中激活环境。
date: 2019-09-17 11:59:40
---

### 初始安装时候自动初始化
每个用户在使用conda的时候，都必须要指定一种shell类型，如bash,fish，tcsh,zsh等，当在安装conda的时候，其实安装过程中已经进行了这样的初始化操作。因而当使用conda activate 命令的时候，能够成功的执行。假设这个时候使用的是root用户进行安装的。

除了base环境外，现在，新建一个环境
```shell
[root]# conda create -n flask python=2.7

#现在有两个虚拟环境
[root]#  conda env list
base                  *  /usr/local/anaconda3
flask                    /usr/local/anaconda3/envs/flask
```
#### 

### 给其他用户初始化conda
现在，我们进行以下操作：
1. 切换到用户www
2. 查看conda有哪些环境
3. 进入flask环境
```shell
[root]# su www
[www]# conda env list
base                  *  /usr/local/anaconda3
flask                    /usr/local/anaconda3/envs/flask

[www]# conda activate flask
CommandNotFoundError: Your shell has not been properly configured to use 'conda activate'.
To initialize your shell, run

    $ conda init <SHELL_NAME>

Currently supported shells are:
  - bash
  - fish
  - tcsh
  - xonsh
  - zsh
  - powershell

See 'conda init --help' for more information and options.

IMPORTANT: You may need to close and restart your shell after running 'conda init'.

```
从以上结果可以看出，conda是生效的，但是当前用户却无法激活某个环境，那是因为目前还没有初始化当前用户在使用conda时应该使用什么shell.查看用户目录下的.bashrc文件，内容如下：
```shell
[www]# vim ~/.bashrc

#============================内容如下=======================================
# .bashrc

# Source global definitions
if [ -f /etc/bashrc ]; then
    . /etc/bashrc
fi

# Uncomment the following line if you don't like systemctl's auto-paging feature:
# export SYSTEMD_PAGER=

# User specific aliases and functions

~
#=========================================================================    
```

如果你的情况如上，没有关于任何conda init相关的任何内容，那么就表明确实没有初始化，现在来初始化：
```shell
[zhangweixi@dev001 ~]$ conda init bash
no change     /usr/local/anaconda3/condabin/conda
no change     /usr/local/anaconda3/bin/conda
no change     /usr/local/anaconda3/bin/conda-env
no change     /usr/local/anaconda3/bin/activate
no change     /usr/local/anaconda3/bin/deactivate
no change     /usr/local/anaconda3/etc/profile.d/conda.sh
no change     /usr/local/anaconda3/etc/fish/conf.d/conda.fish
no change     /usr/local/anaconda3/shell/condabin/Conda.psm1
no change     /usr/local/anaconda3/shell/condabin/conda-hook.ps1
no change     /usr/local/anaconda3/lib/python3.7/site-packages/xontrib/conda.xsh
no change     /usr/local/anaconda3/etc/profile.d/conda.csh
modified      /home/zhangweixi/.bashrc

==> For changes to take effect, close and re-open your current shell. <==
```
如果输出这样的信息，表示初始化成功，再看.bashrc文件，内容如下：
```shell
[zhangweixi@dev001 ~]$ vim ~/.bashrc
#=======================内容如下===========================
# .bashrc

# Source global definitions
if [ -f /etc/bashrc ]; then
    . /etc/bashrc
fi

# Uncomment the following line if you don't like systemctl's auto-paging feature:
# export SYSTEMD_PAGER=

# User specific aliases and functions


# >>> conda initialize >>>
# !! Contents within this block are managed by 'conda init' !!
__conda_setup="$('/usr/local/anaconda3/bin/conda' 'shell.bash' 'hook' 2> /dev/null)"
if [ $? -eq 0 ]; then
    eval "$__conda_setup"
else
    if [ -f "/usr/local/anaconda3/etc/profile.d/conda.sh" ]; then
        . "/usr/local/anaconda3/etc/profile.d/conda.sh"
    else
        export PATH="/usr/local/anaconda3/bin:$PATH"
    fi  
fi
unset __conda_setup
# <<< conda initialize <<<

```
由此可以看出，conda在该文件中写入了一些东西，那么这个试试来试试能激活环境吗？
```shell
[zhangweixi@dev001 ~]$ conda activate flask

CommandNotFoundError: Your shell has not been properly configured to use 'conda activate'.
To initialize your shell, run

    $ conda init <SHELL_NAME>

Currently supported shells are:
  - bash
  - fish
  - tcsh
  - xonsh
  - zsh
  - powershell

See 'conda init --help' for more information and options.

IMPORTANT: You may need to close and restart your shell after running 'conda init'.

```
妹的，还是不行呀？到底啥原因？如果你切换当前账号到root,然后再切换回来www用户来，再执行`conda activate flask`，你会发现，成功的激活环境。原因在于：
**当某用户进入系统后，会自动执行用户目录下的.bashrc文件**，当第一次初始化conda init后，把相应的内容出入了.bashrc文件，可是这些代码并没有在环境中执行，要想立刻生效，可以通过手动执行这个脚本：`source ~/.bashrc`,这样，再去激活环境就没有什么问题了。

### 在shell脚本中激活环境
现在来编写一个很简单的shell，如下：
```shell
[www]# vim test.sh
#==================内如如下==============
#!/bin/bash
conda activate flask
#====================内容结束===========
[www]# bash test.sh

CommandNotFoundError: Your shell has not been properly configured to use 'conda activate'.
To initialize your shell, run

    $ conda init <SHELL_NAME>

Currently supported shells are:
  - bash
  - fish
  - tcsh
  - xonsh
  - zsh
  - powershell

See 'conda init --help' for more information and options.

IMPORTANT: You may need to close and restart your shell after running 'conda init'.
```
操蛋，居然不行，直接告诉如何操作把，把.bashrc文件中初始化conda的那一段代码复制到test.sh文件的上面，变成这样:
```shell
#=========================test.sh内如如下=========================
#!/bin/bash
# >>> conda initialize >>>
# !! Contents within this block are managed by 'conda init' !!
__conda_setup="$('/usr/local/anaconda3/bin/conda' 'shell.bash' 'hook' 2> /dev/null)"
if [ $? -eq 0 ]; then
    eval "$__conda_setup"
else
    if [ -f "/usr/local/anaconda3/etc/profile.d/conda.sh" ]; then
        . "/usr/local/anaconda3/etc/profile.d/conda.sh"
    else
        export PATH="/usr/local/anaconda3/bin:$PATH"
    fi  
fi
unset __conda_setup
# <<< conda initialize <<<

conda activate flask
which python               
#==============================内容结束====================

[www]# bash test.sh 
/usr/local/anaconda3/envs/flask/bin/python
```
有此可见，conda激活成功，只不过是shell执行完毕后，环境也就自动销毁了，但是在shell的过程当中，还是处于所激活的环境的。