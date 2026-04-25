---
---

# Claude Code 技能与MCP完整指南：32个Skills + 8个MCP服务器

> **核心观点**：Skills让Claude更聪明（提示词/工作流），MCP让Claude更能干（工具/API服务），两者搭配才能发挥最大效能。

---

## 一、核心概念：Skills vs MCP

| 对比维度 | Skills 技能 | MCP 服务器 |
|---------|------------|-----------|
| **核心本质** | 提示词/标准化工作流封装 | 本地运行的工具/API服务 |
| **安装方式** | `npx skills add` 一键安装 | 修改 `mcp.json` 配置文件 |
| **运行位置** | Claude 大模型内部 | 本地独立进程 |
| **访问外部资源** | 不支持 | 支持本地系统、浏览器、第三方服务 |
| **额外依赖** | 仅需node环境，无需API Key | 部分需要API Key |

---

## 二、Skills技能全指南（32个精选）

### 2.1 安装与管理命令

```bash
# 搜索社区技能
npx skills find <关键词>

# 安装技能（-y跳过确认，-g全局安装，必加！）
npx skills add <owner/repo@skill> -y -g

# 查看已安装的全部技能
npx skills list -g

# 检查/更新技能
npx skills check
npx skills update
```

> **关键提醒**：安装必须加 `-g` 参数，完成后**必须重启Claude Code**才能生效。

