---
title: 门面模式 (Facade)
created: 2026-04-23
updated: 2026-04-23
type: entity
tags: [software, design-pattern]
sources: [raw/sites/github.com_liu-jianhao_Cpp-Design-Patterns/Facade/README.md]
confidence: high
---

# 门面模式 (Facade)

为子系统中的一组接口提供一个一致(稳定)的界面，Façade模式定义了一个高层接口，这个接口使得这一子系统更加容易使用(复用)。

## 动机

客户和组件中各种复杂的子系统有过多的耦合。

如何简化外部客户程序和系统间的交互接口？如何解耦？

## 要点总结

- 从客户程序角度来看，简化了整个组件系统的接口
- 达到一种"解耦"的效果——内部子系统的任何变化不会影响到 Façade 接口的变化
- 更注重架构的层次去看整个系统，而不是单个类的层次
- Façade 组件中的内部应该是"相互耦合关系比较大的一系列组件"

## 与代理模式的区别

| Facade | Proxy |
|--------|-------|
| 系统间（单向）解耦 | 系统内对象访问控制 |
| 简化整个子系统接口 | 控制对单个对象的访问 |
| 不需要了解子系统内部 | 代理对象对客户端透明 |

## 相关模式

- [[mediator]] - 中介者解耦系统内各个对象（双向）
- [[proxy]] - 控制对对象的直接访问
- [[abstract-factory]] - 门面常配合抽象工厂使用
- [[singleton]] - 门面通常作为单例