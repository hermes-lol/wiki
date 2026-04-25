---
title: 策略模式 (Strategy)
created: 2026-04-23
updated: 2026-04-23
type: entity
tags: [software, design-pattern]
sources: [raw/sites/github.com_liu-jianhao_Cpp-Design-Patterns/Strategy/README.md]
confidence: high
---

# 策略模式 (Strategy)

定义一系列算法，把它们一个个封装起来，并且使它们可互相替换（变化）。该模式使得算法可独立于使用它的客户程序(稳定)而变化（扩展，子类化）。

## 动机

在软件构建过程中，某些对象使用的算法可能多种多样，经常改变，如果将这些算法都编码到对象中，将会使对象变得异常复杂；而且有时候支持不使用的算法也是一个性能负担。

如何在运行时根据需要透明地更改对象的算法？将算法与对象本身解耦？

## 要点总结

- Strategy 及其子类为组件提供了一系列可重用的算法
- 运行时方便地在各个算法之间进行切换
- 消除条件判断语句（在有很多 if/else 时考虑使用）
- 如果 Strategy 对象没有实例变量，可以共享同一个 Strategy 对象，节省开销

## 与其他模式的关系

- 与 [[state]] 模式类似，但意图不同：Strategy 算法可互换，State 行为随状态变化
- 与 [[template-method]] 对比：Template Method 使用继承，Strategy 使用组合
- 常与 [[factory-method]] 结合：由工厂决定使用哪个策略

## 相关模式

- [[state]] - 状态模式结构类似，但意图不同
- [[template-method]] - 类似的反向控制结构
- [[bridge]] - 两者都涉及分离变与不变