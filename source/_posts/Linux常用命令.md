---
title: Linux常用命令 长期更新
date: 2019-09-03
categories:
    - 技术储备
tags:
    - Linux
---

## 查看系统版本

```cmd
uname -an
```

## 查看端口

```cmd
sudo ss -tunlp
```
<!--more-->
## 修改容器为开机自启动

```cmd
docker update --restart=always xxx
```

## 重启

```cmd
systemctl daemon-reload  
systemctl restart docker
```

## 设置静态IP

```cmd
DOOTPROTO="static"
```
