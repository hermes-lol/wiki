---
title: 解释器模式 (Interpreter)
created: 2026-04-23
updated: 2026-04-23
type: entity
tags: [software, design-pattern]
sources: [raw/sites/github.com_liu-jianhao_Cpp-Design-Patterns/Interpreter/README.md]
confidence: low
---

# 解释器模式 (Interpreter)

给定一个语言，定义它的文法的一种表示，并定义一种解释器，这个解释器使用该表示来解释语言中的句子。

## 动机

如果某一特定领域的问题比较复杂，类似的结构不断重复出现，如果使用普通的编程方式来实现将面临非常频繁的变化。

在这种情况下，将特定领域的问题表达为某种语法规则下的句子，然后构建一个解释器来解释这样的句子，从而达到解决问题的目的。

## 要点总结

- 适合"业务规则频繁变化，且类似的结构不断重复出现，并且容易抽象为语法规则的问题"
- 使用面向对象技巧来方便地"扩展"文法
- 适合简单的文法表示，对于复杂的文法表示需要求助语法分析器标准工具

## 现代视角

- 很少直接使用， parser/lexer 工具更强大
- 可用于简单 DSL 的实现

## 相关模式

- [[visitor]] - 解释器常用访问者实现
- [[composite]] - 解释器的 AST 通常是组合结构
- [[flyweight]] - 共享终结符节点