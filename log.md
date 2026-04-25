# Wiki Log

> 所有 wiki 操作的按时间顺序记录。仅追加。
> 格式：`## [YYYY-MM-DD] action | subject`
> 操作类型：ingest, update, query, lint, create, archive, delete
> 当此文件超过 500 条目时，轮转：重命名为 log-YYYY.md，重新开始。

## [2026-04-22] create | Wiki initialized
- Domain: 自然科学的计算方法 (Computational Methods in Natural Sciences)
- Structure created with SCHEMA.md, index.md, log.md
- Subdirectories: raw/, entities/, concepts/, comparisons/, queries/, _archive/

## [2026-04-22] update | Domain expanded
- Domain renamed: 计算科学与工程 (Computational Science and Engineering)
- Added: 计算机科学 (Computer Science) with subcategories
  - 编程语言与范式
  - 数据结构与算法
  - 并发与分布式系统
  - 系统设计与架构
  - 软件工程实践
- New tag categories: Programming, Architecture
- New tags: data-structure, computer-science, programming-language, java, python, c-cpp, concurrency, distributed-system, database, system-design, microservices, performance, scalability, interview
- Files updated: SCHEMA.md, index.md

## [2026-04-22] ingest | JavaGuide imported
- Source: JavaGuide (Snailclimb/JavaGuide GitHub repo)
- Path: raw/sites/javaguide.cn/
- Files: 315 markdown files
- Coverage: Java, 并发, JVM, 数据库, 分布式系统, 系统设计, AI Agent, 面试准备
- Status: raw source archived, pending wiki page generation

## [2026-04-22] delete | humanlayer raw source removed
- Path: raw/articles/humanlayer/
- Reason: user requested deletion
- Status: deleted

## [2026-04-22] ingest | CodeFather.cn imported
- Source: CodeFather.cn (codefather.cn 编程导航网站)
- Path: raw/sites/codefather.cn/
- Files: 910 markdown files
- Coverage: Java, 编程分享, 面试题, 项目实战, 学习路线, 知识碎片
- Status: raw source archived, pending wiki page generation

## [2026-04-22] ingest | 李东风老师电子书 imported
- Source: 李东风老师统计计算与R语言电子书
- Path: raw/sites/math.pku.edu.cn_teachers_lidf/
- Files: 181 markdown files
- Coverage:
  - Rbook/: R编程、ggplot2、统计建模、机器学习
  - atsa/: 应用时间序列分析 (ARIMA、频谱、状态空间)
  - fts/: 金融时间序列 (GARCH、Cointegration、VAR)
  - statcomp/: 统计计算 (数值分析、优化、MCMC、随机数)
- Status: raw source archived, pending wiki page generation

## [2026-04-22] ingest | Cpp-Design-Patterns imported
- Source: liu-jianhao/Cpp-Design-Patterns (GitHub)
- Path: raw/sites/github.com_liu-jianhao_Cpp-Design-Patterns/
- Files: 30 markdown files
- Coverage: 23种设计模式 (Singleton, Factory, Builder, Observer, Strategy, Decorator, etc.)
- Status: raw source archived, pending wiki page generation

## [2026-04-22] lint | Wiki health check
- Issues found: 39
  - Index 未同步: 5 个页面 (docker, jvm, mysql, redis, spring 未列入)
  - 断链: 29 个 (目标页面不存在)
  - 孤儿页面: 5 个 (docker, jvm, mysql, redis, spring 无入站链接)
  - Frontmatter 缺失: 11 个页面
- Action: 同步 index.md，移除待创建占位符

## [2026-04-23] ingest | 设计模式 (Cpp-Design-Patterns)
- Source: liu-jianhao/Cpp-Design-Patterns (GitHub)
- Path: raw/sites/github.com_liu-jianhao_Cpp-Design-Patterns/
- Files processed: 23 种 GoF 设计模式
- Wiki pages created:
  - concepts/design-patterns.md (总览页面)
  - entities/singleton.md
  - entities/factory-method.md
  - entities/abstract-factory.md
  - entities/builder.md
  - entities/prototype.md
  - entities/observer.md
  - entities/strategy.md
  - entities/template-method.md
  - entities/command.md
  - entities/decorator.md
  - entities/proxy.md
  - entities/adapter.md
  - entities/facade.md
  - entities/bridge.md
  - entities/composite.md
  - entities/flyweight.md
  - entities/state.md
  - entities/mediator.md
  - entities/memento.md
  - entities/iterator.md
  - entities/chain-of-responsibility.md
  - entities/visitor.md
  - entities/interpreter.md
- Total wiki pages: 11 → 35
- Cross-references: 每页至少 2 个 wikilinks
- Status: complete

## [2026-04-23] update | Domain expanded
- 新增: 1.5 版本保留策略
  - 当前版本完整保留
  - 上一版本保留差异文档 (delta.md)
  - 更旧版本直接删除
- 新增: 差异文档格式规范 (breaking_changes, deprecations, new_features)
- 新增: 兼容性 Snippet 机制 (raw/snippets/)
- 新增: Wiki 页面版本标注 (version, version_min)
- Files updated: SCHEMA.md

## [2026-04-24] query | 掘金热门文章排行榜
- Source: juejin-skills (https://github.com/wscats/juejin-skills)
- 功能: 获取掘金 8 个分类的热门文章排行榜
- Wiki page: queries/juejin-hot-2026-04-24.md
- 分类: 后端, 前端, Android, iOS, 人工智能, 开发工具, 代码人生, 阅读
- Top 文章: 全面封禁 Cursor！、豆包 AI 编程 9.9/月 等
- Status: complete

## [2026-04-23] create | Git sparse checkout snippet
- 新增: raw/snippets/git-sparse-checkout.md
- 内容: Git 稀疏检出完整流程、参数说明、实际案例
- 来源: 用户实践 (github.com_cucker_docker 仓库 md/ 目录检出)

## [2026-04-23] ingest | github.com_cucker_docker imported
- Source: github.com/cucker/docker (稀疏检出 md/ 目录)
- Path: raw/sites/github.com_cucker_docker/
- Files: 29 文件 (27 md, 2 json)
- Coverage: Docker 核心、Dockerfile 详解、docker-compose、容器卷、镜像操作、多进程容器
- Status: raw source archived, pending wiki page generation

## [2026-04-23] query | Docker 多任务守护脚本分析
- Source: 用户提供的 shell 脚本 (multi-task-watchdog.sh)
- Path: raw/snippets/multi-task-watchdog.md
- Analysis: 脚本四大优化点 + 核心架构 + 使用建议
- Wiki page: queries/docker-multi-task-watchdog-analysis.md
- Total pages: 35 → 36
- Status: complete
