---
title: 百度 iOS 面试真题总结（含一二三面深度解析）
date: 2024-03-02 23:23:39
updated: 2025-07-04
tags:
  - iOS面试
  - iOS面试真题
  - 百度
  - 内存管理
  - 多线程
categories:
  - iOS开发
  - 面试题
abbrlink: 9fcf0002
description: 总结百度 iOS 一二三面面试真题与面试经验，涵盖基础原理、UI 响应机制、内存管理、多线程、Binder 通讯、源码分析等核心知识点。适合准备百度及大厂 iOS 岗位的同学参考。
keywords:
  - 百度 iOS 面试
  - iOS 面试题
  - iOS 内存管理
  - 多线程
  - 响应链
  - OpenGL
  - Binder机制
  - AFNetworking
toc: true
---

本文记录了百度 iOS 岗位的三轮面试经历，涵盖 iOS 基础、UI 响应链、内存管理、多线程、源码阅读、OpenGL 渲染、跨语言知识（如 Python & JVM）等内容，适合希望进入百度等大厂的 iOS 工程师参考。

---

## 百度一面：基础知识 + 跨平台能力

### 1. iOS 基础知识点

涉及 UIViewController 生命周期、视图加载时机、UI 响应链、图片加载、图标内存大小估算等。

### 2. Python 基础知识

考察线程安全、多线程与集合类、类继承、JVM 基础等跨平台知识点，体现广度能力。

### 3. iOS 源码阅读经验

面试官关注是否阅读过 AFNetworking、YYModel、Masonry 等三方库源码，建议提前准备一两处代码逻辑作为回答素材。

### 4. OpenGL & GLSurfaceView 基础

图形绘制流程，包括 Shader 编程、FrameBuffer 配置、OpenGL 渲染管线的调用流程。

### 5. 项目优化实践

主要围绕你负责的模块、性能优化方法（如卡顿检测、图片内存优化）、Crash 分析与修复方案等展开。

### 6. 多进程通信机制

提到 Android 的 Binder/AIDL 机制，如果你有了解，也是体现技术深度和广度的加分项。

---

## 百度二面：深入理解 iOS 原理

### 1. Delegate、Notification 和 KVO 对比

- KVO：适合属性观察，自动触发，容易产生内存泄漏；
- Notification：解耦强，适合广播型通信；
- Delegate：一对一，强类型，结构清晰。

### 2. UI 中心视图实现方式

考察 AutoLayout（约束）、frame 手动设置、居中布局等方法的灵活掌握。

### 3. 触摸事件响应机制

- 事件传递路径：UIApplication → UIWindow → ViewController → View；
- FirstResponder 的确定；
- UIResponder 链关系。

### 4. UIViewController 生命周期问题

重点考察 `loadView`、`viewDidLoad` 的调用时机，以及仅 alloc 未使用的 VC 是否触发视图加载。

### 5. UILabel vs drawRect 性能比较

系统控件（UILabel）通常自带优化；自绘方式性能高但复杂，适用于高度定制需求。

### 6. AFNetworking 是否支持 IPv6？

考察是否了解苹果对 App 上架网络兼容性的要求，AF 在 iOS9+ 使用 NSURLSession 支持 IPv6。

### 7. 64 位架构与 i386 区别

- i386 是 32 位模拟器架构；
- x86_64 是 64 位；
- arm64 是真机架构；
- 64 位系统提供更大的寻址空间、更高性能。

### 8. iOS 应用状态及后台执行

iOS 应用状态包括：Not running、Inactive、Active、Background、Suspended。退到后台时可使用任务保持短时间执行。

### 9. 图片内存计算

例如 200×300 RGBA 图片：`200 × 300 × 4 = 240000 bytes ≈ 234KB`。

### 10. Block 内存泄漏问题

Block 默认强引用外部变量（如 self），容易造成 retain cycle，需使用 `__weak` 或 `__block`。

### 11. [object copy] 是浅拷贝还是深拷贝？

多数情况下是浅拷贝，除非手动实现 `copyWithZone:` 方法。需遵循 `NSCopying` 协议。

### 12. Images.xcassets 的优势

支持 @2x/@3x 自动加载、内存优化、打包统一管理等，推荐使用。

---

## 百度三面：底层机制 + 架构理解

这一面偏重对核心知识点的深度理解，问题极其细致。

### 1. 内存管理原理

涉及 ARC 自动引用计数、弱引用释放机制、AutoreleasePool 生命周期等。

### 2. 多线程基础

了解 GCD、NSOperationQueue 的实现与使用场景。

### 3. 队列与线程模型

主队列、串行/并发队列、栅栏函数、线程同步策略的优劣。

### 4. 线程锁机制

- @synchronized：简单但性能差；
- NSLock、NSRecursiveLock、dispatch_semaphore、pthread_mutex 的应用场景；
- 死锁成因及避免方式。

### 5. MVVM 架构设计

数据与视图的解耦思维、数据绑定技术（如 KVO、RAC）、MVVM 和 MVC 的取舍。

---

## 面试总结 & 准备建议

百度的 iOS 面试难度不低，知识广度 + 深度要求都较高。涵盖了语言底层、UI 响应机制、网络、多线程、内存优化、架构设计等多个维度。

🎯 **推荐重点准备方向：**

- Objective-C Runtime、KVO 实现、消息转发机制
- ARC 内存管理 + Retain Cycle 处理策略
- 多线程并发处理（GCD、锁机制）
- 网络栈基本流程、IPv6 支持
- 自定义 UI 控件、性能优化
- 架构能力（MVC/MVVM、组件化）

---

💡 希望这篇《百度 iOS 面试真题总结》对你有所帮助！如果你正在准备大厂面试，不妨将这些问题整理成知识图谱，逐个击破，祝你早日拿到满意 offer 🚀
