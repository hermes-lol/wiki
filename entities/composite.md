---
title: 组合模式 (Composite)
created: 2026-04-23
updated: 2026-04-23
type: entity
tags: [software, design-pattern]
sources: [raw/sites/github.com_liu-jianhao_Cpp-Design-Patterns/Composite/README.md]
confidence: high
---

# 组合模式 (Composite)

将对象组合成树形结构以表示"部分-整体"的层次结构。Composite使得用户对单个对象和组合对象的使用具有一致性(稳定)。

## 动机

客户代码过多地依赖于对象容器复杂的内部实现结构，对象容器内部实现结构(而非抽象结构)的变化引起客户代码的频繁变化，带来了代码的维护性、扩展性等弊端。

如何将"客户代码与复杂的对象容器结构"解耦？让对象容器自己来实现自身的复杂结构，从而使得客户代码就像处理简单对象一样来处理复杂的对象容器？

## 要点总结

- 采用树性结构来实现普遍存在的对象容器
- 将"一对多"的关系转化为"一对一"的关系
- 客户代码可以一致地(复用)处理对象和对象容器
- 客户代码与纯粹的抽象接口——而非对象容器的内部实现结构——发生依赖
- 如果父对象有频繁的遍历需求，可使用缓存技术来改善效率

## 相关模式

- [[decorator]] - 装饰器可以包装组合对象
- [[iterator]] - 遍历组合结构
- [[visitor]] - 在组合结构上操作
- [[chain-of-responsibility]] - 处理组合结构中的请求