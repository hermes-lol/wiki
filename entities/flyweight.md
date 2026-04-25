---
title: 享元模式 (Flyweight)
created: 2026-04-23
updated: 2026-04-23
type: entity
tags: [software, design-pattern]
sources: [raw/sites/github.com_liu-jianhao_Cpp-Design-Patterns/Flyweight/README.md]
confidence: high
---

# 享元模式 (Flyweight)

运用共享技术有效地支持大量细粒度的对象。

## 动机

在软件系统采用纯粹对象方案的问题在于大量细粒度的对象会很快充斥在系统中，从而带来很高的运行时代价——主要指内存需求方面的代价。

如何在避免大量细粒度对象问题的同时，让外部客户程序仍然能够透明地使用面向对象的方式来进行操作？

## 要点总结

- 面向对象很好地解决了抽象性的问题，但是需要考虑对象的代价问题
- Flyweight 主要解决面向对象的代价问题，一般不触及面向对象的抽象性问题
- 采用对象共享的做法来降低系统中对象的个数
- 需要注意对象状态的处理（内部状态 vs 外部状态）

## 内部状态 vs 外部状态

- **内部状态**：可以共享的状态，存储在 Flyweight 对象中
- **外部状态**：依赖具体场景的状态，由客户端管理

## 相关模式

- [[singleton]] - 工厂方法常返回享元
- [[state]] - 状态对象可作为享元
- [[composite]] - 享元可组合成更大结构