---
title: "Prerequisites"
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

## Install Rust

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

## Clone Hyperfile

```bash
git clone https://github.com/hyperfile/hyperfile.git
```

## Compile

```bash
# default in reactor mode
cargo build --release

# enable WAL feature
cargo build --release --features wal

# compile for blocking mode
cargo build --release --no-default-features --features blocking
```