**官方技能市场**：[skills.sh](https://skills.sh/)

### 2.2 32个精选技能分类清单

#### 🔧 必装入口类（1个）

| 技能 | 安装命令 | 核心能力 | 安装量 |
|-----|---------|---------|-------|
| **find-skills** | `npx skills add find-skills -y -g` | 技能市场内置搜索引擎，支持关键词匹配、热门推荐 | 159.6K |

#### 🎨 前端开发全栈类（9个）

| 技能 | 安装命令 | 核心能力 | 安装量 |
|-----|---------|---------|-------|
| **frontend-design** | `npx skills add frontend-design -y -g` | 网页/Dashboard/落地页设计，React/Vue组件生成，暗黑/玻璃态风格 | 52.7K |
| **web-artifacts-builder** | `npx skills add web-artifacts-builder -y -g` | 带路由/状态管理/组件库的复杂SPA构建 | - |
| **canvas-design** | `npx skills add canvas-design -y -g` | 架构图/流程图/技术示意图生成，支持导出PNG/PDF | 6.1K |
| **theme-factory** | `npx skills add theme-factory -y -g` | 10+预设主题，一键统一文档/PPT/HTML视觉风格 | - |
| **vercel-react-best-practices** | `npx skills add vercel-labs/agent-skills@vercel-react-best-practices -y -g` | React代码规范检查、性能优化、Hooks最佳实践 | 109.8K |
| **web-design-guidelines** | `npx skills add vercel-labs/agent-skills@web-design-guidelines -y -g` | 设计系统规范、响应式布局、视觉一致性检查 | 83.1K |
| **vercel-composition-patterns** | `npx skills add vercel-labs/agent-skills@vercel-composition-patterns -y -g` | 组件复用策略、组合模式、状态管理方案 | 29.7K |
| **shadcn** | `npx skills add shadcn/ui@shadcn -y -g` | shadcn/ui组件使用指导、样式定制、一键生成业务组件 | - |
| **vercel-react-native-skills** | `npx skills add vercel-labs/agent-skills@vercel-react-native-skills -y -g` | RN开发最佳实践、跨平台适配、原生模块集成 | 21.6K |

#### 📄 文档与办公处理类（6个）

| 技能 | 安装命令 | 核心能力 | 安装量 |
|-----|---------|---------|-------|
| **technical-writer** | `npx skills add technical-writer -y -g` | 标准化README/API文档/技术教程生成，中英文翻译 | - |
| **doc-coauthoring** | `npx skills add doc-coauthoring -y -g` | 技术提案(RFC)/系统设计文档/团队规范文档撰写 | - |
| **docx** | `npx skills add docx -y -g` | Word文档创建/编辑/格式转换，Markdown转Word | 8.6K |
| **pptx** | `npx skills add pptx -y -g` | 从文档/Markdown一键生成PPT，编辑/合并/拆分 | 9.2K |
| **pdf** | `npx skills add pdf -y -g` | PDF合并/拆分/OCR/水印/表单填写/格式转换 | 11.1K |
| **xlsx** | `npx skills add xlsx -y -g` | Excel数据清洗/公式计算/图表生成/批量处理 | 8.6K |

#### 🏗️ 架构设计与代码质量类（5个）

| 技能 | 安装命令 | 核心能力 |
|-----|---------|---------|
| **planning-with-files** | `npx skills add planning-with-files -y -g` | 自动拆解任务，生成task_plan.md/progress.md，支持会话恢复 |
| **project-planner** | `npx skills add shubhamsaboo/awesome-llm-apps@project-planner -y -g` | 需求文档/架构设计/分阶段计划/技术风险评估 |
| **architecture-patterns** | `npx skills add wshobson/agents@architecture-patterns -y -g` | 根据场景推荐架构模式，讲解优缺点与适用场景 |
| **architecture-decision-records** | `npx skills add wshobson/agents@architecture-decision-records -y -g` | 标准化ADR架构决策记录，记录背景/选型/备选方案 |
| **requesting-code-review** | `npx skills add obra/superpowers@requesting-code-review -y -g` | 全维度代码审查，发现bug/安全风险，给出优化建议 |

#### 🧠 记忆与上下文管理类（3个）

| 技能 | 安装命令 | 核心能力 |
|-----|---------|---------|
| **memory-intake** | `npx skills add memory-intake -y -g` | 将调试经验/架构决策/踩坑记录存入记忆库，跨会话调用 |
| **memory-audit** | `npx skills add memory-audit -y -g` | 检查记忆库过时/无效内容，生成质量报告 |
| **memory-evolution** | `npx skills add memory-evolution -y -g` | 分析记忆使用模式，精简冗余，优化关联结构 |

#### 🧪 测试与自动化类（2个）

| 技能 | 安装命令 | 核心能力 | 安装量 |
|-----|---------|---------|-------|
| **webapp-testing** | `npx skills add webapp-testing -y -g` | 基于Playwright生成E2E测试，页面导航/表单填写/截图 | 7.6K |
| **test-driven-development** | `npx skills add obra/superpowers@test-driven-development -y -g` | 引导"红绿重构"循环，先写测试再写实现 | 6.5K |

#### ⚡ 开发提效类（4个）

| 技能 | 安装命令 | 核心能力 | 安装量 |
|-----|---------|---------|-------|
| **brainstorming** | `npx skills add obra/superpowers@brainstorming -y -g` | 多角度分析问题，快速生成多套解决方案 | 13.4K |
| **systematic-debugging** | `npx skills add obra/superpowers@systematic-debugging -y -g` | 结构化bug排查流程，逐步定位根因 | 7.5K |
| **writing-plans** | `npx skills add obra/superpowers@writing-plans -y -g` | 拆解复杂任务，生成分步骤实施计划 | 6.4K |
| **executing-plans** | `npx skills add obra/superpowers@executing-plans -y -g` | 按计划分步执行，实时追踪进度 | - |

#### 🔒 安全审计类（1个）

| 技能 | 安装命令 | 核心能力 | 安装量 |
|-----|---------|---------|-------|
| **audit-website** | `npx skills add squirrelscan/skills@audit-website -y -g` | 网站安全漏洞扫描，安全配置检查，生成审计报告 | 15.3K |

#### 🛠️ 自定义技能开发类（1个）

| 技能 | 安装命令 | 核心能力 | 安装量 |
|-----|---------|---------|-------|
| **skill-creator** | `npx skills add skill-creator -y -g` | 引导创建自定义技能，封装重复工作流，支持发布社区 | 26.1K |

### 2.3 一键安装脚本

#### 新手入门包

[... summary truncated for context management ...]