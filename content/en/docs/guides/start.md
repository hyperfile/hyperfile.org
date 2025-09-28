---
title: "Get Started"
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

## Overview

For a long time, as the keystone of persistent storage in the cloud, object storage has been favored by users for its simplicity of use, low cost, on-demand, and pay-as-you-go model.

## Object vs File

Although object storage (represented by Amazon S3) has existed for decades and has been widely accepted by the industry, file interfaces remain the primary way most applications you interact with persistent data.

Reasons including:

- In the IT world, file interfaces have a longer history compared to object storage protocol.
- POSIX defined rich semantics and capabilities for file interface compared to object storage API specifications.
- File services are more universal across the entire IT environment, whether on-premises, in on-premises data centers, or in cloud.
- Objects in object storage have immutability characteristics, most object storage systems currently do not support append writes or random write operations (especially random write), which limits object storage use cases.
- Object read/write latency in object storage is normally higher than local storage.

However, object storage has irreplaceable advantages in cloud environments:

- Exists as a service, available on-demand with high availability and high durability.
- Ultra-low per-GB pricing.
- While latency is higher, bandwidth has tremendous advantages compared to other storage.

Therefore, directly using file interface to operate on object in object storage has corresponding demand in many scenarios.

## Current Solutions

To achieve the goal of using object storage for data persistence while using file interfaces for data operations, numerous tools and solutions have emerged attempting to integrate object storage with file interfaces.

These tools and solutions can generally be categorized into the following major directions:

### Mapping Object API into File Operation

**Characteristics:** Uses S3's native prefixes and object names as directory structure and filenames, implementing direct conversion between file interfaces and object storage APIs.

**Pros**: Simple implementation, maintains native S3 prefix and data organization structure.

**Cons**: After partial data content updates, the corresponding object on S3 needs to be completely overwritten, which has significant performance impact when files are large.

**Typical Projects**:
- [s3fs](https://github.com/s3fs-fuse/s3fs-fuse)
- [goofys](https://github.com/kahing/goofys)
- [mountpoint-s3](https://github.com/awslabs/mountpoint-s3)

### Gateway (Cache) Mode

**Characteristics**: Relies on S3 native data indexing to implement directory structure, provides temporary persistence space for newly written data through write caching, and writes back all modified data through background processes.

**Pros**: Capability to achieve full POSIX semantics, maintains native S3 data organization structure.

**Cons**: Data in the cache layer and data on S3 are eventually consistent, requiring additional resources for the cache layer.

**Typical Projects**:
- [Amazon S3 File Gateway](https://aws.amazon.com/storagegateway/file/s3/)
- [Amazon FSx for Lustre](https://aws.amazon.com/fsx/lustre/)
- [Alluxio](https://www.alluxio.io/)

### Independent Metadata Services

**Characteristics**: Decouples and reconstructs metadata, data on S3 existing in chunks.

**Pros**: Capability to achieve POSIX semantics.

**Cons**: Requires maintaining consistency between metadata services and data chunks on S3, and needs additional resources for metadata services. Breaking S3's original data organization structure.

**Typical Projects**:
- [JuiceFS](https://juicefs.com/)

### Store metadata and file data together in object storage

**Characteristics**: Reconstructs metadata, metadata and file data are all persistent on S3.

**Pros**: No external services required.

**Cons**: Breaking S3's original data organization structure.

**Typical Projects**:
- [s3ql](https://github.com/s3ql/s3ql)
- [ZeroFS](https://github.com/Barre/ZeroFS)

### Comparison

| Approach | Keep native S3 object as file | Keep native S3 prefix as directory  | Reconstruct metadata | Need additinal metadata services |
| ---- | ---- | ---- | ---- | ---- |
| Direct API mapping | ✅ | ✅ | ❌ | ❌ |
| Gateway (Cache) | ✅ | ✅ | ❌ | ✅ |
| Independent metadata service | ❌ | ❌ | ✅ | ✅ |
| Metadata and file data all in S3 | ❌ | ❌ | ✅ | ❌ |

## Hyperfile's approach

According to the above classification, compared to existing solutions, Hyperfile adopts the design of **storing metadata and file data together in object storage**. However, what differs is that the Hyperfile project currently focuses on solving persistence issues for individual files on S3. Therefore, Hyperfile currently does not provide a complete file system implementation.

## Further Reading

- [Amazon S3 objects overview](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingObjects.html)
- [Appending data to objects in directory buckets](https://docs.aws.amazon.com/AmazonS3/latest/userguide/directory-buckets-objects-append.html)
