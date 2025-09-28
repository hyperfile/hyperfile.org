---
title: "配置"
description: ""
summary: ""
date: 2025-09-27T05:00:59Z
lastmod: 2025-09-27T05:00:59Z
draft: false
weight: 850
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

## 快速配置
```Rust
    ...
    // open file for read
    let flags = FileFlags::rdonly();
    let mut hyper = Hyper::fs_open(client, uri, flags).await.unwrap();

    // get stat
    let stat = hyper.fs_getattr()?;

    // read from offset for len of buf
    let read_bytes = hyper.fs_read(offset, &mut buf).await.unwrap();

    // close file
    let _ = hyper.fs_release().await?;
    ...
```

## 高级配置
```Rust
    ...
    // overwrite default runtime config
    let mut runtime_config = HyperFileRuntimeConfig::default_large();
    runtime_config.data_cache_dirty_max_flush_interval = u64::MAX;
    runtime_config.data_cache_dirty_max_bytes_threshold = usize::MAX;
    runtime_config.data_cache_dirty_max_blocks_threshold = usize::MAX;

    // open for read
    let flags = FileFlags::rdonly();
    let mut hyper = Hyper::fs_open_opt(&client, &uri, flags, &runtime_config).await.unwrap();

    // read from offset for len of buf
    let read_bytes = hyper.fs_read(offset, &mut buf).await.unwrap();

    // close file
    let _ = hyper.fs_release().await?;
    ...
```
