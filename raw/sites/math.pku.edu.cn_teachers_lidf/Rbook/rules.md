---
crawl_time: '2026-01-17 14:20:02'
framework: rbook
title: 59 使用经验 | R语言教程
url: https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/rules.html
---

# [R语言教程](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/)

# 59 使用经验

## 59.1 文件管理

### 59.1.1 工作空间

R和RStudio软件提供了保存工作空间的功能。
我建议不要自动保存工作空间，
而是用`save()`函数保存重要的数据、变量到硬盘中，
需要时用`load()`函数载入。

## 59.2 程序格式

源程序必须按照一定的格式编写。其中比较重要的是：

* 必须有完善的注释。
  每个源文件和每个函数都需要有完整注释。
  函数内的算法不是很显然的情况下也应该用注释说明。
* 按照一定规则进行缩进。
  函数体必须缩进；
  `if`、`while`等结构必须缩进，
  结构嵌套时进行更多的缩进对齐。

**未完待续**