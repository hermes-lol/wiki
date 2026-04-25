---
title: Pretext：值得关注的文本排版引擎
source_url: https://www.cnblogs.com/guangzan/p/19796050
author: guangzan
date: 2026-04-25
category: 前端
tags: [前端, TypeScript, 文本引擎, 性能优化]
---

# Pretext：值得关注的文本排版引擎

**来源**: [博客园 - guangzan](https://www.cnblogs.com/guangzan/p/19796050)  
**发布时间**: 2026-03-30 16:15

---

## 概述

Pretext 是一个用 TypeScript 实现的多行文本精确测量和布局引擎。**不碰 DOM，不触发 reflow**，却能完美匹配浏览器字体引擎在各种语言、emoji、混合文字方向下的真实表现。作者 Cheng Lou（曾参与 React、ReasonML、ReScript、Midjourney）。

> "这是过去十年里最值得关注的文本引擎之一。它不是小打小闹的优化，而是把文本这块一直卡着大家脖子的核心问题彻底解决掉了。"

---

## 核心痛点与解决方案

### 传统问题
- 动态文本（聊天消息、文章卡片、虚拟列表、自动换行输入框、响应式排版）需要知道文本高度
- 传统方案：扔进 DOM → 读 `getBoundingClientRect` / `offsetHeight` → 写回样式 → **触发 reflow**
- 后果：长列表、频繁 resize、AI 实时流式输出时性能雪崩

### Pretext 方案
- `prepare` 预计算 → `layout` 纯算术（毫秒级）
- 跨浏览器一致，绕过 DOM 重排

---

## 核心 API

```typescript
import { prepare, layout } from '@chenglou/pretext'

const prepared = prepare('AGI 春天到了. بدأت الرحلة 🚀', '16px Inter')
const { height, lineCount } = layout(prepared, 300, 24)
```

| 操作 | 500条混合文本 |
|------|--------------|
| prepare | ~19ms |
| layout | ~0.09ms |

---

## 技术原理

1. **prepare 阶段**：Intl.Segmenter 按 grapheme 切分、Canvas.measureText 测量宽度并缓存。作者用 Claude Code 迭代数周确保与浏览器渲染 100% 一致
2. **layout 阶段**：纯 JS 实现，完全抛弃 DOM，毫秒级出结果

---

## 应用场景

- 虚拟化列表 120fps
- Canvas/SVG/WebGL 渲染一致性
- 编辑器实时流式输出
- 响应式高级排版（文字绕图、多栏）
- 布局偏移（CLS）彻底消失

> "Pretext 不是一个库，而是一次前端文本能力的重启。"
