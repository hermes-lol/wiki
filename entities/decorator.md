---
title: 装饰器模式 (Decorator)
created: 2026-04-23
updated: 2026-04-23
type: entity
tags: [software, design-pattern]
sources: [raw/sites/github.com_liu-jianhao_Cpp-Design-Patterns/Decorator/README.md]
confidence: high
---

# 装饰器模式 (Decorator)

动态（组合）地给一个对象增加一些额外的职责。就增加功能而言，Decorator模式比生成子类（继承）更为灵活（消除重复代码 & 减少子类个数）。

## 动机

在某些情况下我们可能会"过度地使用继承来扩展对象的功能"，由于继承为类型引入的静态特质，使得这种扩展方式缺乏灵活性；并且随着子类的增多（扩展功能的增多），各种子类的组合会导致更多子类的膨胀。

如何使"对象功能的扩展"能够根据需要来动态地实现？同时避免"扩展功能的增多"带来的子类膨胀问题？

## 要点总结

- 采用组合而非继承，在运行时动态扩展对象功能
- 可以根据需要扩展多个功能
- 避免使用继承带来的"灵活性差"和"多子类衍生问题"
- 在接口上表现为 is-a 继承关系（Decorator 继承 Component）
- 在实现上表现为 has-a 组合关系（Decorator 包含 Component）
- 解决"主体类在多个方向上的扩展功能"——"装饰"的含义

## 适用场景

- 需要动态添加或撤销职责
- 处理那些可以撤销的职责
- 继承产生大量子类无法管理时

## 相关模式

- [[proxy]] - 代理模式结构类似，但目的不同
- [[composite]] - 装饰器可以包装组合对象
- [[strategy]] - 两者都使用组合而非继承
- [[adapter]] - 适配器也是包装，但目的是转换接口