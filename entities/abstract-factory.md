---
title: 抽象工厂模式 (Abstract Factory)
created: 2026-04-23
updated: 2026-04-23
type: entity
tags: [software, design-pattern]
sources: [raw/sites/github.com_liu-jianhao_Cpp-Design-Patterns/Abstract Factory/README.md]
confidence: high
---

# 抽象工厂模式 (Abstract Factory)

提供一个接口，让该接口负责创建一系列"相关或者相互依赖的对象"，无需指定它们具体的类。

## 动机

在软件系统中，经常面临着"一系列相互依赖的对象工作"；同时，由于需求的变化，往往存在更多系列对象的创建工作。

如何应对这种变化？如何绕过常规的对象创建方法(new)，提供一种"封装机制"来避免客户程序和这种"多系列具体对象创建工作"的紧耦合？

## 要点总结

- 如果没有应对"多系列对象创建"的需求变化，则没有必要使用
- "系列对象"指在某一个特定系列的对象之间有相互依赖、或作用的关系
- 不同系列的对象之间不能相互依赖
- 主要在于应用"新系列"的需求变动
- **缺点**：难以应对"新对象"的需求变动

## 与工厂方法对比

| Factory Method | Abstract Factory |
|----------------|------------------|
| 创建单个对象 | 创建一系列相关对象 |
| 继承（子类决定） | 组合（接口实现） |
| 解决单个对象需求变化 | 解决系列对象需求变化 |

## 相关模式

- [[factory-method]] - 工厂方法常实现为抽象工厂的具体工厂
- [[singleton]] - 具体工厂常作为单例
- [[prototype]] - 原型模式可替代抽象工厂
- [[bridge]] - 常与桥接模式配合