---
title: 桥接模式 (Bridge)
created: 2026-04-23
updated: 2026-04-23
type: entity
tags: [software, design-pattern]
sources: [raw/sites/github.com_liu-jianhao_Cpp-Design-Patterns/Bridge/README.md]
confidence: high
---

# 桥接模式 (Bridge)

将抽象部分(业务功能)与实现部分(平台实现)分离，使它们都可以独立地变化。

## 动机

由于某些类型的固有的实现逻辑，使得它们具有两个变化的维度，乃至多个纬度的变化。

如何应对这种"多维度的变化"？如何利用面向对象技术来使得类型可以轻松地沿着两个乃至多个方向变化，而不引入额外的复杂度？

## 要点总结

- 使用"对象间的组合关系"解耦了抽象和实现之间固有的绑定关系
- 抽象和实现可以沿着各自的维度来变化（"子类化"它们）
- 类似于多继承方案，但多继承违背单一职责原则
- Bridge模式是比多继承方案更好的解决方法
- 应用在"两个非常强的变化维度"

## 与其他模式的关系

- 与 [[strategy]] 类似，都涉及分离变与不变
- 与 [[adapter]] 对比：Adapter 解决接口不兼容，Bridge 解决多维度变化

## 相关模式

- [[strategy]] - 结构类似，但分离的是算法
- [[abstract-factory]] - 常与桥接模式配合使用
- [[decorator]] - 两者都使用组合