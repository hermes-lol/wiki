---
title: 单例模式 (Singleton)
created: 2026-04-23
updated: 2026-04-23
type: entity
tags: [software, design-pattern]
sources: [raw/sites/github.com_liu-jianhao_Cpp-Design-Patterns/Singleton/README.md]
confidence: high
---

# 单例模式 (Singleton)

保证一个类仅有一个实例，并提供一个该实例的全局访问点。

## 动机

在软件系统中，经常有这样一些特殊的类，必须保证它们在系统中只存在一个实例，才能确保它们的逻辑正确性、以及良好的效率。

如何绕过常规的构造器，提供一种机制来保证一个类只有一个实例？这应该是类设计者的责任，而不是使用者的责任。

## 实现要点

### 核心特征
- 私有构造函数
- 静态实例指针
- 静态获取实例方法

### 多线程安全
- **双检查锁 (Double-Checked Locking)**：在锁前后两次检查实例是否为空
- 双重检查防止多次加锁，提高性能

### 注意事项
- 构造函数可设置为 `protected` 以允许子类派生
- 一般不要支持拷贝构造函数和 Clone 接口
- 谨防序列化、反射破坏单例

## 相关模式

- [[factory-method]] - 工厂方法常配合单例使用
- [[flyweight]] - 享元模式处理大量单例对象
- [[decorator]] - 单例类的装饰器实现