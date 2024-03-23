---
title: docker容器一键启动和关闭
categories:
    - [计算机学科,java,spring,springcloud,Docker]
tags:
    - 计算机学科
    - springcloud
    - Docker
---

docker中 启动所有的容器命令
docker start $(docker ps -a | awk '{ print $1}' | tail -n +2)
docker中    关闭所有的容器命令
docker stop $(docker ps -a | awk '{ print $1}' | tail -n +2)