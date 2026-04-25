---
title: 设计模式
created: 2026-04-23
updated: 2026-04-23
type: concept
tags: [software, best-practices]
sources: [raw/sites/github.com_liu-jianhao_Cpp-Design-Patterns/README.md]
confidence: high
---

# 设计模式

设计模式是软件设计中常见问题的典型解决方案。它们不是直接的代码复制，而是经过验证的思维框架，指导如何组织代码来解决特定类型的问题。

## 核心理念

### 解决复杂性的两种方式
1. **分解**：将大问题分解为多个小问题
2. **抽象**：忽视非本质细节，处理理想化的对象模型

### 面向对象设计原则 (SOLID + 更多)

| 原则 | 描述 |
|------|------|
| 依赖倒置 (DIP) | 高层模块不应依赖低层模块，二者都依赖抽象 |
| 开放封闭 (OCP) | 对扩展开放，对更改封闭 |
| 单一职责 (SRP) | 一个类只有一个引起变化的原因 |
| Liskov 替换 (LSP) | 子类必须能替换基类 |
| 接口隔离 (ISP) | 不强迫客户依赖不用的方法 |
| 优先组合 | 对象组合优于类继承 |
| 封装变化点 | 使用封装创建对象之间的分界层 |
| 针对接口编程 | 声明接口而非具体类 |

## 模式分类

### 组件协作
- [[template-method]] - 定义算法骨架，子类实现特定步骤
- [[observer]] - 一对多依赖关系，状态变化通知观察者
- [[strategy]] - 定义算法族，可互相替换

### 单一职责
- [[decorator]] - 动态给对象增加职责
- [[bridge]] - 分离抽象与实现，各自独立变化

### 对象创建
- [[factory-method]] - 延迟到子类决定实例化哪个类
- [[abstract-factory]] - 创建一系列相关对象
- [[prototype]] - 通过克隆原型创建对象
- [[builder]] - 构建复杂对象与其表示分离

### 对象性能
- [[singleton]] - 保证单实例
- [[flyweight]] - 共享细粒度对象

### 接口隔离
- [[facade]] - 提供统一高层接口
- [[proxy]] - 控制对象访问
- [[mediator]] - 中介封装对象交互
- [[adapter]] - 转换接口兼容性

### 状态变化
- [[memento]] - 捕获并恢复内部状态
- [[state]] - 内部状态改变时改变行为

### 数据结构
- [[composite]] - 树形结构，部分整体一致
- [[iterator]] - 顺序访问聚合对象
- [[chain-of-responsibility]] - 链式传递请求

### 行为变化
- [[command]] - 将请求��装为对象
- [[visitor]] - 不改变元素类前提下定义新操作

### 领域问题
- [[interpreter]] - 解释特定语言句子

## 现代使用建议

### 常用模式（高频）
Singleton, Factory Method, Observer, Strategy, Decorator, Proxy, Facade

### 较少使用模式
Builder, Mediator, Memento, Iterator, Chain of Responsibility, Command, Visitor, Interpreter

> 现代编程中，许多模式已被语言特性或库替代（如 Iterator 在 C++ 中被模板替代）。

## 相关概念

- [[solid-principles]] - 面向对象设计原则
- [[refactoring]] - 重构与模式应用
- [[jvm]] - Java 中的设计模式实践