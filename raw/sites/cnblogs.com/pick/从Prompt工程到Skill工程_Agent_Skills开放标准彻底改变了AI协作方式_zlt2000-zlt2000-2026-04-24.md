# 从Prompt工程到Skill工程：Agent Skills开放标准彻底改变了AI协作方式 - zlt2000

> Author: zlt2000
> Date: 2026-02-05
> Source: https://www.cnblogs.com/zlt2000/p/19577443

---

一、为什么 Agent Skill 突然火了？
你是不是也有过这样的崩溃时刻？ 每次让 Claude 写代码，都要重复粘贴 请使用我们的代码规范：驼峰命名、2空格缩进、必须写单元测试 ——像极了每天入职新公司；
好不容易调教好的 Prompt 换个项目就完全失效，之前的调教经验归零；
团队里每个人给 AI 的指令不一样，导致输出的内容一会儿像资深架构师，一会儿像刚毕业的新手。 这些问题的根源，其实是 AI 的 专业能力无法沉淀。直到 2025 年 10 月 Anthropic 推出 Agent Skill（又名 Claude Code Skill）正是为解决这些问题而生。这不仅是 Claude 的新功能，更是一个 开放的跨平台标准，目前已被 OpenAI、Cursor、Trae 等主流工具跟进支持。
本文将带你从 是什么 到 怎么用在实际工作中，彻底掌握这个比 Prompt 更高级、比 MCP 更易用的 AI 编程神器。 二、到底什么是 Agent Skill？
用最通俗的比喻：Agent Skill 是 AI 的 入职手册 + 工具箱。
想象你招了一位天才实习生 Claude 他智商极高但不懂你们公司的业务。传统的做法是每次布置任务都口头交代一遍 Prompt 而 Agent Skill 则是给他一本完整的标准作业程序 SOP： 📋 入职手册（SKILL.md）：包含岗位描述、工作流程、注意事项
🧰 工具箱（Scripts）：处理特定任务的脚本和代码
📚 参考资料（References）：行业规范、模板素材、API文档 技术本质：Agent Skill 是一个标准化的文件夹结构，核心必须包含 SKILL.md 文件（YAML元数据 + Markdown说明），可选包含脚本、模板等资源文件。
my-skill/ # 技能包根目录
├── SKILL.md # 📄 核心文件：元数据 + 工作流指令（必须）
├── scripts/ # 🔧 可选：自动化脚本（Python/Bash）
├── references/ # 📖 可选：专业文档、API手册、FAQ
└── assets/ # 🎨 可选：模板、示例、静态资源 当 AI 检测到相关任务时，会自动 翻开 对应的手册，严格按照既定流程执行，无需你每次都重复交代。 三、Skill工作原理
Skill 最精妙的设计，是它的 渐进式加载机制 —— 就像你查字典，先看目录，再翻对应章节，最后查附录，不会一上来就把整本书塞进脑子里。
3.1. 三层加载：用最少的 Token 做最多的事 加载层级
内容类型
加载时机
作用 L1
元数据（名片）
Agent 启动时自动加载
让AI知道“有什么技能可用” L2
说明文档（正文）
匹配用户需求时加载
教AI“具体怎么做” L3
资源文件（脚本 / 模板）
执行中按需加载
提供“工具/素材支持” 3.2. 四步执行流程 🎯 意图匹配：AI 扫描所有 Skill 的元数据，找到最匹配当前任务的技能
📖 读取指南：加载对应 SKILL.md，掌握执行步骤、检查点、输出规范
🔧 按需执行：调用 scripts/ 中的脚本，查询 references/ 中的资料
✅ 反馈结果：按模板输出成果，或询问缺失信息 四、现有技术的对比
4.1. Agent Skill vs Prompt 维度
普通 Prompt
Agent Skill 性质
临时指令，用完即走
标准化流程，永久复用 加载方式
每次全量输入
按需渐进加载 稳定性
依赖模型"记忆"，易漂移
固化检查点，强制执行 管理
分散在聊天记录里
文件化、版本可控 共享
复制粘贴，易丢失格式
整包分享，开箱即用 一句话总结：Prompt 是 口头交代，Skills 是书面 SOP + 工具箱。 4.2. Agent Skill vs 多 Agent 架构 维度
多 Agent 架构
Agent Skill 复杂度
重量级，需要架构设计
轻量级，单个文件夹即可 适用场景
复杂并行任务（如研究+写作+审核同时进行）
单领域深度任务（如专业代码审查） 资源消耗
高，需调度多个 Agent 实例
低，单 Agent 内能力切换 启动成本
需要搭建 Agent 框架
零成本，复制文件夹即可 关系
体系级解决方案
单元级能力模块，可被多 Agent 调用 4.3. Agent Skill vs MCP 维度
MCP
Agent Skill 定位
连接协议：AI 与外部系统的"USB 接口"
执行标准：AI 做事的"操作手册" 解决的问题
能不能连（访问数据库、API、文件系统）
怎么做（流程、规范、最佳实践） 技术形态
需要运行 MCP Server（TypeScript/Python）
静态文件夹（Markdown + 脚本） 加载时机
启动时建立连接
按需渐进加载 关系
互补：MCP 提供“工具”
Skills 提供“使用指南” MCP 让 AI 能连上数据库，Skill 教 AI 怎么按你们公司的规范查数据、生成报表、处理异常。两者配合，AI 才能真正成为"懂行的专家"。 五、创建你的第一个 Agent Skill
下面用 会议纪要整理助手 为例，从零创建一个 Skill
场景：开会录音转文字后，需要整理成结构化会议纪要。不同会议类型（周会/项目复盘/客户沟通）需要不同的整理模板。
5.1. 创建 Skill 文件夹结构
新建一个名为 meeting-minutes 的文件夹，总体的文件结构如下：
/meeting-minutes/
├── SKILL.md # L1：技能元数据，L2：内容
├── references/ # L3：按会议类型按需加载
│ ├── weekly-rule.md # 周会模板
│ ├── retro-rule.md # 复盘模板
│ └── client-rule.md # 客户沟通模板 5.2. SKILL.md（核心文件）
5.2.1. 元数据
在 SKILL.md 文件最开头以上下两个 --- 作为元数据标识
---
name: meeting-minutes
description: 办公室通用会议纪要整理助手，支持周会/项目复盘会/客户沟通会三类场景，自动识别会议类型，按需加载对应会议规则，智能提取关键信息，输出结构化纪要。
--- 5.2.2. SKILL内容 5.3. 编写模块化配置references 通过文件分离，AI每次只读取当前任务所需的规则，避免 Context 污染 5.4. 测试你的 Skill（以 Trae 为例）
Trae 作为国内的 AI IDE 已原生支持 Agent Skills 官网：https://www.trae.cn/ 下载并安装 TRAE IDE 5.4.1. 导入Skill 创建一个文件夹，例如 my_skills
使用 TRAE IDE 打开这个文件夹
将 meeting-minutes 文件夹复制到 my_skills/.trae/skills/ 目录下 5.4.2. 输入提示词
需要切换为 SOLO 模式，然后在对话框输入以下提示词：
帮我生成周会会议纪要 原始文本：
小明：用户模块我搞完了，已经提测。
小红：接口文档我还没弄，我负责写，周五前给出来。
张三：测试环境那个问题搞不定，需要运维老陈帮忙看看。
李四：下周我打算开始订单模块，周三前出个技术方案看看。
王五：数据库设计谁review一下？
小明：我来吧，不过得明天才有空。 5.4.3. 执行Skill 5.4.4. 最终输出以下内容 六、本文Skill下载地址
本文案例 会议纪要整理助手 Skill 的下载地址如下： Gitee地址： https://gitee.com/zlt2000/my-agent-skill/tree/master/meeting-minutes Github地址： https://github.com/zlt2000/my-agent-skill/tree/master/meeting-minutes 在实际使用过程中本文 Skill 还可以进行以下迭代优化： 在 references 里扩展更多的 会议类型 模板；
在 script 文件夹写 Python 脚本，实现输出内容 导出word文档 或者 同步给飞书。 七、总结
Agent Skills 的正式发布，标志着 AI 协作从 提示词工程 正式迈入 技能工程 的全新范式。它将人类专家的经验、标准化流程与行业最佳实践，封装成 AI 可理解、可执行、可复用的数字资产。
核心价值优势： 降本增效： 通过渐进式披露、按需加载机制，大幅减少 Token 消耗，同时让 AI 聚焦核心任务，推理效率与执行稳定性同步提升；
跨平台互通： 作为开放标准，实现 “一次构建、多端复用”，Skill 可无缝适配 Claude、Cursor、Trae、Copilot 等主流平台，打破工具壁垒；
Skill 市场： 构建起类似 VS Code 插件市场的 Skill 生态，官方与社区共同打造技能商店，让专业能力可分享、可迭代、可规模化应用。