---
title: 如何在 .NET 中使用 SIMD
source_url: https://www.cnblogs.com/eventhorizon/p/19214931
author: eventhorizon
date: 2026-04-25
category: 后端
tags: [.NET, 性能优化, SIMD, C#]
---

# 如何在 .NET 中使用 SIMD

## 什么是 SIMD

**SIMD (Single Instruction, Multiple Data)** — 单指令多数据，一条指令同时操作多个数据元素。

## 性能对比（数组加法，100万元素）

| Method | Mean |
|--------|------|
| NormalAdd | 901.8 us |
| SimdAdd | **300.2 us** → 3倍提升 |

## 核心 API（System.Runtime.Intrinsics）

| 类型 | 描述 |
|------|------|
| Vector128<T> | 128位向量 |
| Vector256<T> | 256位向量 |
| Vector512<T> | 512位向量 |

```csharp
int simdLength = Vector128<float>.Count; // 4
var va = Vector128.LoadUnsafe(ref arrA[i]);
var vb = Vector128.LoadUnsafe(ref arrB[i]);
(va + vb).CopyTo(resultArray, i);
```

## 硬件加速检查

```csharp
Vector128.IsHardwareAccelerated  // 是否硬件加速
Vector128<float>.IsSupported    // 是否支持（可降级）
```

## System.Numerics 命名空间

- `Vector<T>` — 通用向量，自动选最佳大小
- `Vector2/3/4` — 图形物理计算
- `Matrix4x4` — 变换投影

> 推荐：优先用 `LoadUnsafe`（方便），极致性能用 `Load`（需 unsafe）
