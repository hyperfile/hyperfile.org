---
title: "What is Hyperfile"
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

Hyperfile's history originates from a very simple and fundamental need: **performing file operations directly on S3**.

Based on [B+tree](https://en.wikipedia.org/wiki/B%2B_tree) data structure, Hyperfile implements [an object format that enables random read write operations](/docs/guides/architecture/#random-read-write-object-format) directly on S3. Through file-like interface APIs, applications can perform file operations directly.

Key features:

- Appending data
- Random write
- Truncate
- Write zero aware

Hyperfile project currently focuses on file-level feature support, so it is not a implementation of filesystem. This means that filesystem level operations, such as: mv, mkdir, etc., are currently not supported.

Hyperfile also support following advanced features:

- Customized block size
- Copy-on-Write (CoW)
- Snapshot
- Write-Ahead-Log (WAL)
- local cache for file data and meta data

## Typical use cases

- Virtual disk image
- Dump of virtual memory content (e.g. firecracker)
- Files that are kept open for long periods and have contents keep appending

## Components

- [btree-ondisk](https://github.com/daiyy/btree-ondisk) Implementation of Btree
- [hyperfile-reactor](https://github.com/hyperfile/hyperfile-reactor) A lightweight single-threaded task execution framework
- [hyperfile](https://github.com/hyperfile/hyperfile) Hyperfile core
- [hyperfile-cleaner](https://github.com/hyperfile/hyperfile-cleaner) Hyperfile segments cleanup
- [hypercli](https://github.com/hyperfile/hypercli) Command line toolkits
- [hypernbd](https://github.com/hyperfile/hypernbd) Block device implementation as a NBD device
