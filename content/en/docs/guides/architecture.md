---
title: "Architecture"
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

Hyperfile organizes and persists data on S3 in a random read-write (log-structured) format by constructing data indexes using B+tree structures.

After data is written, it resides in memory as dirty data (when WAL is enabled, new data is immediately persisted to backend storage).

During flush operations, all modified (dirty) data blocks and their metadata blocks are collected and built into on-disk segment format, completing persistence on the backend storage.

## Terminology

- Staging - where data is stored in random read write format.
- Meta block - index of file address space organized in B+tree.
- Data block - block chunks of file content.
- Segment - container of all modified(dirty) data, including header, meta blocks and data blocks.

## Random Read Write Object Format{#random-read-write-object-format}

The core concept of Hyperfile is organizing file data using B+tree structures. B+trees exist in two forms: in-memory structures and on-disk structures.

**Read Path** - Uses B+tree to search for the location descriptor of the corresponding data block in backend storage by block ID, then reads that data block.

**Write Path** - Uses B+tree to search for the corresponding data block by block ID, executes copy-on-write (CoW) strategy as needed. Newly written data resides in memory as dirty data, and the corresponding path in the B+tree is updated.

**Flush Process** - Collects all dirty data blocks, including B+tree metadata blocks and file data blocks, constructs new segment and writes to backend storage."

![Hyperfile btree map](svgs/hyperfile-light.svg)
