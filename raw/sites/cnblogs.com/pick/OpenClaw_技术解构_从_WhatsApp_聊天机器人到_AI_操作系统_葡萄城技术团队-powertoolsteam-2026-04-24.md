# OpenClaw 技术解构：从 WhatsApp 聊天机器人到 AI 操作系统 - 葡萄城技术团队

> Author: powertoolsteam
> Date: 2026-03-09
> Source: https://www.cnblogs.com/powertoolsteam/p/19688243

---

结构决定功能，历史揭示设计。本文从用户视角出发，向底层追问"它是怎么做到的"。 OpenClaw 是什么？
你在任何聊天窗口给它发一条消息，它就能帮你操作电脑——执行命令、读写文件、浏览网页、操控桌面应用、管理定时任务，甚至语音对话。
和常见的 AI 聊天机器人不同，OpenClaw 运行在你自己的电脑上，不依赖云端服务器。它支持 WhatsApp、Telegram、Discord、Slack、Signal、iMessage 等海外主流平台，也通过插件支持飞书、企业微信、qq等国内渠道。除了消息平台，还有 macOS / iOS / Android 原生应用，以及终端命令行和 Web 控制台。
简单说：20+ 种入口，一个本地 AI 大脑，一套工具集。
graph TB subgraph IN ["用户入口"] MSG["消息渠道<br/>WhatsApp · Telegram · Discord<br/>Slack · Signal · iMessage<br/>飞书 · LINE · Matrix ..."] APP["原生应用<br/>macOS · iOS · Android"] CLI["终端与 Web<br/>TUI 命令行 · Control UI"] end subgraph GW ["Gateway 网关"] R["消息路由"] --- Q["消息队列"] Q --- S["会话管理"] S --- CR["Cron 调度"] CR --- HK["Hook 系统"] HK --- PL["Plugin 注册"] end subgraph AG ["Agent 运行时"] PI["Pi SDK agent loop"] --- PR["Provider 路由"] PR --- FO["Model Failover"] FO --- CE["Context Engine"] end subgraph TL ["工具集"] T1["系统操作<br/>exec · read · write · cron"] T2["网络与感知<br/>browser · web_search · memory"] T3["多媒体与 GUI<br/>peekaboo · canvas · tts"] end MSG --> GW APP --> GW CLI --> GW GW --> AG AG --> T1 AG --> T2 AG --> T3 style MSG fill:#bbdefb,stroke:#333 style APP fill:#bbdefb,stroke:#333 style CLI fill:#bbdefb,stroke:#333 style R fill:#fff9c4,stroke:#333 style Q fill:#fff9c4,stroke:#333 style S fill:#fff9c4,stroke:#333 style CR fill:#fff9c4,stroke:#333 style HK fill:#fff9c4,stroke:#333 style PL fill:#fff9c4,stroke:#333 style PI fill:#e8d5f5,stroke:#333 style PR fill:#e8d5f5,stroke:#333 style FO fill:#e8d5f5,stroke:#333 style CE fill:#e8d5f5,stroke:#333 style T1 fill:#c8e6c9,stroke:#333 style T2 fill:#c8e6c9,stroke:#333 style T3 fill:#c8e6c9,stroke:#333 演化史
OpenClaw 历经 Warelay → Clawdis → Clawdbot → OpenClaw 四次更名，我们看看每次更名时的架构变化。
Warelay："一条管道"
项目名 warelay = WA Relay（WhatsApp 中继）。用户通过 WhatsApp 或短信给 AI 发消息，收到文字回复。没有 Gateway、没有 Agent、没有会话管理——就是一个 webhook 脚本。
graph LR A["Twilio / Baileys<br/>接收消息"] --> B["Express 路由"] --> C["AI API<br/>生成回复"] --> D["原路返回"] style A fill:#f9f,stroke:#333 style D fill:#9f9,stroke:#333
关键选择：用 Baileys（开源 WhatsApp 协议库）而非商业 API 收发 WhatsApp。好处是免费且不依赖第三方服务，代价是 Baileys 要求每台机器只能维持一个 WhatsApp 会话。
Clawdis：最关键的跃迁
这是变化最剧烈的阶段——三件大事同时发生：
引入 Pi SDK：Pi 是一个外部 agent 框架，提供了"消息 → prompt → 调大模型 → 解析工具调用 → 执行 → 循环"的核心 agent 循环（架构详见后文 Pi Agent Runtime 一节）。OpenClaw 从此不再是"收到消息调一次 API"，而是一个真正的 AI agent。
2 周内接入 5 个渠道：Telegram、Discord、Signal、iMessage、WhatsApp——每个平台的消息格式、API 风格、群组概念都不同。多渠道的差异催生了 Adapter 模式和 Channel Dock（统一注册中心），每个渠道只实现它需要的接口子集（详见后文通道适配器一节）。
Gateway 诞生：从一个 CLI 命令行工具变为常驻后台服务，所有客户端通过 WebSocket 统一接入，内含消息路由、队列、会话管理、Cron 调度、Hook 系统和 Plugin 注册。
Clawdbot → OpenClaw：生产化与平台化
核心思路是让新功能通过插件生长，核心代码库不再膨胀。为此落地了三层扩展机制：
插件系统：Plugin SDK + jiti
社区开发插件时，导出一个 register(api) 函数即可声明能力——可注册的类型包括： 渠道（registerChannel）—— 接入新的消息平台
工具（registerTool）—— 给 AI 新的操作能力
钩子（registerHook）—— 在消息流水线的特定节点插入逻辑
HTTP 路由 / CLI 子命令 / 后台服务 —— 扩展 Gateway 和命令行 Node.js 生态的模块格式分裂（ESM vs CJS）是插件加载的主要障碍。OpenClaw 用 jiti（运行时 TypeScript 编译加载器）统一处理：插件不需要预编译，写完直接安装即可运行。目前 40+ 个扩展（飞书、LINE、Matrix、Twitch、语音通话……）都以插件形式存在。
本地记忆：sqlite-vec
让 AI 拥有跨会话的长期记忆。文本切片后转为向量，存入本地 SQLite，用 sqlite-vec 扩展做余弦相似度检索。搜索采用混合策略——向量语义匹配 + BM25 关键词匹配——兼顾"意思相近"和"关键词命中"。所有数据留在本地磁盘，也可通过 MCP 桥接对接外部知识库。
技能市场：ClawHub
插件解决了渠道和工具的扩展，但 AI 的行为模式怎么共享？ClawHub（clawhub.ai）是公开的技能注册中心。技能本质是注入 system prompt 的声明文件，描述 AI 在特定场景下该怎么做（比如"操控 macOS 桌面"、"生成代码后自动运行测试"）。 安装：clawhub install <slug>
加载优先级：workspace > 本地 > 内置
社区治理：点赞、评论、举报，有独立审核机制 项目的 VISION.md 明确要求：新技能应先发布到 ClawHub，不要默认加入核心仓库。 核心子系统拆解
Pi Agent Runtime：OpenClaw 的大脑
OpenClaw 的 agent 能力不是从零自研——它站在 Pi SDK 的肩膀上。Pi 是一个 7 包 monorepo，从底向上分三层：
graph TB subgraph PI_AI ["pi-ai · LLM 抽象层"] direction LR PROV["Provider 注册<br/>Anthropic · OpenAI · Google<br/>Mistral · Bedrock 等 16+"] MODEL["Model 注册表<br/>340KB 自动生成<br/>成本 · 窗口 · 能力"] STREAM["EventStream<br/>统一流式协议<br/>text / thinking / toolcall"] PROV --- MODEL --- STREAM end subgraph PI_AGENT ["pi-agent-core · Agent 运行时"] direction LR LOOP["Agent Loop<br/>prompt → stream → parse<br/>→ execute → loop"] TOOL["Tool 系统<br/>TypeBox schema 校验<br/>流式进度回调"] EVENT["事件总线<br/>turn_start · message_update<br/>tool_execution_end ..."] STEER["Steering + FollowUp<br/>运行中插入新指令<br/>排队后续任务"] LOOP --- TOOL --- EVENT --- STEER end subgraph PI_APP ["pi-coding-agent · SDK 层"] direction LR SDK["createAgentSession()<br/>工具 · 钩子 · 会话一站式初始化"] SESS["SessionManager<br/>JSONL 树形持久化<br/>分支 · 恢复 · 压缩"] CTX["Context 管线<br/>transformContext 裁剪注入<br/>convertToLlm 格式转换"] SDK --- SESS --- CTX end PI_AI --> PI_AGENT --> PI_APP style PROV fill:#e8d5f5,stroke:#333 style MODEL fill:#e8d5f5,stroke:#333 style STREAM fill:#e8d5f5,stroke:#333 style LOOP fill:#d5e8f5,stroke:#333 style TOOL fill:#d5e8f5,stroke:#333 style EVENT fill:#d5e8f5,stroke:#333 style STEER fill:#d5e8f5,stroke:#333 style SDK fill:#c8e6c9,stroke:#333 style SESS fill:#c8e6c9,stroke:#333 style CTX fill:#c8e6c9,stroke:#333
pi-ai（LLM 抽象层） 把 16+ 家模型供应商统一成一个 stream(model, context) 调用。每个供应商实现一个 StreamFunction，把各家私有的流式响应转成标准事件序列（text_delta、thinking_delta、toolcall_start/end）。模型注册表是自动生成的，包含每个模型的成本、上下文窗口、支持的输入类型（文本/图片）和推理能力。
pi-agent-core（Agent 运行时） 提供核心循环：用户消息进入 → 流式调用 LLM → 解析工具调用 → 按序执行工具 → 结果返回 LLM → 继续循环直到 end_turn。工具用 TypeBox 定义参数 schema，运行时自动校验。。
pi-coding-agent（SDK 层） 提供 createAgentSession() 工厂方法，一次性组装工具集、上下文钩子和会话存储。SessionManager 用 JSONL 文件存储对话树（每条消息有 id + parentId），支持分支、恢复和压缩。上下文管线分两步：transformContext() 在 AgentMessage 层面裁剪/注入（比如删掉过旧的消息），convertToLlm() 再把自定义消息类型转成 LLM 能理解的标准格式。
OpenClaw 在 Pi 之上包了六部分，让它从一个通用 agent 框架变成多渠道 AI 助手：
graph LR subgraph PI ["Pi SDK"] PA["agent 循环引擎"] PB["LLM 流式推理"] PC["SessionManager<br/>会话持久化"] end PI --> L1 & L4 subgraph OC1 ["OpenClaw 包装 — 调度层"] L1["Provider 路由<br/>适配不同 LLM 供应商"] L2["Model Failover<br/>主模型挂了自动切备用"] L3["Context Engine<br/>长对话不丢上下文"] end subgraph OC2 ["OpenClaw 包装 — 接入层"] L4["工具注册<br/>exec / browser / memory ..."] L5["Prompt 构建<br/>系统指令 + Skills"] L6["会话订阅<br/>流式回调给渠道"] end style PI fill:#e8d5f5,stroke:#333 style OC1 fill:#fff3e0,stroke:#333 style OC2 fill:#fff3e0,stroke:#333
消息全链路：从收到到回复
一条消息从进入系统到收到回复，经过这样的流水线：
graph TD A["1 收到原始消息"] --> B["2 归一化<br/>不同渠道的消息统一成相同格式"] B --> C["3 去重"] C --> D subgraph D ["4 Hook + 路由"] direction LR D1["触发插件钩子<br/>如自动翻译、日志"] -.- D2["路由解析<br/>确定交给哪个 Agent"] end D --> E subgraph E ["5 媒体理解"] direction LR E1["语音转文字"] -.- E2["图片生成描述"] end E --> F subgraph F ["6 入队策略"] direction LR F1["eager<br/>立即处理"] -.- F2["batch<br/>短时间多条合并处理"] -.- F3["debounce<br/>等用户停止输入再处理"] end F --> G subgraph G ["7 Agent 执行"] direction LR G1["Pi SDK 循环"] -.- G2["Steering 转向"] end G --> H subgraph H ["8 回复调度"] direction LR H1["保证顺序"] -.- H2["仿人打字间隔"] end H --> I["9 投递到渠道"] style A fill:#bbdefb,stroke:#333 style B fill:#bbdefb,stroke:#333 style C fill:#bbdefb,stroke:#333 style D fill:#fff9c4,stroke:#333 style E fill:#fff9c4,stroke:#333 style E1 fill:#fff9c4,stroke:#333 style E2 fill:#fff9c4,stroke:#333 style F fill:#fff9c4,stroke:#333 style F1 fill:#fff9c4,stroke:#333 style F2 fill:#fff9c4,stroke:#333 style F3 fill:#fff9c4,stroke:#333 style G fill:#e8d5f5,stroke:#333 style G1 fill:#e8d5f5,stroke:#333 style G2 fill:#e8d5f5,stroke:#333 style H fill:#c8e6c9,stroke:#333 style H1 fill:#c8e6c9,stroke:#333 style H2 fill:#c8e6c9,stroke:#333 style I fill:#c8e6c9,stroke:#333
两个值得注意的设计： Steering（转向）：agent 正在生成回复时，你又发了一条消息。普通系统会让你等上一条处理完。OpenClaw 通过 session.steer() 把新消息实时注入当前 agent 运行，AI 会立刻调整回复方向——就像你跟人说话时补了一句"等等，我改主意了"。 仿人延迟：回复不是一次性弹出，而是分块发送，块间插入随机间隔，模拟真人打字节奏。在微信、Telegram 这类即时通讯里，这让对话体验更自然。 通道适配器：如何统一 20+ 种渠道
WhatsApp 有"已读回执"，Telegram 有"群组管理 API"，Discord 有"权限体系"，Signal 几乎什么管理接口都没有——每个平台的能力完全不同。
OpenClaw 的做法不是设计一个大而全的接口让每个通道都实现，而是拆成 10 种细粒度 Adapter，每个通道只实现它需要的子集：
graph TB DOCK["Channel Dock 注册中心<br/>渠道发现 · 生命周期管理"] --> A_TG["Telegram"] DOCK --> A_DC["Discord"] DOCK --> A_WA["WhatsApp"] DOCK --> A_MORE["..."] subgraph IFACE ["Adapter 接口（按需实现）"] direction LR subgraph LIFE ["生命周期"] direction LR IF1["Setup"] -.- IF2["Config"] -.- IF6["Status"] -.- IF8["Heartbeat"] end subgraph MSG ["消息收发"] direction LR IF4["Messaging"] -.- IF3["Outbound"] -.- IF10["Media"] end subgraph PERM ["权限与管理"] direction LR IF7["Security"] -.- IF9["Auth"] -.- IF5["Group"] end end A_TG --> IFACE A_DC --> IFACE A_WA --> IFACE A_MORE --> IFACE style DOCK fill:#d5e8f5,stroke:#333 style IFACE fill:#eef4fb,stroke:#333 style LIFE fill:#e8f5e9,stroke:#a5d6a7 style MSG fill:#fff3e0,stroke:#ffcc80 style PERM fill:#fce4ec,stroke:#ef9a9a WhatsApp 实现了 HeartbeatAdapter（Baileys 连接不稳定，需要心跳保活）
Discord 实现了 SecurityAdapter（有复杂的权限体系）
Signal 不需要 GroupAdapter（没有群管理 API） 社区开发新渠道插件（比如飞书、LINE）时，用 Plugin SDK 实现同样的接口即可接入，不需要改动 OpenClaw 核心代码。
Peekaboo Bridge：让 AI 看见并操控 macOS 桌面
你在 WhatsApp 里说"帮我看看屏幕上是什么"，AI 就能截屏、理解画面内容、告诉你答案。你说"点击登录按钮"，它就能找到按钮并点击。你说"打开 Safari 访问某个网址"，它能启动应用、导航页面、在输入框里打字。
这背后是 Peekaboo——一个 macOS 桌面自动化工具，让 AI 拥有"眼睛"和"手"： 看：截屏、录屏、标注 UI 元素（给每个按钮/输入框分配 ID），配合 AI 视觉分析理解画面
操作：点击、打字、按键组合、滚动、拖拽，支持定位到具体 UI 元素
管理：启动/切换/关闭应用，管理窗口大小和位置，切换 macOS 空间，读写剪贴板 典型工作流是：截屏标注 → AI 识别元素 ID → 点击/输入 → 再截屏确认结果，全程在聊天窗口完成。
Peekaboo 内部分为五层：
graph TB subgraph CONSUMER ["消费层"] direction LR CLI["peekaboo CLI<br/>Commander 框架"] APP["macOS App<br/>SwiftUI"] MCP["MCP Server<br/>供外部 AI 调用"] AGENT["内置 Agent<br/>Tachikoma 驱动"] end subgraph BRIDGE ["Bridge IPC 层"] direction LR CLIENT["BridgeClient<br/>连接 UNIX Socket"] HOST["BridgeHost<br/>监听 Socket"] VERIFY["代码签名验证<br/>Team ID 白名单"] CLIENT --> HOST HOST --> VERIFY end subgraph ORCH ["服务编排层"] UAS["UIAutomationService<br/>统一调度入口"] SNAP["SnapshotManager<br/>元素状态缓存"] VIS["Visualizer<br/>点击动画 · 高亮反馈"] UAS --- SNAP UAS --- VIS end DET["ElementDetection<br/>AX 树遍历 · 元素分类<br/>B1/T1 编号"] CAP["ScreenCapture"] CLK["Click · Type<br/>Scroll · Hotkey"] APPCTL["App · Window<br/>Space · Menu"] AX["Accessibility<br/>AXorcist 封装"] SCK["ScreenCaptureKit"] CG["CGEvent<br/>鼠标 · 键盘"] AS["AppleScript<br/>应用管理"] CLI --> BRIDGE APP --> ORCH MCP --> BRIDGE AGENT --> ORCH BRIDGE --> ORCH ORCH --> DET & CAP & CLK & APPCTL DET --> AX CAP --> SCK CLK --> CG APPCTL --> AS style CLI fill:#bbdefb,stroke:#333 style APP fill:#bbdefb,stroke:#333 style MCP fill:#bbdefb,stroke:#333 style AGENT fill:#bbdefb,stroke:#333 style CLIENT fill:#fff9c4,stroke:#333 style HOST fill:#fff9c4,stroke:#333 style VERIFY fill:#fff9c4,stroke:#333 style UAS fill:#e8d5f5,stroke:#333 style SNAP fill:#e8d5f5,stroke:#333 style VIS fill:#e8d5f5,stroke:#333 style DET fill:#c8e6c9,stroke:#333 style CAP fill:#c8e6c9,stroke:#333 style CLK fill:#c8e6c9,stroke:#333 style APPCTL fill:#c8e6c9,stroke:#333 style AX fill:#ffe0b2,stroke:#333 style SCK fill:#ffe0b2,stroke:#333 style CG fill:#ffe0b2,stroke:#333 style AS fill:#ffe0b2,stroke:#333
几个关键设计：
Bridge 权限代理：macOS 的屏幕录制和辅助功能权限是按应用授予的，但 AI agent 调用的是命令行工具 peekaboo。解决方案是 Bridge 架构——CLI 通过 UNIX Socket 连接到 OpenClaw.app 内的 BridgeHost 进程，BridgeHost 先验证调用方的代码签名和 Team ID，通过后才借出 app 已获得的权限执行操作。用户只需在系统设置里授权一次 OpenClaw.app。
元素检测与编号：see 命令截屏时同步遍历 Accessibility 树（通过 AXorcist 封装），把每个可交互元素按类型编号——按钮 B1、B2，输入框 T1、T2。遍历有严格限制（深度 8 层、子节点 200 个、150ms 超时），检测结果缓存 1.5 秒避免重复遍历。后续 click B1 就能直接定位到目标元素。
内置 Agent 模式：Peekaboo 自带基于 Tachikoma 的 agent 循环（支持 OpenAI / Anthropic / Ollama 等模型），可以独立执行多步桌面任务——不经过 OpenClaw，直接 peekaboo agent "打开备忘录写一条待办" 就能截屏→分析→点击→输入→确认，循环直到完成。
Skill 注入：Peekaboo 在 OpenClaw 中不是代码依赖，而是一个 Skill——通过声明文件标注"仅限 macOS、需要 peekaboo 命令"，运行时按需注入到 AI 的 system prompt 中。非 macOS 用户完全不会感知到它的存在。
Cron 调度器：不只是定时器
OpenClaw 的定时任务不是简单的 setInterval，而是一个生产级调度系统： SHA256 Stagger：如果设了 100 个"每天早上 9 点"的定时任务，不会同时触发——用每个任务 ID 的 SHA256 哈希做确定性偏移（默认在 5 分钟窗口内分散），避免所有任务同一秒涌入，导致 CPU 和 API 调用瞬间过载
两种执行模式：systemEvent（把消息注入主会话，像用户发了一条消息）和 agentTurn（启动独立的 agent 运行，互不干扰）
容错：失败后指数退避重试（30 秒 → 1 分钟 → 5 分钟 → 15 分钟 → 1 小时），连续失败触发告警，临时 session 自动回收 Context Engine：长对话不丢记忆
大模型有上下文窗口限制（比如 200K token），聊久了上下文就超出。Pi SDK 提供了底层的两步上下文管线（transformContext 裁剪 + convertToLlm 格式转换），OpenClaw 在此基础上实现了自己的 Context Engine，定义了 5 个生命周期钩子：
bootstrap（初始化）→ ingest（新消息进入）→ assemble（组装发给模型的上下文）→ compact（上下文太长时压缩）→ afterTurn（一轮对话结束后清理）
默认实现用 token 计数 + LLM 摘要来压缩历史对话。但整个 Context Engine 是可插拔的——社区可以写一个完全不同的实现（比如基于向量检索的长期记忆），替换进去不需要改一行核心代码。 结语
阿瑟·克拉克在《2001：太空漫游》里写过一个场景：猿人月亮观察者捡起一块石头，第一次意识到自己的手可以延伸。从那一刻起，工具就不再是身体之外的东西——它是意志的投射。
我们今天拆解的这些——Gateway、Adapter、Bridge、Agent Loop——本质上都是同一件事的不同切面：让意图穿透介质。用户在 WhatsApp 里打一行字，意图穿过消息协议、穿过归一化流水线、穿过 LLM 的概率空间、穿过工具调用，最终变成屏幕上的一次点击、磁盘上的一个文件、终端里的一行输出。中间的每一层抽象，都是为了让这条路径上的摩擦再少一点。
这让人想起维纳在《控制论》里的判断：智能的本质不是计算，而是与环境之间有效的信息交换。OpenClaw 做的事情，与其说是"让 AI 用工具"，不如说是在缩短人的意图与世界的状态之间的距离。 本文是由葡萄城技术开发团队发布，转载请注明出处：葡萄城官网
了解企业级低代码开发平台，请前往活字格
了解可嵌入您系统的在线 Excel，请前往SpreadJS纯前端表格控件
了解嵌入式的商业智能和报表软件，请前往Wyn Enterprise