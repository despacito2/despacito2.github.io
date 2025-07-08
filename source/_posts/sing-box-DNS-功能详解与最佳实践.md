---
title: sing-box DNS 功能详解与最佳实践
tags:
  - sing-box
  - DNS
  - 科学上网
  - 分流
  - 广告拦截
categories:
  - sing-box
  - 配置详解
description: >-
  本文全面介绍 sing-box 中 DNS 模块的功能，包括 DoH/DoT/DoQ 支持、DNS 分流、广告拦截、DNS
  劫持、缓存设置等，并提供功能速查表与示例配置，助你实现高效稳定的 DNS 管理。
toc: true
abbrlink: 57dee22
date: 2025-07-07 09:00:00
---

`sing-box` 的 `dns` 模块不仅实现了基本的域名解析功能，还支持诸多进阶特性，广泛适用于分流、隐私保护、广告拦截、性能优化等场景。

---

<!-- more -->

## 🧠 一、基本功能

### 1. DNS 查询解析

- 支持多种协议：
  - `UDP`
  - `TCP`
  - `DoH`（DNS over HTTPS）
  - `DoT`（DNS over TLS）
  - `DoQ`（DNS over QUIC）
- 可配置多个 DNS 服务器，支持优先级、备用等机制。

### 2. 自定义域名解析（hosts）

可替代系统的 `hosts` 文件，配置方式如下：

```json
{
  "hosts": {
    "example.com": "1.2.3.4"
  }
}
```

---

## 🧰 二、增强功能

### 3. 域名分类与分流（规则匹配）

可通过 `rules` 使用不同 DNS 服务器，例如：

```json
{
  "rules": [
    {
      "domain": "geosite:cn",
      "server": "dns_local"
    },
    {
      "domain": "geosite:category-ads-all",
      "server": "dns_block"
    }
  ]
}
```

### 4. 域名缓存配置

```json
"cache": {
  "enabled": true,
  "size": 4096,
  "min_ttl": 60,
  "max_ttl": 86400
}
```

### 5. 负载均衡（IP 选择策略）

- 可按 `round-robin` 等方式选择多个解析结果。
- 支持 fallback 自动切换。

### 6. DNS fallback

主 DNS 不可用时自动使用备用 DNS。

---

## 🧱 三、进阶功能

### 7. DNS 劫持（Hijack）

```json
{
  "hijack": [
    {
      "domain": "example.com",
      "ip": "127.0.0.1"
    }
  ]
}
```

### 8. 广告域名拦截

结合规则 `geosite:category-ads-all` 可实现广告拦截。

### 9. 过滤污染/无效解析

- 过滤非 A/AAAA 类型的记录；
- 屏蔽伪造 IP 返回（如被污染的解析）。

---

## 🌐 四、隐私与安全相关

### 10. 支持加密 DNS 协议

- ✅ DoH
- ✅ DoT
- ✅ DoQ

### 11. 防泄漏能力

- 可关闭系统 DNS；
- 配合 TUN 模式劫持所有 DNS 请求；
- 本地 DNS 与远程 DNS 联合使用，提升安全性。

---

## 🧭 五、与 TUN 分流协同使用

- DNS 是 TUN 分流的核心依据；
- 搭配 `sniffing` 功能，根据解析出的域名进行策略分流；
- DNS 结果直接影响实际走哪个节点。

---

## ✅ 功能速查表

| 功能项                     | 支持情况 |
|----------------------------|----------|
| UDP/TCP DNS                | ✅        |
| DoH / DoT / DoQ 支持       | ✅        |
| 自定义 hosts               | ✅        |
| DNS 分流（geosite等规则） | ✅        |
| 广告域名拦截               | ✅        |
| 缓存 TTL 控制             | ✅        |
| fallback 多 DNS 支持       | ✅        |
| hijack 劫持域名解析        | ✅        |
| DNS 查询隐私保护          | ✅        |
| 系统 DNS 禁用             | ✅        |
| sniffing 联动              | ✅        |

---

如需示例配置文件或完整模板，欢迎留言获取。
