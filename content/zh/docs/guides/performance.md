---
title: "性能"
description: ""
summary: ""
date: 2025-10-07T01:49:34Z
lastmod: 2025-10-07T01:49:34Z
draft: false
weight: 870
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

通常来说, 块设备或者共享文件系统可以提供亚毫秒级的 IO 延迟, 而对象存储的延迟在个位或十位数毫秒左右.

所以, **不要期待** hyperfile 能提供与本地或者共享文件系统一样的性能.

把 hyperfile 模拟成一个块设备后的基线性能, 请参考 [benchmark](https://github.com/hyperfile/hypernbd/blob/main/docs/benchmark.md).
