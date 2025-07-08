---
title: sing-box 域名分类与分流策略实战：多 DNS 配置与规则使用详解
date: 2025-07-07 10:30:00
categories:
  - sing-box
  - 配置详解
tags:
  - DNS
  - 分流
  - 域名分类
  - sing-box
  - Clash
  - 网络策略
description: 本文全面讲解如何根据域名功能、地域等进行分类，并结合 DNS 分流规则实现精细化网络控制。适用于 sing-box、Clash 等代理工具，提升网络访问效率与隐私安全。
abbrlink: domain-routing
toc: true
---

在现代网络代理工具（如 sing-box、Clash）中，实现高效、安全的访问控制，域名的**分类与分流策略**是核心技术之一。通过规则划分不同类型的域名，并指定使用的 DNS 或代理出口，可以实现广告拦截、隐私保护、速度优化等目标。

---

<!-- more -->

## 📘 目录

- [1. 域名分类概述](#1-域名分类概述)
- [2. 分流策略简介](#2-分流策略简介)
- [3. 多 DNS 配置与规则使用](#3-多-DNS-配置与规则使用)
- [4. sing-box 分流配置示例](#4-sing-box-分流配置示例)
- [5. 应用总结与实战建议](#5-应用总结与实战建议)

---

## 1. 域名分类概述

常见的域名分类方式如下：

| 分类方式     | 说明                                       | 示例                                  |
|--------------|--------------------------------------------|---------------------------------------|
| 功能分类     | 按网站用途分类，如视频、社交、办公等       | `youtube.com`, `office365.com`       |
| 地域分类     | 根据域名归属地划分，用于国内国际识别       | `geosite:cn`, `geolocation-!cn`      |
| 风险分类     | 恶意、广告、色情、钓鱼等黑名单域名         | `category-ads-all`, `category-malware` |
| 自定义分类   | 用户自建域名分组                           | `custom-academic`, `lan-domain`      |

---

## 2. 分流策略简介

根据分类结果，对不同域名进行差异化处理：

| 策略         | 描述                                   | 示例配置                      |
|--------------|----------------------------------------|-------------------------------|
| 国内直连     | 中国域名走直连                         | `geosite:cn -> direct`        |
| 国外代理     | 非中国域名走代理                       | `geosite:geolocation-!cn -> proxy` |
| 视频专线     | 视频类网站使用特定节点                 | `youtube.com -> us-node`      |
| 拦截广告     | 广告类域名统一阻断                     | `category-ads-all -> block`   |
| 自定义出口   | 某些特殊域名走指定代理或 DNS           | `academic-site -> hk-dns`     |

---

## 3. 多 DNS 配置与规则使用

在 sing-box 中，可以为不同域名指定不同的 DNS 上游服务器：

```jsonc
{
  "dns": {
    "servers": [
      {
        "tag": "dns_local",
        "address": "223.5.5.5",
        "detour": "direct"
      },
      {
        "tag": "dns_doh_google",
        "address": "https://dns.google/dns-query",
        "detour": "proxy"
      },
      {
        "tag": "dns_doh_cf",
        "address": "https://1.1.1.1/dns-query",
        "detour": "proxy"
      }
    ],
    "rules": [
      {
        "domain_suffix": ["baidu.com", "bilibili.com"],
        "server": "dns_local"
      },
      {
        "domain_category": "geolocation-!cn",
        "server": "dns_doh_google"
      },
      {
        "domain_category": "category-ads-all",
        "server": "dns_doh_cf"
      }
    ],
    "strategy": "prefer_ipv4"
  }
}
```

> ✅ 提示：DNS 的 detour 可以配合实际节点策略，如 `proxy` 表示使用代理转发 DNS 查询请求。

---

## 4. sing-box 分流配置示例

```jsonc
{
  "route": {
    "rules": [
      {
        "domain_category": "category-ads-all",
        "outbound": "block"
      },
      {
        "domain_category": "cn",
        "outbound": "direct"
      },
      {
        "domain_category": "geolocation-!cn",
        "outbound": "proxy"
      },
      {
        "domain_suffix": ["youtube.com", "netflix.com"],
        "outbound": "us"
      }
    ]
  }
}
```

搭配 DNS 规则后，将实现“按域名类型定向 DNS + 按域名分流出口”的双重控制。

---

## 5. 应用总结与实战建议

| 场景                   | 建议策略                                     |
|------------------------|----------------------------------------------|
| 科学上网               | 非国内域名使用 DoH，并走代理                 |
| 广告拦截               | `category-ads-all` 拦截，避免加载资源         |
| 视频/媒体优化访问     | 指定视频域名走高速节点，并配置专用 DNS        |
| 区分办公与娱乐流量     | 企业域名直连，娱乐流量走代理或拦截           |
| 内网解析               | 内部域名通过本地 DNS（如局域网 DNS）解析     |

---

## 🔚 总结

通过灵活组合 DNS 服务器与域名分流规则，我们可以打造一个**智能、安全、高效的网络访问系统**。无论是用于日常科学上网、广告拦截、企业办公还是网络优化，这套机制都能提供强有力的支持。

如需生成完整配置、导入 geosite、domain-set 或其他分流模板，可继续向我提问。

