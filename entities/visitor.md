---
title: 访问者模式 (Visitor)
created: 2026-04-23
updated: 2026-04-23
type: entity
tags: [software, design-pattern]
sources: [raw/sites/github.com_liu-jianhao_Cpp-Design-Patterns/Visitor/README.md]
confidence: high
---

# 访问者模式 (Visitor)

表示一个作用与某对象结构中的各元素的操作。使得可以在不改变(稳定)各元素的类的前提下定义(扩展)作用于这些元素的新操作(变化)。

## 动机

由于需求的变化，某些类层次结构中常常需要增加新的行为(方法)，如果直接在基类中做这样的更改，将会给子类带来很繁重的变更负担，甚至破坏原有设计。

如何在不更改类层次结构的前提下，在运行时根据需要透明地为类层次结构上的各个类动态添加新的操作，从而避免上面的问题？

## 要点总结

- 通过双重分发实现在不更改 Element 类层次结构的前提下，运行时透明地添加新操作
- 双重分发：accept 方法的多态辨析 + visitElement 方法的多态辨析
- **最大缺点**：扩展类层次结构(添加新的 Element 子类)会导致 Visitor 类的改变
- 适用于"Element 类层次结构稳定，而其中的操作却经常面临频繁改动"

## 适用场景

- 对象结构稳定，但操作经常变化
- 需要对对象结构中的对象进行很多不同的操作
- 将业务逻辑与对象结构分离

## 相关模式

- [[composite]] - 访问者常用于组合结构
- [[interpreter]] - 解释器常用访问者实现
- [[command]] - 都是封装操作，但方式不同