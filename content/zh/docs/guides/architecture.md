---
title: "架构"
description: ""
summary: ""
date: 2025-09-27T05:00:59Z
lastmod: 2025-09-27T05:00:59Z
draft: false
weight: 830
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

Hyperfile 通过使用 B+树结构重建数据索引，以随机读写（日志结构）格式在 S3 上组织持久化数据.

数据写入后，会以脏数据的形式驻留在内存里（开启 WAL 以后，新数据会立即持久化到后端存储）.

执行刷新时，所有已修改（脏）的数据块及其元数据块都会被收集并构建成磁盘上的段格式并完成在后端存储上的持久化.

## 术语

- Staging - 以随机读写对象格式存储数据的持久化空间
- Meta block - B+树组织的文件地址空间索引
- Data block - 文件内容的数据块
- Segment - 所有修改（脏）数据的容器，包括标头、元块和数据块

## 随机读写对象格式{#random-read-write-object-format}

Hyperfile 的核心是以 B+树的方式组织文件数据，B+树的存在形式分为内存结构和磁盘结构.

**读路径上** - 通过 B+树以块 id 搜索对应数据块在后端存储上的位置描述，然后读取该数据块.

**写路径上** - 通过 B+树以块 id 搜索对应数据块，按需执行写时复制 (CoW) 策略，新写入的数据以脏数据形式驻留内存中，并更新 B+树的对应路径.

**刷新流程** - 归集所有的脏数据块，包括 B+树的元数据块和文件数据块，构建新的段 (Segment) 后写入后端存储.

![Hyperfile btree map](svgs/hyperfile-light.svg)
