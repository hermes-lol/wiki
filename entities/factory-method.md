---
title: 工厂方法模式 (Factory Method)
created: 2026-04-23
updated: 2026-04-23
type: entity
tags: [software, design-pattern]
sources: [raw/sites/github.com_liu-jianhao_Cpp-Design-Patterns/Factory Method/README.md]
confidence: high
---

# 工厂方法模式 (Factory Method)

定义一个用于创建对象的接口，让子类决定实例化哪一个类。Factory Method 使得一个类的实例化延迟到子类。

## 动机

在软件系统中，经常面临着创建对象的工作；由于需求的变化，需要创建的对象的具体类型经常变化。

如何应对这种变化？如何绕过常规的对象创建方法(new)，提供一种"封装机制"来避免客户程序和这种"具体对象创建工作"的紧耦合？

## 要点总结

- 隔离类对象的使用者和具体类型之间的耦合关系
- 通过面向对象手法，将所要创建的具体对象工作延迟到子类
- 实现一种扩展（而非更改）的策略
- 解决"单个对象"的需求变化
- **缺点**：要求创建方法/参数相同

## 相关模式

- [[abstract-factory]] - 工厂方法变体，处理一系列相关对象
- [[singleton]] - 工厂方法常返回单例
- [[prototype]] - 另一种对象创建方式，通过克隆
- [[builder]] - 构建复杂对象的工厂方法变体