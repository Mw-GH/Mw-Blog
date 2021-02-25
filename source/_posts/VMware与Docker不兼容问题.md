---
title: VMware与Docker不兼容解决办法
date: 2019-08-30 16:17:56
categories:
    - 技术储备
tags:
    - WMware
---

<!-- more -->
## 1.使用用docker

第一步：在控制面板中勾选Hyper -v
第二步：在cmd，以管理员身份运行：
bcdedit /set hypervisorlaunchtype auto
第三步:重启

## 2.使用虚拟机

第一步：在控制面板中取消勾选Hyper -v
第二步：在cmd中，以管理员身份运行：
bcdedit /set hypervisorlaunchtype off
第三步:重启

## 这俩哥们不能共存
