---
title: "开始"
description: ""
summary: ""
date: 2025-09-27T05:00:59Z
lastmod: 2025-09-27T05:00:59Z
draft: false
weight: 810
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

## 概览

长久以来，作为云上持久化存储的基石，对象存储以它的使用简单，单价便宜，按需使用，按量计费等特点受到了用户的青睐。

## 对象 vs 文件

虽然对象存储（以 Amazon S3 为代表）已经存在了很多年内，也已经被行业所接受，但文件接口仍然是绝大部分应用与持久化数据交互所使用的方式。

主要由以下一些原因：

- 在 IT 的世界里，文件接口有着比对象存储协议更长的发展和使用历史
- 符合 POSIX 规范的文件接口相比于对象存储的 API 规范定义了更丰富的语义和能力
- 文件服务在整个 IT 的环境里更具通用型，无论是在本地还是线下数据中心，或者在云上环境
- 对象存储上的对象具有不可变性，绝大部分的对象存储目前不支持追加写，或者随机读写（尤其是随机写），从而限制了对象存储的使用场景
- 对象存储上对象读写延迟比本地存储高了几个数量级别

但，对象存储在云环境下有着不可取代的优势：

- 以服务形式存在，随时可用，并且具有高可用性和高持久性
- 超低的每 GB 价格
- 延迟虽然较高，但与其他存储形态相比，带宽有巨大的优势

因此，直接使用文件的接口来操作对象存储上的对象在很多场景下都相应的诉求。

## 现有的解决方案

为了实现使用对象存储进行数据持久化，并且使用文件接口进行数据操作的目标，已经涌现了非常多的工具/方案来试图整合对象存储与文件接口。

这些工具和方案总结下来大概可以归纳为以下几个大的方向：

### 转义对象协议到文件协议

**特点**: 使用 S3 原生的前缀和对象名作为目录结构和文件名，实现文件接口与对象存储 API 的直接转换.

**优势**: 实现简单，保持原生 S3 的数据组织结构.

**不足**: 部分更新数据内容后需要对 S3 上对应的对象进行完全覆盖重写，在文件比较大的情况下有明显的性能影响.

**相关项目**:
- [s3fs](https://github.com/s3fs-fuse/s3fs-fuse)
- [goofys](https://github.com/kahing/goofys)
- [mountpoint-s3](https://github.com/awslabs/mountpoint-s3)

### 网关（缓存）模式

**特点**: 依赖 S3 原生数据索引实现目录结构，通过写缓存为新写入的数据提供临时持久化空间，通过后台进程回写所有的修改数据.

**优势**: 可以在最大程度上实现 POSIX 语义，保持原生 S3 的数据组织结构.

**不足**: 缓存层的数据与 S3 上的数据为最终一致，需要为缓存层提供额外的资源.

**相关项目**:
- [Amazon S3 File Gateway](https://aws.amazon.com/storagegateway/file/s3/)
- [Amazon FSx for Lustre](https://aws.amazon.com/fsx/lustre/)
- [Alluxio](https://www.alluxio.io/)

### 独立元数据服务

**特点**: 解耦并重建元数据，S3 上的持久化数据以分片的形式存在.

**优势**: 可以在最大程度上实现 POSIX 语义.

**不足**: 需要维护元数据服务与 S3 上数据分片的一致性，并且需要为元数据服务提供额外的资源，无法保持 S3 原来的数据组织结构.

**相关项目**:
- [JuiceFS](https://juicefs.com/)

### 元数据和数据一起存储于对象存储

**特点**: 重建元数据，并且把元数据和数据一起持久化在 S3 上.

**优势**: 无需额外的资源.

**不足**: 无法保持 S3 原来的数据组织结构.

**相关项目**:
- [s3ql](https://github.com/s3ql/s3ql)
- [ZeroFS](https://github.com/Barre/ZeroFS)

### 特性比较

| 方法 | 保持原生 S3 对象 | 保持原生 S3 目录结构 | 重建元数据 | 需要额外服务 |
| ---- | ---- | ---- | ---- | ---- |
| 协议转义 | ✅ | ✅ | ❌ | ❌ |
| 网关（缓存）| ✅ | ✅ | ❌ | ✅ |
| 独立元数据 | ❌ | ❌ | ✅ | ✅ |
| 元数据共存 | ❌ | ❌ | ✅ | ❌ |

## Hyperfile 的技术路线

按以上的分类方法，与已有的方案相比，Hyperfile 符合采用了"**元数据和数据一起存储于对象存储**"的设计, 而不同的是Hyperfile项目目前着重解决单个文件级别在 S3 上的持久性问题，因此，Hyperfile目前并不提供一个完整的文件系统实现.

## 进一步阅读

- [Amazon S3 对象概述](https://docs.aws.amazon.com/zh_cn/AmazonS3/latest/userguide/UsingObjects.html)
- [向目录存储桶中的对象追加数据](https://docs.aws.amazon.com/zh_cn/AmazonS3/latest/userguide/directory-buckets-objects-append.html)
