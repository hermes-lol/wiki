---
title: 职责链模式 (Chain of Responsibility)
created: 2026-04-23
updated: 2026-04-23
type: entity
tags: [software, design-pattern]
sources: [raw/sites/github.com_liu-jianhao_Cpp-Design-Patterns/Chain of Resposibility/README.md]
confidence: low
---

# 职责链模式 (Chain of Responsibility)

使多个对象都有机会处理请求，从而避免请求的发送者和接收者之间的耦合关系。将这些对象连成一条链，并沿着这条链传递请求，直到有一个对象处理它为止。

## 动机

一个请求可能被多个对象处理，但是每个请求在运行时只能有一个接收者，如果显式指定，将必不可少地带来请求发送者与接收者的紧耦合。

如何使请求的发送者不需要指定具体的接收者？让请求的接收者自己在运行时决定来处理请求，从而使两者解耦。

## 要点总结

- 应用于"一个请求可能有多个接受者，但是最后真正的接受者只有一个"的场景
- 请求发送者与接受者有可能出现"变化脆弱"的症状，职责链解耦
- 有些过时，现代语言有更好的事件处理机制

## 应用场景

- 事件处理链
- 权限验证链
- 日志处理链

## 相关模式

- [[composite]] - 职责链可以用组合结构实现
- [[command]] - 命令可以沿着链传递
- [[observer]] - 观察者也可以形成链