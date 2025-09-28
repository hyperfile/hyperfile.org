---
title: "简介"
description: ""
summary: ""
date: 2025-09-27T05:00:59Z
lastmod: 2025-09-27T05:00:59Z
draft: false
weight: 820
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

Hyperfile 项目的历史起源于一个非常朴素的原始需求: **直接在 S3 进行文件操作**.

基于 [B+tree](https://en.wikipedia.org/wiki/B%2B_tree) 数据结构, Hyperfile 实现了一种可以在 S3 直接进行[随机读写的对象格式](/zh/docs/guides/架构/#random-read-write-object-format), 通过类似文件接口的 API, 应用程序可以直接进行文件操作.

支持的重要文件特性：

- 追加写
- 随机写
- 文件截断(truncate)
- 写零感知

Hyperfile 项目目前专注于文件级别的特性支持，所以，它并不是一个文件系统，这也意味着文件系统的级别的操作，比如: mv, mkdir 等目前都不支持.

除了文件操作本身外，Hyperfile 还支持以下一些高级特性:

- 自定义块大小
- 写时复制(CoW)
- 快照回滚
- 预写式日志(WAL)
- 本地数据及元数据缓存

## 典型场景

- 虚拟磁盘镜像
- 虚拟内存导出文件(典型的: firecracker)
- 长时间打开并追加内容的文件

## 项目组件

- [btree-ondisk](https://github.com/daiyy/btree-ondisk) btree 数据结构的实现
- [hyperfile-reactor](https://github.com/hyperfile/hyperfile-reactor) 一个轻量级的单线程任务执行框架
- [hyperfile](https://github.com/hyperfile/hyperfile) Hyperfile 的核心功能实现
- [hyperfile-cleaner](https://github.com/hyperfile/hyperfile-cleaner) Hyperfile 数据清理逻辑的实现
- [hypercli](https://github.com/hyperfile/hypercli) 命令行管理工具
- [hypernbd](https://github.com/hyperfile/hypernbd) 基于 Hyperfile 实现的 NBD 块设备
