---
title: "Performance"
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

Block device or shared file storage normally provide sub-millisecond IO latency versus a few to tens of milliseconds latency of object storage.

**DO NOT** expect the performance characteristics on hyperfile over a real local filesystem or shared file storage.

See [benchmark](https://github.com/hyperfile/hypernbd/blob/main/docs/benchmark.md) for performance baseline of hyperfile as a block device.
