---
title: 模板方法模式 (Template Method)
created: 2026-04-23
updated: 2026-04-23
type: entity
tags: [software, design-pattern]
sources: [raw/sites/github.com_liu-jianhao_Cpp-Design-Patterns/Template Method/README.md]
confidence: high
---

# 模板方法模式 (Template Method)

定义一个操作中的算法的骨架 **(稳定)** ，而将一些步骤延迟 **(变化)** 到子类中。Template Method使得子类可以不改变(复用)一个算法的结构即可重定义该算法的某些特定步骤。

## 动机

在软件构建过程中，对于某一项任务，它常常有稳定的整体操作结构，但各个子步骤却有很多改变的需求，或者由于固有的原因（比如框架与应用之间的关系）而无法和任务的整体结构同时实现。

如何在确定稳定操作结构的前提下，来灵活应对各个子步骤的变化或者晚期实现需求？

## 要点总结

- 非常基础性的设计模式，在面向对象系统中有着大量的应用
- 为很多应用程序框架提供了灵活的扩展点
- **"不要调用我，让我来调用你"** 的反向控制结构是典型应用
- 被调用的虚方法可以具有实现，也可以没有任何实现（抽象方法）
- 一般推荐将虚方法设置为 protected

## 与策略模式对比

| Template Method | Strategy |
|-----------------|----------|
| 使用继承 | 使用组合 |
| 算法骨架固定，子类实现特定步骤 | 算法可完全替换 |
| 编译时决定 | 运行时决定 |

## 相关模式

- [[strategy]] - 类似的反向控制结构，但使用组合
- [[factory-method]] - 常配合模板方法使用
- [[observer]] - 两者都涉及反向控制