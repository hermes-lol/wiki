# Wiki Schema

## Domain
计算科学与工程 (Computational Science and Engineering)

覆盖领域：
- 计算物理 (Computational Physics)
- 计算化学 (Computational Chemistry)
- 计算生物学 (Computational Biology)
- 计算医学 (Computational Medicine)
- 计算数学 (Computational Mathematics)
- 科学机器学习 (Scientific Machine Learning)
- 计算机科学 (Computer Science)
  - 编程语言与范式 (Programming Languages & Paradigms)
  - 数据结构与算法 (Data Structures & Algorithms)
  - 并发与分布式系统 (Concurrency & Distributed Systems)
  - 系统设计与架构 (System Design & Architecture)
  - 软件工程实践 (Software Engineering Practices)

核心主题：算法、数值方法、软件工具、代码实现、性能优化、系统架构、应用案例

## Conventions
- 文件名：小写，连字符，无空格 (如 `molecular-dynamics.md`)
- 每个页面以 YAML frontmatter 开头
- 使用 `[[wikilinks]]` 链接页面 (每页至少 2 个出站链接)
- 更新页面时更新 `updated` 日期
- 新页面必须添加到 `index.md` 对应分类下
- 每次操作必须追加到 `log.md`
- **来源标记：** 综合 3+ 来源的页面，在段落末尾追加 `^[raw/articles/source.md]` 标记

## Frontmatter
```yaml
---
title: 页面标题
created: YYYY-MM-DD
updated: YYYY-MM-DD
type: entity | concept | comparison | query | summary
tags: [from taxonomy below]
sources: [raw/articles/source-name.md]
# Optional quality signals:
confidence: high | medium | low        # 结论的支持程度
contested: true                        # 存在未解决的矛盾
contradictions: [other-page-slug]      # 冲突的页面
---
```

## Tag Taxonomy
- **Methods (方法):** numerical-method, simulation, optimization, ml-method, algorithm, data-structure
- **Domains (领域):** physics, chemistry, biology, medicine, mathematics, computer-science, cross-domain
- **Programming (编程):** programming-language, java, python, c-cpp, concurrency, distributed-system
- **Software (软件):** software, library, framework, benchmark, database
- **Architecture (架构):** system-design, microservices, performance, scalability
- **Applications (应用):** drug-discovery, materials-design, protein-folding, medical-imaging, quantum-sim
- **Theory (理论):** theory, mathematical-foundations, convergence, complexity
- **Meta (元):** comparison, tutorial, best-practices, controversy, open-problem, interview

## Page Thresholds
- **创建页面：** 实体/概念出现在 2+ 来源中，或在一个来源中是核心内容
- **更新现有页面：** 来源提及已覆盖的内容
- **不创建页面：** 轻微提及、次要细节、或超出领域范围
- **拆分页面：** 当页面超过 200 行时，拆分为子主题并交叉链接
- **归档页面：** 当内容完全被取代时，移至 `_archive/`，从 index 移除

## Entity Pages
一个实体一个页面。包括：
- 概述 / 是什么
- 关键事实和日期
- 与其他实体的关系 ([[wikilinks]])
- 来源引用

## Concept Pages
一个概念一个页面。包括：
- 定义 / 解释
- 当前知识状态
- 开放问题或争议
- 相关概念 ([[wikilinks]])

## Comparison Pages
对比分析。包括：
- 比较什么以及为什么
- 比较维度 (表格格式优先)
- 结论或综合
- 来源

## Versioned Knowledge Policy
对于版本化的知识（如开发文档、框架 API、工具链）：

### 1.5 版本保留策略
- **当前版本 (1.0)**：完整保留 raw source
- **差异文档 (0.5)**：保留上一版本与当前版本的差异说明文件
- **更旧版本**：直接删除，不归档（快速迭代领域，旧版本无价值）

### 差异文档格式
```
raw/docs/
├── spring-boot-3.4/           # 当前版本完整文档
├── spring-boot-3.3-delta.md   # 3.3 → 3.4 差异说明
└── (3.2 及更早版本已删除)
```

差异文档 `*-delta.md` 格式：
```yaml
---
from_version: "3.3"
to_version: "3.4"
date: YYYY-MM-DD
breaking_changes: [item1, item2]
deprecations: [item3]
new_features: [item4, item5]
---
# 3.3 → 3.4 变更摘要

## Breaking Changes
- ...

## Deprecations
- ...

## New Features
- ...

## Migration Notes
- ...
```

### 兼容性 Snippet
当版本差异导致兼容性问题时，单独创建 snippet：
```
raw/snippets/
└── spring-boot-3.3-to-3.4-migration.md  # 迁移指南
```

Wiki 页面引用时标注版本范围：
```yaml
version: "3.4"
version_min: "3.3"  # 最低兼容版本
```

## Update Policy
当新信息与现有内容冲突时：
1. 检查日期 — 较新的来源通常取代较旧的
2. 如果真正矛盾，同时记录两个观点及其日期和来源
3. 在 frontmatter 标记矛盾：`contradictions: [page-name]`
4. 在 lint 报告中标记为用户审查