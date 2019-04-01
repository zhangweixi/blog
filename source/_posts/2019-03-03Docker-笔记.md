---
title: Docker 笔记
tags:
  - docker
originContent: ''
categories:
  - Docker
toc: false
date: 2019-03-03 02:20:25
---

1. docker容器内部无法联网：加启动参数--net host,如:docker run -it -P --net host web /bin/bash
