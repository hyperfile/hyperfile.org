---
title: "准备"
description: ""
summary: ""
date: 2025-09-27T05:00:59Z
lastmod: 2025-09-27T05:00:59Z
draft: false
weight: 840
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

## 安装 Rust

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

## 克隆项目

```bash
git clone https://github.com/hyperfile/hyperfile.git
```

## 编译

```bash
# 默认 reactor 模式
cargo build --release

# 增加 WAL 特性
cargo build --release --features wal

# blocking 模式
cargo build --release --no-default-features --features blocking
```
