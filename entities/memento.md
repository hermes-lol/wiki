---
title: 备忘录模式 (Memento)
created: 2026-04-23
updated: 2026-04-23
type: entity
tags: [software, design-pattern]
sources: [raw/sites/github.com_liu-jianhao_Cpp-Design-Patterns/Memento/README.md]
confidence: low
---

# 备忘录模式 (Memento)

在不破坏封装性的前提下，捕获一个对象的内部状态，并在该对象之外保存这个状态。这样以后就可以将该对象恢复到原先保存的状态。

## 动机

某些对象的状态转换过程中，可能由于某中需要，要求程序能够回溯到对象之前处于某个点的状态。

如果使用一些公开接口来让其他对象得到对象的状态，便会暴露对象的细节实现。

如何实现对象状态的良好保存与恢复？但同时又不会因此而破坏对象本身的封装性？

## 要点总结

- 备忘录存储原发器(Originator)对象的内部状态，在需要时恢复原发器状态
- 有些过时，现代语言有更好的序列化/反序列化机制

## 应用场景

- 撤销功能
- 事务回滚
- 检查点/快照

## 相关模式

- [[command]] - 结合实现撤销功能
- [[iterator]] - 备忘录可以保存迭代器状态
- [[prototype]] - 深拷贝可实现类似功能