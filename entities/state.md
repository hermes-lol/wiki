---
title: 状态模式 (State)
created: 2026-04-23
updated: 2026-04-23
type: entity
tags: [software, design-pattern]
sources: [raw/sites/github.com_liu-jianhao_Cpp-Design-Patterns/State/README.md]
confidence: high
---

# 状态模式 (State)

允许一个对象在其内部状态改变时改变它的行为。从而使对象看起来似乎修改了其行为。

## 动机

对象状态如果改变，其行为也会随之而发生变化，比如文档处于只读状态，其支持的行为和读写状态支持的行为就可能完全不同。

如何在运行时根据对象的状态来透明地改变对象的行为？

## 要点总结

- 将所有与一个特定状态相关的行为都放入一个 State 的子对象中
- 在对象状态切换时，切换相应的对象
- 维持 State 的接口，实现具体操作与状态转换之间的解耦
- 转换是原子性的
- 与 [[strategy]] 模式结构类似，但意图不同

## 状态模式 vs 策略模式

| State | Strategy |
|-------|----------|
| 状态决定行为 | 算法可互换 |
| 状态对象通常知道其他状态 | 策略对象通常独立 |
| 状态转换是自动的 | 策略选择是外部的 |

## 相关模式

- [[strategy]] - 结构类似，但意图不同
- [[singleton]] - 具体状态常作为单例
- [[flyweight]] - 状态对象可共享