---
title: 观察者模式 (Observer)
created: 2026-04-23
updated: 2026-04-23
type: entity
tags: [software, design-pattern]
sources: [raw/sites/github.com_liu-jianhao_Cpp-Design-Patterns/Observer/README.md]
confidence: high
---

# 观察者模式 (Observer)

定义对象间的一种一对多（变化）的依赖关系，以便当一个对象(Subject)的状态发生改变时，所有依赖于它的对象都得到通知并自动更新。

## 动机

在软件构建过程中，我们需要为某些对象建立一种"通知依赖关系" —— 一个对象（目标对象）的状态发生改变，所有的依赖对象（观察者对象）都将得到通知。

使用面向对象技术，可以将这种依赖关系弱化，并形成一种稳定的依赖关系。从而实现软件体系结构的松耦合。

## 要点总结

- 目标与观察者松耦合：可以独立改变目标与观察者
- 目标发送通知时，无需指定观察者
- 观察者自己决定是否需要订阅通知
- 是基于事件的 UI 框架中的常用模式
- MVC 模式的重要组成部分

## 变体

- **Push 模式**：主题主动推送数据
- **Pull 模式**：观察者按需拉取数据
- **事件总线**：使用中央事件调度中心

## 相关模式

- [[mediator]] - 中介者模式可以替代观察者
- [[singleton]] - 主题常作为单例
- [[command]] - 命令模式可封装通知