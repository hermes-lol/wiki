---
title: 适配器模式 (Adapter)
created: 2026-04-23
updated: 2026-04-23
type: entity
tags: [software, design-pattern]
sources: [raw/sites/github.com_liu-jianhao_Cpp-Design-Patterns/Adapter/README.md]
confidence: high
---

# 适配器模式 (Adapter)

将一个类的接口转换成客户希望的另一个接口。Adapter模式使得原本由于接口不兼容而不能一起工作的那些类可以一起工作。

## 动机

由于应用环境的变化，常常需要将"一些现存的对象"放在新的环境中应用，但是新环境要求的接口是这些现存对象所不满足。

如何应对这些"迁移的变化"？

## 类型

- **类适配器**：通过继承（多重继承）
- **对象适配器**：通过组合（更灵活）

## 应用场景

- 遗留代码复用
- 类库迁移
- 第三方库集成

## 相关模式

- [[decorator]] - 都是包装器，但目的不同
- [[proxy]] - 都是包装器，但目的不同
- [[bridge]] - 两者都涉及接口转换，但目的不同
- [[facade]] - 适配器改变接口，门面简化接口