---
title: docker一键启动和关闭，删除所有镜像
categories:
    - [计算机学科,java,spring,springcloud,Docker]
tags:
    - 计算机学科
    - springcloud
    - Docker
    - 知识点
---

docker中 启动所有的容器命令
```bash
docker start $(docker ps -a | awk '{ print $1}' | tail -n +2)
```
docker中    关闭所有的容器命令
```bash
docker stop $(docker ps -a | awk '{ print $1}' | tail -n +2)
```
docker中 删除所有镜像
```bash
docker rmi $(docker images -q)
# 如果删除不成功使用如下命令：
docker rmi -f $(docker images -q)
```