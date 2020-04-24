---
title: GIT配置ssh操作
tags:
  - 工具
categories:
  - 工具
toc: false
date: 2020-04-23 13:34:38
---

#### 生成及添加公钥

[https://gitee.com/help/articles/4181#article-header0](https://gitee.com/help/articles/4181#article-header0)

#### 新项目Clone
使用git上面的SSH方式，而不是https方式，
![image.png](/images/2020/04/23/6d686850-8523-11ea-ab9f-57ebd82557ba.png)

#### 如果已经存在了项目，更新Git的操作方式为SSH
```bash
git remote remove origin  #删除旧的更新方式
git remote add origin git@github.com:Username/Your_Repo_Name.git #更新为正式的方式

git branch --set-upstream-to=origin/master master #设置新的方式生效
```